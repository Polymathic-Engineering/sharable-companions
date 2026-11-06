---
name: status
description: >
  Session start orientation. Reads current hypothesis, recent insights, open decisions,
  and questions to orient the PM at the beginning of a session.
disable-model-invocation: true
---

# /status — Session Orientation

Orient to the current state of the product strategy.

---

## Step 1: Product Brief & Current Hypothesis

Read `context/product-brief.md`. If it does not exist, stop and interview the Controller to create it
(see CLAUDE.md) — do not produce specs until the product and its hypothesis are established.

Read `tracking/hypothesis.md`. Display:
- The core hypothesis (one sentence)
- Confidence level
- Last updated date

```
## Current Hypothesis
[hypothesis summary]
Confidence: [level] | Last updated: [date]
```

If `context/sensitivity-model.md` is missing, note that the model is undefined — a blocker to flag.

---

## Step 2: Recent Insights

Read `tracking/insights-log.md`. Display the last 3 entries.

```
## Recent Insights
- [date]: [insight]
- [date]: [insight]
- [date]: [insight]
```

If no entries exist: "No insights captured yet."

---

## Step 3: Open Questions

Read `context/questions.md`. Display all open questions.

```
## Open Questions
- [question] — [why it matters]
```

If no open questions: "No open questions — consider running /explore to discover gaps."

---

## Step 4: Recent Decisions

Read `context/decisions.md`. Display the last 5 decisions.

```
## Recent Decisions
| Decision | Date |
|----------|------|
```

---

## Step 5: Sibling Companion Check

Scan sibling companion context directories for recent changes:
- `../engineer/context/decisions.md`
- `../security-engineer/context/decisions.md`
- `../developer/context/decisions.md`
- `../designer/context/decisions.md`

If any sibling has decisions that aren't reflected in PM context, flag them:

```
## Cross-Companion Updates
- [companion] made a decision on [date]: [summary]
```

If no relevant updates: skip this section.

---

## Step 6: Suggest Focus

Based on the above, suggest what to work on this session:

```
## Suggested Focus
[1-2 sentence recommendation based on current state]

Available skills:
  /explore [topic]  — Strategic exploration
  /hypothesis       — View or update hypothesis
  /spec [topic]     — Generate implementation spec
  /checkpoint       — Capture session insights
  /synthesize       — Find patterns across insights
```
