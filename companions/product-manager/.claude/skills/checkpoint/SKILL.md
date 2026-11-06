---
name: checkpoint
description: >
  Session end capture. Summarizes the session, captures insights, updates tracking files,
  and lands new artifacts on the handoff surface for other companions.
disable-model-invocation: true
---

# /checkpoint — Session Capture

Capture what happened this session so nothing important stays only in conversation context.

---

## Step 1: Summarize Session

Review the conversation and identify:
- Topics discussed
- Decisions made
- Insights surfaced
- Questions raised
- Specs or artifacts produced

Present a brief summary:
```
## Session Summary

**Date:** [today]
**Topics:** [what we covered]
**Key outcomes:**
- [outcome 1]
- [outcome 2]
```

---

## Step 2: Capture Insights

For each insight not already in `tracking/insights-log.md`, append it:

```
| [date] | [insight] | [source] | [connected to] |
```

Report what was captured:
```
**Insights captured:** [N] new entries added to insights-log.md
```

---

## Step 3: Update Decisions

For each decision made this session not already in `context/decisions.md`, append it:

```
| [decision] | [rationale] | [date] |
```

Report:
```
**Decisions recorded:** [N] new entries added to decisions.md
```

---

## Step 4: Update Questions

- Add any new open questions to `context/questions.md`
- Mark any resolved questions as resolved

Report:
```
**Questions:** [N] new, [N] resolved
```

---

## Step 5: Log Session

Append to `tracking/session-log.md`:

```
| [date] | [duration estimate] | [focus area] | [key outcomes summary] |
```

---

## Step 6: New Artifacts — Handoff Landing File

Enumerate every file created or materially updated this session:
- `docs/plans/` — implementation specs, ticket revisions
- `tracking/hypothesis.md` — if the hypothesis was updated or challenged
- `tracking/insights-log.md` — if significant new insights were recorded

**Landing file:** append this session's entry to the top of `tracking/checkpoints.md`
(most-recent-first). Each entry includes session date, focus, outcomes, a "New Artifacts Since Last
Checkpoint" sub-section with one line per artifact (path — description — downstream companion),
handoffs, and next actions. If `tracking/checkpoints.md` does not exist yet, create it with a
one-paragraph header explaining its role as the handoff surface.

This is the **handoff surface** (distinct from `tracking/session-log.md`, which is PM self-reference):
developer, engineer, and all other companions read the top entry during their own `/brief` or
`/checkpoint` cycles to pick up scope and spec changes. If no new artifacts were published, write
"None this session." so downstream readers know it was checked.

---

## Step 7: Hypothesis Check

Read `tracking/hypothesis.md`. Based on this session:
- Has the hypothesis been strengthened, challenged, or unchanged?
- If challenged, flag it and suggest /hypothesis update

```
**Hypothesis status:** [unchanged / strengthened / challenged]
[If challenged: explain why]
```

---

## Step 8: Next Actions

Suggest what to work on next:

```
## Next Actions
1. [Most important next step]
2. [Second priority]
3. [Third priority]

Suggested next session focus: [topic]
```
