---
name: test
description: >
  Run tests, write new tests, or investigate test failures across the project's codebases.
disable-model-invocation: true
argument-hint: "[codebase or test name]"
---

# /test — Testing

Run, write, or investigate tests.

**Usage:**
- `/test <codebase>` — Run all tests in a codebase
- `/test <component>` — Run tests for a specific component
- `/test write <component>` — Write new tests for a component
- `/test` — Run all tests across all codebases

---

## Step 1: Determine Scope

If `$ARGUMENTS` names a codebase, run that codebase's test runner (whatever it already uses).
If `$ARGUMENTS` starts with "write", create new tests for the named component.
If no arguments, run tests across all codebases and report results.

---

## Step 2: Run Tests

Execute the appropriate test command. Report results:
```
## Test Results: [scope]

**Passed:** [N]
**Failed:** [N]
**Skipped:** [N]

### Failures
- [test name]: [error summary]
```

---

## Step 3: Investigate Failures (if any)

For each failure:
1. Read the failing test.
2. Read the code being tested.
3. Determine: is this a test bug or a code bug?
4. Fix or report.

---

## Step 4: Write Tests (if requested)

When writing new tests:
- One test per behavior, not per function.
- Test error paths explicitly — the failure behavior the sensitivity model requires must be verified.
- Integration tests at component boundaries.
- For timing- or tolerance-sensitive code, test the tolerance band, not just the happy path.
- Name tests descriptively: e.g. `test_session_aborts_on_manifest_hash_mismatch`.
