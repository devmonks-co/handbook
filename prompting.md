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
> `signIn()` in `auth/session.ts` intermittently returns 401 for valid credentials. Trace from a failing request:
> ```
> TokenExpiredError: jwt expired
>     at verifyToken (auth/jwt.ts:42)
>     at signIn (auth/session.ts:88)
>   expiredAt: 2026-10-12T09:14:03Z
> ```
> The token was issued 2 seconds before this, with a 15-minute lifetime. It only fails on requests that hit `api-2`, whose clock runs ~40s ahead. So it's clock skew, not bad credentials. Add a 60s `clockTolerance` to the `jwt.verify` call in `verifyToken`. Don't change the token lifetime.

The clear version gives the agent the evidence, your reading of it, and the exact change. You did the understanding; the agent does the typing.

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

### For bugs

```
[What's broken] + [the evidence: trace, log, failing input] + [what you think it means] + [what to change, and what not to touch]
```

- **Always paste the evidence.** The error trace, the log line, the input that fails. Never just the symptom.
- **Say what you think it means.** Reading the trace yourself is how you stay able to check the fix.
- **Don't know the cause yet?** Ask the agent to find and explain the root cause first, and to stop before fixing. Fix once you understand it.

## Why this matters

A weak prompt like *"Can you fix this component?"* and a strong one like *"Refactor the `UserCard` component to use the new `useUser` hook, remove the local state, and keep the existing prop API unchanged"* don't just produce different outputs — the strong one is dramatically better.

Writing clear, direct prompts is the same discipline as writing clear, direct tickets, commits, and PR descriptions. If you can do it for one, you can do it for the others.

## Related

- [Principles](principles.md) — including our rules for owning AI output
