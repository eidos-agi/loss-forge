# loss-audit — Validate Loss Function Quality

Audit a project's loss functions for completeness, correctness, and coverage. Are the metrics actually measuring what matters? Are any missions unmeasured? Are any losses gaming-prone?

## Trigger

User says `/loss-audit` or asks to evaluate the quality of existing loss functions.

## Instructions

### Step 1: Read Existing Loss Functions

Find the project's loss module (typically `loss.py`, `loss.ts`, or similar). Read it completely.

### Step 2: Check Coverage

**Mission coverage:**
- Read the project's README/CLAUDE.md for stated goals
- Does every stated goal have a corresponding M-score?
- Are there implicit goals (like "bills on time") that aren't being measured?
- Flag: "This project says it does X but doesn't measure X"

**Loss coverage:**
- Does the project have loss functions for:
  - [ ] Data correctness (are the outputs right?)
  - [ ] Data freshness (are the inputs current?)
  - [ ] Knowledge decay (are rules/config still relevant?)
  - [ ] Codebase health (is complexity growing?)
  - [ ] Agent efficiency (is the AI getting faster or slower?)
  - [ ] Feedback loop (can you verify changes quickly?)

**Agent-specific coverage (GUARD-003):**
- Is there a metric for tokens consumed?
- Is there a metric for files/context needed per operation?
- Is there a metric for time-to-complete?
- If not: flag "Agent efficiency is unmeasured — you're flying blind on cost"

### Step 3: Check Quality

For each metric, evaluate:

| Check | Pass/Fail |
|-------|-----------|
| **Computable** — Can it run without human input? | |
| **Deterministic** — Same state → same number? | |
| **Fast** — Completes in <30 seconds? | |
| **Directional** — Clear whether up or down is better? | |
| **Actionable** — If it's bad, do you know what to fix? | |
| **Non-gameable** — Can't improve by deleting data or adding noise? | |
| **Components exposed** — Shows what drove the number? | |

### Step 4: Check Weights

- Are weights assigned?
- Does the highest-weighted metric match the project's stated #1 priority?
- Are there metrics with weight 0 or equal weights across the board? (Symptom of not thinking about priorities)

### Step 5: Report

Output a structured audit:

```
LOSS AUDIT — <project>
Date: YYYY-MM-DD

COVERAGE:
  Missions: X/Y goals measured (list gaps)
  Losses: X/Y categories covered (list gaps)
  Agent metrics: yes/no

QUALITY:
  L1: PASS (all checks)
  L2: WARN — not actionable (high freshness doesn't tell you WHICH account is stale)
  M3: FAIL — gameable (categorization score goes to 100% if you delete uncategorized transactions)
  ...

WEIGHTS:
  Highest weighted: M1 bills_on_time (10) — matches stated priority #1 ✓
  Unweighted metrics: none ✓

RECOMMENDATIONS:
  1. Add agent efficiency metric (tokens per session) — currently flying blind
  2. Fix M3 to use categorized/total, not just count categorized
  3. Add component breakdown to L2 (which accounts are stale?)
```

## Rules

- **Coverage > precision.** It's better to have 7 rough metrics than 3 perfect ones. You can refine later; you can't improve what you don't measure.
- **Flag gameability.** If an agent can improve a score by doing something useless (deleting data, adding empty files, inflating counts), the metric is broken.
- **Agent metrics are mandatory.** Per GUARD-003, loss-forge requires that the agent's own efficiency is measured. A project without agent metrics fails the audit.
