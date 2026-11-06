---
name: blocker-queue
description: >
  Blocker management. Four operations — list (default), add, update, escalate,
  close — over tracking/blocker-queue.md. Enforces the aging rule: any blocker
  with age_days > 14 must escalate or close; such items are flagged on every
  list. Read-only across siblings; this file is owned by program-manager.
disable-model-invocation: true
---

# /blocker-queue — Blocker Management

Single source of truth for cross-team blockers. Calm, operational, read-only-of-siblings. All reads
and writes target `tracking/blocker-queue.md`.

---

## Schema (one row per blocker)

| Field | Meaning |
|---|---|
| `id` | Sequential integer; next = max(id) + 1 |
| `owner` | Responsible companion (e.g., engineer, security-engineer, Controller) |
| `created_date` | YYYY-MM-DD when added |
| `age_days` | Auto-computed: today − created_date |
| `blocking` | What is blocked (milestone, ticket, step-gate, companion workstream) |
| `blocked_by` | What is blocking (another companion, external dep, decision, ticket) |
| `escalation_status` | `none` \| `flagged` \| `escalated` \| `closed` |
| `notes` | Free-form context, last update date prefixed |

Table form in `tracking/blocker-queue.md`:
```
| id | owner | created_date | age_days | blocking | blocked_by | escalation_status | notes |
```

---

## Aging Rule (enforced on every invocation)

Recompute `age_days` from `created_date` on every read. Any row where `age_days > 14` **and**
`escalation_status` is `none` or `flagged` is out of policy — it must escalate or close. Mark such
rows with a leading `!` in the rendered list and name them explicitly under an **Aging violations**
heading before the table.

---

## Operation Dispatch

Parse the argument after `/blocker-queue`:

- **(no argument)** — list all. Recompute ages, surface aging violations, render the table.
- **`add`** — append a new row. Prompt for `owner`, `blocking`, `blocked_by`, optional `notes`. Set
  `created_date` = today, `age_days` = 0, `escalation_status` = `none`, `id` = next sequential. Write,
  then list.
- **`update <id>`** — edit an existing row's `notes` and/or `escalation_status` (typical: `none` →
  `flagged`). Prepend today's date to the note. Write, then list.
- **`escalate <id>`** — set `escalation_status` = `escalated`, prepend "escalated YYYY-MM-DD:
  <reason>" to notes, and surface to the Controller (or route to PM if product-priority framing).
  Write, then list.
- **`close <id>`** — set `escalation_status` = `closed`, prepend "closed YYYY-MM-DD: <resolution>" to
  notes. Keep the row (historical record). Write, then list.

Unknown subcommand: reply with the five valid forms and stop.

---

## List Output Shape

```
# Blocker Queue — YYYY-MM-DD

## Aging violations (age_days > 14, not escalated/closed)
- (none) | ![id] [owner] [blocking] — age [N]d, status [none|flagged]

## Active
| id | owner | created | age | blocking | blocked_by | status | notes |

## Closed (last 30 days)
| id | owner | created | closed_on | blocking | resolution |
```

If the file is missing or empty of the header, treat as empty queue and render the shape with "(none)"
rows — do not create the file from here (it is initialized in its own ticket).

---

## Rules

- Program-manager owns this file. Siblings do not edit it. Blockers about a sibling surface via `/ask`
  to that companion; the outcome gets recorded here.
- Escalation is surfacing, not deciding. Offer options to the Controller; never rescope on a sibling's
  behalf.
- Closing requires a one-line resolution in notes. No silent closes.
- Calm operational voice. Lead with the state, then the consequence, then the options.
