---
name: monitor
description: Define or review monitoring and alerting for a component.
disable-model-invocation: true
argument-hint: "[component]"
---

# /monitor — Monitoring Design

Define monitoring and alerting for a component, staying strictly on the observable surface.

**Usage:**
- `/monitor <component>` — Design monitoring for a specific component
- `/monitor` — Review overall monitoring coverage

---

## Observable Metrics (privacy-safe)

Only metrics on the surface the sensitivity model leaves open can be collected — these create no
inference channel into protected contents. Typical examples:

- Request / session counts (by component, by tier)
- Durations (request, provisioning, boot, teardown)
- Outcomes (success / failure / rejected / revoked)
- Resource utilization (compute, memory, pool/capacity headroom)
- Outbound call count and byte count (not content)
- Certificate / credential expiry dates
- Cost per unit

Confirm each metric against `context/sensitivity-model.md` before adding it — the exact safe set is
whatever that model leaves open for your product.

## What Must NOT Be Monitored

Anything the sensitivity model protects:

- The protected contents themselves (payloads, stored data, results)
- Any field or value the model classifies above the observable surface
- Per-item or per-request timing patterns fine-grained enough to correlate to protected data —
  aggregate only

If answering a monitoring question would require observing protected contents, it cannot be answered
that way; record the gap instead.

## Design Template

```markdown
# Monitoring: [Component]

| Metric | Source | Alert Threshold | Severity |
|--------|--------|----------------|----------|
| [metric] | [where to collect] | [when to alert] | [P1/P2/P3] |
```
