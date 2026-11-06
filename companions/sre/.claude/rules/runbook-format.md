---
globs: ["docs/runbooks/*.md"]
---

# Runbook Format

Every runbook follows this section order so on-call reaches the right action fast:

1. **Detection** — the signal that triggers this runbook (alert, symptom, telemetry)
2. **Diagnosis** — how to confirm the condition and rule out neighbors
3. **Resolution** — the exact steps to restore service, commands included
4. **Escalation** — who to page, when, and what context they need
5. **Prevention** — the follow-up change that stops recurrence

## Required Header

Each runbook starts with: title, date authored, owner, last-drilled date.

## Writing Standards

- Commands copy-paste ready; no placeholders without a `# replace with` comment
- Every step names the observable-surface check it relies on (metadata and control-plane signals
  only — never the contents the sensitivity model protects)
- Resolution steps assume no shell/exec into the confidential workloads — act through the platform's
  control-plane APIs
- Cite the architecture decision, contract, or security review that grounds the procedure
- Review on a set cadence; update the last-drilled date after every real or simulated use
