---
type: entity
entity_type: organization
org_type: vendor
title: "Elastic"
created: 2026-05-07
updated: 2026-09-28
tags:
  - organizations
  - elastic
  - observability
  - detection-engineering
  - opentelemetry
status: seed
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
homepage: "https://www.elastic.co"
related:
  - "[[unprompted-conference-march-2026]]"
  - "[[agent-observability]]"
  - "[[mika-ayenson]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[openai-daybreak]]"
sources:
  - "https://www.elastic.co"
  - "https://openai.com/daybreak/partners-new/"
verified: 2026-09-28
verified_against:
  - ".raw/articles/openai-daybreak-defense-network-2026-09-28.md"
verified_findings: 0
verified_note: "Diff-scoped read of the Defense Network line and the new scope axes."
---

# Elastic

**Sources:** [Elastic (homepage)](https://www.elastic.co) · [Elastic Security](https://www.elastic.co/security)

Search, observability, and security analytics vendor (NYSE: ESTC; maker of Elasticsearch, Kibana, and the Elastic Stack). In the context of this wiki, Elastic appears as a contributor to the **GenAI endpoint observability** thread at [[unprompted-conference-march-2026|Unprompted March 2026]]: [[mika-ayenson|Mika Ayenson]] (Threat Research & Detection Engineer) presented [[genai-endpoint-observability-talk|GenAI Endpoint Observability]] ("Can You See What Your AI Saw?", Day 1 / Stage 2 / 11:35), surveying telemetry gaps for AI-spawned processes and arguing for extending **OpenTelemetry semantic conventions** to GenAI tool activity on endpoints.

Elastic is also one of the twenty partners listed by the [[openai-daybreak|OpenAI Daybreak]] Defense Network, the partner programme through which OpenAI brings its cyber capabilities into partner products and services.[^daybreak-network]

## See also

- [[unprompted-conference-march-2026|Unprompted March 2026]] — talk venue
- [[agent-observability|Agent Observability]] — practice page covering the telemetry surface

[^daybreak-network]: [OpenAI — Daybreak Defense Network](https://openai.com/daybreak/partners-new/), undated, fetched 2026-09-28: the twenty listed partners and the programme's aim of bringing cyber capabilities into partner products and services. Local copy: `.raw/articles/openai-daybreak-defense-network-2026-09-28.md`.
