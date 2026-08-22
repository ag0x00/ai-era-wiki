---
type: talk
title: "Exploring the AI Automation Boundary for Threat Hunting"
address: c-000146
created: 2026-05-25
updated: 2026-08-21
tags:
  - papers
  - talks
  - agentic-soc
  - threat-hunting
  - ai-in-sec-defense
status: stub-summary
scope_axis: [ai-in-sec-defense]
year: 2026
authors: ["Arthi Nagarajan"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 4, 2026 (Day 2 / Stage 1 / 15:05)"
key_claim: "Threat hunting is bottlenecked by analysts' ability to navigate overwhelming telemetry, not by a lack of it; an orchestrator-subagent system can automate hypothesis-driven query generation, iterative refinement, and narrowing toward pivotal evidence, provided the team defines an explicit 'automation boundary' between where AI accelerates defensive work and where it introduces new risk."
methodology: "Practitioner talk on Datadog's internal threat detection work. The published abstract is the only primary source captured here; slides and transcript are not yet ingested."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[datadog|Datadog]]"
  - "[[oversight-layer|Oversight Layer]]"
  - "[[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
---

# Exploring the AI Automation Boundary for Threat Hunting

A practitioner talk by Arthi Nagarajan ([[datadog|Datadog]]) at Unprompted (March 2026), on applying AI to threat hunting across large, schema-diverse telemetry. Abstract-only; slides and video are not yet captured.

## The Argument

The talk's framing is that modern threat hunting is limited not by a lack of telemetry but by humans' ability to navigate overwhelming volumes of it. Datadog automated three parts of the hunting workflow: hypothesis-driven query generation, iterative refinement, and narrowing toward pivotal evidence. The system evolved from a single agent into an **orchestrator-subagent** architecture. The talk's central contribution is the idea of an **automation boundary**: an explicit account of where AI accelerates defensive work, where it creates new risk (trust, hallucinations, evaluation under real-world constraints), and which design decisions establish trust with human threat hunters.

## Placement

This is direct evidence for the threat-hunting capability in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis. The single-agent → orchestrator-subagent migration matches the supervisor-worker pattern named in the [[oversight-layer|Oversight Layer]] and in the Salesforce [[beyond-the-chatbot-talk|Beyond the Chatbot]] Agentic SOC talk. The "automation boundary" is the operational form of the thesis's action-authority question: which steps an agent runs autonomously versus where a human stays in the loop.

The [[agentic-soc-ra-threat-hunting|Agentic SOC Threat Hunting Surface]] is where the boundary becomes a design artifact in this wiki. That page treats the automation boundary as a bound independent of the [[agentic-soc-cmm|Agentic SOC CMM]]'s maturity gate: a SOC can be mature enough to earn high hunting autonomy and still be right to hold it lower, because past the boundary added autonomy produces plausible-but-wrong hypotheses instead of findings. It reads the boundary's position as a per-team judgment from this account rather than a calibrated threshold, which is the open calibration question the talk leaves for the function.
