---
name: contract
description: >
  Define or review an integration contract between two components. Produces a structured interface
  contract document, including the sensitivity invariants the boundary must enforce.
disable-model-invocation: true
argument-hint: "[component-a] [component-b]"
---

# /contract — Integration Contract

Define the interface contract between two components or services.

**Usage:**
- `/contract client server` — Define/review the client↔server contract
- `/contract` — List existing contracts and their status

---

## Step 1: Identify Parties

If `$ARGUMENTS` specifies two components, use them.
If no arguments, list existing contracts in `docs/contracts/` and ask which to create or review.

---

## Step 2: Read Context

Read `context/product-brief.md` (the sensitivity map) and any relevant specs for both sides, plus
any existing contract in `docs/contracts/`.

---

## Step 3: Define Contract

```markdown
# Integration Contract: [Component A] ↔ [Component B]

**Date:** [today]
**Status:** [Draft / Active / Superseded]
**Version:** 1.0

## Interface Summary
[One paragraph describing what crosses this boundary]

## Data Sensitivity at this Boundary
[What sensitivity level is each field that crosses? What is the highest level? Per the product's
sensitivity model, this drives auth, encryption, and the invariants below.]

## Endpoints

| Endpoint | Direction | Protocol | Auth | Max level crossing |
|----------|-----------|----------|------|--------------------|
| [name] | A→B | [transport] | [auth method] | [level] |

## Request/Response Schemas

### [Endpoint 1]
**Request:** [schema with types]
**Response:** [schema with types]
**Error cases:** [enumerated error conditions and handling]

## Invariants
- [What must always be true at this boundary]
- [What must never cross this boundary — especially: no higher-sensitivity data than declared]

## Verification
- [How each side verifies the other, proportionate to the level crossing]

## Breaking Change Policy
- [What constitutes a breaking change, and how it's coordinated]
```

---

## Step 4: Validate

- Does the contract match what both sides' specs say?
- Are all error cases enumerated?
- Is the sensitivity invariant explicit and enforceable (not just asserted)?
- Can both sides be implemented independently?

---

## Step 5: Record

Write to `docs/contracts/[a]-[b]-contract.md`. Update `context/decisions.md` with any new decisions.
Flag for security-engineer if the boundary carries data at a level the model protects.
