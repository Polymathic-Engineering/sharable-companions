---
name: design-test-plan
description: >
  Author a methodology spec for an empirical test. Used for conformance validation,
  channel measurement, and any other cross-companion empirical procedure. Collaborative
  with the engineer (architectural context) and the security-engineer (threat-model
  implications). Output lives in docs/methodology/.
disable-model-invocation: true
argument-hint: "[client/companion]"
---

# /design-test-plan — Test-Plan Authoring

Write a methodology spec that a downstream measurement run can follow without re-interpreting intent.
A test plan is an artifact other companions consume — it must stand alone.

---

## Step 1: Articulate the Hypothesis

Name what is empirically being verified. A good hypothesis is **falsifiable**:

- Bad: "The measurement is sensitive to changes."
- Good: "Any runtime-version change produces a measurement hash that differs from the prior version's
  hash, across at least N distinct runtime-change categories."
- Bad: "Latency is normalized."
- Good: "Response latency stays within the spec's normalization window for every request class, with no
  class distinguishable from another by timing at 95% confidence."

Capture:
- Hypothesis under test.
- What would falsify it (the observation that would say "no").
- Why this test matters (which downstream review, gate, or decision depends on it).

---

## Step 2: Identify Collaborators

Every test plan in this ecosystem has at least two co-authors. Name them explicitly:

- **Engineer** — architectural context, implementation realism, what the system can actually expose as
  a measurable signal.
- **Security-engineer** — threat-model implications, inference-channel safety of the measurement
  itself, pass/fail criteria that map to a security verdict.

Optionally:
- **SRE** — operational telemetry feasibility.
- **Developer** — whether the measurement hooks can be implemented in the test harness.

Write the collaborator list at the top of the spec. If the test involves a novel measurement channel,
security-engineer pre-clearance of the methodology (via `/measure-channel` Step 2) is required before
any run.

---

## Step 3: Draft the Methodology

Write the procedure. At minimum:

- **Preconditions** — what state must exist before running.
- **Procedure** — step-by-step, executable by someone who did not author it.
- **What is measured** — observable-surface signals, units, precision.
- **Sample design** — n, stratification, request/query mix if relevant.
- **Statistics** — which estimator, which CI method, which significance level.
- **Pass/fail criteria** — numeric thresholds tied to the Step 1 hypothesis.
- **Observable-surface pre-check** — explicit confirmation that the measurement stays on the observable
  surface and draws only from the closed allowed-source list.

Cite the primary authority for the measurement (the governing spec section, design doc, or standard).

---

## Step 4: Co-Author Round

Share the draft with the Step 2 collaborators. Collect:

- **Engineer** — correctness of architectural assumptions, realism of hooks, any missed preconditions.
- **Security-engineer** — threat-model signoff, inference-channel safety, verdict mapping.

Record each reviewer's sign-off (name, date, methodology version) in the spec itself. **The spec is not
final until every named collaborator has signed off.**

---

## Step 5: Write Methodology Spec

Write to `docs/methodology/<topic>-<scope>.md`. Structure:

1. Frontmatter (status: draft | co-authored | final; version; co-authors).
2. Hypothesis.
3. Why this test matters (downstream gate / review / decision).
4. Collaborators and sign-offs.
5. Methodology (Step 3 content).
6. Observable-surface pre-check affirmation.
7. What the test does **not** verify (explicit scope limits).

---

## Step 6: Handoff for Measurement

The methodology spec is the input to a downstream measurement run. Name the next skill and owner:

- If a channel measurement → `/measure-channel`, owned by data-scientist.
- If a conformance measurement the engineer runs → engineer's run, consuming this spec.
- If a cost / operational baseline → `/baseline`, owned by data-scientist.

Record the handoff in the methodology spec footer.

```
Available next skills:
  /measure-channel    — Run a channel measurement per this methodology
  /baseline           — Run a cost / operational baseline per this methodology
  /checkpoint         — Capture session work
```
