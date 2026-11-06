---
name: critical-path
description: >
  Milestone tracker. Cross-references PM's tickets/plan (canonical product plan)
  with developer's build-progress and security-engineer's review-gates, and maps
  live state onto the build's milestone sequence and whatever steps the
  security-engineer gates. Refreshes tracking/critical-path.md and
  tracking/step-gates.md, and renders an inline dashboard for the current session.
disable-model-invocation: true
---

# /critical-path — Milestone Tracker

The central delivery artifact. Keeps the build's milestone sequence and the security-gated steps in
one view — grounded in what siblings have actually shipped, not a wish list.

**The milestone sequence is not fixed in this template.** You build it from the actual sibling plans:
engineer's milestone/build sequence and PM's ticket plan. Do not use a canned step list, canned step
count, or canned week roll-up — those describe some other product. If the siblings have not defined a
sequence yet, say so and render an empty dashboard rather than inventing one.

PM's product plan (its `tickets.yaml` / `docs/plans/`) is the **authoritative product plan** — never
override or rescope it here. When delivery reality conflicts with it, surface the tension; do not edit
it.

Read-only across siblings. Writes only `tracking/critical-path.md` and `tracking/step-gates.md`.

---

## Step 1: Refresh Own Tracking

Re-read `tracking/critical-path.md` and `tracking/step-gates.md` to know the prior snapshot. This run
will overwrite both with the current truth.

---

## Step 2: Re-read Sibling Sources of Truth

- **Product plan (authoritative):** `../product-manager/docs/plans/tickets.yaml` (or its
  `docs/plans/`)
- **Milestone sequence:** `../engineer/tracking/`, `../engineer/docs/adrs/` — the engineer owns the
  build's milestone sequence; derive the ordered milestones from there plus developer requirements
- **Implementation status:** `../developer/docs/plans/build-progress.md`, `../developer/tracking/`
- **Gate verdicts + the gated set:** `../security-engineer/tracking/review-gates.md`,
  `../security-engineer/docs/plans/build-progress.md` — security-engineer declares *which* steps are
  gated
- **Upstream artifacts:** `../designer/tracking/`, `../data-scientist/tracking/`

Missing file is a signal — record it; do not create it.

---

## Step 3: Build the Milestone Sequence from Real Sibling Plans

Assemble the ordered milestones from engineer's sequence and developer's requirements — the actual
steps this build has, in dependency order. For each milestone, determine whether security-engineer
gates it (from `review-gates.md`): those are **this build's** security-gated steps, not a fixed list.
Coordinate the gated set with security-engineer; if it isn't declared yet, mark the gate column
`TBD` and flag it.

For every PM ticket, assign the milestone it feeds. Tickets that don't feed a build milestone
(discovery, spec, GTM) map to "supporting" and sit outside the sequence.

If no sequence has been defined by the siblings yet, stop here: report "No milestone sequence defined
yet — critical path is empty until engineer and PM establish one," and skip the dashboard.

---

## Step 4: Produce the Critical-Path Dashboard

One row per milestone, in dependency order — fill from the real sibling plans. Status: `pending` /
`in-progress` / `in-review` / `approved` / `blocked`. Gate: `SEC` if security-engineer gates it, `—`
otherwise, `TBD` if the gated set isn't declared yet. Owner is the companion on the hook; ETA is
sibling-stated or `—`.

This is the table **shape**, with placeholder rows — replace them with the build's actual milestones:

```
## Critical Path — [YYYY-MM-DD]

| # | Milestone | Owner | Status | Gate | ETA | Blocker |
|---|-----------|-------|--------|------|-----|---------|
| 1 | [first milestone from engineer's sequence]  | [owner] | [status] | [—/SEC] | [ETA] | [blocker] |
| 2 | [next milestone]                            | [owner] | [status] | [—/SEC] | [ETA] | [blocker] |
| 3 | [next milestone]                            | [owner] | [status] | [—/SEC] | [ETA] | [blocker] |
| … | [continue for the build's actual steps]     | …       | …        | …       | …     | …         |
```

Do not pad to a fixed number of rows. The sequence is as long as the build's real milestone list.

---

## Step 5: Roll Up to a Phase View

Group the milestones into whatever phases the build's own plan uses (sprint weeks, named
sub-milestones, release stages — read them from PM's plan and engineer's sequence; do not impose a
fixed week count). Summarize which milestones each phase lands and which gate reviews fall in it.

```
## Phase View

- **[Phase name / Week N]** — [milestones landing], [gate reviews due in this phase]
- **[Phase name / Week N]** — [milestones landing], [gate reviews due]
- … [as many phases as the build's plan defines]
```

Mark slippage against this projection and name the downstream consequence. If the plan defines no
phases yet, say so and omit this section rather than inventing one.

---

## Step 6: Write Tracking Files and Inline Output

Write:
- `tracking/critical-path.md` — Step 4 dashboard + Step 5 phase view + one-paragraph health call
- `tracking/step-gates.md` — only the steps security-engineer gates for this build: reviewer, queued
  date, verdict, blocker

Render the same dashboard inline, plus:
- **Health call** — on-track / at-risk / slipping, with the single most consequential driver named
- **Handoff surface** — upstream artifacts that just landed and unblock a downstream companion
