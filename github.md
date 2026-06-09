# GitHub conventions

How we use GitHub for issues, pull requests, and open-source contributions.

## Repository naming

- All lowercase, hyphens as separators — `my-project`, not `MyProject` or `my_project`
- Descriptive and specific — name it after what it does, not what it is (`invoice-service`, not `backend`)
- No version numbers or dates in the name — that belongs in tags and releases

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

## Workflow

1. Clone the repo, then `git checkout dev`
2. Create a feature branch from `dev` — use `/devmonks-git:branch`
3. Make changes. Commit atomically — every small, logical change is its own commit — use `/devmonks-git:commit`
4. Push the branch
5. Open a PR targeting `dev` — use `/devmonks-git:create-pr`
6. Link the PR to its issue: in the PR's right panel under **Development**, select the issue — the issue closes automatically when the PR is merged
7. Address review comments, push additional commits to the same branch
8. A senior engineer merges after approval

## README

Every project repo must have a `README.md`. It is the first thing anyone reads — keep it accurate and up to date.

**Required sections, in order:**

1. **Project name + one-line description** — what it is and what it does
2. **Quick start** — the minimum steps to clone, configure, and run locally
3. **Environment setup** — list every required env variable; reference `.env.example`
4. **Available scripts** — key commands (start, build, test, lint)
5. **Contributing** — link to this handbook; note the branch and PR workflow

**Rules:**
- Quick start must actually work — test it on a clean setup
- No walls of text; use code blocks for commands
- If a step is not obvious, explain the *why* in one line
- Keep it current — outdated READMEs are worse than none

## Contributing to third-party libraries

When a dependency is missing something we need, we contribute upstream rather than patching locally.

**Process:**

1. Identify the gap — confirm it doesn't already exist in the repo or a pending PR
2. Fork the repo into the `devmonks-co` GitHub org
3. Clone the fork locally
4. Read the repo's existing code — match its style, conventions, and patterns before writing anything
5. Create a branch and make changes with atomic commits
6. Push the branch to your fork
7. On the fork, click **Contribute** → opens a PR against the upstream repo — write a clear description: what the change does, why it's needed, and how to test it
8. Create an issue in our forked repo that records:
   - What was implemented
   - Link to the upstream PR
   - Action on merge: switch from the fork to the official package once the PR is merged and released

**After the upstream PR merges:** update our projects to use the official release and archive or delete the fork if it's no longer needed.
