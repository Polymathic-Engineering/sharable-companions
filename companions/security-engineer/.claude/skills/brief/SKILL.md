---
name: brief
description: >
  Session start orientation for the security-engineer companion. Shows the sensitivity model status,
  open vulnerabilities, review-gate status, and cross-companion updates needing review.
disable-model-invocation: true
---

# /brief — Security Session Orientation

Orient to the current state of the security work.

---

## Step 1: Product & Sensitivity Model

Read `context/product-brief.md` and `context/sensitivity-model.md`. If the sensitivity model does not
exist, stop and flag it to the Controller — reviews can't grade "is this structural?" against an
undefined model (see CLAUDE.md). Display the levels in use and which demand structural prevention.

---

## Step 2: Open Vulnerabilities

Read `tracking/vulnerability-tracker.md`. Display open items grouped by severity
(critical / high / medium / low).

---

## Step 3: Review Gate Status

Read `tracking/review-gates.md`. Show which security-gated build steps have passed, are conditional,
or are pending review.

---

## Step 4: Cross-Companion Updates

- `../engineer/context/decisions.md` — new architecture decisions needing security review.
- `../developer/tracking/checkpoints.md` — build steps approaching a security gate.
- `../program-manager/tracking/` — which steps are designated security-gated and their timing.

Flag anything that needs a threat model, audit, or gate review.

---

## Step 5: Suggest Focus

```
## Suggested Focus
[What to work on this session based on open vulnerabilities, pending gates, and cross-companion updates]

Available skills:
  /threat-model [component]  — Create/update a threat model
  /audit [code path]         — Security audit of code or config
  /review-gate [step]        — Build-step security gate
  /checkpoint                — Capture session work
```
