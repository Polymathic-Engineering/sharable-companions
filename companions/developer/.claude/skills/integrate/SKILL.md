---
name: integrate
description: >
  Link two components together, implementing against the engineer's integration contract. Reads the
  contract, assesses the gap against the current code, and wires the two sides.
disable-model-invocation: true
argument-hint: "[component-a] [component-b]"
---

# /integrate — Component Integration

Link two components or services, driven by the engineer's integration contract.

**Usage:**
- `/integrate <component-a> <component-b>` — Wire two components together
- `/integrate` — Show integration status across the current build plan

---

## Step 1: Integration Status

If no arguments, read `tracking/progress.md` and display the integration items in flight:

```
## Integration Status

1. [Component A] ↔ [Component B]
   Status: [not started / in progress / done]
   Blocker: [if any]

2. [Component C] ↔ [Component D]
   Status: [not started / in progress / done]
   Blocker: [if any]
```

---

## Step 2: Read Integration Contract

If arguments specify two components:
1. Read `../engineer/docs/contracts/[a]-[b]-contract.md` if it exists — this is the authority for
   what crosses the boundary, the sensitivity invariants, and the verification each side performs.
2. If no contract exists, escalate to the engineer before wiring — don't invent the interface.
3. Read the relevant spec sections and any ADRs that govern the boundary.

---

## Step 3: Assess Gap

Compare the contract with what currently exists in the code:
1. Read the current code on both sides of the interface.
2. Identify what's missing, stubbed, or incompatible.
3. List the specific changes needed.

```
## Integration Gap: [Component A] ↔ [Component B]

**Contract says:** [what the interface should be]
**Code has:** [what currently exists]

**Changes needed:**
1. [Change on side A]
2. [Change on side B]
3. [Shared changes]
```

---

## Step 4: Implement

Follow the `/implement` workflow for each change needed.
Run integration tests after connecting.

The acceptance criterion is contract conformance: each side implements only to the contract. **If one
side needs changes that the contract did not call for, the contract was not met — escalate to the
engineer** rather than papering over the gap in code.

---

## Step 5: Record

Update `tracking/progress.md` with integration status. Note any contract gaps surfaced for the
engineer in `tracking/blockers.md`, and flag security-relevant boundaries for the security-engineer.
