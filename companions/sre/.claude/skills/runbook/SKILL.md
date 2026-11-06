---
name: runbook
description: Create or update an operational runbook for a specific scenario.
disable-model-invocation: true
argument-hint: "[scenario]"
---

# /runbook — Operational Runbook

Create or update a runbook for an operational scenario.

**Usage:**
- `/runbook <scenario>` — Runbook for a specific scenario
- `/runbook` — List existing runbooks and suggest gaps

---

## Step 1: Identify Scenario

If arguments given, use that scenario. If none, scan `docs/runbooks/` and compare against the
operational scenarios your product can hit. Common examples:

- Service provisioning failure
- Dependency / upstream outage
- Certificate or credential expiry
- Capacity exhaustion (pool, quota, resource limits)
- Key compromise (fast-response scenario)
- Deploy rollback

(Derive the real list from `context/product-brief.md` and the engineer's contracts — these are
illustrative, not fixed.)

---

## Step 2: Write Runbook

Write to `docs/runbooks/[scenario].md`. Resolution steps act through the platform's control-plane
APIs — never by shelling into a confidential workload — and every step stays on the observable
surface.

```markdown
# Runbook: [Scenario]

**Last updated:** [date]
**Owner:** [who]
**Last drilled:** [date]
**Severity:** [P1/P2/P3]
**SLA:** [response-time requirement, if any]

## Detection
How to detect this scenario:
- [signal 1]
- [signal 2]

## Diagnosis
Steps to confirm the condition and rule out neighbors:
1. [step — names the observable-surface check it relies on]
2. [step]

## Resolution
Steps to restore service (control-plane APIs only, copy-paste-ready commands):
1. [step]
2. [step]

## Escalation
When to escalate and to whom, and what context they need.

## Prevention
The follow-up change that stops recurrence, with owner.
```
