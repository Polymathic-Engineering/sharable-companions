---
name: audit-ux
description: Audit existing UI for language violations, dark patterns, sensitivity-legibility gaps, or accessibility issues.
disable-model-invocation: true
argument-hint: "[product or screen]"
---

# /audit-ux — UX Audit

Audit existing UI for design violations before it ships or hands off.

**Usage:**
- `/audit-ux onboarding` — Audit the first-run flow
- `/audit-ux [product]` — Audit a product surface end to end

---

## Checklist

For every screen:
1. **Language:** Any technical terms visible to users? Check against `docs/designs/language-rules.md`.
2. **Sensitivity legibility:** Are the sensitivity levels (per `context/sensitivity-model.md`)
   visually distinct at a glance?
3. **Consent:** Is access to the most sensitive levels always a separate confirmation step, never
   bundled into a bulk approval?
4. **Dark patterns:** Any buried or de-emphasized decline buttons, manipulative copy, fake urgency,
   or upsell inside a privacy decision?
5. **Status pacing:** Where the sensitivity model treats pacing as a leak channel, is status shown on
   a fixed cadence with no progress percentages and no real-time stream?
6. **Accessibility:** Text contrast, touch-target size, screen-reader labels, keyboard paths.
7. **Empty states:** What does the user see with no data?
8. **Error states:** Are errors in plain language, with a clear next step?

## Output

Write the audit to `docs/designs/audits/[product-or-screen].md`:

```markdown
# UX Audit: [Product/Screen]

| # | Issue | Severity | Location | Recommendation |
|---|-------|----------|----------|----------------|

handoff-ready: true | false
```

`handoff-ready: true` only when the issue table is empty AND `language-rules.md` compliance passes.
This is the developer handoff gate — see the "Developer handoff" row in `context/decisions.md` for
the full readiness criteria.
