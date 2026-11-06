---
name: checkpoint
description: >
  Session end capture for the engineer companion. Records decisions, updates tracking, and lands
  new artifacts on the handoff surface for other companions.
disable-model-invocation: true
---

# /checkpoint — Engineering Session Capture

Capture architecture decisions and work from this session.

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

## Step 2: Record Decisions

For each decision not already in `context/decisions.md`, append it with rationale (and the
sensitivity tier it concerns, where relevant).

---

## Step 3: Update Open Decisions

Update `tracking/open-decisions.md` — remove resolved, add newly surfaced.

---

## Step 4: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session under:
- `docs/adrs/` — new ADRs (security-engineer reviews; developer implements)
- `docs/contracts/` — new or revised integration contracts (developer implements against these)
- `docs/reviews/` — new architecture reviews (security-engineer co-signs where applicable)

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry has session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with one line per artifact (path — description — downstream companion), the
handoffs below, and next actions. If `tracking/checkpoints.md` does not exist, create it with a
one-paragraph header explaining its role as the handoff surface.

```
## New Artifacts Since Last Checkpoint
- `docs/adrs/YYYY-MM-DD-<slug>.md` — [one-line description] — security-engineer (review), developer (implement)
- `docs/contracts/<name>-v<X.Y>.md` — [one-line description] — developer (implement), security-engineer (review)
```

If nothing was published, write "None this session." so downstream readers know it was checked.

---

## Step 5: Cross-Companion Handoffs

```
## Handoffs
- **Security Engineer:** [items needing security review]
- **Developer:** [architecture decisions ready for implementation]
- **PM:** [items that affect requirements or scope]
- **SRE:** [items that affect operations]
```

---

## Step 6: Next Actions

```
## Next Actions
1. [Most important next step]
2. [Second priority]
```
