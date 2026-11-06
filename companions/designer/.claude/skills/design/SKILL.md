---
name: design
description: >
  Design a screen, flow, or component for the product, in plain language, with the sensitivity model
  made visually legible.
disable-model-invocation: true
argument-hint: "[screen or flow name]"
---

# /design — UI/UX Design

Design a screen, flow, or component.

**Usage:**
- `/design onboarding` — Design the first-run setup flow
- `/design item-card` — Design the component that represents a stored item
- `/design access-request` — Design the inline flow where something requests access
- `/design` — Assess what needs design work

---

## Step 1: Read Context

- `context/product-brief.md` — the product, its users, and their comprehension level.
- `context/sensitivity-model.md` — the levels the UI must make distinct, and which require a separate
  confirmation step. If missing, stop and flag it (see CLAUDE.md).
- `docs/designs/language-rules.md` — the canonical term-substitution table; note the current version.
- `../product-manager/context/requirements.md` — the requirement this design serves.

---

## Step 2: Design

Produce:
1. **Screen layout** (ASCII or low-fidelity wireframe) — structure and hierarchy, not pixels.
2. **Component anatomy** — each element, its purpose, its states (default / hover / pressed /
   disabled / error / loading), the data it shows, and the real source of that data. No invented
   data sources.
3. **Interaction flow** — numbered user path, including entry points, success exit, and every
   decline / cancel / back path. Decline is never buried. Access to the most sensitive levels is
   always its own separate confirmation step.
4. **Copy** — exact user-visible text, following `language-rules.md`. No technical terms leak
   through.
5. **Edge cases** — at minimum: empty state, error state, offline / no-network, permission denied,
   most-sensitive-level gate, long-content truncation, and loading (fixed cadence — no percentages,
   no real-time stream where the sensitivity model treats pacing as a leak channel).

Apply the language-substitution table. Make each sensitivity level visually distinct. Test against
the 70-year-old principle.

---

## Step 3: Review

```
Does this design:
- [ ] Use only plain language (no technical terms from language-rules.md)?
- [ ] Make the sensitivity levels visually distinct?
- [ ] Put most-sensitive access behind a separate confirmation step?
- [ ] Keep cancel / decline visible and prominent?
- [ ] Avoid dark patterns (no fake urgency, no buried options, no manipulative copy)?
- [ ] Cover empty, error, offline, and permission-denied states?
- [ ] Meet accessibility basics (contrast, touch targets, screen-reader labels)?
- [ ] Pass the 70-year-old test?
```

---

## Step 4: Record

Write the spec to `docs/designs/[component-name].md` (kebab-case), declaring the
`language_table_version` it was checked against. Update `context/decisions.md` with any design
decision and its rationale. If a new technical term surfaced, add it to `docs/designs/language-rules.md`
and bump its version.
