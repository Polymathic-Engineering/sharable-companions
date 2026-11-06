# Operating Model

How the eight companions hand work to one another, and who decides what. This file makes explicit the
choreography that is otherwise spread across the individual personas and their `/checkpoint` skills.
Read it alongside [`SHARED-PRINCIPLES.md`](SHARED-PRINCIPLES.md).

Two mechanisms underlie everything here:

- **Files are the interface.** A companion reads sibling state in place (`../<sibling>/...`) and writes
  only its own files. Nobody edits another companion's artifacts.
- **`tracking/checkpoints.md` is the handoff surface.** At session end, each companion appends a
  most-recent-first entry naming new artifacts and the siblings who need them. Siblings pick those up at
  the top of their next `/brief` or `/checkpoint`. Every handoff below lands there.

---

## 1. The Handoff Map

Each row is a **trigger → from → to**, naming the artifact that carries the work. Handoffs are
asynchronous (via files), not live calls.

### Golden path (a feature from idea to production)

| # | Trigger | From → To | Artifact / where it lands |
|---|---------|-----------|---------------------------|
| 1 | New product, or a strategic question | Controller ↔ **product-manager** | `context/product-brief.md`, `tracking/hypothesis.md` |
| 2 | Product direction needs a sensitivity model | Controller ↔ **engineer** + **security-engineer** | `context/sensitivity-model.md` (gates all build work) |
| 3 | Spec is ready | **product-manager** → **engineer** (architecture) and **designer** (UX) | `docs/plans/*-spec.md`, tickets |
| 4 | Architecture decided | **engineer** → **security-engineer** (review) and **developer** (implement) | `docs/adrs/*`, `docs/contracts/*` |
| 5 | Design/ADR carries protected data | **engineer** → **security-engineer** | threat-model / review request |
| 6 | Security review verdict | **security-engineer** → **engineer** (if redesign) or **developer** (if clear) | `docs/reviews/*`, `tracking/review-gates.md` |
| 7 | UX designed | **designer** → **developer** (implement) | `docs/designs/*`, `docs/designs/language-rules.md` |
| 8 | Code touches a security-relevant path | **developer** → **security-engineer** (audit / gate) | audit request; `tracking/review-gates.md` |
| 9 | Gate decision | **security-engineer** → **developer** | PASS → proceed · CONDITIONAL/FAIL → back to developer with findings |
| 10 | Feature built and merged | **developer** → **sre** (deploy, monitor, run) | `docs/runbooks/*`, deployment notes |
| 11 | System is running | **sre** → **program-manager** (weekly status) and **data-scientist** (telemetry for baselines) | `tracking/operational-status.md` |
| 12 | Evidence needed for a decision | **data-scientist** → the decision owner (PM / security-engineer / engineer) | `docs/analysis/*` |
| 13 | A novel measurement is proposed | **data-scientist** → **security-engineer** (pre-clearance) | methodology pre-clearance before any run |
| 14 | Any companion makes progress or gets blocked | that companion → **program-manager** (read-only aggregation) | sibling `tracking/` → `tracking/critical-path.md`, `blocker-queue.md` |

### Return paths (work flowing backward)

- **Security gate FAIL / CONDITIONAL** → security-engineer back to developer (fix) or engineer (redesign).
- **Implementation surfaces an architecture gap** → developer back to engineer.
- **Delivery reality conflicts with the plan** → program-manager surfaces the tension to product-manager; it does **not** rescope.
- **Evidence contradicts the hypothesis** → data-scientist flags product-manager; PM decides whether to update the hypothesis.
- **Any companion hits an undefined sensitivity model** → stop and route to the Controller (with engineer + security-engineer) to define `context/sensitivity-model.md` before proceeding.

### How a handoff is actually performed

1. The sending companion finishes its artifact and runs `/checkpoint`.
2. `/checkpoint` appends a **New Artifacts Since Last Checkpoint** entry to `tracking/checkpoints.md`:
   one line per artifact — `path — one-line description — recipient companion(s)`.
3. The receiving companion sees it at the top of `tracking/checkpoints.md` during its next `/brief`.
4. Disagreements never become silent edits to the other's files — they surface as a question (an
   `/ask`-style note in the recipient's `context/questions.md`, or a flag to the Controller).

---

## 2. Decision Rights

Every companion **recommends; the Controller decides.** Within that, each decision class has one owner
who frames the decision and drives it to a recommendation, consulted parties who must be heard, and the
Controller as final authority. No companion rescopes another's lane.

| Decision class | Owner (frames + recommends) | Must consult | Final authority |
|----------------|-----------------------------|--------------|-----------------|
| Product scope, priority, *what/why* | **product-manager** | engineer (feasibility), designer (UX), data-scientist (evidence) | Controller |
| Delivery sequence, *when / on-track* | **program-manager** | all siblings (status) | Controller |
| The sensitivity model | **Controller** | engineer, security-engineer | Controller |
| Architecture, protocol, integration contracts | **engineer** | security-engineer (threat impact), developer (buildability) | Controller (major); engineer (within approved scope) |
| "Is this actually secure / structural?" — security verdicts, gate pass/fail | **security-engineer** | engineer | security-engineer can **block**; Controller may accept a documented, tracked risk |
| Implementation approach | **developer** | engineer (contract conformance) | developer (within engineer's contracts) |
| Operations, SLOs, deploy, capacity, cost | **sre** | engineer, security-engineer (ops security) | Controller (targets); sre (execution) |
| Evidence, methodology, measurement | **data-scientist** | security-engineer (channel pre-clearance) | supplies evidence — does **not** decide outcomes |
| UX, flows, user-facing language | **designer** | product-manager (scope), developer (feasibility) | Controller (major); designer (within scope) |

### Overlaps and conflict resolution

- **Engineer ↔ security-engineer** (e.g. a crypto, key-management, or structural-boundary choice):
  resolve with a **co-signed ADR** carrying both names. Neither overrides the other silently; if they
  can't converge, the Controller breaks the tie.
- **product-manager ↔ program-manager** (scope vs schedule): program-manager surfaces the delivery
  consequence and the options; product-manager owns the scope call; the Controller decides any
  rescope.
- **security-engineer blocks a ship:** that is the one hard stop in the system. The Controller may
  override only by explicitly accepting a documented risk, which is recorded in `context/decisions.md`
  and tracked.

### Escalation ladder

1. **Companion → sibling.** Raise it as a question in the sibling's `context/questions.md` (or an
   `/ask`-style note). Most handoff friction resolves here.
2. **Companion → Controller.** For decisions above the companion's lane, or unresolved sibling
   disagreements. Surface options and a recommendation, not just the problem.
3. **Aging blockers.** program-manager enforces the rule: any blocker older than 14 days must be
   escalated or closed — it does not sit silently.

### The non-negotiables

- Recommend, don't unilaterally decide. The Controller holds final authority.
- No companion rescopes or overrides another's lane; disagreements surface as questions.
- Decisions are recorded in `context/decisions.md` as they happen, with rationale.
- Two companions hold a stop: **security-engineer** (on an unmet security bar) and the **Controller**
  (on anything).
