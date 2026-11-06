---
globs: ["docs/analysis/**", "src/analysis/**", "docs/methodology/**", "docs/**/*analysis*.md"]
---

# Observable-Surface-Only Rule

Every analysis artifact and analysis script in these paths operates on the **observable surface** only —
the metadata, receipts, summaries, and telemetry the architecture intentionally exposes — never the
protected contents the system is built to keep. This discipline is what makes the architecture
trustworthy: the analyst cannot see what the analyst must not.

> **Glob-scope note.** These globs bind the rule to analysis paths structurally. A boundary that
> depends on the author remembering to opt in is a policy control; a glob is closer to structural.
> **When you add a new analysis path, extend this glob list in the same commit** — do not rely on
> remembering to apply the rule by hand.

## Allowed Data Sources (closed list — populate per product)

This list is **closed**. If an input is not on it, do not use it — amend the list here first, with
security-engineer review. Expanding it case by case inside an analysis is how a closed list stops being
one.

The entries below are a **template for the Controller and security-engineer to populate** for the
product at hand, anchored to `context/sensitivity-model.md` (which defines what counts as protected
content vs observable metadata). Each entry should name the source and a one-line justification that it
carries no protected content.

```
- <source name> — <what it exposes>; <why it carries no protected content>. Added <date> per <reviewer>.
- ...
```

Typical observable-surface categories (keep only those that apply, and name the concrete sources):

- **Receipts / attestation evidence** — e.g. transaction stubs, runtime-version evidence, measurement
  reports.
- **Aggregated request/response metadata** — gateway or proxy summaries with no payload contents.
- **Orchestration / routing telemetry** — dispatch and routing metadata.
- **Operator-side control-plane logs** — resource lifecycle events the operator necessarily generates
  by running the infrastructure (no principal-attributable or content field).
- **Operator-side cost / resource exports** — billing and usage metadata.
- **Sibling `tracking/` state** — sre operational status, security-engineer review-gate state, etc.
- **Controller-provided datasets** — any dataset the Controller explicitly hands off for analysis,
  recorded with its provenance.
- **Public / on-chain commitments** — published manifests or commitments.

> **When the sensitivity model or this list is undefined**, do not run analyses on real data. Help the
> Controller and security-engineer populate `context/sensitivity-model.md` and this list first.

## External-Disclosure Caution

Internal collection of observable-surface metadata is one thing; **externalizing it is another.**
Aggregated session or usage metadata that is safe to hold internally can, once published, re-identify
principals or become a **content oracle** — a figure that lets an outside observer distinguish what
protected content a flow carried (for example, duration or volume that correlates with the contents of
a session).

**Get security-engineer review before any external release of aggregated session/usage metadata**,
including evidence packages attached to a data room. Watch in particular for:

- **Re-identification** — per-session identifiers, timestamps, or small-bucket counts that fingerprint
  individuals or activity.
- **Content oracles** — pairing a metadata figure (duration, byte count) with a description of what the
  session did, which converts the figure into a signal about protected content.
- **Differencing attacks** — publishing one baseline now and a related distribution later can let an
  observer subtract one from the other to recover something neither was meant to reveal.

The specific redaction thresholds (what aggregation level, what minimum n, which fields to strip) are
**product-specific and defined with the security-engineer** — do not invent them. Ask before you
externalize.

## Pre-Commit Checklist

- [ ] Every input is from the enumerated closed allowed-source list above
- [ ] Report names what was observed and what was intentionally not observed
- [ ] Small-sample methodology: confidence intervals, not point estimates; claims labeled measured vs
      analyst-judgment
- [ ] **Novel metadata aggregations cleared with security-engineer BEFORE running** — not after
- [ ] Code references pinned dependencies for reproducibility
- [ ] Caveats and uncertainty stated plainly
- [ ] **If any material may go external: security-engineer review obtained before release**
