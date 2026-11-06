---
name: checkpoint
description: >
  Session end capture. Summarizes what was aggregated, updates tracking
  artifacts, logs the session, and identifies next session focus.
disable-model-invocation: true
---

# /checkpoint — Session Capture

Persist what was learned this session. Program-manager only writes its own files — sibling tracking
stays read-only.

---

## Step 1: Summarize Session

Identify what sibling tracking was aggregated, what slips/risks/handoffs surfaced, what was reported
to the Controller, and any delivery decisions made.

Present:
```
## Session Summary

**Date:** [today]
**Focus:** [e.g. weekly status, blocker triage, critical-path refresh]
**Key outcomes:**
- [outcome 1]
- [outcome 2]
```

---

## Step 2: Update Tracking Artifacts

Capture findings into program-manager's own tracking files:
- `tracking/critical-path.md` — milestone status, ETA, blocker changes
- `tracking/step-gates.md` — step-gate transitions (in-flight / awaiting-review / approved)
- `tracking/blocker-queue.md` — new blockers (age = 0d), closed, escalations

Do not edit sibling tracking. Disagreements become `/ask` items, never silent edits.

Report: `Tracking updated: critical-path [N], step-gates [N], blockers [N new / N closed]`

---

## Step 3: Record Delivery Decisions (optional)

If delivery decisions landed this session (cadence changes, escalation rules, scope-vs-timeline
tradeoffs surfaced to PM), append to `context/decisions.md`:

```
| [decision] | [rationale] | [date] |
```

Product, technical, and security decisions stay with their owners.

---

## Step 4: Log Session

Append to `tracking/session-log.md`:

```
| [date] | [focus] | [what was aggregated] | [key outcomes] |
```

---

## Step 5: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session:
- `tracking/critical-path.md` — milestone or ETA changes
- `tracking/step-gates.md` — gate transitions recorded
- `tracking/blocker-queue.md` — new or closed blockers
- `tracking/weekly-status/YYYY-MM-DD.md` — weekly status report published

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry includes session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with the bullets above, handoffs, and next actions. If
`tracking/checkpoints.md` does not exist yet, create it with a one-paragraph header explaining its
role as the handoff surface.

`tracking/checkpoints.md` is the **handoff surface**: all sibling companions read the top entry during
their own `/brief` or `/checkpoint` cycles for temporal sequencing and milestone context. If no new
artifacts were published, write "None this session."

---

## Step 6: Next Session Focus

```
## Next Session Focus

**Primary:** [most important next aggregation or report]
**Watching:** [blocker or gate approaching threshold]
**Upcoming cadence:** [next weekly status date]
```

Keep it calm and operational. Surface, don't prescribe.
