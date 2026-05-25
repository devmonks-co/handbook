# Huly

We use [Huly](https://huly.app/workbench/designmonks/tracker) for task tracking and project timelines.

## Issues

Every piece of work is an issue. Create one before starting — assign it to the right person and link it to the relevant project.

**When creating an issue:**
- Title should be clear and actionable
- Add a description with enough context to start work
- Assign to the person doing the work
- Set a timeline if the task has a deadline

Use the **activity comments** for any updates, blockers, or decisions — not DMs or side channels.

## Issue lifecycle

```
Backlog → Todo → In Progress → In Review → Completed
                            ↑         |
                            └─────────┘ (if changes needed)
```

| Status | When |
|--------|------|
| **Backlog** | Issue exists but is deferred — touched and acknowledged, not yet scheduled |
| **Todo** | Scheduled and ready to be picked up |
| **In Progress** | Work has begun |
| **In Review** | PR is open; reassign to the reviewer |
| **Completed** | PR merged and approved |

### Flow in detail

1. Pick up an issue — move it to **In Progress**
2. Do the work following the [git workflow](git.md) — branch, commit, push, open PR to `dev`
3. Move to **In Review** — reassign to the senior engineer reviewing the PR
4. Reviewer approves → mark **Completed**
5. Reviewer requests changes → move back to **In Progress**, fix, push, repeat from step 3

## Timelines

Set a timeline on an issue when there's a deadline or the task is part of a planned phase. Keep it updated — a stale timeline is worse than none.
