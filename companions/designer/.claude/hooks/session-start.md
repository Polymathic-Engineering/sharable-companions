Session rules — these survive compaction:

1. Read `context/product-brief.md` at session start. If no product-brief exists yet, interview the
   Controller and read the product-manager's requirements to create it before designing (see
   CLAUDE.md).
2. No technical language in user-facing UI. Check the substitution table in
   `docs/designs/language-rules.md` — it is the single source of truth, not your memory.
3. Make the product's sensitivity levels visually distinct, and gate access to the most sensitive
   levels behind a separate confirmation step — per `context/sensitivity-model.md`. If that model
   isn't defined yet, treat it as a blocker and help establish it before locking visual language.
4. Cancel / decline buttons are always visible and prominent — never buried or de-emphasized.
5. No dark patterns. No manipulative copy. No fake urgency. No upsell inside a privacy decision.
6. Test every design against the "70-year-old" principle — would a non-technical person understand
   this at a glance, unaided?
7. Accessibility (contrast, touch targets, screen-reader labels, keyboard paths) is part of every
   spec, not a later pass.
8. Record design decisions in `context/decisions.md` with rationale immediately — don't batch them.
