# Definition of done

"Done" is not a feeling. It's a checklist with no judgment calls in it.

## A task is done when all six are true

1. **Merged** to the target branch. Not "ready to merge". Merged.
2. **CI is green** on the merge commit.
3. **Review feedback is resolved**, by you, or handed to a named person who has accepted it.
4. **You verified it yourself** on preview or staging, doing what a user would do. Not "it should work".
5. **Acceptance criteria are ticked**, one by one, in the ticket.
6. **You said so** in the project channel, with the PR link.

Miss one and it's not done. No partial credit.

## Status words

| Say | Means |
|-----|-------|
| **Merged** | All six above. The only word that means finished |
| **In review** | Pushed, CI green, waiting on a reviewer |
| **Blocked** | Waiting on someone or something. Always name who or what |
| **In progress** | Being worked on |

We don't say "done", "almost done" or "done except". If it needs a qualifier, it's *In progress*.

## When to raise your hand

- **Stuck on a problem for 30 minutes** → ask for help (see [principles](principles.md#2-dont-suffer-alone)).
- **Blocked on a person or thing for 2 hours** → reach them. Message, then call. Still nothing? Then:
  - **Go ahead** if it's your call to make: your lead didn't ask to review it, it isn't public-facing, and it's easy to undo. Write the assumption you made in the issue or PR, and tell them when they're back.
  - **Otherwise, switch** to your next task. Keep this one marked **Blocked**, naming who you're waiting on.
- **Raise soft blockers too:** a slipping date, a review that hasn't landed, a requirement you now doubt. A slip flagged Tuesday is a scheduling problem. Flagged Friday evening, it's a client problem.
- **Put the deadline inside the ask.** "Can you review this by 4 PM? It has to merge today." Not "when you get a chance".
- **Say what you can't do as soon as you know it.** A hard stop today is a 9 AM conversation, not a 6 PM one.

Raising something too early has never counted against anyone.

## Self-review before you request review

Read your own diff top to bottom as if someone else wrote it, then answer:

- Does the PR description match what the code does? Is every claim enforced, and where?
- Can this be reached another way? A check that lives only in the UI leaves the API open.
- What happens when a call fails? Is the error checked, or does failure look like success?
- Did I duplicate something this PR just introduced?
- What's the weakest part? Write it in the PR description.

Ten minutes. It catches most of what a reviewer would.

## See also

- [Ownership](ownership.md): what owning a task means here
- [Code review](code-review.md): the author and reviewer checklists
