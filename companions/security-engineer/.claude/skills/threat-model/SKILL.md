---
name: threat-model
description: >
  Create or update a threat model for a component or integration boundary, using STRIDE and the
  product's sensitivity model.
disable-model-invocation: true
argument-hint: "[component]"
---

# /threat-model — Threat Modeling

Create or update a threat model for a component or boundary.

**Usage:**
- `/threat-model gateway` — Threat model for the egress gateway
- `/threat-model client-server` — Threat model for the client↔server boundary
- `/threat-model` — List components needing threat models

---

## Step 1: Identify Component

Read the relevant spec, contract, or ADR (check `../engineer/docs/`). Identify:
- What the component does
- What boundaries it crosses
- What data it handles, and the sensitivity level of each flow (per `context/sensitivity-model.md`)
- What trust assumptions it makes

If the sensitivity model is undefined, flag it — you can't grade mitigations against a missing bar.

---

## Step 2: Enumerate Threats

For each boundary and data flow, evaluate using STRIDE:
- **Spoofing:** Can an attacker impersonate a party? (identity / attestation)
- **Tampering:** Can data be modified in transit or at rest? (encryption, signing)
- **Repudiation:** Can an action be denied? (receipts / audit log)
- **Information Disclosure:** Can protected data be observed, inferred, or reconstructed — by the
  operator, the cloud provider, a third-party integrator, or any other party? (isolation, plus
  timing, volume, and egress inference channels)
- **Denial of Service:** Can the system be made unavailable? (resource limits)
- **Elevation of Privilege:** Can a party exceed its declared scope? (enforcement boundary)

---

## Step 3: Assess Mitigations

For each threat, assess whether the mitigation is:
- **Structural** (hardware- or architecture-enforced) — acceptable
- **Cryptographic** (math-enforced) — acceptable
- **Code-enforced** (software) — acceptable only where the model does not require structural
  prevention for that level, or where structural isn't achievable; must document why
- **Policy-enforced** (operational procedure) — not acceptable where the model requires a structural
  guarantee for the level; find a structural alternative or document and track the gap

Grade against the level the sensitivity model assigns each flow — no more, no less.

---

## Step 4: Write Threat Model

Write to `docs/threat-models/YYYY-MM-DD-[component].md`:

```markdown
# Threat Model: [Component]

**Date:** [today]
**Scope:** [what's covered]
**Spec reference:** [path to relevant spec/contract/ADR]
**Sensitivity of flows:** [per-flow levels, per the sensitivity model]

## Component Summary
[What it does, boundaries, data flows]

## Threats and Mitigations

| # | Threat | Category | Level | Mitigation | Type | Residual Risk |
|---|--------|----------|-------|-----------|------|---------------|
| T-1 | [threat] | [STRIDE] | [level] | [mitigation] | Structural | [risk] |

## Open Items
- [Items needing resolution, including any documented structural-prevention gaps and how they're tracked]

## Verdict
[Assessment of overall security posture against the required handling]
```

Update `tracking/vulnerability-tracker.md` with any open items.
