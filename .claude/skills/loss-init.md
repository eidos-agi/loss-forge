# loss-init — Define Loss Functions for a Project

Define measurable loss functions and mission scores for any project. The output is a `loss.py` module (or equivalent) that computes all metrics in one call, plus a CLI command that prints the instrument panel.

## Trigger

User says `/loss-init` or asks to add loss functions to a project.

## Instructions

### Step 1: Identify End Missions

Ask: **"What does this project exist to do?"** Not what features it has — what outcomes it must deliver.

Examples:
- Finance system: bills on time, transactions categorized, books balanced, tax ready
- Trading system: signals generated, trades executed, risk limits maintained, returns positive
- Content system: articles published, quality maintained, distribution working

Each mission becomes an **M-score** (0.0 to 1.0). Define 5-8 missions. Weight them by importance.

For each mission:
1. Write a one-line purpose
2. Define what 0.0 means (nothing accomplished)
3. Define what 1.0 means (fully complete)
4. Write the SQL query, file check, or API call that computes the score
5. Assign a weight (1-10, higher = more important)

### Step 2: Identify System Health Losses

Ask: **"What can go wrong?"** Not hypotheticals — what actually breaks or decays.

Standard loss categories (adapt per project):

| Loss | Measures | Universal? |
|------|----------|-----------|
| Data accuracy | Incorrect/inconsistent data | Most projects |
| Data freshness | Stale inputs | Data-dependent projects |
| Knowledge utilization | Dead rules, unread docs, unused config | Projects with accumulated knowledge |
| Audit health | Unresolved warnings, open findings | Projects with quality checks |
| Codebase complexity | File sizes, line counts, duplication | All code projects |
| Agent efficiency | Tokens, time, context needed per operation | All agent-operated projects |
| Feedback loop speed | Time to verify a change worked | All projects |

Each loss is an **L-score** (0.0 = perfect, higher = worse). Define 5-10 losses. Weight them.

For each loss:
1. Write what it measures and why it matters
2. Define the formula (inputs → number)
3. Write the query/computation
4. Define target (usually 0) and acceptable threshold
5. Assign a weight (1-10)

### Step 3: Write the Loss Module

Create a file in the project (e.g., `loss.py`, `loss.ts`, `loss.sh`) that:

```python
def compute_all(session_or_context) -> dict:
    """Returns missions, losses, and composites."""
    return {
        "loss_composite": float,      # weighted average of all L-scores (lower = better)
        "mission_composite": float,   # weighted average of all M-scores (higher = better, max 1.0)
        "losses": { "L1_name": {"value": float, "components": {...}} },
        "missions": { "M1_name": {"score": float, "components": {...}} },
        "snapshot_date": str,
    }
```

Rules for the module:
- **Pure computation.** No side effects. No writes. Read-only queries.
- **Self-contained.** One file, no external dependencies beyond the project's own.
- **Fast.** Must complete in under 30 seconds. If a metric is slow, approximate it.
- **Components exposed.** Don't just return the composite — show what drove it.
- **Deterministic.** Same state → same numbers. No randomness, no LLM calls.

### Step 4: Wire the CLI Command

Add a command to the project's CLI:

```
<project> loss          # human-readable instrument panel
<project> loss --json   # machine-readable for tracking
```

The human-readable output should show:
1. Composite loss and mission scores (the headline numbers)
2. Mission progress bars (visual, 0-100%)
3. Loss details with components (what's driving each number)

### Step 5: Establish Baseline

Run the loss command. Record the output. This is the **baseline snapshot**. Every future change is measured against this.

Write the baseline to `LOSS-BASELINE.md` in the project root:

```markdown
# Loss Baseline — YYYY-MM-DD
Loss: X.XX | Mission: X.XX

## Missions
M1: XX% — description
M2: XX% — description
...

## Losses
L1: X.X — description
L2: X.X — description
...
```

### Step 6: Add the Rule

Add to the project's CLAUDE.md or equivalent:

> **Every session starts with `<project> loss` and ends with `<project> loss`.
> If loss goes up without a corresponding mission improvement, the change is wrong.**

## Rules

- **Missions first, losses second.** Missions answer "did we accomplish the goal?" Losses answer "is anything broken?" Both matter, but a project with high mission scores and some losses is better than zero losses and zero mission completion.
- **5-8 missions, 5-10 losses.** Fewer = not enough signal. More = noise.
- **Weight honestly.** The most important mission gets weight 10. Nice-to-haves get weight 1-3. Don't give everything weight 5 — that's the same as no weights.
- **Include agent efficiency.** The agent operating the system is part of the system. Its token cost, speed, and context requirements are first-class metrics.
- **Baselines before changes.** Never start improving without knowing where you are.
- **No vanity metrics.** If a metric can go to 100% by deleting all the data, it's measuring the wrong thing.
