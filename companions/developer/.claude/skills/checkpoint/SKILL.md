---
name: checkpoint
description: >
  Session end capture for the developer companion. Records what was built, updates progress, and
  lands new artifacts on the handoff surface for other companions.
disable-model-invocation: true
---

# /checkpoint — Developer Session Capture

Capture implementation work from this session.

---

## Step 1: Session Summary

```
## Session Summary
**Date:** [today]
**Focus:** [what was worked on]
**Code changes:**
- [file] — [what changed]
**Tests:** [pass/fail/not run]
```

---

## Step 2: Update Progress

Update `tracking/progress.md` with any progress made.

---

## Step 3: Update Blockers

Update `tracking/blockers.md`:
- Remove resolved blockers.
- Add new blockers discovered.

---

## Step 4: Record Decisions

Append implementation decisions to `context/decisions.md`, with the sensitivity level they concern
where relevant.

---

## Step 5: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session:
- Code changes across the project's codebases (list by path).
- `tracking/progress.md` rollups or significant updates.
- Implementation-level decisions that belong in `context/decisions.md`.

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry includes session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with the bullets above, the handoffs below, and next actions. If
`tracking/checkpoints.md` does not exist yet, create it with a one-paragraph header explaining its
role as the handoff surface.

This is the shared **handoff surface**: sibling companions (especially security-engineer, engineer,
and program-manager) read the top entry during their own `/brief` or `/checkpoint` cycles — the
developer is the implementation pivot. If no new artifacts were published, write "None this session."

---

## Step 6: Cross-Companion Handoffs

```
## Handoffs
- **Security Engineer:** [code needing security review]
- **Engineer:** [architecture questions surfaced during implementation]
- **PM:** [scope issues or requirement clarifications needed]
- **SRE:** [operational considerations from implementation]
```

---

## Step 7: Next Actions

```
## Next Actions
1. [Most important next step]
2. [Second priority]
```
