# Engineering principles

## Our mantra

**Research → Understand → Discuss → Implement**

It's a loop, not a line. If implementation reveals that the plan was wrong — bad assumptions, missing context, unexpected complexity — **stop and go back**. Restart from Research or Understand.

Looping back is cheap. Pushing through a broken plan is expensive.

---

## 1. Use AI, own every line

We use AI tools (Claude Code, Copilot, Cursor, etc.) to move faster. This is expected, not optional — it's a competitive advantage.

But AI is **fallible**:

- It hallucinates APIs, functions, and syntax that don't exist
- It misreads context and produces plausible-looking but wrong code
- It's confident even when it's wrong
- Faster output means **more output to review**, not less

The rules are non-negotiable:

- **You own every line you commit.** AI generates, you verify.
- **Read it, understand it, test it.** If you can't explain what a piece of code does, don't commit it.
- **Lint must be clean before commit.** No lingering warnings or errors. Every project has a working linter setup.
- **No client data in public AI tools.** Treat AI tools like any third-party service.
- **No secrets in prompts.** Use a password manager.
- **No blind copy-paste.** AI output is a starting point, not a finished product.

The goal: ship faster *without* sacrificing quality, security, or understanding.

## 2. Don't suffer alone

If you're stuck for more than 30 minutes:

- Ask for help. Explain what you've tried.
- Often just explaining the problem reveals the solution.
- Raising it early prevents blown estimates and missed deadlines.

There's no penalty for not knowing something. There is for staying stuck silently.

## 3. Be proactive — raise problems with solutions

If you spot something wrong — a bug, a bad pattern, a risk, a confusing decision — **say something**. But don't just dump the problem on someone else's plate.

- Describe the problem clearly
- Propose at least one solution (even if rough)
- Flag the tradeoffs you see

"Here's a problem and one way to fix it" is far more useful than "this is broken, someone should look at it."

## 4. Let your code history tell the story

- Every PR links to a task or ticket
- PR descriptions explain the *why*, not just the *what*
- Commit messages are meaningful (see [git conventions](git.md))

Someone reading the git log six months from now should understand what happened and why.

## 5. Review your own work first

Before requesting a review:

- Read your own PR on GitHub as if you're the reviewer
- Run the app and test the changes yourself
- Check for leftover debug code, console logs, TODOs
- Confirm the linter is clean
- Step away for a few minutes, then look again with fresh eyes

If you wouldn't approve your own PR, don't ask someone else to.

---

## See also

- [Coding guidelines](coding-guidelines.md) — how we actually write code
- [Code review](code-review.md) — how we review each other's work
- [Prompting AI tools](prompting.md) — how to get good output from AI assistants
