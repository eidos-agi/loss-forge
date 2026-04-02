# loss-forge

Measurable improvement for AI-operated systems.

An AI agent operating a system cannot improve what it cannot measure. loss-forge teaches agents to define loss functions for any project — operational loss that goes beyond test coverage to measure: are the end missions being accomplished? Is the agent getting more efficient? Is complexity growing or shrinking?

## Skills

| Skill | What it does |
|-------|-------------|
| `/loss-init` | Define 5-8 mission scores + 5-10 loss functions for a project. Writes the loss module, CLI command, and baseline snapshot. |
| `/loss-check` | Run before/after a change. Computes deltas. Returns verdict: IMPROVEMENT, REGRESSION, TRADEOFF, or LATERAL. |
| `/loss-audit` | Audit existing loss functions for coverage gaps, gameability, and missing agent metrics. |

## The Two Numbers

Every project gets two composite scores:

- **Loss** (lower is better) — weighted average of everything that's broken: stale data, dead rules, unresolved findings, codebase complexity, agent token cost
- **Mission** (higher is better, max 1.0) — weighted average of end-mission completion: bills on time, transactions categorized, books balanced, net worth tracked

## Example Output

```
============================================================
  REEVES-FINANCE INSTRUMENT PANEL  |  2026-04-01
============================================================
  LOSS: 6.21  (lower is better)
  MISSION: 0.57  (higher is better, max 1.0)
============================================================

  ████████████████████ 100%  M1_bills_on_time  (w:10)
  ███░░░░░░░░░░░░░░░░░  15%  M2_cash_position  (w:8)
  ████████████████████ 100%  M3_categorization  (w:6)
  ██████████░░░░░░░░░░  50%  M4_reconciliation  (w:4)
  ███████████████░░░░░  75%  M5_tax_readiness   (w:5)
  ░░░░░░░░░░░░░░░░░░░░   0%  M6_net_worth       (w:4)
  ░░░░░░░░░░░░░░░░░░░░   0%  M7_cash_flow       (w:3)
```

## Usage

```bash
# Clone the forge
git clone https://github.com/eidos-agi/loss-forge ~/repos-eidos-agi/loss-forge

# In any project, tell Claude Code:
# "Use loss-forge to add loss functions to this project"
# or invoke directly:
# /loss-init
```

## Philosophy

> "If loss goes up and missions don't improve, the change is wrong. That's the only rule."

## License

MIT
