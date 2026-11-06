---
name: review-gate
description: >
  Security review gate for build steps. Evaluates whether a security-gated build step has passed its
  required review before the developer can proceed.
disable-model-invocation: true
argument-hint: "[step number or name]"
---

# /review-gate — Build Step Security Gate

Evaluate whether a build step passes its security review before the developer proceeds.

**Usage:**
- `/review-gate 3` — Review gate for build Step 3
- `/review-gate egress-gateway` — Gate for a named step
- `/review-gate` — Show which steps need review

---

## Which Steps Are Gated

The security-gated steps are whatever the team defines for this build — there is no fixed list. Find
them by coordinating with the program-manager's step-gates (`../program-manager/tracking/`) and the
engineer (`../engineer/docs/`). A step is security-gated when it touches something the sensitivity
model protects: identity/attestation, key material, launch or enforcement policy, access control,
egress, or a boundary carrying data above the lowest level.

For each gated step, decide the review type(s) it needs:
- **Arch** — evaluate the design against the architecture principles and the required handling.
- **Vuln** — audit the implementation for security issues.
- **Threat** — enumerate threats (STRIDE) and verify mitigations.

Most gates need Arch; those landing new code need Vuln; those crossing a new trust boundary need
Threat.

---

## Step 1: Identify Step

If `$ARGUMENTS` specifies a step, use it. If no arguments, read the program-manager's step-gates and
the developer's `tracking/checkpoints.md`, and show which gated steps are pending review.

---

## Step 2: Conduct Review

Run the review type(s) the step needs:
- **Architecture review** — design vs principles and the sensitivity model.
- **Vulnerability review** — `/audit` the step's code.
- **Threat model** — `/threat-model` the step's new boundary.

Reference prior threat models and reviews in `docs/` to avoid re-covering ground. Grade every item
against the level the sensitivity model assigns the flows involved.

---

## Step 3: Gate Decision

```
## Review Gate: Step [N / name]

**Review type(s):** [Arch / Vuln / Threat]
**Status:** PASS / CONDITIONAL / FAIL

### Findings
[findings if any — each with severity, exploit scenario, recommended fix]

### Conditions (if conditional)
[what must be fixed before proceeding, and whether fixes may land in parallel]

### Verdict
[Developer MAY / MAY NOT proceed to the next step]
```

Update `tracking/review-gates.md` with the decision and
`tracking/vulnerability-tracker.md` with any findings.
