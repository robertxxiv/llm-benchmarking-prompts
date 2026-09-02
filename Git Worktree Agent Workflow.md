# Git Worktree Agent Workflow

You are working inside a Git repository and must use an isolated Git worktree for your task.

## Workflow

1. Inspect the repository and identify:
   - repository root
   - current branch
   - current Git status
   - existing worktrees
   - existing branches

2. Never modify the user's main working tree unless explicitly instructed.

3. Create a dedicated branch and Git worktree for the task.

Use a descriptive branch name:

```bash
agent/<task-name>
```

Example:

```bash
agent/api-auth
```

Prefer keeping worktrees outside the main repository, for example:

```text
~/projects/myapp
~/worktrees/myapp-api-auth
```

Create the worktree with:

```bash
git worktree add <worktree-path> -b <branch-name>
```

If the branch already exists:

```bash
git worktree add <worktree-path> <branch-name>
```

4. Perform all implementation work inside the new worktree.

5. Before making changes:
   - inspect relevant files
   - understand the existing implementation
   - identify tests, linting, formatting, and build commands
   - avoid modifying unrelated files

6. Implement the requested change using the smallest reasonable scope.

7. Verify the implementation:
   - run relevant tests
   - run lint/type checks when available
   - run the build when appropriate
   - inspect runtime errors when applicable

8. If verification fails:
   - investigate the failure
   - fix the issue
   - rerun verification
   - continue until the implementation is working or a genuine blocker is identified

9. Before finishing:

```bash
git status
git diff
```

Review the complete diff for:
- accidental changes
- debug code
- secrets
- generated files that should not be committed
- unrelated modifications
- security regressions

10. Commit the completed work on the worktree branch with a concise descriptive commit message.

Example:

```bash
git add .
git commit -m "Implement API authentication"
```

11. Do NOT automatically merge into `main` unless explicitly authorized.

At completion, report:

```text
Branch:
Worktree:
Commit:
Tests:
Build:
Known issues:
Recommended merge command:
```

Example merge command:

```bash
cd <main-repository>
git switch main
git merge agent/api-auth
```

## Safety Rules

- Never use `git reset --hard` unless explicitly authorized.
- Never use `git clean -fd` unless explicitly authorized.
- Never force-push.
- Never rewrite existing user commits.
- Never delete branches or worktrees containing uncommitted changes.
- Never overwrite unrelated user changes.
- Never commit secrets, credentials, API keys, `.env` contents, or sensitive files.
- Check `git status` before and after significant operations.
- Treat the main worktree as read-only unless the task explicitly requires otherwise.

## Parallel Agent Rules

When multiple agents are working simultaneously:

- every agent gets its own worktree
- every agent gets its own branch
- agents must never modify another agent's worktree
- agents must not share an active branch
- coordinate shared-file changes before merging
- resolve integration conflicts only during the merge/integration phase

Example structure:

```text
main
├── agent/backend
│   └── ~/worktrees/project-backend
├── agent/frontend
│   └── ~/worktrees/project-frontend
└── agent/tests
    └── ~/worktrees/project-tests
```

## Completion Criterion

Do not consider the task complete merely because code was written.

The task is complete only when:

- requested functionality is implemented
- relevant tests pass
- build/lint/type checks pass where applicable
- the diff has been reviewed
- changes are committed to the dedicated branch
- the main worktree remains untouched
- the exact branch, worktree, commit, and verification results are reported