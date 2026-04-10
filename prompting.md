# Prompting AI tools

How you prompt directly shapes what you get back. A clear prompt saves hours of cleanup; a vague one wastes both your time and the model's.

This guide is for **day-to-day coding with AI assistants** (Claude Code, Copilot, Cursor, etc.) — not for building applications with the API.

## Be clear and direct

The opening line of a prompt does most of the work. Lead with what you want, plainly.

- Use plain language, no hedging
- Use instructions, not questions
- Start with action verbs: **Write, Refactor, Fix, Debug, Explain, Generate**
- Specify the file, function, or scope when you can

**Vague:**
> Something is wrong with my login thing, users are saying it's broken sometimes, can you look at it?

**Clear:**
> Debug why `signIn()` in `auth/session.ts` intermittently returns a 401 for valid credentials. Start by checking the token expiry logic.

---

**Indirect:**
> The dashboard endpoint feels slow, any ideas?

**Direct:**
> Profile the `/api/dashboard` endpoint and identify the top 3 sources of latency. Suggest one fix for each.

## A simple template

```
[Action verb] [what to produce] [key constraints/context]
```

Examples:

- *"Refactor this function to remove the nested if-statements without changing behaviour."*
- *"Generate a Postgres migration that adds an index on `users.email`."*
- *"Explain why this test is flaky and propose a fix."*

## Why this matters

A weak prompt like *"Can you fix this component?"* and a strong one like *"Refactor the `UserCard` component to use the new `useUser` hook, remove the local state, and keep the existing prop API unchanged"* don't just produce different outputs — the strong one is dramatically better.

Writing clear, direct prompts is the same discipline as writing clear, direct tickets, commits, and PR descriptions. If you can do it for one, you can do it for the others.

## Related

- [Principles](principles.md) — including our rules for owning AI output
