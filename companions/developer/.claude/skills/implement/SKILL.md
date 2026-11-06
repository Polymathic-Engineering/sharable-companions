---
name: implement
description: >
  Implement a feature, fix, or build step from a spec or ticket. Reads the spec, reads existing code,
  writes code, runs tests.
disable-model-invocation: true
argument-hint: "[ticket ID or topic]"
---

# /implement — Build a Feature

Implement a spec, ticket, or build step.

**Usage:**
- `/implement 3` — Implement ticket #3 from the product-manager's current tickets
- `/implement <feature-name>` — Implement a named feature
- `/implement` — Check what's next on the current build plan

---

## Step 1: Determine What to Build

If `$ARGUMENTS` is a number, read the product-manager's tickets for that ticket.
If `$ARGUMENTS` is a topic, find the relevant spec or build step.
If no arguments, read `tracking/progress.md` for the next unblocked item.

Display:
```
## Implementing: [title]
**Source:** [ticket/spec/build-step reference]
**Output files:** [expected outputs]
**Acceptance criteria:**
- [criterion 1]
- [criterion 2]
```

---

## Step 2: Read Context

1. Read the spec or ticket description fully.
2. Read the relevant source code in the project's codebases.
3. Read the engineer's decisions (`../engineer/context/decisions.md`) and ADRs (`../engineer/docs/adrs/`)
   for architecture context.
4. Read any integration contract in `../engineer/docs/contracts/` if crossing a component boundary.
5. **Establish the sensitivity level** of the data this feature touches (`context/sensitivity-model.md`,
   see `../../SHARED-PRINCIPLES.md`) — it drives the handling and failure mode you implement.

---

## Step 3: Plan Implementation

Before writing code, outline the approach:
```
## Implementation Plan
1. [Step 1 — what to do and which file]
2. [Step 2]
3. [Step 3]

**Files to modify:**
- [path] — [what changes]

**Files to create:**
- [path] — [what it does]

**Sensitivity level of data touched:** [level — and the handling/failure mode it requires]

**Risks:**
- [anything that could go wrong]
```

Ask for confirmation before proceeding if the change is large or crosses a component boundary.

---

## Step 4: Implement

Write the code. Follow these rules:
- Read existing code first — match patterns already in use.
- Implement the handling and failure mode the sensitivity model assigns the data's level; enforcement
  paths at protected levels are explicit and fail-closed where the model requires.
- Explicit error types — no generic errors in enforcement paths.
- Follow the language conventions in `.claude/rules/code-standards.md`.
- Reference the spec, ADRs, and contracts for requirements.

---

## Step 5: Test

Run the test runner each affected codebase already uses. If tests fail, fix before proceeding.

---

## Step 6: Record

1. Update `tracking/progress.md` with completion.
2. Update `context/decisions.md` with any implementation decisions.
3. Flag security-relevant changes for the security-engineer companion.
4. Note any issues or follow-ups in `tracking/blockers.md`.

```
## Implementation Complete: [title]
**Files modified:** [list]
**Files created:** [list]
**Tests:** [pass/fail]
**Security review needed:** [yes/no — if yes, what to review]
```
