---
globs: ["tracking/incidents.md"]
---

# Incident Tracking

Every incident entry captures these four fields so pattern-learning compounds:

- **What happened** — observable symptoms, time window, blast radius, scope of affected units
  (described on the observable surface only)
- **Root cause** — the underlying condition, grounded in metadata and control-plane evidence (never
  the contents the sensitivity model protects)
- **Resolution** — what restored service, including the runbook used and any deviations
- **Prevention** — the structural change queued to stop recurrence, with owner and target date

## Entry Header

Each entry starts with: incident ID, date, severity, duration, on-call responder.

## Discipline

- Log entries within 24 hours of resolution while context is fresh
- Security-classified incidents are co-owned with security-engineer — link their review
- Link the runbook used; if none existed, open a runbook ticket in the same commit
- Preserve the observable-surface boundary: no protected contents in the writeup
