---
name: checkpoint
description: >
  Session end capture for the security-engineer companion. Records findings and gate decisions,
  updates tracking, and lands new artifacts on the handoff surface for other companions.
disable-model-invocation: true
---

# /checkpoint — Security Session Capture

Capture reviews, findings, and gate decisions from this session.

---

## Step 1: Session Summary

```
## Session Summary
**Date:** [today]
**Focus:** [reviews conducted, components audited, gates evaluated]
**Outcomes:**
- [outcome 1]
- [outcome 2]
```

---

## Step 2: Update Vulnerability Tracker

Update `tracking/vulnerability-tracker.md` — add new findings (each with severity, exploit scenario,
recommended fix), and update the status of existing ones.

---

## Step 3: Update Review Gates

Update `tracking/review-gates.md` with any gate decisions made this session (PASS / CONDITIONAL /
FAIL and the developer-may-proceed verdict).

---

## Step 4: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session under:
- `docs/threat-models/` — new or updated threat models
- `docs/reviews/` — security review reports and audits
- `tracking/review-gates.md` — gate decisions recorded
- `tracking/vulnerability-tracker.md` — new findings or status updates

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry has session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with one line per artifact (path — description — downstream companion), the
handoffs below, and next actions. If `tracking/checkpoints.md` does not exist, create it with a
one-paragraph header explaining its role as the handoff surface.

```
## New Artifacts Since Last Checkpoint
- `docs/reviews/YYYY-MM-DD-<slug>-audit.md` — [one-line description] — developer (fixes), engineer (if redesign)
- `docs/threat-models/YYYY-MM-DD-<component>.md` — [one-line description] — engineer (design), developer (implement)
```

If nothing was published, write "None this session." so downstream readers know it was checked.

---

## Step 5: Cross-Companion Handoffs

```
## Handoffs
- **Developer:** [findings that need code fixes]
- **Engineer:** [architecture issues that need redesign]
- **Program Manager:** [gate decisions that affect the schedule or step-gates]
- **Product Manager:** [security constraints that affect scope or priority]
```

---

## Step 6: Next Actions

```
## Next Actions
1. [Highest-priority security work]
2. [Second priority]
```
