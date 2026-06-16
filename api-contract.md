# API Contract Protocol

The OpenAPI spec is the single source of truth. The backend produces it cleanly;
the frontend consumes it through generated code. Neither side hand-writes what the
other already declares.

```
backend  →  OpenAPI spec  →  codegen  →  generated client  →  feature facade  →  component
            (one source)     (one cmd)    (never edited)       (one import point)
```

Violating either side breaks the contract silently. A URL hand-written on the frontend,
a missing `response_model` on the backend, a renamed function without regenerating — all
produce the same class of bug: frontend and backend diverge while tests pass.

---

## Backend contract

### 1. Operation IDs — one naming rule, applied globally

Every endpoint gets a stable, predictable `operationId` derived from its handler function
name using one global rule set at app startup. The rule must produce:

- A unique ID across the entire API (collisions break codegen)
- A human-readable name that matches the function (`list_notes`, not `list_notes_api_v1_notes_get`)
- Stable across refactors — rename the function only when you intend to rename the generated client

**Name functions as `verb_resource`** (`create_note`, `list_notes`, `get_note`). They are
unique app-wide by convention and map directly to predictable client names.

### 2. One tag per resource

All endpoints for a resource share exactly one tag. The tag name is the resource noun
(`notes`, `projects`, `users`). Codegen groups by tag — one tag per resource means one
generated file per resource, which maps cleanly to one feature folder on the frontend.

Never split a resource across multiple tags. Never group multiple resources under one tag.

### 3. Every endpoint declares a schema and status codes

| Required | Why |
|---|---|
| Response schema on every endpoint | No schema → untyped (`any`) in the generated client |
| Explicit non-200 status codes (201, 204) | Codegen uses the declared status to type the response |
| One-line description on every endpoint | Codegen lifts it into inline docs on the generated client |

Declare expected error codes (404, 409, etc.) even when the error body follows the single
error shape. Never return an inline anonymous object — give every response a named schema.

### 4. Standard resource URL shape

Every resource follows the same structure:

| Verb | Path | Operation |
|---|---|---|
| `POST` | `/resource` | create |
| `GET` | `/resource` | list |
| `GET` | `/resource/{id}` | get one |
| `PUT` | `/resource/{id}` | update |
| `DELETE` | `/resource/{id}` | delete |

No trailing slashes. Non-CRUD actions (verify, exchange, reset) live under the resource
they modify and use the same naming convention.

### 5. One error shape

Every error response from the API has the same shape: `{ "detail": "<message>" }`.
The generated client reads `.detail`. Never use `{ "error": ... }` or `{ "message": ... }`.

Success acknowledgements that return a body (non-resource actions) use the same shape.
A 204 response carries no body — the status code is the signal.

### 6. Spec at a stable, known URL

The spec is always reachable at one path agreed upon between backend and frontend (e.g.
`/api/v1/openapi.json`). The codegen tool is pointed at this URL. Don't move it.

### 7. Paginated list envelope

Every list endpoint returns the same envelope:

```
{ items: T[], total: number, limit: number, offset: number }
```

`limit` and `offset` are injected automatically by the pagination library — do not
declare them in the handler signature. The frontend reads `items` and uses `total`
for page counts.

### 8. Schema naming

Two patterns, never mixed:

**Resources → `Read` / `Create` / `Update`**

- `Read` — response shape: includes every field, including server-owned ones (`id`, timestamps).
- `Create` — POST body: only client-set fields. Never includes `id` or server-owned fields.
- `Update` — PUT/PATCH body: same mutable fields as `Create`, all optional. Always standalone
  (never inherits from `Create` — all-optional can't share a required base).

Server-owned fields (`id`, `owner_id`, `created_at`, `updated_at`) live **only in `Read`**.
The client can't set them; `id` is in the URL path for mutations.

**Actions (non-CRUD) → `Request` / `Response`**

e.g. `OAuthExchangeRequest`, `AccessTokenResponse`.

Envelope schemas are named for their shape, not a domain noun (`AckResponse`, not
`MessageResponse` — domain nouns may collide with future resources).

Default to flat schemas. Add a `{Resource}Base` only when `Create` and `Read` share
several identical fields. A base never used directly doesn't appear in the spec.

### 9. Filter and sort params are individual query parameters

Filters and sort options must be emitted as individual query params in the spec, not as a
single nested object parameter. Individual params produce clean generated function signatures:

```
useListNotes({ isImportant?, q?, limit?, offset? })   ✓
useListNotes({ filters: { isImportant, q } })          ✗  (nested object — codegen emits a $ref param)
```

Filters apply to list endpoints only. Detail endpoints identify one row by path `id` —
they have no filter params. Text search and sort order follow the same individual-param rule.

---

## Frontend contract

### 1. Never hand-write what the spec already declares

Point the codegen tool at the spec URL and run it after every backend change. The generated
output provides all URLs, types, query keys, and request shapes. Hand-writing any of these
creates a second source of truth that silently drifts.

```
# run after any backend change
npm run generate-api   # (or equivalent codegen command)
```

TypeScript (or your type checker) flags every broken call site immediately after regeneration.
Never edit generated files.

### 2. Feature facade — one import point per resource

Components never import from generated code directly. Every resource has a facade module
(`features/<resource>/api.ts`) that re-exports the generated hooks or clients under stable,
friendly names:

```
features/notes/api.ts   →  re-exports useNotes, useNote, useCreateNote, …
features/auth/api.ts    →  re-exports useLogin, useRegister, useVerifyOtp, …
```

When the backend renames a function (changing the generated hook name), only the facade
re-export line changes — not every component that uses it.

The facade is **pure re-exports** — no logic, no wrappers, no transport code. It is an
alias layer, not a service layer.

### 3. One global cache invalidation rule

After any successful mutation, invalidate all active server-state queries. This lives
**once** at the query client level — not per mutation, not per feature.

```
on mutation success → invalidate all active queries
```

Only active queries refetch, so broad invalidation is cheap and keeps every feature
consistent automatically. Narrow it per mutation only when you measure a specific hot path.

### 4. State ownership

Never sync copies of the same data across multiple layers:

| Kind | Where it lives |
|---|---|
| Server data (resources from the API) | Query cache (the generated hooks) |
| Shared page state (filter, page, selection) | URL query params |
| Global client state (auth token, theme) | Client state store (e.g. Zustand) |
| Local UI state (dropdown open/closed) | Component state (`useState`) |

Server data must not be copied into the client state store — that recreates the drift
problem. Shared page state belongs in the URL so it survives refresh and is linkable.

---

## Don't

**Backend**
- Return an inline anonymous object from any endpoint
- Use multiple tags for one resource, or one tag for multiple resources
- Change a handler function name without re-running codegen on the frontend
- Return errors in any shape other than `{ "detail": "..." }`

**Frontend**
- Import from generated code inside a component — go through the facade
- Hand-write a URL, type, or query key the codegen already provides
- Call the HTTP layer directly (raw `fetch`, `axios`) outside of the generated client
- Add per-mutation or per-feature cache invalidation wiring
- Copy server data into the global client state store

---

## Stack-specific implementation

| Stack | Document |
|---|---|
| FastAPI backend | `fastapi-copier-template/template/api-protocol-fastapi.md` |
| Next.js + Orval frontend | `nextjs-copier-template/template/api-protocol-nextjs.md` |
