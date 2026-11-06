---
name: weekly-status
description: >
  Weekly status report. Same template every week — shipped, slipped, blocked,
  at-risk, decisions-pending, upcoming. Aggregates own tracking + every sibling's
  tracking/ and build-progress; pays special attention to SRE's operational-status
  per the SRE handoff protocol. Writes to tracking/weekly-status/YYYY-MM-DD.md.
disable-model-invocation: true
---

# /weekly-status — Weekly Report

One report per week. Same six sections every time so the Controller can read it in five minutes and
recognize the shape instantly. Read-only across siblings — aggregate their state, never edit their
artifacts.

---

## Step 1: Gather Inputs

Read own working memory:
- `tracking/critical-path.md` — milestone state
- `tracking/step-gates.md` — verdicts for the steps security-engineer gates
- `tracking/blocker-queue.md` — active blockers with age
- Previous `tracking/weekly-status/*.md` — last week's baseline for diffing

Read every sibling's tracking + build-progress (fresh read, not cached) — all 7:
- `../product-manager/`, `../designer/`, `../engineer/`, `../developer/`, `../security-engineer/`,
  `../sre/`, `../data-scientist/`
- For each: `tracking/**` and `docs/plans/build-progress.md` where present. Missing tracking is a
  signal to note, not to create.

---

## Step 2: SRE Operational-Status (Special Attention)

Per the SRE handoff protocol, SRE provides `tracking/operational-status.md` as a scheduled handoff
sized for the weekly report. Pull explicitly whatever SRE reports there, typically:
- **Incidents** — anything this week, severity, status
- **Capacity** — headroom and resource health for the live system
- **SLO burn** — against whatever reliability targets the build has defined (SRE owns the targets;
  report against those, don't invent numbers)
- **Cost trajectory** — spend vs. whatever budget target has been set

Fold SRE's state into the relevant sections below (usually at-risk and upcoming). If SRE has no
operational-status.md yet, note the gap and proceed.

---

## Step 3: Generate Report Using the 6-Section Template Verbatim

Use this template exactly. Same shape every week — reproducibility is the point.

```
# Weekly Status — YYYY-MM-DD

## Shipped
- [companion] [item] — [landed date, downstream effect]

## Slipped
- [companion] [item] — [original ETA → new ETA, cause, consequence on dependents]

## Blocked
- [companion] [item] — [blocker, owner, age, escalation status]

## At-risk
- [companion] [item] — [risk, signal, mitigation options]

## Decisions-pending
- [decision] — [owner, deadline, what unblocks when decided]

## Upcoming
- [companion] [item] — [ETA this week or next, dependencies]

## Call
[one sentence: the most consequential thing for the Controller to know or decide this week]
```

Rules per section:
- **Shipped** — completed work since last weekly. One line each. Name the downstream effect if any.
- **Slipped** — missed ETAs with new ETAs and cause. Surface consequence on dependents plainly.
- **Blocked** — active blockers from blocker-queue.md. Show age.
- **At-risk** — not yet slipped but trajectory is wrong. Offer options, not verdicts.
- **Decisions-pending** — open decisions from siblings' decisions.md / open-decisions.md that gate
  delivery.
- **Upcoming** — landing this week or next. Flag cross-companion handoffs.

Omit a section only if truly empty — write "(none)" rather than deleting the heading, so the shape
stays recognizable.

---

## Step 4: Write to tracking/weekly-status/YYYY-MM-DD.md

Compute YYYY-MM-DD as today's date. Write the report to `tracking/weekly-status/YYYY-MM-DD.md`.
Overwrite if the file already exists (one report per date).

Return the report inline to the Controller — they read it here, not from the file.
