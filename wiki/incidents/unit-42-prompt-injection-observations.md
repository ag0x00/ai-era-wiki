---
type: incident
title: "Unit 42 In-the-Wild Prompt Injection Observations"
created: 2026-04-30
updated: 2026-04-30
tags:
  - incidents
  - prompt-injection
  - telemetry
  - in-the-wild
status: developing
scope_axis:
  - sec-of-ai
incident_class: prompt-injection
attack_with_or_on_ai: "on AI"
date_observed: 2026-03-03
date_disclosed: 2026-03-03
target: "Production LLM/agent deployments at Palo Alto Networks customers"
threat_actor: "multiple, observed in production telemetry"
impact: "First confirmed in-the-wild **indirect** prompt injection observations from production telemetry; documented 22 distinct techniques including AI-based ad-review evasion."
related:
  - "[[mitre-atlas]]"
  - "[[owasp-llm-top-10]]"
  - "[[palo-alto-networks]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[ai-agents-are-here-so-are-the-threats-unit42]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# Unit 42 In-the-Wild Prompt Injection Observations

## Summary

On March 3, 2026, Palo Alto Networks Unit 42 published the first publicly confirmed **in-the-wild indirect prompt injection observations from production telemetry**. The report documents **22 distinct techniques**, including a notable AI-based ad-review evasion technique — using prompt injection to manipulate LLM-driven content moderation pipelines.

## Significance

Until this report, indirect prompt injection had been demonstrated in research settings and widely discussed but not corroborated by production telemetry across multiple enterprises. Unit 42's data turns the attack class from "predicted" to "observed" — closing the framework-vs-reality loop and giving defenders a concrete technique inventory to plan for.

## Defensive Lessons

- The 22 techniques should map directly into [[mitre-atlas|MITRE ATLAS]] and into the [[owasp-llm-top-10|OWASP LLM Top 10]] — concrete evidence for the prompt-injection slot.
- **Detection > prevention** is the practical posture, since deterministic prevention of prompt injection remains unsolved. Behavioural / anomaly-detection (agent behavioral monitoring — see [[agent-observability|Agent Observability]]) is the corresponding control.
- This is the strongest existing evidence base for argument that platform-level enforcement (input filtering, egress control, capability-based authorization) must precede prompt-level guardrails — central thesis of [[ai-security-standards-in-q1-2026|AI Security Standards in Q1 2026]].

## Sources
- See frontmatter.
