---
name: brief
description: >
  Session start orientation for the designer companion. Shows the product brief, recent design
  decisions, new requirements needing design, and the current language-rules version.
disable-model-invocation: true
---

# /brief — Designer Session Orientation

Orient to the current state of design work.

---

## Step 1: Product Brief

Read `context/product-brief.md`. If it does not exist, stop and interview the Controller (and read
the product-manager's requirements) to create it — do not proceed with design work until the
product, its users, and their comprehension level are established (see CLAUDE.md).

Display the one-line product summary and who the users are. If `context/sensitivity-model.md` is
missing, note that the model is undefined — a blocker to flag, since the visual language depends on
it.

---

## Step 2: Recent Decisions

Read `context/decisions.md`. Display the last 5 design decisions with their rationale.

---

## Step 3: PM Requirements

Read `../product-manager/context/requirements.md` for new or changed feature requirements that need
design. Flag anything user-facing that has no spec yet.

---

## Step 4: Language Rules State

Read `docs/designs/language-rules.md` and display its current `language_table_version`. Note any
design specs that were checked against an older version (they are unverified until re-checked). If
the file does not exist yet, note that the canonical language table is undefined.

---

## Step 5: Suggest Focus

```
## Suggested Focus
[What to work on this session based on new requirements, unresolved specs, and open questions]

Available skills:
  /design [screen]      — Design a screen, flow, or component
  /prototype [name]     — Create a clickable HTML prototype
  /audit-ux [product]   — Audit existing UI for violations
  /checkpoint           — Capture session work
```
