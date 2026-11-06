---
name: hypothesis
description: >
  Manage the product hypothesis — the central strategic bet. View, update, test,
  or review history. The hypothesis is the decision lens for all PM work.
disable-model-invocation: true
argument-hint: "[update|test|history]"
---

# /hypothesis — Product Hypothesis Management

The product hypothesis is the central decision lens. It holds whatever strategic bet the Controller
has made; every scope, priority, and feature decision is evaluated against it.

**Usage:**
- `/hypothesis` — View current hypothesis
- `/hypothesis update` — Refine based on new evidence
- `/hypothesis test` — Propose a cheap test
- `/hypothesis history` — Show evolution

---

## /hypothesis (view)

Read and display `tracking/hypothesis.md` in full:
- What we believe
- Who it's for
- Why it will win
- How we'd test it
- Confidence level
- Evidence for and against

After displaying, ask:
```
Does this still feel right? If something has shifted, run /hypothesis update.
```

---

## /hypothesis update

1. Read current hypothesis from `tracking/hypothesis.md`
2. Ask: "What changed? New evidence, shifted thinking, or a correction?"
3. Walk through each section of the hypothesis:
   - Does "what we believe" still hold?
   - Has the target user shifted?
   - Is the "why it will win" still the strongest argument?
   - Any new evidence for or against?
4. Update `tracking/hypothesis.md` with changes
5. Add entry to the History table at the bottom
6. Update confidence level:
   - **Exploring** — Still discovering, no clear bet yet
   - **Forming** — Pattern emerging, directionally clear
   - **Testable** — Specific enough to design a test
   - **Validated** — Evidence supports the hypothesis

After updating:
```
Hypothesis updated. Confidence: [level]

Key change: [what shifted and why]

This may affect:
- [decisions or specs that were based on the prior hypothesis]
```

---

## /hypothesis test

1. Read current hypothesis
2. Identify the riskiest assumption (the one that, if wrong, invalidates everything)
3. Propose a test:
   ```
   ## Hypothesis Test Proposal

   **Riskiest assumption:** [what]
   **Why it's riskiest:** [what happens if wrong]

   **Proposed test:**
   [Description — should be cheap and fast]

   **Cost:** [time and money]
   **Timeline:** [how long]
   **Success signal:** [what we'd see if the assumption holds]
   **Failure signal:** [what we'd see if it doesn't]

   Want to run this test?
   ```

---

## /hypothesis history

Read `tracking/hypothesis.md` and display the History table:

```
## Hypothesis Evolution

| Date | Change | Trigger |
|------|--------|---------|
```

Then ask: "Any patterns in how the hypothesis has evolved? This can reveal strategic drift or convergence."
