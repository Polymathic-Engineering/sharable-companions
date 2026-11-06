---
globs: ["docs/**", "context/**"]
---

Security review checklist for every component. Grade each item against the Controller's sensitivity
model (`context/sensitivity-model.md`) — if that model is undefined, flag it first; the checklist
can't be graded against a missing bar.

1. **Privacy boundary.** Does the privacy boundary hold for the levels the model protects? Can the
   operator, cloud provider, a third-party integrator, or any other party observe data above its
   permitted level?
2. **Attestation / identity.** Where trust depends on it, is the other party's genuineness verified
   (attestation, signed identity) rather than self-asserted?
3. **Failure mode.** Does every verification failure fail in the way the model assigns the level —
   fail-closed with no fallback where the model requires it, graceful degradation where it allows it?
4. **Isolation.** Does any state cross a boundary it has no reason to cross — session to session,
   tenant to tenant, level to level?
5. **Inference channels.** Are timing, volume, and egress channels constrained so observable metadata
   can't reconstruct higher-sensitivity content, for every flow above the lowest level?
6. **Key custody.** Are keys held in the right trust boundary for their level (HSM, secure enclave,
   TEE, or as the model specifies), and zeroed after use?
7. **Post-quantum & crypto-agility.** Is all new asymmetric crypto post-quantum-capable (NIST ML-KEM /
   ML-DSA / SLH-DSA, hybrid during transition)? Are primitives swappable rather than hard-coded? Is
   any long-lived secret exposed to harvest-now-decrypt-later? Any quantum-vulnerable crypto needs a
   documented, Controller-accepted exception + migration plan.
8. **Supply chain.** Build provenance (SLSA), maintained SBOM, signed artifacts, pinned & verified
   dependencies — is each dependency accounted for as an attack path?
9. **Input validation.** Are all external inputs validated before they reach an enforcement or trust
   decision? For app/API surfaces, is OWASP ASVS / Top 10 covered?

Every finding needs: **severity** (critical / high / medium / low), an **exploit scenario**, and a
**recommended fix**.
