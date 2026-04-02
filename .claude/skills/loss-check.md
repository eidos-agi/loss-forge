# loss-check — Run Loss Functions and Evaluate a Change

Run the project's loss command before and after a change to determine if the change was an improvement, regression, or lateral move.

## Trigger

User says `/loss-check` or asks to evaluate whether a change helped.

## Instructions

### Step 1: Run Pre-Change Snapshot

If you haven't already, run the project's loss command:
```
<project> loss --json > /tmp/loss-before.json
```

Record the composite loss and mission scores.

### Step 2: Make the Change

Do whatever work is planned. Commit nothing yet.

### Step 3: Run Post-Change Snapshot

```
<project> loss --json > /tmp/loss-after.json
```

### Step 4: Compute Deltas

For each metric, compute: `delta = after - before`

For losses (lower is better):
- **Negative delta = improvement** (loss went down)
- **Positive delta = regression** (loss went up)
- **Zero = no change**

For missions (higher is better):
- **Positive delta = improvement** (mission score went up)
- **Negative delta = regression** (mission score went down)

### Step 5: Verdict

Apply these rules:

| Situation | Verdict |
|-----------|---------|
| Loss down, missions same or up | **IMPROVEMENT** — commit it |
| Loss same, missions up | **IMPROVEMENT** — commit it |
| Loss up, missions up more (weighted) | **TRADEOFF** — explain and ask user |
| Loss up, missions same or down | **REGRESSION** — revert or fix |
| Loss same, missions same | **LATERAL** — was the change necessary? |

Present the verdict with specific numbers:
```
VERDICT: IMPROVEMENT
  Loss:    6.21 → 4.83 (-1.38)
  Mission: 0.57 → 0.62 (+0.05)

  Biggest wins:
    L4 knowledge_utilization: 1.875 → 0.3 (pruned 900 dead rules)
    M5 tax_readiness: 75% → 82% (mapped 7 more accounts)

  Regressions:
    L6 codebase_complexity: 29.4 → 30.1 (added loss.py, net +275 lines)
```

### Step 6: Record

If the change is committed, append to the project's loss history:
```
# logs/loss-history.csv
date,loss_composite,mission_composite,change_description
2026-04-01,6.21,0.57,baseline
2026-04-01,4.83,0.62,pruned dead rules + mapped tax accounts
```

## Rules

- **Never skip the pre-check.** If you don't know where you started, you can't measure improvement.
- **Compare weighted composites, not individual metrics.** A change that improves L4 by 1.0 but worsens L6 by 0.1 is a net win if L4 has higher weight.
- **Regressions require justification.** If loss goes up, the agent must explain why and get user approval before committing.
- **Track over time.** The loss history CSV is how you see trends. One-off checks are useful; trajectories are powerful.
