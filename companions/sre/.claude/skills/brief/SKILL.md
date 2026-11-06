---
name: brief
description: >
  Session start orientation for the SRE companion. Shows current system state, recent incidents,
  cost vs target, and deployments needing operational support.
disable-model-invocation: true
---

# /brief — SRE Session Orientation

Orient to the current operational state.

---

## Step 1: Product & Substrate Check

Confirm `context/product-brief.md` records what the system runs on and the Controller's SLOs and
cost target. If it does not exist, stop and establish it with the Controller before operating (see
CLAUDE.md). If `context/sensitivity-model.md` is missing, note that the observable surface is
undefined — a blocker to flag.

---

## Step 2: Operational Status

Read `tracking/operational-status.md`. Display current system state.

---

## Step 3: Recent Incidents

Read `tracking/incidents.md`. Show any recent incidents. If none: "No recent incidents."

---

## Step 4: Cost Tracking

Read `tracking/cost-tracking.md`. Show current cost per unit vs the Controller's target.

---

## Step 5: Cross-Companion Updates

Read sibling context for deployments or changes needing operational support:
- `../developer/tracking/checkpoints.md` — new deployments or fixes landing
- `../engineer/tracking/checkpoints.md` — infrastructure or contract changes that affect operations
- `../security-engineer/context/decisions.md` — new operational security constraints

---

## Step 6: Suggest Focus

```
## Suggested Focus
[What to work on this session based on status, incidents, cost, and cross-companion updates]

Available skills:
  /runbook [scenario]   — Create/update an operational runbook
  /monitor [component]  — Define or review monitoring
  /incident [desc]      — Incident response
  /checkpoint           — Capture session work
```
