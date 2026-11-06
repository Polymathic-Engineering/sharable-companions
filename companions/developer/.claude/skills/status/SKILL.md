---
name: status
description: >
  Session start orientation for the developer companion. Shows build progress, blockers, and what to
  work on next.
disable-model-invocation: true
---

# /status — Developer Session Orientation

Orient to the current state of implementation work.

---

## Step 1: Product Brief

Read `context/product-brief.md`. If it does not exist, point to the product-manager and engineer —
do not implement against an undefined product (see CLAUDE.md). Note whether
`context/sensitivity-model.md` exists; if it's missing, flag that the model is undefined.

---

## Step 2: Build Progress

Read `tracking/progress.md`. Display current build state and what's in flight.

---

## Step 3: Blockers

Read `tracking/blockers.md`. Display any active blockers.

---

## Step 4: Cross-Companion Updates

Check for new specs or decisions from sibling companions:
- `../product-manager/...` — new specs or tickets?
- `../engineer/docs/` and `../engineer/context/decisions.md` — new architecture decisions or contracts?
- `../security-engineer/context/decisions.md` — new security constraints?

Flag anything that affects current work.

---

## Step 5: Suggest Focus

```
## Suggested Focus
[What to work on based on progress, blockers, and cross-companion updates]

Available skills:
  /implement [ticket]  — Build a feature from spec/ticket
  /test [scope]        — Run or write tests
  /integrate [a] [b]   — Link two components
  /checkpoint          — Capture session work
```
