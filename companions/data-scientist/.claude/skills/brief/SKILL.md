---
name: brief
description: >
  Session start orientation for the data-scientist companion. Shows the analysis
  backlog, methodology references, open questions, blockers, and suggested focus.
disable-model-invocation: true
---

# /brief — Data Scientist Session Orientation

Orient to the current state of the data-science work.

---

## Step 1: Product Context

Read `context/product-brief.md` and `context/sensitivity-model.md`. If either is missing, note it as a
blocker — the allowed-source list and the observable-surface boundary depend on the sensitivity model.
Do not run analyses on real data until both exist (see CLAUDE.md).

---

## Step 2: Analysis Backlog

List files in `docs/analysis/` (if the directory exists). Flag the most recently modified file as
ACTIVE.

```
## Analysis Backlog
- [filename] — [description]  ← ACTIVE
```

If empty or missing: "No analyses yet."

---

## Step 3: Methodology References

List files in `docs/methodology/`. If empty or missing: "No methodology docs yet."

---

## Step 4: Open Questions

Read `context/questions.md`. Display only unresolved items.

```
## Open Questions
- [question text]
```

If none: "No open questions."

---

## Step 5: Blockers

Note blockers from `context/constraints.md`, plus the two standing ones to check:
- Is the sensitivity model (`context/sensitivity-model.md`) and the allowed-source list defined?
- Is any required dataset not yet handed off by the Controller?

```
## Blockers
- [blocker — what's needed to clear it]
```

If none: "No blockers."

---

## Step 6: Cross-Companion Check

Read sibling contexts for requests affecting data work:
- `../product-manager/context/questions.md` — segmentation or funnel questions needing analysis?
- `../security-engineer/tracking/` — instrumentation, logging, or channel-clearance constraints?
- `../engineer/docs/adrs/` — schema or data-availability decisions?
- `../sre/tracking/operational-status.md` — telemetry available for a baseline?

Flag anything requiring a data-science response.

---

## Step 7: Suggested Focus

```
## Suggested Focus
[What to work on this session based on backlog, questions, and blocker status]

Available skills:
  /analyze             — Privacy-respecting segmentation / analysis of a Controller-provided dataset
  /measure-channel     — Inference-channel measurement (timing / volume / egress)
  /design-test-plan    — Author a co-authored methodology spec
  /baseline            — Cost / operational baseline from sre telemetry
  /checkpoint          — Capture session work
```
