---
globs: ["docs/**", "context/**"]
---

Architecture decisions follow this evaluation order:

1. **Sensitivity first.** What is the sensitivity level of each data flow this touches? Classify per
   flow (see ../../SHARED-PRINCIPLES.md). Handling is proportionate to the highest level in the flow.
2. **Apply the Controller's model.** Look up the required handling for that level in
   `context/sensitivity-model.md` — access, encryption, enforcement posture, failure mode. If that
   file does not exist, the model is undefined: stop and help the Controller define it rather than
   guessing.
3. **Structural vs policy.** Where the model requires a structural guarantee for a level, is it
   preserved structurally (hardware- or architecture-enforced) rather than by policy? If only policy
   is available, document why structural isn't, and track the gap.
4. **Failure mode.** Does the flow fail in the way the model assigns its level (fail-closed/no-fallback
   vs graceful degradation)? Don't invent a posture the model doesn't specify.
5. **Isolation.** Does state cross a boundary it has no reason to cross (session, tenant, level)?
6. **Inference channels.** Can observable metadata (timing, volume, egress) reconstruct
   higher-sensitivity content? Consider this for every flow above the lowest level.
7. **Determinism.** Are enforcement decisions deterministic? Model reasoning is acceptable when
   bounded inside the trust boundary; probabilistic output must not reach enforcement for the levels
   the model protects structurally.

When trade-offs arise between performance and privacy on levels the model protects, privacy wins.
When trade-offs arise between complexity and correctness, correctness wins.
Don't over-protect low-sensitivity flows — a control that buys nothing is a cost.

Document every trade-off and every gap in an ADR, naming the sensitivity level it applies to and the
rejected alternatives.
