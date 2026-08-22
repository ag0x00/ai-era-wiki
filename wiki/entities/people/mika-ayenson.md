---
type: entity
entity_type: person
title: "Mika Ayenson"
address: c-000175
created: 2026-06-02
updated: 2026-06-02
tags:
  - entities
  - people
  - detection-engineering
  - observability
  - elastic
status: seed
role: "Team Lead, Threat Research & Detection Engineering"
affiliation: "[[elastic|Elastic]]"
related:
  - "[[elastic|Elastic]]"
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability for Detection Engineers]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[opentelemetry-gen-ai|OpenTelemetry gen_ai.* Semantic Conventions]]"
sources:
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability (Unprompted March 2026)]]"
  - "[[.raw/talks/2026-03-03_Mika-Ayenson_GenAI-Endpoint-Observability_transcript.md]]"
---

# Mika Ayenson

Team lead for Threat Research and Detection Engineering at [[elastic|Elastic]]. At [[unprompted-conference-march-2026|Unprompted March 2026]] he presented [[genai-endpoint-observability-talk|"Can You See What Your AI Saw?"]], the case that endpoint detection engineers cannot reconstruct what a GenAI agent did because a developer and an agent produce near-identical EDR telemetry. His proposed fix routes agent-hook signals into [[opentelemetry-gen-ai|OpenTelemetry GenAI semantic conventions]] and on to the SIEM; Elastic has merged contributions into that conventions workstream.
