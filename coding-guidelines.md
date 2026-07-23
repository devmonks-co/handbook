# Coding guidelines

How we actually write code at devmonks. Language- and framework-agnostic — these apply everywhere.

## 1. DRY — don't repeat yourself

- If you're writing the same logic twice, extract it.
- If you've written the same logic three times, you definitely should have extracted it the second time.
- But: **don't abstract prematurely**. Three similar lines aren't a pattern. Wait until the duplication is clearly stable before extracting.

## 2. Simple and small

- Small functions. Small files. Small PRs.
- One function should do one thing.
- If a function is hard to name, it's probably doing too much.
- Readable code beats clever code. Always.

## 3. Clean as you go

- See a mess? Fix it. Don't walk past broken windows.
- Found a typo, dead variable, or confusing name while working on something else? Fix it in the same PR (if small) or a follow-up (if larger).
- Don't leave the code worse than you found it. Ideally, leave it better.

## 4. No lingering linter warnings or errors

- Every project has a properly configured linter.
- Lint must be clean before you commit. No warnings. No errors. No exceptions ignored without a comment explaining why.
- If the linter is wrong about something, fix the rule — don't disable it silently.
- A codebase with hundreds of warnings is a codebase where real problems hide in plain sight.

## 5. When complexity rises, go back to the drawing board

If your solution is getting tangled, hard to follow, or you can't hold it in your head — **stop coding**.

- Step back
- Re-read the requirements
- Research how others have solved similar problems
- Discuss the approach with the team
- Then come back with a simpler plan

Complexity is a signal. Listen to it.

## 6. Comments explain *why*, not *what*

- The code already shows *what* it does.
- Comments should explain *why* — the intent, the tradeoff, the non-obvious context.
- If you need a comment to explain *what* the code does, the code is probably too complex (see #2).

## 7. No commented-out code

- Delete it. Git remembers.
- Commented-out code is noise that ages into confusion.

## 8. No hardcoding

Nothing environment-specific, secret, or configurable should live in source code — backend or frontend.

- **Secrets and credentials** → environment variables, never committed
- **API URLs, endpoints, base paths** → environment variables or config files
- **Feature flags, limits, timeouts** → config, not literals
- **Magic numbers and strings** → named constants with clear intent
- **Environment-specific values** (dev/staging/prod) → config, never inline

If a value might change between environments, between clients, or over time, it doesn't belong in the code.

`.env.example` should always exist and stay up to date so anyone can spin up the project.

## 9. Handle errors at the top, not everywhere

Don't sprinkle `try/catch` blocks throughout the code. They make logic harder to read and often swallow errors silently.

### Think of the server as a room

A server is a room. Work comes and goes, but the room has a fixed number of **gates** — and every gate gets a **guard**.

- The **main gate** is the request/response cycle: a client walks in with a request, walks out with a response.
- The **side gates** are the other ways work enters — a cron job waking on a schedule, a webhook from a third party, a message pulled off a queue. No client is waiting, but work is still crossing a threshold.

The rule: **put a guard on every gate, and nowhere else.** The guard checks everyone on the way in, and — the part people forget — on the way out too. Nobody leaves the room unexamined. If something broke inside, the exit guard catches it, decides what the outside world sees, and writes it down once in the logbook.

That is what "handle errors at the top" means. The gates are the top. A `try/catch` buried deep inside is a guard posted in an empty hallway — it blocks the natural flow, usually waves everyone through anyway, and clutters the place. Guard the gates, keep the room clear.

Every entry point — each route handler, cron job, webhook receiver, queue consumer — gets exactly one guard: a top-level handler wrapping it. That is your choke point. Everything else just lets errors walk toward the nearest gate.

### Default behaviour

- **Let errors propagate** to the top level of the application and handle them in one place.
- **Catch lower only when you have a specific reason** — retrying a transient failure, providing a fallback, or adding context before re-throwing.
- **Never catch just to log and continue.** If you can't handle it meaningfully, let it propagate.
- **No empty catches.** A swallowed error is a future bug you can't debug.

### Operational vs programmer errors

Two different things, handled differently:

- **Operational errors** are expected runtime failures — network timeouts, missing files, invalid input from users. Handle these gracefully.
- **Programmer errors** are bugs — null dereferences, wrong arguments, type errors. **Let them crash.** Catching bugs hides them; fix the code instead.

Treating bugs as recoverable failures is the most common mistake. Don't do it.

### Catch narrowly, not broadly

- Catch specific error types or classes — not "any error."
- Bare catches (e.g. `except:` in Python, `catch (e)` in JS without checks) hide the errors you didn't expect, including critical signals like cancellations or interrupts.

### Async errors

- Always `await` inside the `try` block — naked promises swallow errors.
- Never leave fire-and-forget async calls without an explicit error handler.
- Set up a global handler for unhandled promise rejections and uncaught exceptions. Log and exit — don't try to keep running in an unknown state.

### Custom error classes

For non-trivial codebases, define a small hierarchy of domain-specific error types (e.g. `ValidationError`, `NotFoundError`, `ExternalServiceError`).

- Enables narrow catches
- Makes the top-level handler easy to write (one error type → one response)
- Improves grouping in error reporting tools
- Keep the hierarchy shallow — 2-3 levels max

### Preserve the error chain

When you do catch and re-throw, **wrap, don't replace**. Keep the original error attached as the cause so the stack trace and context survive.

Anti-pattern: catching an error and throwing a brand new one with just the message — you lose the stack trace and the root cause.

### Logging

- **Log once, at the top.** Logging at every catch creates duplicate noise.
- Never log and re-throw in the same place — pick one.
- Log structured data with enough context to debug: request IDs, user IDs, relevant inputs.

One well-designed top-level handler beats fifty defensive `try/catch` blocks.

---

## See also

- [Engineering principles](principles.md) — including our mantra and AI rules
- [Code review](code-review.md) — these guidelines are what reviewers check for
