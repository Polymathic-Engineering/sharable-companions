---
name: checkpoint
description: >
  Session end capture for the designer companion. Records decisions, updates tracking, and lands new
  design artifacts on the handoff surface for the developer and product-manager.
disable-model-invocation: true
---

# /checkpoint — Designer Session Capture

Capture design work from this session.

---

## Step 1: Session Summary

```
## Session Summary
**Date:** [today]
**Focus:** [what we worked on]
**Outcomes:**
- [designs created]
- [prototypes built]
- [audits conducted]
```

---

## Step 2: Record Decisions

For each design decision not already in `context/decisions.md`, append it with rationale (and the
sensitivity level it concerns, where relevant).

---

## Step 3: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session under:
- `docs/designs/` — design specs, component definitions, wireframes (developer implements)
- `docs/designs/language-rules.md` — if the canonical language-substitution table was revised
  (developer re-lints; note the new version)
- `docs/designs/prototypes/` — HTML prototypes (reference for the developer)
- `docs/designs/audits/` — UX audits (developer resolves issues before shipping)

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry has session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with one line per artifact (path — description — downstream companion), the
handoffs below, and next actions. If `tracking/checkpoints.md` does not exist, create it with a
one-paragraph header explaining its role as the handoff surface.

```
## New Artifacts Since Last Checkpoint
- `docs/designs/<name>.md` — [one-line description] — developer (implement)
- `docs/designs/language-rules.md` v<X.Y.Z> — [what changed] — developer (re-lint)
```

If nothing was published, write "None this session." so downstream readers know it was checked.

---

## Step 4: Cross-Companion Handoffs

```
## Handoffs
- **Developer:** [specs ready for implementation; language-table version to lint against]
- **PM:** [design constraints that affect scope or requirements]
```

---

## Step 5: Next Actions

```
## Next Actions
1. [Most important next design task]
2. [Second priority]
```
