---
name: decide
description: >
  Make and record an architecture decision. Evaluates alternatives, documents trade-offs,
  and produces an Architecture Decision Record (ADR).
disable-model-invocation: true
argument-hint: "[decision topic]"
---

# /decide — Architecture Decision

Evaluate alternatives and make a documented architecture decision.

**Usage:**
- `/decide session storage strategy` — Decide a specific topic
- `/decide` — Check open decisions needing resolution

---

## Step 1: Identify Decision

If `$ARGUMENTS` contains a topic, use it.
If no topic, read `tracking/open-decisions.md` and present unresolved items.

---

## Step 2: Frame the Decision

```
## Decision: [Topic]

**Context:** [Why this decision is needed now]
**Data sensitivity:** [which sensitivity levels the decision touches, per the product's model]
**Constraint:** [What limits our options]
**Stakeholders:** [Which companions are affected]
```

---

## Step 3: Evaluate Alternatives

For each viable alternative:
1. Describe the approach.
2. Evaluate against the architecture principles — especially sensitivity-proportionate handling,
   structural-vs-policy, and failure mode for the sensitivity levels involved (per
   `context/sensitivity-model.md`).
3. Identify trade-offs (what you gain vs what you lose).
4. Assess implementation complexity and timeline impact.

```
### Option A: [Name]
- Approach: [description]
- Handling vs level: [is each flow protected to the level its model requires, no more, no less?]
- Trade-off: [gain vs lose]
- Complexity: [low/medium/high]
- Timeline impact: [none/minor/significant]

### Option B: [Name]
[same structure]
```

---

## Step 4: Recommend and Confirm

```
**Recommendation:** Option [X]
**Rationale:** [Why this option, referencing the trade-off analysis]
**Risk:** [What could go wrong with this choice]

Agree? If so, I'll record this as an ADR.
```

---

## Step 5: Record ADR

After confirmation, write to `docs/adrs/YYYY-MM-DD-[topic-slug].md`:

```markdown
# ADR: [Title]

**Date:** [today]
**Status:** Accepted
**Deciders:** Engineer + Controller

## Context
[Why this decision was needed, including the sensitivity levels in play]

## Decision
[What was decided]

## Alternatives Considered
[Brief summary of rejected alternatives and why]

## Consequences
- [Positive consequence]
- [Negative consequence / trade-off]
- [Any structural-prevention gap this leaves, and how it's tracked]
- [Impact on other companions]
```

Update `context/decisions.md` and remove the item from `tracking/open-decisions.md` if applicable.
