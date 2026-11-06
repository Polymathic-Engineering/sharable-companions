---
name: analyze
description: >
  Privacy-respecting segmentation and analysis of a dataset the Controller provides.
  Confirms the data is on the observable surface, frames a hypothesis, segments on
  inferable attributes, computes small-sample-honest confidence intervals, and writes
  a markdown evidence report to docs/analysis/ for a sibling companion to consume.
disable-model-invocation: true
argument-hint: "[dataset / client / companion]"
---

# /analyze — Privacy-Respecting Segmentation & Analysis

Produce a rigorous, privacy-respecting analysis of a dataset the Controller hands off. Output is an
evidence package for whichever companion asked for it (often the product-manager). General-purpose: the
dataset and the segmentation dimensions come from the request, not from this skill.

---

## Step 1: Privacy-Boundary Pre-Check

Before touching any data, classify it against the observable-surface boundary and record the verdict:

```
Dataset:      [name / description of the dataset]
Fields:       [column inventory]
Observable?:  [Is every field observable-surface metadata, or does any field expose protected
               content / user activity? Check against context/sensitivity-model.md and the closed
               allowed-source list.]
Verdict:      CLEAR — on the observable surface  |  BLOCKED — contains protected content
Provenance:   [Controller handoff — path / location, confirmed in Step 2]
Channel risk: [Does the planned aggregation open a new inference channel? If NOVEL, security-engineer
               pre-clearance is REQUIRED before running — see .claude/rules/observable-surface-only.md.]
```

If any field goes beyond observable-surface metadata, **stop** and flag to the Controller +
security-engineer before continuing. If the aggregation is novel, get pre-clearance first.

---

## Step 2: Locate and Ingest the Dataset

Check in priority order:
1. The path named in the request or in `context/questions.md` (a resolved data-location entry).
2. A sibling `tracking/` folder, if the dataset was handed off through one.

If found, record: absolute path, format (CSV / sheet / JSON), column inventory, row count.

**If not found, output and stop:**

```
Status: DATA SOURCE NOT YET AVAILABLE

This analysis requires the dataset from the Controller handoff. No file was found at
any expected path.

Required action: Controller to drop the dataset at a known path and confirm the
location in context/questions.md, then re-run /analyze.

Nothing has been written. No analysis has run. Exiting.
```

---

## Step 3: Frame the Hypothesis (before segmenting)

State what you expect to find and what would change your mind, before running anything:

- **Question** — what decision does this analysis unblock, and for whom?
- **Hypothesis** — what you expect the segments to show.
- **Falsifier** — the result that would say your expectation is wrong.

Record these at the top of the report. Hypothesis-before-data is non-negotiable.

---

## Step 4: Segment on Inferable Attributes

Choose the segmentation dimensions from the request and the fields actually present. Document which raw
fields map to each dimension, and whether each mapping is **measured** (directly in the data) or
**analyst-judgment** (inferred, e.g. keyword-coded or bucketed). Typical shape:

```
Dimension A — [name]  (from [fields])  → labels: [...]   [measured | analyst-judgment]
Dimension B — [name]  (from [fields])  → labels: [...]   [measured | analyst-judgment]
Dimension C — [name]  (from [fields])  → labels: [...]   [measured | analyst-judgment]
```

Keep inference conservative: name the keyword/rule for each inferred label so another companion can
reproduce or challenge it. Add an `unknown` bucket wherever the mapping is uncertain — never force a
row into a label.

---

## Step 5: Compute Confidence Intervals

For proportions, use an interval appropriate to small n (e.g. Wilson score) — not a normal
approximation. Report every proportion as `% [95% CI]`. **Mark any cell where n is too small to support
a conclusion as UNRELIABLE** and draw nothing from it.

If the request asks for a target subset (an intersection of segments — e.g. rows matching dimensions A
AND B AND C), produce it as a count and a **row-index list only — no principal-identifying fields in the
report body.**

---

## Step 6: Write the Markdown Report

Create `docs/analysis/YYYY-MM-DD-<topic>-analysis.md` (today's date):

```markdown
# [Topic] Analysis
**Date:** YYYY-MM-DD  |  **n:** [row count]  |  **Source:** [path — Controller handoff]

## Privacy Statement
[Step 1 verdict verbatim. What was observed and what was not. Channel-risk disposition
and any security-engineer pre-clearance record.]

## Question & Hypothesis
[Step 3 — the decision this unblocks, the hypothesis, the falsifier.]

## Methodology
Dimensions: [list]. CI method: [e.g. Wilson score, 95%]. Cells with small n marked UNRELIABLE.
Field mapping: [raw fields → dimension, each labeled measured vs analyst-judgment].
Reproducibility: [code reference, pinned dependencies].

## Results
### [Dimension A]  |  [table: segment, n, %, 95% CI]
### [Dimension B]  |  [table: segment, n, %, 95% CI]
### [Dimension C]  |  [table: segment, n, %, 95% CI]

## Target Subset (if requested)
Row indices only. n = X rows meeting the intersection criteria.

## Caveats
- Self-reported / inferred fields may misrepresent reality — state which.
- Inferred labels are analyst-judgment, coded by rule; false positives expected.
- Small n limits resolution. Treat segments as directional, not definitive.

## Open Questions
- [Ambiguities or missing fields surfaced during analysis]
```

---

## Step 7: Handoff Note

Output a ready-to-share summary for the requesting companion:

```
Handoff → [companion]

Analysis complete.
Report: docs/analysis/YYYY-MM-DD-<topic>-analysis.md

Directional findings (see CIs in report):
  [Dimension A]:  [top 1–2 segments]
  [Dimension B]:  [top 1–2 segments]
  Target subset:  [n rows flagged, with 95% CI where applicable]

Next step: pull the detailed list or raise follow-ups via the handoff surface.
Open questions noted in report caveats. If any figure may go external, route it for
security-engineer external-disclosure review first.
```

```
Available next skills:
  /measure-channel    — Inference-channel measurement
  /design-test-plan   — Author a methodology spec
  /checkpoint         — Capture session work
```
