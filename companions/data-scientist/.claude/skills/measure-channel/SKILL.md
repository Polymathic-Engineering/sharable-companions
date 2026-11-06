---
name: measure-channel
description: >
  Inference-channel measurement workflow for timing, volume, and egress channels.
  Produces a methodology-plus-results report. Methodology must be pre-cleared with the
  security-engineer before any run. Use for timing-conformance checks, volume-accumulation
  analysis, and egress characterization against spec-defined thresholds.
disable-model-invocation: true
argument-hint: "[client/companion]"
---

# /measure-channel — Inference-Channel Measurement

Empirical measurement of a single inference channel. Outputs a report with methodology and results as
separate sections, so downstream reviewers (the security-engineer) can assess each independently.

---

## Step 1: Draft the Methodology

Name the channel of interest. Pick exactly one per run:

- **Timing** — latency-based leakage; does response time vary with protected content, or does it stay
  within the normalization window the spec defines?
- **Volume** — accumulation-based leakage; byte-count distributions over the session or request
  lifecycle.
- **Egress** — destination / size / frequency characterization of outbound traffic from the trust
  boundary.

Write a methodology draft covering:
- **Hypothesis under test** — what observation would falsify invariance / normalization / closure?
- **Measurement surface** — which observable-surface fields contribute (receipts, timestamp deltas,
  gateway summaries, telemetry). All from the closed allowed-source list.
- **Sample design** — n, stratification, request/query mix.
- **Pass/fail criteria** — numeric thresholds and tolerance bands. These are **Controller/spec-defined**
  — cite the governing spec; do not invent thresholds.

---

## Step 2: Security-Engineer Pre-Clearance (MANDATORY)

A novel channel measurement may itself reveal a new channel if the collection design is poor. **Never
skip this step.**

Write the draft methodology to a scratch markdown file and request review from the security-engineer
(via the handoff surface or an inline clearance request). Outcomes:

- **CLEARED** — proceed to Step 3 with any noted methodology changes.
- **BLOCKED** — the security-engineer identifies a concern; revise the methodology and re-request.

Record the clearance decision (who, when, which methodology version) at the top of the report in Step 6.

---

## Step 3: Observable-Surface Pre-Check

Confirm the measurement operates on the observable surface only:

- Timestamps, byte-counts, receipt hashes, summaries, telemetry — all from the closed allowed-source
  list in `.claude/rules/observable-surface-only.md`.
- Never protected contents, never plaintext payloads.

State this explicitly in the run log. If any field in the sampling plan touches protected content, halt
and return to Step 1.

---

## Step 4: Raw-Data Capture

Run the measurement per the cleared methodology. Capture:

- **Timing:** per-request latency (t_request → t_response), plus any stratification labels.
- **Volume:** per-session total bytes in / out; per-message byte counts.
- **Egress:** destination, response size, frequency histogram.

Raw samples land in `src/analysis/runs/YYYY-MM-DD-<channel>-raw.csv` (or equivalent). Every row carries
a pseudonymous session ID only — no principal-linking fields.

---

## Step 5: Analysis + Verdict

Compute:

- Sample statistics (mean, median, stddev, relevant quantiles).
- Confidence intervals at 95% (bootstrap or closed-form per distribution).
- Pass/fail against the Step 1 criteria.

Render a verdict in one of three buckets:

- **CONFORMANT** — results land within the pass criterion with CI overlap.
- **NON-CONFORMANT** — CI sits outside the pass criterion; the channel is not closed.
- **INDETERMINATE** — sample size insufficient; request more runs before calling it.

---

## Step 6: Report Output

Write to `docs/analysis/YYYY-MM-DD-<channel>-measurement.md` with two clearly separated sections:

### Methodology (pre-cleared Step 2 version)

- Hypothesis, measurement surface, sample design, pass/fail criteria, governing spec.
- Security-engineer clearance record (who, when, methodology version).
- Observable-surface pre-check affirmation.

### Results

- Raw-data summary + link to CSV.
- Computed statistics with CIs.
- Verdict (CONFORMANT / NON-CONFORMANT / INDETERMINATE) with reasoning.
- What was not observed (explicit gaps).
- Caveats and follow-ups.

---

## Step 7: Handoff

Post a handoff note to the security-engineer for verdict review. They own pass/fail certification; this
skill produces the evidence. If any figure may be externalized, flag it for external-disclosure review
per `.claude/rules/observable-surface-only.md`.

```
Available next skills:
  /design-test-plan    — Author a new methodology spec
  /checkpoint          — Capture session work
```
