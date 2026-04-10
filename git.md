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

## Branches

**From a ticket:**
Use the ticket ID as prefix: `dm-123-add-oauth-login`

**Without a ticket:**
Use type prefix: `feat-add-oauth-login`, `fix-expired-card`

## Workflow

1. Branch from `main` (or `development` if the project uses it)
2. Make focused commits as you work
3. Open a draft PR early if you want feedback
4. Rebase onto latest base branch before requesting review
5. Squash merge into base branch after approval

## Rules

- Never force push (`git push -f`) on shared branches. Use `--force-with-lease` if you must.
- Keep PRs small and focused. One PR = one concern.
- Don't let PRs sit open for days — review and merge promptly.
