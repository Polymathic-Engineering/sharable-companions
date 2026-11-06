---
name: audit
description: >
  Security audit of code or configuration. Reads code, checks against the security standard and the
  sensitivity model, produces a findings report.
disable-model-invocation: true
argument-hint: "[codebase or file path]"
---

# /audit — Security Audit

Audit code or configuration for security vulnerabilities.

**Usage:**
- `/audit src/gateway/` — Audit the gateway code
- `/audit src/crypto/` — Audit cryptographic code
- `/audit` — Check what needs auditing based on build step

---

## Step 1: Read Code

Read the target code. Also read:
- Prior threat models and reviews in `docs/threat-models/` and `docs/reviews/`
- `context/sensitivity-model.md` — the required handling for each level the code touches
- Relevant specs/contracts for expected behavior (check `../engineer/docs/`)
- `tracking/vulnerability-tracker.md` for known issues

---

## Step 2: Check Against Standards

For each file/module:
1. **Error handling:** are all failures explicit? Any silent failures? Does the failure mode match
   the sensitivity model for the level involved?
2. **Crypto:** correct algorithm? Correct parameters? Key material in the right boundary and zeroed
   after use? **Post-quantum-capable** (NIST ML-KEM / ML-DSA / SLH-DSA, hybrid during transition) and
   crypto-agile? Any quantum-vulnerable asymmetric crypto without a documented, Controller-accepted
   exception is a finding.
3. **Supply chain:** dependencies pinned and verified? Build provenance and SBOM present? Any
   unaccounted-for dependency is an attack path.
4. **Boundaries:** does data cross a boundary it shouldn't, above its permitted level?
5. **Input validation:** are all external inputs validated before reaching a trust/enforcement decision?
6. **Inference channels:** any operation whose timing, volume, or egress could leak protected content?
7. **Memory / lifetime:** any data that persists longer than its session or level scope?

---

## Step 3: Produce Findings

Write to `docs/reviews/YYYY-MM-DD-[scope]-audit.md`:

```markdown
# Security Audit: [Scope]

**Date:** [today]
**Files reviewed:** [list]
**Prior reviews checked:** [list]
**Sensitivity of flows:** [per-flow levels]

## Findings

| # | Severity | Finding | File:Line | Exploit Scenario | Recommendation |
|---|----------|---------|-----------|------------------|----------------|
| F-1 | [critical/high/medium/low] | [finding] | [location] | [how it's exploited] | [fix] |

## Summary
- Critical: [N]
- High: [N]
- Medium: [N]
- Low: [N]

## Verdict
[PASS / PASS WITH CONDITIONS / FAIL]
```

Update `tracking/vulnerability-tracker.md` with new findings.
