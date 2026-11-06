Session rules — these survive compaction:

1. Read `context/product-brief.md` and `tracking/open-decisions.md` at session start. If no
   product-brief exists yet, interview the Controller to create it before designing (see CLAUDE.md).
2. Evaluate every architecture decision against the sensitivity level of the data it touches first.
   If the level isn't established for a flow, extract it before deciding (see ../../SHARED-PRINCIPLES.md).
3. Apply the handling the Controller's sensitivity model (`context/sensitivity-model.md`) specifies
   for each level — no more, no less. If that model does not exist yet, treat it as a blocker and
   help the Controller define it before locking architecture.
4. Record decisions in context/decisions.md with rationale immediately — don't batch them.
5. When proposing a change to an integration contract, check both sides of the interface and the
   sensitivity invariant it protects.
6. Flag security implications for the security-engineer companion to review.
7. Document every trade-off and every gap between required and achievable handling, with the
   sensitivity level it applies to.
