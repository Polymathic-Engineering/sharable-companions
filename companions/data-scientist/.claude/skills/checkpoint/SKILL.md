---
name: checkpoint
description: >
  Session end capture for the data-scientist companion. Records analyses progressed,
  methodology decisions made, new questions surfaced, and handoff state on the shared
  handoff surface.
disable-model-invocation: true
---

# /checkpoint — Analysis Session Capture

Capture what progressed this session so no analytical work or methodology choice stays only in
conversation context.

---

## Step 1: Session Summary

Review the conversation and identify analyses progressed, methodology choices made, findings surfaced,
and new questions raised.

```
## Session Summary
**Date:** [today]
**Focus:** [what we analyzed]
**Analyses progressed:** [docs/analysis/ or docs/methodology/ files touched]
**Key outcomes:**
- [outcome 1]
- [outcome 2]
```

---

## Step 2: Record Decisions

For each methodology choice not already in `context/decisions.md`, append it with rationale:

```
| [decision] | [rationale] | [date] |
```

Report:
```
**Decisions recorded:** [N] new entries added to decisions.md
```

---

## Step 3: Update Questions

- Add new open questions to `context/questions.md`
- Mark any resolved questions as resolved

Report:
```
**Questions:** [N] new, [N] resolved
```

---

## Step 4: Handoff State

Identify which companion can act next, or what is blocking progress:

```
## Handoff
- **Ready for:** [security-engineer / product-manager / engineer / sre — and what they need]
- **Blocked on:** [data access / decision / channel pre-clearance / external input — or "none"]
- **Analysis state:** [complete / in-progress / needs review]
```

---

## Step 5: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session under:
- `docs/analysis/` — analysis reports (segmentation, channel measurements, baselines, conformance results)
- `docs/methodology/` — reusable methodology specs

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md` (most-recent-first).
Each entry includes session date, focus, outcomes, a "New Artifacts Since Last Checkpoint" sub-section
with one line per artifact (path — description — downstream companion), the handoffs above, and next
actions. If `tracking/checkpoints.md` does not exist yet, create it with a one-paragraph header
explaining its role as the handoff surface.

This is the **handoff surface**: the security-engineer and engineer read the top entry during their own
`/brief` or `/checkpoint` cycles to pick up empirical evidence (channel verdicts, conformance data,
baselines) they depend on. If no new artifacts were published, write "None this session." so downstream
readers know the section was checked, not skipped.

---

## Step 6: Next Actions

```
## Next Actions
1. [Most important next step]
2. [Second priority]

Suggested next session focus: [topic or analysis]
```
