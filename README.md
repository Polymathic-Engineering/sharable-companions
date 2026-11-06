# Privacy-Infrastructure Build Team

A reusable **Claude Code companion team** for building privacy-respecting software on
confidential-computing foundations. It is a set of role companions — product manager, program
manager, engineer, developer, security engineer, SRE, data scientist, designer — each a focused
persona with its own capabilities (slash-command skills, session rules, and guardrails), wired to
hand off to the others through files.

This is distilled from a working production build team and genericized as a best practice. It ships
with **no product defined and no sensitivity model defined**: you point it at your own product and it
interviews you to establish both.

---

## What makes this team distinctive

1. **A sensitivity-driven handling discipline.** Not all information deserves the same handling. The
   Controller defines a sensitivity model for the product — the levels (for example public,
   connected, sensitive, private) and how each is handled — and every companion classifies data
   *per data flow* and applies that model. Handling scales to sensitivity instead of one-size-fits-all.
   **The model itself is deliberately left for the Controller to define** — see
   [`SHARED-PRINCIPLES.md`](SHARED-PRINCIPLES.md).
2. **Clear ownership boundaries and an explicit operating model.** Each companion owns one lane and
   never silently overrides another. Work flows: product-manager (what) → engineer (how) →
   security-engineer (is it sound) → developer (build it) → sre (run it), with program-manager
   tracking delivery and data-scientist supplying evidence. The full handoff map and decision-rights
   model are in [`OPERATING-MODEL.md`](OPERATING-MODEL.md).
3. **Files as the interface.** Companions read each other's state in place and write only their own,
   so the team composes without stepping on each other.

---

## The companions

| Companion | Owns | Model |
|-----------|------|-------|
| [`product-manager`](companions/product-manager/) | Strategy, scope, priority, specs (*what* / *why*) | `claude-sonnet-5-5` |
| [`program-manager`](companions/program-manager/) | Milestones, dependencies, delivery, risk (*when* / *on-track?*) | `claude-haiku-5-5` |
| [`engineer`](companions/engineer/) | Architecture, protocol design, integration contracts | `claude-opus-5-5` |
| [`developer`](companions/developer/) | Implementation — code, tests, integration | `claude-opus-5-5` |
| [`security-engineer`](companions/security-engineer/) | Threat modeling, security review, crypto verification | `claude-opus-5-5` |
| [`sre`](companions/sre/) | Deploys, monitoring, incidents, cost, production readiness | `claude-sonnet-5-5` |
| [`data-scientist`](companions/data-scientist/) | Measurement, methodology, evidence (observable surface only) | `claude-sonnet-5-5` |
| [`designer`](companions/designer/) | UX/UI — making privacy comprehensible | `claude-sonnet-5-5` |

> **Model choices are a starting point.** Reasoning-heavy roles default to the most capable model;
> tracking and routine roles use lighter, faster models. Adjust the `model:` line in each
> `CLAUDE.md` to the models available on your account.

---

## How to adopt it

Each companion folder is a self-contained Claude Code working directory:

```
companions/<role>/
  CLAUDE.md                     the persona + role + constraints (Claude's context for this role)
  .claude/
    hooks/session-start.md      session rules that survive context compaction
    rules/*.md                  path-scoped guardrails
    skills/<name>/SKILL.md      slash-command capabilities (/brief, /spec, /review, ...)
```

1. **Copy `companions/` and `SHARED-PRINCIPLES.md`** into your project (keep them side by side; the
   personas reference the shared file).
2. **Open a companion folder as the working directory** in Claude Code (one role per session). Its
   `CLAUDE.md` loads as context; its skills become slash commands.
3. **Run the first session.** Start with `product-manager` or `engineer`. The companion will see it
   has no product defined and will interview you to fill `context/product-brief.md`, and to establish
   the product's sensitivity model at `context/sensitivity-model.md` — the shared decision lenses the
   rest of the team reads.
4. **Work the lanes.** product-manager writes specs → engineer designs → security-engineer reviews →
   developer implements → sre operates; program-manager tracks; data-scientist measures; designer
   shapes the experience. Each `/checkpoint` records handoffs the others pick up.

The `context/`, `docs/`, and `tracking/` directories are created by the companions as they work —
you don't need to pre-create them.

---

## Conventions worth knowing before you start

- **"The Controller"** = the human directing the build. Companions recommend; the Controller decides.
- **The sensitivity model is yours to define.** Before building, the Controller defines how each
  level of information sensitivity is handled; every companion then classifies data *per flow* and
  applies it, asking first if a flow's level isn't stated. See [`SHARED-PRINCIPLES.md`](SHARED-PRINCIPLES.md).
- **Decisions get written down** in `context/decisions.md` as they happen.
- **Handoffs land in `tracking/checkpoints.md`** — the top entry is what siblings read.

---

## Provenance

Genericized from a production confidential-computing build team. Product names, protocols, and
infrastructure specifics have been removed; what remains is the operating model — the roles, the
boundaries, the capabilities, and the sensitivity discipline — ready to point at a new product.
