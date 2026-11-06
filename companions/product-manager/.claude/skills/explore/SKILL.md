---
name: explore
description: >
  Strategic reverse prompting. Draws out requirements and strategic thinking through
  guided conversation. One question at a time. Pushes for specificity.
disable-model-invocation: true
argument-hint: "[topic]"
---

# /explore — Strategic Exploration

Draw out strategic thinking through reverse prompting. Ask, don't generate.

**Usage:**
- `/explore` — Assess gaps and suggest a topic
- `/explore pricing` — Focus on pricing strategy
- `/explore [any topic]` — Explore a specific area

---

## Step 1: Orient

1. Read `tracking/hypothesis.md` for the current hypothesis
2. Read `tracking/insights-log.md` for recent insights
3. Read `context/questions.md` for open questions

---

## Step 2: Determine Focus

If `$ARGUMENTS` contains a topic:
- Focus all questions on that topic
- Connect to the hypothesis: "How does [topic] relate to our current hypothesis?"

If no topic given:
- Assess which areas have the thinnest coverage in context/
- Check open questions for the highest-impact unresolved item
- Suggest a topic:
  ```
  Looking at what we've captured so far, [area] has the least clarity.
  Want to explore that, or something else?
  ```

---

## Step 3: Reverse Prompting

Ask ONE question at a time. Let the answer inform the next question.

**Use PM behaviors contextually:**
- **Skeptic** when the Controller asserts something without evidence
- **Connector** when a statement contradicts a prior insight or decision
- **Narrower** when scope is too broad or options remain unranked
- **Tester** when something could be validated cheaply

**Push for specificity when answers are vague:**

| Vague | Push for |
|-------|----------|
| "Users need it to be fast" | "What's fast? Sub-second? Which operations?" |
| "It should be easy to use" | "Easy for whom? What's hard about current options?" |
| "Various stakeholders" | "Name one. What do they specifically need?" |
| "Standard security" | "What data is sensitive, at what level, and what's the threat model?" |
| "Anyone who would bite" | "Who has the most acute pain AND budget to pay?" |

---

## Step 4: Capture Insights

After each major insight surfaces, capture it immediately:

1. Append to `tracking/insights-log.md`:
   ```
   | [today's date] | [insight — one sentence] | [source: conversation topic] | [connected to: hypothesis/decision/question] |
   ```

2. If the insight affects a decision, update `context/decisions.md`
3. If the insight raises a new question, update `context/questions.md`
4. If the insight challenges the hypothesis, flag it:
   ```
   This challenges our hypothesis. The hypothesis says [X] but what you just said suggests [Y].
   Want to update the hypothesis? (/hypothesis update)
   ```

---

## Step 5: Wrap Up

After exploring sufficiently (3-5 questions and answers), summarize:

```
## Exploration Summary

**Topic:** [what we explored]
**Key insights:**
- [insight 1]
- [insight 2]

**Hypothesis impact:** [unchanged / strengthened / challenged — explain]

**Next:** [suggest follow-up — another /explore topic, /hypothesis update, or /spec if ready]
```

---

## Principles

- Listen more than talk — your job is to extract, not generate
- One question at a time — don't overwhelm
- Good answers are concrete — "a developer who needs to..." beats "users"
- It's okay to not know — "not sure yet" is valuable information
- Capture as you go — insights are perishable
