---
type: entity
entity_type: person
title: "Matt Rittinghouse"
created: 2026-05-03
updated: 2026-05-03
tags:
  - people
  - salesforce
  - observability
  - behavioral-anomaly-detection
status: seed
affiliation: "Salesforce — Cybersecurity Operations Center"
role: "Security practitioner, Cybersecurity Operations Center"
related:
  - "[[millie-rittinghouse]]"
  - "[[salesforce]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[agentforce]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
---

# Matt Rittinghouse

Matt Rittinghouse is a security practitioner at Salesforce's Cybersecurity Operations Center (CSOC). He works on detecting and responding to threats across the [[agentforce|Agentforce]] agentic AI platform.

At [[unprompted-conference-march-2026|Unprompted March 2026]], he co-presented [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts: Hunting Abuse in a User-Defined Agent Ecosystem"]] alongside [[millie-rittinghouse|Millie Rittinghouse]]. His portion of the talk focused on the technical architecture of the behavioral anomaly detection model: the three-level ensemble approach (user / agent / org context), feature design and selection, incremental historical profiling, and the roadmap toward hot-path inline scoring and auto-containment.

## Contributions to the wiki

- **Three-level ensemble anomaly detection model** — first production description of layering user / agent / organization behavioral contexts for agentic AI anomaly detection.
- **Feature selection methodology** — PCA-inspired approach to culling feature sets; "measure contribution first, then cull to minimum."
- **Deviation-based scoring as confidence proxy** — per-axis statistical deviation scores as a confidence-interval analog for alert prioritization.

> [!gap] Name disambiguation
> The conference agenda (as captured) lists this speaker pair as "Matt Rittinghouse + Millie Huang." This page follows the transcript's file metadata which uses "Millie and Matt Rittinghouse." Verify when external confirmation is available.
