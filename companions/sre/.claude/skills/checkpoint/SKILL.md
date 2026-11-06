---
name: checkpoint
description: >
  Session end capture for the SRE companion. Updates operational tracking and lands new artifacts on
  the handoff surface for other companions.
disable-model-invocation: true
---

# /checkpoint — SRE Session Capture

Capture operational work from this session.

---

## Step 1: Session Summary

```
## Session Summary
**Date:** [today]
**Focus:** [what we worked on]
**Outcomes:**
- [outcome 1]
- [outcome 2]
```

---

## Step 2: Update Tracking

Update `tracking/operational-status.md`, `tracking/incidents.md`, and `tracking/cost-tracking.md`
with the state as of this session.

---

## Step 3: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session:
- `docs/runbooks/` — new or updated runbooks
- `tracking/operational-status.md` — operational snapshot updates
- `tracking/incidents.md` — new incident entries
- `tracking/cost-tracking.md` — cost metric updates

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry has session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with one line per artifact (path — description — downstream companion), the
handoffs below, and next actions. If `tracking/checkpoints.md` does not exist, create it with a
one-paragraph header explaining its role as the handoff surface.

```
## New Artifacts Since Last Checkpoint
- `docs/runbooks/<scenario>.md` — [one-line description] — on-call (operate), security-engineer (review if security-classified)
```

If nothing was published, write "None this session." so downstream readers know it was checked.

---

## Step 4: Cross-Companion Handoffs

```
## Handoffs
- **Developer:** [deployment requirements or fixes needed]
- **Engineer:** [infrastructure design issues surfaced in operation]
- **Security Engineer:** [operational security concerns; co-owned incidents]
- **Program Manager:** weekly operational status. If today is the weekly cadence day, confirm
  `tracking/operational-status.md` has a fresh `## Snapshot: YYYY-MM-DD` section at the top —
  program-manager reads it for the weekly report. If a SEV-1 or SEV-2 incident is open, send an
  out-of-band update instead of waiting for the regular write.
```

---

## Step 5: Next Actions

```
## Next Actions
1. [Highest-priority operational work]
2. [Second priority]
```
