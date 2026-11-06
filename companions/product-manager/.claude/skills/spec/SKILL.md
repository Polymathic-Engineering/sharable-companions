---
name: spec
description: >
  Generate implementation specs from strategic decisions. Turns PM thinking into
  structured documents the developer companion can execute from.
disable-model-invocation: true
argument-hint: "[topic/feature]"
---

# /spec — Implementation Spec Generation

Turn decisions into structured, executable specs. This is the primary output of the PM companion.

**Usage:**
- `/spec [feature-slug]` — Generate spec for a specific feature/topic
- `/spec` — Without a topic, ask what to spec

---

## Step 1: Determine Topic

If `$ARGUMENTS` contains a topic, use it.

If no topic:
```
What do you want to spec? Here are areas that might be ready:

[List topics from context/questions.md that have been answered,
 or decisions from context/decisions.md that need implementation]

Or describe what you want to spec.
```

---

## Step 2: Hypothesis Alignment Check

Read `tracking/hypothesis.md`. Evaluate whether the spec topic aligns with the hypothesis.

If aligned:
```
This aligns with the hypothesis: [brief explanation]
```

If misaligned:
```
⚠ Risk: This doesn't obviously connect to our hypothesis.
The hypothesis says: [summary]
This spec is about: [topic]

Is this intentional? (Sometimes important work doesn't touch the hypothesis directly.)
```

Proceed regardless — flag but don't block.

---

## Step 3: Gather Context

Read all relevant context:
- `context/product-brief.md` — the product and its per-flow sensitivity map
- `context/sensitivity-model.md` — how each sensitivity level is handled (flag if missing)
- `context/requirements.md` — for relevant requirements
- `context/constraints.md` — for relevant constraints
- `context/decisions.md` — for decisions related to this topic
- `tracking/insights-log.md` — for relevant insights
- Sibling companion context files if the topic crosses boundaries:
  - `../engineer/context/*` for architecture topics
  - `../security-engineer/context/*` for security topics
  - `../designer/context/*` for UX topics

---

## Step 4: Clarifying Questions

Before writing the spec, ask clarifying questions about anything unclear. One at a time.

Focus on:
- **Scope:** What's in v1 vs later?
- **Dependencies:** What must exist before this can be built?
- **Success:** How do we know it works?
- **Sensitivity:** What's the sensitivity level of each data flow this touches? (Extract it if
  unstated; see `../../SHARED-PRINCIPLES.md`.)
- **Risks:** What could go wrong?

Use Narrower behavior here: push for specific scope boundaries.

When clarity is sufficient, shift to Shaper mode:
```
I have enough to write the spec. Let me draft it.
```

---

## Step 5: Generate Spec

Create the spec file at `docs/plans/YYYY-MM-DD-[topic-slug]-spec.md`.

Use `templates/spec-template.md` as the base structure. Fill in all sections.

**Critical requirements for specs:**
- Reference actual paths in the project, not placeholders
- Include specific file paths for input/output in implementation notes
- Scope boundaries must be explicit — "in v1" and "out of scope" lists
- Success criteria must be measurable and verifiable
- Name the sensitivity level of each data flow and the handling it requires
- Dependencies on other companions must be named

---

## Step 6: Present for Review

Display the full spec and ask:

```
## Spec: [Title]

[Full spec content]

---

Does this capture it? Anything to add, remove, or change?

After approval:
- I'll record this in context/decisions.md
- The developer companion can use this spec for implementation
- Want me to break it into tickets?
```

---

## Step 7: Finalize

After approval:
1. Write the spec file if not already written
2. Update `context/decisions.md` with any new decisions from the spec
3. Log in `tracking/insights-log.md` if the spec process surfaced new insights

```
Spec written: docs/plans/[filename]

Next steps:
- /explore to investigate open questions from the spec
- /checkpoint to capture this session
```
