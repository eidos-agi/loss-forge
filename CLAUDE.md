# loss-forge

Measurable improvement for AI-operated systems. Loss functions + mission scores that let agents know whether they're helping or hurting.

## What This Is

A knowledge forge (skills + templates) that teaches AI agents to:
1. Define what success looks like (mission scores, 0.0-1.0)
2. Define what failure looks like (loss functions, 0.0 = perfect)
3. Measure baselines before making changes
4. Evaluate every change against the numbers
5. Track improvement trajectories over time

## Skills

| Skill | Purpose |
|-------|---------|
| `/loss-init` | Define loss functions and mission scores for a project |
| `/loss-check` | Run before/after comparison to evaluate a change |
| `/loss-audit` | Validate loss function quality, coverage, and gameability |

## Templates

- `loss-module.py.tmpl` — Parameterized loss module scaffold
- `LOSS-BASELINE.md.tmpl` — Baseline snapshot document

## Key Concepts

**Missions** — What the project exists to do. Scored 0.0 to 1.0. "Bills paid on time" = 1.0.

**Losses** — What's broken or decaying. Scored 0.0+, lower is better. "26 unresolved audit findings" = 26.0.

**Agent efficiency** — The AI agent is part of the system. Its token cost, speed, and context requirements are first-class metrics (GUARD-003).

**The rule:** Every session starts and ends with the loss command. If loss goes up without a mission improvement, the change is wrong.

## Guardrails

1. No software — skills and templates only
2. Every metric must be measurable from the project itself (no external services, no human judgment)
3. Loss functions measure the agent, not just the project

## Related Forges

- **improve-forge** — Scores projects on 9 dimensions. Loss-forge provides the ongoing measurement that improve-forge sessions optimize against.
- **brutal-forge** — Finds problems. Loss-forge quantifies how bad they are.
- **test-forge** — Tests functionality. Loss-forge measures outcomes.
