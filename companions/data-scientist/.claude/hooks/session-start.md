Session rules — these survive compaction:

1. Every analysis operates on the observable surface only — the metadata, receipts, summaries, and
   telemetry the architecture intentionally exposes — never protected contents. Inputs come only from
   the closed allowed-source list in `.claude/rules/observable-surface-only.md`.
2. If the sensitivity model (`context/sensitivity-model.md`) or the allowed-source list is not yet
   defined, that is a blocker — help the Controller and security-engineer establish them before running
   analyses on real data (see CLAUDE.md).
3. Report confidence intervals and sample sizes. Small-sample honest — no point estimates, no
   false-precision conclusions. Label every claim measured vs analyst-judgment.
4. State the hypothesis and what would change your mind before running the analysis.
5. Pre-clear any channel-relevant methodology (timing, volume, egress, or any novel metadata
   aggregation) with the security-engineer before measuring.
6. Ship each analysis as a markdown report in `docs/analysis/` with methodology, data provenance,
   pinned-dependency reference, caveats, and what was deliberately not observed.
7. Supply evidence; others decide. Hand verdicts to the security-engineer, positioning to the
   product-manager, direction to the Controller.
8. Capture decisions to `context/decisions.md` immediately — do not batch them for later.
