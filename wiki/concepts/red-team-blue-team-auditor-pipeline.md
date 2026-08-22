---
type: concept
title: "Red Team / Blue Team / Auditor Pipeline"
address: c-000113
created: 2026-05-24
updated: 2026-08-21
tags:
  - concepts
  - red-team
  - adversarial-pipeline
  - agentshield
status: seed
scope_axis: [sec-of-ai, ai-in-sec-offense, sec-against-ai]
complexity: intermediate
domain: ai-security-tooling
no_public_url: "Wiki-internal analytical construct named from the AgentShield announcement; no external canonical source."
aliases:
  - Red Team / Blue Team / Auditor Pipeline
related:
  - "[[agentshield|AgentShield]]"
  - "[[agentshield-announcement|AgentShield Announcement]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
sources:
  - "[[agentshield-announcement|AgentShield Announcement]]"
---

# Red Team / Blue Team / Auditor Pipeline

## Definition

The Red Team / Blue Team / Auditor pipeline is a three-agent adversarial review pattern. A **Red Team** agent attacks the target (proposing exploits or bypasses), a **Blue Team** agent defends (proposing mitigations and rebutting the attack), and an **Auditor** agent adjudicates the exchange and produces the graded finding. Splitting the roles forces an explicit attack-and-rebuttal record rather than a single model's unchecked verdict.

When the target is an AI system, the Red Team agent draws its attack scenarios from a threat taxonomy rather than improvising: [[mitre-atlas|MITRE ATLAS]] supplies the catalog of adversarial techniques against AI that defines what the Red Team role attempts and what the Auditor grades coverage against.

## Occurrences

[[agentshield|AgentShield]] runs this pattern as its optional `--opus` mode: three Claude Opus 4.6 agents over a scanned Claude Code config tree, layered on top of the rule-based scan (see [[agentshield-announcement|the announcement]]). It is one instance of the broader [[adversarial-reflexion|Adversarial Reflexion]] discipline, where a defender model is asked to prove or disprove its own findings before they are reported.

> [!gap] Seed page
> Created to resolve dead links from the AgentShield pages. A fuller treatment would compare it to single-agent self-critique and to LLM-as-judge ensembles.
