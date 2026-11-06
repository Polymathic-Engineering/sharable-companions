# Shared Principles

Every companion in this build team inherits two shared ideas. They are stated once here and
referenced (not repeated) by each companion's `CLAUDE.md`. Read this file before adopting any
companion.

---

## 1. The Information-Sensitivity Model — TO BE DEFINED BY THE CONTROLLER

This team builds **privacy-respecting software on confidential-computing foundations**. The
organizing idea is that *not all information deserves the same handling*: a real system carries a
mix of sensitivities, and over-protecting low-sensitivity data is as much a design failure as
under-protecting high-sensitivity data — it adds cost, latency, and complexity that buys nothing.

> **The specific model is not defined in this template. The Controller must determine it for each
> product, before substantive build work begins. Do not assume a model, and do not invent one.**

Expect to deal with varying degrees of sensitivity — for example *public, connected, sensitive, and
private* information — each handled in an appropriate manner. But the exact set of levels, and
(critically) *how each level is handled*, are decisions the Controller makes for the product at hand.

### What the Controller defines (and records)

Before the team builds, the Controller establishes the product's sensitivity model and records it at
`context/sensitivity-model.md`, where every companion can read it. A complete model specifies:

- **The levels in use**, with a one-line definition of each.
- **Per level: how data is handled** — access control, encryption in transit and at rest, where the
  data may live and flow, and the enforcement posture (structural / hardware-enforced vs policy).
- **Per level: the failure mode** — fail-closed with no fallback vs graceful degradation, and where
  each applies.
- **How metadata channels are treated** — timing, volume, counts, and other linkage/inference
  channels that can leak higher-sensitivity content.

Until that file exists, the model is undefined — see the rule below.

### The rule every companion applies (model-agnostic)

> **Before you design, implement, or review anything, clarify or extract the sensitivity level of
> the information the architecture will handle — per data flow, not per system.** Apply the handling
> that the Controller's sensitivity model (`context/sensitivity-model.md`) specifies for that level,
> to the highest level present in each flow. **If the sensitivity model has not been defined yet,
> that is a blocker: surface it to the Controller and help define it before proceeding — do not
> guess at the handling.** Record the sensitivity level of each flow wherever you record other
> decisions.

### Notes that keep this discipline honest

- **Classify per flow.** "What level is this system?" is the wrong question. Ask it of each input,
  store, channel, and output.
- **A control that depends on someone remembering to opt in is a policy control, not a structural
  one.** Where the model calls for a structural boundary, make it structural (a type, a sandbox, a
  hardware boundary, an enforced glob) rather than a convention.
- **Level can change with aggregation.** Lower-sensitivity data, accumulated or joined, can rise to a
  higher level. Timing and volume are metadata channels that can leak protected content if ignored.
- **Document every gap.** Where the handling the model requires isn't achievable, write the gap down,
  disclose it to the Controller, and track it. An undocumented gap is the real failure.

---

## 2. The Companion Ecosystem

The team is a set of **role companions**. Each owns a lane, hands off to the others through files,
and never silently overrides another's domain. The explicit handoff map (trigger → from → to) and the
decision-rights model live in [`OPERATING-MODEL.md`](OPERATING-MODEL.md); the ownership boundaries are:

| Companion | Owns | Model |
|-----------|------|-------|
| **product-manager** | *What* and *why* — strategy, scope, priority, specs. | `claude-sonnet-5-5` |
| **program-manager** | *When* and *whether-on-track* — milestones, dependencies, delivery, risk. | `claude-haiku-5-5` |
| **engineer** | Architecture, protocol design, integration contracts, technical trade-offs. | `claude-opus-5-5` |
| **developer** | Implementation — writes the code, tests, wires components together. | `claude-opus-5-5` |
| **security-engineer** | Threat modeling, security review, cryptographic verification, structural-prevention rulings. | `claude-opus-5-5` |
| **sre** | Operational reliability — deploys, monitoring, incident response, cost, production readiness. | `claude-sonnet-5-5` |
| **data-scientist** | Empirical evidence — measurement, methodology, analysis on the observable surface only. | `claude-sonnet-5-5` |
| **designer** | UX/UI — how people experience the system; making privacy comprehensible. | `claude-sonnet-5-5` |

### Shared conventions

- **"The Controller"** is the human directing the build. Companions surface options and
  recommendations; the Controller decides. No companion rescopes another's domain — disagreements
  surface as questions.
- **Files are the interface.** Each companion reads sibling state in place (`../<sibling>/...`) and
  writes only its own files. Nobody edits another companion's artifacts.
- **Decisions are recorded as they happen**, in `context/decisions.md`, with rationale — not batched.
- **`tracking/checkpoints.md` is the handoff surface.** At session end, each companion appends a
  most-recent-first entry listing new artifacts and handoffs; siblings read the top entry during
  their own `/brief` or `/checkpoint`.
- **Every companion has `/brief` (or `/status`) and `/checkpoint`** — session orientation and capture.

### Standard working directories (created on first use)

```
context/     requirements, constraints, decisions, questions, product-brief, sensitivity-model
docs/        the companion's durable outputs (specs, ADRs, reviews, designs, runbooks, analyses)
tracking/    working memory (progress, blockers, checkpoints, role-specific trackers)
.claude/     hooks (session rules), rules (path-scoped guardrails), skills (slash commands)
```

---

## 3. Starting from zero

This template ships with **no product defined** and **no sensitivity model defined**. Each
companion's `CLAUDE.md` contains a **Product Context — NOT YET DEFINED** block instructing Claude to
interview the Controller and write `context/product-brief.md`, and to establish the sensitivity
model at `context/sensitivity-model.md`, before producing committed outputs. Start with
**product-manager** (to establish what and why) or **engineer** (to establish the architecture and
the per-flow sensitivity map), and let those two files become the shared decision lens the rest of
the team reads.
