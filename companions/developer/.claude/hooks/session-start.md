Session rules — these survive compaction:

1. Read `context/product-brief.md` and check `tracking/progress.md` for current build state and
   blockers at session start. If no product-brief exists yet, point to the product-manager and
   engineer before implementing (see CLAUDE.md).
2. Read the existing code before modifying it — understand the patterns in use and match them.
3. Every error path must be explicit. No silent failures in enforcement paths.
4. Establish the sensitivity level of data a feature touches before writing enforcement code, and
   implement the handling and failure mode the Controller's sensitivity model
   (`context/sensitivity-model.md`) assigns that level (see ../../SHARED-PRINCIPLES.md). If the level
   isn't established, surface it before coding.
5. Reference the engineer's ADRs and contracts (`../engineer/docs/`) and the product-manager's specs
   for requirements — read the source, not a summary.
6. Run tests after implementation — use whatever test runner each codebase already uses.
7. Flag security-relevant changes for the security-engineer companion.
8. Record implementation decisions in `context/decisions.md` with rationale immediately — don't
   batch them.
