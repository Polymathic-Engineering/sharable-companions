---
name: synthesize
description: >
  Find patterns across accumulated insights. Surfaces connections the Controller
  may not have seen and suggests hypothesis refinements.
disable-model-invocation: true
---

# /synthesize — Cross-Session Pattern Discovery

Find patterns across accumulated insights that aren't obvious from any single session.

---

## Step 1: Load Insights

Read `tracking/insights-log.md` in full. Count total entries.

If fewer than 5 entries:
```
Only [N] insights captured so far. /synthesize works best with 5+ entries
across multiple sessions. Keep using /explore and /checkpoint to build up the log.
```
Stop here if fewer than 3 entries.

---

## Step 2: Cluster by Theme

Group insights by theme. Look for:
- Repeated patterns (the same insight surfacing from different angles)
- Contradictions (insights that pull in opposite directions)
- Convergence (multiple insights pointing to the same conclusion)
- Gaps (areas with no insights despite being important)

Present the clusters:
```
## Insight Clusters

**[Theme 1]** ([N] insights)
- [insight summary]
- [insight summary]
Pattern: [what these have in common]

**[Theme 2]** ([N] insights)
- [insight summary]
Pattern: [what these have in common]

**Contradictions:**
- [insight A] vs [insight B] — [why they conflict]

**Gaps:**
- [area with no insights] — [why this matters]
```

---

## Step 3: Hypothesis Implications

Read `tracking/hypothesis.md`. For each cluster:
- Does it strengthen the hypothesis?
- Does it challenge the hypothesis?
- Does it suggest a refinement?

```
## Hypothesis Implications

**Strengthened by:** [clusters that support the hypothesis]
**Challenged by:** [clusters that contradict it]
**Suggests refinement:** [clusters that suggest the hypothesis needs updating]
```

If refinement is suggested, propose specific language:
```
Current: "[current hypothesis statement]"
Suggested: "[refined hypothesis statement]"

Want to update? (/hypothesis update)
```

---

## Step 4: Surface Non-Obvious Connections

Look for connections between insights that the Controller may not have made:
- An insight from one domain that has implications for another
- A user insight that connects to a market or competitive insight
- A technical constraint that enables (not just limits) a strategic option

```
## Connections
- [Connection 1]: [insight A] + [insight B] suggests [implication]
- [Connection 2]: [insight C] combined with [decision D] means [implication]
```

---

## Step 5: Flag Stale Insights

Identify insights that may no longer apply:
- Insights older than 30 days with no recent reinforcement
- Insights that have been contradicted by newer evidence
- Insights tied to decisions that were later reversed

```
## Potentially Stale
- [date]: [insight] — [why it may be stale]

Consider removing or re-validating these.
```
