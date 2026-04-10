# Code review

Every change goes through code review. No exceptions.

## For the author

Before requesting a review:

- [ ] PR links to the relevant ticket
- [ ] Description explains *what* changed and *why*
- [ ] You've reviewed your own diff on GitHub
- [ ] You've tested the changes locally
- [ ] No debug code, console logs, or commented-out code left behind
- [ ] If AI-generated code is involved, you've verified and understood every line

## For the reviewer

### How to review

1. **Read the ticket first.** Understand what this PR is supposed to do.
2. **Review line by line.** Don't skim.
3. **Pull and test.** Run the code locally, especially for non-trivial changes.
4. **Check the basics:**
   - Does it solve the right problem?
   - Is the approach reasonable? (Not over-engineered, not a hack)
   - Are there missing edge cases?
   - Is the code readable and maintainable?
   - Are there adequate tests?
   - Any security concerns?
5. **Check for AI artifacts.** Over-abstraction, unnecessary error handling for impossible cases, verbose comments explaining obvious code — these are signs of unreviewed AI output.

### How to give feedback

- Use **"we"** not **"you"** — "we could simplify this" not "you made this too complex"
- Explain *why* something should change, not just *what*
- Suggest alternatives when pointing out issues
- Acknowledge good work — positive feedback reinforces good practices
- Be direct but respectful

### Severity levels

Use prefixes to signal intent:

- **`nit:`** — minor style/preference, non-blocking
- **`suggestion:`** — take it or leave it, but consider it
- **`issue:`** — this needs to be addressed before merge
- **`question:`** — I don't understand this, please clarify

## Turnaround

- Respond to review requests within one working day
- Don't let PRs go stale — if you're blocked on review, speak up
