---
type: talk
title: "Rethinking Security Agent Evaluation"
address: c-000149
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - talks
  - agentic-soc
  - evaluation
  - benchmarks
  - ai-in-sec-defense
status: stub-summary
scope_axis: [ai-in-sec-defense]
year: 2026
authors: ["Mudita Khurana"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 4, 2026 (Day 1 / Stage 1 / 12:10)"
key_claim: "Outcome-only benchmarks misjudge security agents: they record whether an agent produced a correct answer, not how it got there or whether the behavior stays stable in deployment. A capability-centric evaluation that observes how an agent plans, reasons, uses tools, and carries context across the find→confirm→patch→validate loop better predicts real-world readiness."
methodology: "Practitioner talk proposing an evaluation framework, based on Airbnb security-engineering experience. The published abstract is the only primary source captured here; slides and transcript are not yet ingested."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[evaluating-ai-soc-agents|Evaluating AI SOC Agents]]"
  - "[[defensebench|DefenseBench]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[airbnb|Airbnb]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
---

# Rethinking How We Evaluate Security Agents for Real-World Use

A practitioner talk by Mudita Khurana ([[airbnb|Airbnb]]) at Unprompted (March 2026) arguing that security-agent evaluation is rooted in too-narrow, outcome-only benchmarks. Abstract-only; slides and video are not yet captured.

## The Argument

Outcome-only benchmarks tell whether an agent produced a correct answer, but not how it arrived there or whether the behavior will stay stable once deployed. In practice, security is a connected, end-to-end workflow: a **find → confirm exploit → patch → validate** loop. Agents that score well on task-specific benchmarks often fail in multi-stage settings because of contextual loss and brittle transitions between steps. The talk proposes a **capability-centric** framework that emphasizes observability into how agents plan, reason, use tools, and carry context across the security lifecycle, so teams can judge real-world readiness rather than single-answer accuracy.

## Placement

This is the practitioner counterpart to the buyer-side and benchmark-side evaluation work the wiki already tracks. It aligns with [[evaluating-ai-soc-agents|Gartner's evaluation framework]] (outcomes and reasoning quality over "alerts processed") and explains *why* a single-task scoreboard like [[defensebench|DefenseBench]]'s BOTSv3 is necessary but not sufficient: a multi-stage loop needs multi-stage evaluation. Together these three — Gartner's criteria, DefenseBench's scores, and Khurana's capability-centric, observability-first method — frame the evaluation gap in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis.