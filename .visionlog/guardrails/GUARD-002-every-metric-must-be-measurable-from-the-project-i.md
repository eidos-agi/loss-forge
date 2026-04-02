---
id: "GUARD-002"
type: "guardrail"
title: "Every metric must be measurable from the project itself"
status: "active"
date: "2026-04-01"
---

Loss functions must compute from data already in the project — database queries, file counts, git history, test results. Never require external services, paid APIs, or human judgment to compute. If a human has to look at something to score it, it's not a loss function — it's an opinion. The instrument panel must run unattended.
