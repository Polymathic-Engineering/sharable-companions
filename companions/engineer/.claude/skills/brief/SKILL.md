---
name: brief
description: >
  Session start orientation for the engineer companion. Shows the product brief, open decisions,
  recent architecture changes, and current build state.
disable-model-invocation: true
---

# /brief — Engineering Session Orientation

Orient to the current state of the architecture and engineering work.

---

## Step 1: Product Brief

Read `context/product-brief.md`. If it does not exist, stop and interview the Controller to create it
(see CLAUDE.md) — do not proceed with architecture work until the product and its per-flow
sensitivity map are established.

Display the one-line product summary and the sensitivity map (which flows are at which level). If
`context/sensitivity-model.md` is missing, note that the model is undefined — a blocker to flag.

---

## Step 2: Open Decisions

Read `tracking/open-decisions.md`. Display all unresolved architecture decisions.

```
## Open Decisions
- [decision topic] — [why it's blocked or pending]
```

If none: "No open architecture decisions."

---

## Step 3: Recent Decisions

Read `context/decisions.md`. Display the last 5 decisions.

---

## Step 4: Build State

Read `tracking/build-dependencies.md` for current build-step status.

```
## Build State
Current focus: [what's being built]
Blockers: [any blockers]
Next: [what's needed]
```

---

## Step 5: Cross-Companion Check

Read sibling context for relevant updates:
- `../product-manager/context/decisions.md` — new requirements?
- `../security-engineer/context/decisions.md` — new security constraints?
- `../developer/context/decisions.md` — implementation issues?

Flag anything that affects architecture.

---

## Step 6: Suggest Focus

```
## Suggested Focus
[What to work on this session based on open decisions, build state, and cross-companion updates]

Available skills:
  /review [topic]     — Architecture review
  /decide [topic]     — Make an architecture decision
  /contract [a] [b]   — Integration contract
  /checkpoint         — Capture session work
```
