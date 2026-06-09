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
| `refactor` | Code restructure, no behaviour change |
| `perf` | Performance improvement |
| `docs` | Documentation changes |
| `test` | Adding or updating tests |
| `style` | Formatting, no logic change |
| `build` | Build system or dependency changes |
| `ci` | CI/CD configuration |
| `deploy` | Deployment-related changes |
| `chore` | Maintenance tasks that don't fit elsewhere |
| `revert` | Reverts a previous commit |

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

## Rules

- Never force push (`git push -f`) on shared branches. Use `--force-with-lease` if you must.
- Keep PRs small and focused. One PR = one concern.
- Don't let PRs sit open for days — review and merge promptly.

## See also

- [GitHub conventions](github.md) — issues, PR workflow, README, and open-source contributions
