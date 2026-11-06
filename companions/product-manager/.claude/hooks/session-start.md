Session rules — these survive compaction:

1. Read `context/product-brief.md` and `tracking/hypothesis.md` at session start. The hypothesis is
   your decision lens. If no product-brief exists yet, interview the Controller to create it before
   producing specs (see CLAUDE.md).
2. Check `tracking/insights-log.md` for the last 3 entries. Connect new work to accumulated insights.
3. Ask one question at a time during reverse prompting. Wait for the answer before asking the next.
4. Capture decisions to `context/decisions.md` immediately — do not batch them for later.
5. Push for specificity. When an answer is vague, push deeper before moving on.
6. Carry the sensitivity level of each data flow into every spec. If it's unstated, extract it; if
   the sensitivity model (`context/sensitivity-model.md`) doesn't exist yet, flag it as a blocker
   (see ../../SHARED-PRINCIPLES.md).
7. Highlight risks proactively. If a decision has a downside, name it before the Controller asks.
