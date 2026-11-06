---
name: brief
description: >
  Session orientation. Reads every sibling's tracking/ and most-recent weekly-status
  to summarize what's in flight, blocked, and shipped since last session.
disable-model-invocation: true
---

# /brief — Session Orientation

Ground the program-manager in current team state. Read-only across siblings. Output is compact and
actionable — not a wall of text.

---

## Step 1: Read Own Tracking

Read the program-manager's working memory:
- `tracking/critical-path.md` — current milestone state
- `tracking/step-gates.md` — whatever steps security-engineer gates for this build
- `tracking/blocker-queue.md` — active blockers

Note the most-recent file in `tracking/weekly-status/`. That date is the "last session baseline." If
none exists, baseline is empty.

If the critical path is still template placeholders (no real sibling milestones), note that — status
can't be reported against a plan that doesn't exist yet.

---

## Step 2: Sibling Tracking Scan

Read tracking/ for all 7 siblings (read-only — never edit):

`../product-manager/tracking/`, `../designer/tracking/`, `../engineer/tracking/`,
`../developer/tracking/`, `../security-engineer/tracking/`, `../sre/tracking/`,
`../data-scientist/tracking/`

For each sibling, scan tracking/ for dates and status markers. Where present, also read
`../<sibling>/docs/plans/build-progress.md` for ticket movement. Missing tracking/ is a signal, not an
error — note it.

## Step 3: Diff Against Baseline

Using the last weekly-status date, classify:
- **Shipped since baseline** — completed/done with a date after baseline
- **In flight** — in-progress or active
- **Blocked** — explicitly blocked, blocker listed, or idle past ETA
- **New** — did not exist at last baseline

---

## Step 4: Compact Summary

Produce this shape. One line per bullet. Aim for ~10 bullets total, not thirty.

```
## Where we are — [today's date]
Baseline: [last weekly-status date or "first session"]

### Shipped since baseline
- [companion] [item] — [date]

### In flight
- [companion] [item] — [owner, ETA if known]

### Blocked
- [companion] [item] — blocked on [what/who], age [days]

### Step gates
- [gated step]: [state]  (only steps that moved or are blocked)

### Notes
- [missing tracking, divergences, handoffs to surface]
```

---

## Step 5: Suggested Focus

One or two lines. Name the most consequential thing for this session — a slip to discuss, a blocker to
escalate, a weekly-status due, a handoff to surface. If nothing is urgent, say so plainly.

```
## Suggested focus
[1-2 sentences]
Skills: /where-are-we  /weekly-status  /blocker-queue  /critical-path  /checkpoint
```
