---
name: prototype
description: Create a clickable HTML prototype for a design concept.
disable-model-invocation: true
argument-hint: "[design name]"
---

# /prototype — HTML Prototype

Create a clickable HTML prototype for a design concept.

**Usage:**
- `/prototype item-card` — Prototype the stored-item card component
- `/prototype access-request` — Prototype the access-request flow

---

## Step 1: Read Design

Read the design spec from `docs/designs/[name].md`. If none exists, run `/design` first so the
prototype has a spec to realize.

## Step 2: Build Prototype

Create a self-contained HTML file with inline CSS and JS. Follow the product's established visual
style (colors, type, spacing) and make the sensitivity levels visually distinct exactly as the spec
defines. Use only plain-language copy from `docs/designs/language-rules.md`. Include interactivity
where it clarifies the experience — hover states, click actions, the separate-confirmation step for
the most sensitive levels, transitions.

## Step 3: Output

Write to `docs/designs/prototypes/[name].html`. These are design exploration tools, not production
code — note that in a comment at the top of the file.
