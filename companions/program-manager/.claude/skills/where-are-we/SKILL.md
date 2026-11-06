---
name: where-are-we
description: >
  On-demand team status. Mid-session re-read of sibling tracking/ plus own
  critical-path, step-gates, and blocker-queue. Produces a 5-8 bullet current-state
  snapshot focused on cross-companion dependencies and step gates.
disable-model-invocation: true
---

# /where-are-we — On-Demand Status

Ad-hoc snapshot the Controller invokes mid-session. Different from `/brief`: `/brief` grounds at
session start and diffs against the last weekly-status baseline. `/where-are-we` is a fresher re-read
focused on right-now state — cross-companion dependencies, step-gate movement, and any newly-blocked
or newly-unblocked items since the last session interaction. No baseline diff. Tighter output.

Read-only across siblings. Never edit their artifacts.

---

## Step 1: Re-read Own Latest Deltas

Read the program-manager's working memory for the freshest view:
- `tracking/critical-path.md` — milestone state right now
- `tracking/step-gates.md` — current verdicts for the steps security-engineer gates
- `tracking/blocker-queue.md` — active blockers, ages, escalation status

Note anything edited since the last session interaction — those are the deltas this snapshot should
surface.

---

## Step 2: Re-scan Sibling Tracking (Read-Only)

Re-read tracking/ for all 7 siblings — this is a fresh read, not cached:

`../product-manager/tracking/`, `../designer/tracking/`, `../engineer/tracking/`,
`../developer/tracking/`, `../security-engineer/tracking/`, `../sre/tracking/`,
`../data-scientist/tracking/`

For each, also re-read `../<sibling>/docs/plans/build-progress.md` where present. Missing tracking/ is
a signal — note it; do not create it.

---

## Step 3: Identify Cross-Companion Signals

Focus — not a full diff, just what matters right now:
- **Newly blocked** — any sibling item that became blocked since last interaction
- **Newly unblocked** — any upstream artifact landed that releases a downstream companion (a handoff
  to surface)
- **Step-gate movement** — any gated step changed verdict (in-flight → review → approved, or review
  stalled past ETA)
- **Cross-companion dependency at risk** — a slip upstream that will hit a downstream ETA

---

## Step 4: Compact Snapshot

One line per bullet. Aim for 5-8 bullets total. If nothing fits a section, omit the section — don't
pad.

```
## Where we are — [today's date, HH:MM]

### Step gates
- [gated step]: [state]  (only steps in-flight, blocked, or just moved)

### In flight
- [companion] [item] — [owner, ETA]

### Newly blocked / newly unblocked
- [companion] [item] — [cause or released-by upstream]

### Dependencies at risk
- [downstream] waiting on [upstream] — [consequence]
```

---

## Step 5: One-Line Call

End with a single line: the most consequential thing right now. A slip to escalate, a handoff to
surface, a gate review stalled — or "no urgent action." Trust the Controller to decide.

```
## Call
[one sentence]
```
