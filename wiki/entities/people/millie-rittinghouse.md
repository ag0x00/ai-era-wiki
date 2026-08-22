---
type: entity
entity_type: person
title: "Millie Rittinghouse"
created: 2026-05-03
updated: 2026-05-03
tags:
  - people
  - salesforce
  - observability
  - behavioral-anomaly-detection
status: seed
affiliation: "Salesforce — Cybersecurity Operations Center"
role: "Security Data Scientist, Cybersecurity Operations Center"
related:
  - "[[matt-rittinghouse]]"
  - "[[salesforce]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[agentforce]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
---

# Millie Rittinghouse

Millie Rittinghouse is a Security Data Scientist at Salesforce's Cybersecurity Operations Center (CSOC). She works on behavioral threat detection for the [[agentforce|Agentforce]] agentic AI platform. Her title at the time of the talk was described as "security database scientist" (transcript), which maps most closely to Security Data Scientist.

At [[unprompted-conference-march-2026|Unprompted March 2026]], she co-presented [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts: Hunting Abuse in a User-Defined Agent Ecosystem"]] alongside [[matt-rittinghouse|Matt Rittinghouse]]. Her portions of the talk covered the threat landscape (platform-target attacks vs. abuse of legitimate agency), the limitations of content-moderation-only defenses (reasoning vs. execution blindness; the blocking dilemma; post-generation blindness), and the roadmap toward real-time auto-containment.

## Contributions to the wiki

- **Execution-layer defense framing** — articulated why defenses must operate at the execution layer (what the agent *does*), not the reasoning layer (what the agent *says*), in a privacy-preserving multi-tenant context.
- **Abuse of legitimate agency** — named and defined this threat class as distinct from platform-target attacks: valid, authorized capabilities used maliciously in context.
- **Auto-containment roadmap** — described the architectural path from batch detection to hot-path inline scoring to automated session kill / token revoke / bot-level lockdown.

> [!gap] Name disambiguation
> The conference agenda (as captured) lists this speaker pair as "Matt Rittinghouse + Millie Huang." This page follows the transcript's file metadata which uses "Millie and Matt Rittinghouse." If Millie's last name is Huang, this page should be moved to `millie-huang.md`. Verify when external confirmation is available.
