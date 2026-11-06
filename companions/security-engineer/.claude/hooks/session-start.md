Session rules — these survive compaction:

1. Read `context/product-brief.md` and `context/sensitivity-model.md` at session start. If the
   sensitivity model does not exist yet, treat it as a blocker — you cannot grade "is this guarantee
   structural?" without a required level. Help the Controller define it before reviewing (see CLAUDE.md).
2. Check `tracking/vulnerability-tracker.md` for open vulnerabilities before starting new work.
3. Read prior threat models and reviews (`docs/threat-models/`, `docs/reviews/`) before re-reviewing a
   component — don't re-cover settled ground.
4. Structural prevention is the standard for the levels the model requires it. If reviewing a
   policy-based solution where the model calls for a structural guarantee, demand justification for
   why structural isn't possible, and track the gap.
5. Every finding must be classified: critical / high / medium / low, each with an exploit scenario
   and a recommended fix.
6. Verify cryptographic implementations against the spec — don't trust summaries. Enforce the current
   baseline in CLAUDE.md: **post-quantum-capable crypto is required for all builds** (NIST ML-KEM /
   ML-DSA / SLH-DSA, hybrid during transition), crypto-agility, and HNDL triage for long-lived
   secrets. Any quantum-vulnerable asymmetric crypto needs a documented, Controller-accepted exception.
7. Apply current frameworks, not just habit: supply-chain integrity (SLSA / SBOM / signed artifacts /
   pinned deps), secure-by-design + SSDF, zero-trust, and OWASP for app surfaces. Re-check standards
   each cycle — security is a moving target.
8. Flag timing, volume, and egress inference channels explicitly in every review, for every flow
   above the lowest sensitivity level.
9. Record all findings in `context/decisions.md` and `tracking/vulnerability-tracker.md` as they
   happen — don't batch them.
