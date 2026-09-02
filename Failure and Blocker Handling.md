## Failure and Blocker Handling

When something fails, do not immediately stop.

Classify the issue first.

### Recoverable Failure

Examples:

- test failure caused by your change
- lint/type error
- missing import
- command syntax error
- build failure
- dependency mismatch that can be resolved locally
- merge conflict inside your own worktree
- incorrect assumption about the codebase

For recoverable failures:

1. Inspect the complete error output.
2. Identify the most likely root cause.
3. Make the smallest corrective change.
4. Rerun the failed command.
5. Repeat until the issue is resolved or becomes a genuine blocker.

Do not repeatedly retry the same command without changing anything.

Track failed approaches and avoid cycling through the same unsuccessful solution.

### Environmental Failure

Examples:

- dependency registry unavailable
- network unavailable
- required service is down
- Docker daemon unavailable
- insufficient disk space
- missing system package
- permission problem
- unavailable GPU/device
- required external API unavailable

For environmental failures:

1. Confirm the failure is environmental rather than caused by the implementation.
2. Collect the exact command and relevant error.
3. Attempt safe, non-destructive remediation when reasonable.
4. Do not make unrelated code changes merely to bypass an infrastructure problem.
5. Continue with any verification that does not depend on the failed component.

### Blockers

A blocker is a condition that prevents safe or correct completion and cannot reasonably be resolved within the current task.

Examples:

- required credentials or secrets are missing
- task requirements are contradictory
- a required external service is inaccessible
- necessary files or dependencies are unavailable
- the requested change requires a destructive operation that was not authorized
- implementation depends on a decision that cannot be inferred safely
- the repository is already in a corrupted or unsafe Git state
- required tests cannot execute because of an unresolved environment failure

When blocked:

1. Stop making speculative changes.
2. Preserve all completed work.
3. Do not discard or reset working changes.
4. Commit useful partial work only when it is internally consistent and clearly identified as partial.
5. Record:
   - what is blocked
   - why it is blocked
   - what you already attempted
   - exact error messages or failing commands
   - what information or action is required to proceed
   - whether the current branch is safe to resume later

Use this completion format:

```text
Status: BLOCKED

Branch:
Worktree:
Commit:

Completed:
- ...

Blocker:
- ...

Evidence:
- Command:
- Error:

Attempted:
- ...

Required to continue:
- ...

Tests completed:
- ...

Tests not completed:
- ...

Repository state:
- clean / modified / partially committed

Safe to resume:
- yes / no
```

### Partial Success

If part of the task is complete but another independent part is blocked:

- preserve the completed portion
- verify it independently where possible
- clearly distinguish completed work from blocked work
- do not claim the entire task succeeded

Use:

```text
Status: PARTIAL

Completed:
- ...

Blocked:
- ...

Verification:
- ...

Remaining work:
- ...
```

### Failure Escalation Rules

Stop and report a blocker rather than guessing when any of the following occurs:

- the same failure persists after several materially different fixes
- proceeding would require destructive Git operations
- proceeding would overwrite user changes
- credentials or private keys are required
- production data could be affected
- task requirements cannot be determined from repository context
- fixing the issue would require changing unrelated architecture
- a dependency or external system must be modified outside the task scope

### Never Hide Failures

Never:

- claim tests passed when they were not run
- suppress failing test results
- remove tests just to make the suite pass
- disable linting or type checks merely to avoid errors
- replace a real implementation with a stub unless explicitly requested
- silently skip verification steps
- describe a blocked task as complete

If verification could not be performed, state that explicitly in the final report.