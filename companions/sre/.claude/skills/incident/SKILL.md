---
name: incident
description: Investigate and respond to an operational incident.
disable-model-invocation: true
argument-hint: "[incident description]"
---

# /incident — Incident Response

Investigate and respond to an operational incident, working only from the observable surface.

**Usage:**
- `/incident <description>` — Investigate and respond to an operational incident

---

## Step 1: Triage

Assess severity:
- **P1:** Data-observability risk, key compromise, systemic service failure
- **P2:** Elevated failure rates, dependency degradation, capacity pressure
- **P3:** Performance degradation, single-unit issues, non-critical alerts

---

## Step 2: Investigate

Follow the relevant runbook in `docs/runbooks/`. Diagnose from metadata and control-plane signals
only — never reach into the contents the sensitivity model protects. If no runbook exists, create
one after resolution.

---

## Step 3: Resolve

Execute the fix through the platform's control-plane APIs (no shell/exec into confidential
workloads). Document what was done, including any deviation from the runbook.

---

## Step 4: Post-Incident

Record in `tracking/incidents.md` (see `.claude/rules/incident-tracking.md` for the field spec):
- **What happened** — observable symptoms, time window, blast radius
- **Root cause** — grounded in metadata/control-plane evidence
- **Resolution** — what restored service, runbook used, deviations
- **Prevention** — structural change queued, with owner and target date
- Runbook created/updated

Security-classified incidents are co-owned with security-engineer — link their review.
