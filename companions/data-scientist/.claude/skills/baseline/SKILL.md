---
name: baseline
description: >
  Cost / operational baseline workflow. Reads the sre's operational telemetry, computes
  per-session and per-unit cost or resource figures, and compares against the Controller's
  target. Gracefully surfaces the dependency when the required telemetry is not yet
  instrumented.
disable-model-invocation: true
argument-hint: "[client/companion]"
---

# /baseline — Cost / Operational Baseline

Empirical measurement that answers a question of the form: "Are we actually at or under the target
cost / resource figure per session (or per unit of work)?" Consumes operational telemetry from the sre;
produces a markdown verdict.

---

## Step 1: Observable-Surface Pre-Check

Operational telemetry is metadata by design — session counts, provisioning events, commit/transaction
counts, egress byte-totals, cost line-items. It carries no protected content.

State this explicitly in the run log, and confirm each field is on the closed allowed-source list in
`.claude/rules/observable-surface-only.md`. If any telemetry field would expose protected content (e.g.
full request payloads), halt and escalate to the security-engineer.

---

## Step 2: Confirm the Target

Read the target figure from the Controller's spec or `context/` (e.g. a per-session or per-hour cost
ceiling, a resource budget). **The target is Controller/spec-defined** — do not invent one. If no
target is recorded, note that the baseline can still be computed but the verdict will be descriptive
(no PASS/FAIL), and flag the missing target as an open question.

---

## Step 3: Check SRE Telemetry Availability

Read `../sre/tracking/operational-status.md` and any cost/resource tracking the sre maintains.

Check for the minimum instrumentation set the baseline needs, for example:
- Per-session runtime (start/stop timestamps).
- Per-session work counts (commits, transactions, requests — whatever the unit is).
- Per-session egress byte-totals.
- Total attributable cost / resource for the measurement window.

If any required field is missing, **exit gracefully:**

```
Telemetry not yet instrumented by sre:
- [missing field 1]
- [missing field 2]

No baseline can be computed without these fields. Post a dependency note to sre and
revisit when instrumentation lands. Skill exits without writing a report.
```

If all fields are present, proceed.

---

## Step 4: Ingest Operational Telemetry

Pull the measurement window from the sre-provided exports in `../sre/tracking/`. Join on the session
pseudonym to collect per-session fields, for example:

- `runtime_seconds` — from provisioning start/stop.
- `work_units` — count per session (commits / transactions / requests).
- `egress_bytes` — outbound per session.
- `attributed_cost` — summed per session across contributing sources.

Raw rows land in `src/analysis/runs/YYYY-MM-DD-baseline-raw.csv`. Every row carries a pseudonymous
session ID — no principal-linking fields.

---

## Step 5: Compute Per-Session and Per-Unit Figures

For each session, compute the per-session figure and normalize to the target's unit (per hour, per work
unit, etc.). Aggregate across sessions:

- Mean, median, and the relevant high percentile (e.g. p95).
- 95% confidence interval on the mean (bootstrap; small-sample-honest).
- Sample size (n sessions in window).

If n is too small to support a conclusion, flag the result as UNRELIABLE and recommend a larger window.

---

## Step 6: Compare to Target

Render a verdict (only if a target exists — see Step 2):

- **PASS** — the upper CI bound sits at or below the target.
- **INVESTIGATE** — point estimate under target but CI crosses it; request more data.
- **FAIL** — the lower CI bound sits above the target.

Decompose the figure into contributing sources (runtime, per-unit gas/fees, egress, …) so an
INVESTIGATE / FAIL verdict points to the right component for optimization.

---

## Step 7: Write Report

Write to `docs/analysis/YYYY-MM-DD-baseline.md` with:

- **Methodology** — measurement window, telemetry source, aggregation method, CI method,
  observable-surface pre-check affirmation.
- **Data** — sample size, raw-data link, measurement-window boundaries.
- **Results** — per-session distribution, normalized figure with CI, decomposition by source.
- **Verdict** — PASS / INVESTIGATE / FAIL with reasoning (or descriptive, if no target).
- **What was not observed** — telemetry gaps, excluded session types.
- **Caveats** — small-sample honesty, window-effect warnings.
- **Handoff** — to the product-manager for pricing/scope decisions; to the sre for optimization if FAIL.

```
Available next skills:
  /measure-channel    — Run a channel measurement
  /design-test-plan   — Author a methodology spec
  /checkpoint         — Capture session work
```
