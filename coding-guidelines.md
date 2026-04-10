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

---

## See also

- [Engineering principles](principles.md) — including our mantra and AI rules
- [Code review](code-review.md) — these guidelines are what reviewers check for
