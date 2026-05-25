# Git conventions

## Commit messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
[TICKET-ID] type(scope): subject

Optional body explaining why, not what.
```

### Types

| Type | When |
|------|------|
| `feat` | New feature |
| `fix` | Bug fix |
| `chore` | Dependencies, refactoring, maintenance |
| `docs` | Documentation changes |
| `test` | Adding or updating tests |
| `deploy` | Deployment-related changes |
| `style` | Formatting, no logic change |

### Rules

- Header max 72 characters
- Write in English
- Use imperative mood: "add feature" not "added feature"
- Include ticket ID when applicable

### Examples

```
[DM-123] feat(auth): add Google OAuth login
[DM-456] fix(payments): handle expired card edge case
chore: update dependencies to latest patch versions
```

## Issues

Create an issue before starting any non-trivial work — new features, requirement changes, bugs worth discussing, or anything planned for a future phase.

**When to open one:**
- A new requirement or change comes in
- Something needs team discussion before work begins
- Work is planned but not starting yet

**What to include:**
- Clear title — what needs to happen
- Description — the context, the why, and any constraints
- Reference related issues, PRs, or tickets if applicable

The team discusses and agrees on a solution path in the issue thread. Work only begins — using the workflow below — after that path is clear.

Every feature branch and PR should reference the issue it resolves.

---

## Permanent branches

| Branch | Purpose |
|--------|---------|
| `main` | Production — always stable |
| `dev` | Active development — all work merges here first |

Never commit directly to either. All changes go through a feature branch and PR.

## Branches

Use `/devmonks-git:branch` to create branches. It enforces naming and pushes with upstream tracking.

**From a ticket:**
Use the ticket ID as prefix: `dm-123-add-oauth-login`

**Without a ticket:**
Use type prefix: `feat/add-oauth-login`, `fix/expired-card`

Always branch from `dev`.

## Workflow

1. Clone the repo, then `git checkout dev`
2. Create a feature branch from `dev` — use `/devmonks-git:branch`
3. Make changes. Commit atomically — every small, logical change is its own commit — use `/devmonks-git:commit`
4. Push the branch
5. Open a PR targeting `dev` — use `/devmonks-git:create-pr`
6. Address review comments, push additional commits to the same branch
7. A senior engineer merges after approval

## Rules

- Never force push (`git push -f`) on shared branches. Use `--force-with-lease` if you must.
- Keep PRs small and focused. One PR = one concern.
- Don't let PRs sit open for days — review and merge promptly.
