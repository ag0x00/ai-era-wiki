---
type: entity
entity_type: organization
org_type: vendor
title: "Datadog"
created: 2026-05-04
updated: 2026-06-21
tags:
  - entities
  - organizations
  - observability
  - apm
  - ai-monitoring
status: seed
homepage: "https://www.datadoghq.com"
related:
  - "[[agent-observability]]"
  - "[[opentelemetry-gen-ai]]"
  - "[[ai-automation-boundary-threat-hunting-talk]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - "https://www.datadoghq.com/product/llm-observability"
---

# Datadog

**Sources:** [Datadog (homepage)](https://www.datadoghq.com) · [LLM Observability](https://www.datadoghq.com/product/llm-observability/)

> [!gap] Stub
> Observability / APM vendor with first-mover position on LLM and agent monitoring. **Datadog AI Monitoring / LLM Observability** cited at CMM D9 (Operations & Human Factors) for latency / cost spans on agent workloads, alongside New Relic AI Monitoring and Sentry AI Tracing. Built on Datadog's existing distributed-tracing pipeline; integrates OpenTelemetry [[opentelemetry-gen-ai|`gen_ai.*` semantic conventions]].
>
> Pending content: company overview, full LLM-observability product description, integration with agent-aware SIEM playbooks at D7, public case studies.

## At Unprompted (March 2026)

Datadog presented two talks at the [[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]. In [[ai-automation-boundary-threat-hunting-talk|Exploring the AI Automation Boundary for Threat Hunting]], Arthi Nagarajan describes the migration from a single threat-hunting agent to an orchestrator-subagent system and frames the "automation boundary" — the line between where AI accelerates defensive work and where it introduces new risk. A second talk, by Olivia Gallucci, covers AI-assisted macOS vulnerability research.
