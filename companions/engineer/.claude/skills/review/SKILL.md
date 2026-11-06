---
name: review
description: >
  Architecture review. Evaluates a design, spec, or implementation against the team's architecture
  principles and the information-sensitivity model. Produces a structured review document.
disable-model-invocation: true
argument-hint: "[topic or file path]"
---

# /review — Architecture Review

Evaluate a design against the architecture principles (CLAUDE.md) and the sensitivity model
(../../SHARED-PRINCIPLES.md).

**Usage:**
- `/review gateway` — Review a specific component
- `/review docs/contracts/foo.md` — Review a specific file
- `/review` — Ask what to review

---

## Step 1: Determine Subject

If `$ARGUMENTS` contains a file path, read it.
If `$ARGUMENTS` contains a topic, find the relevant material.
If no arguments, check `tracking/open-decisions.md` for items needing review.

---

## Step 2: Read Context

Read the subject material plus:
- `context/product-brief.md` — especially the per-flow sensitivity map
- `context/requirements.md` and `context/constraints.md`

---

## Step 3: Establish the Sensitivity of What's Under Review

Before evaluating, state which data flows the subject touches and the sensitivity level of each.
If a flow's level isn't clear from the product-brief, ask the Controller. The rest of the review is
graded against these levels and the handling `context/sensitivity-model.md` requires for them. If
that model doesn't exist yet, flag it — the review can't grade handling against an undefined model.

---

## Step 4: Evaluate Against Principles

```
## Architecture Review: [Subject]

**Date:** [today]
**Reviewer:** Engineer companion
**Sensitivity of subject:** [per-flow levels]

### Sensitivity-Proportionate Handling
Does each flow get the handling the sensitivity model requires for its level? Any flow under- or
over-protected?
- [Assessment]

### Structural vs Policy
Where the model requires a structural guarantee for a level, are enforcement mechanisms structural
rather than policy?
- [Assessment, with any documented gaps]

### Failure Mode
Does each flow fail in the way the model assigns its level?
- [Assessment]

### Isolation
Does any state cross a boundary it has no reason to cross?
- [Assessment]

### Inference Channels
Can observable metadata (timing, volume, egress) reconstruct higher-sensitivity content?
- [Assessment]

### Determinism
Are enforcement decisions deterministic? Does any probabilistic output reach enforcement for a level
the model protects structurally?
- [Assessment]

---

### Verdict
[PASS / PASS WITH NOTES / NEEDS REVISION]

### Issues
- [Issue — severity: critical/important/minor]

### Recommendations
- [Recommendation]
```

---

## Step 5: Record

Write the review to `docs/reviews/YYYY-MM-DD-[subject]-review.md`.
Update `context/decisions.md` if the review produced new decisions.
Flag items for the security-engineer companion if security-relevant.
