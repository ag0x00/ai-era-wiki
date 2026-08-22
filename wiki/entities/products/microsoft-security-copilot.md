---
type: entity
entity_type: product
title: "Microsoft Security Copilot"
address: c-000024
created: 2026-05-13
updated: 2026-05-13
tags:
  - products
  - microsoft
  - security-copilot
  - ai-in-sec-defense
  - soc-automation
  - copilot
status: seed
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
vendor: "Microsoft"
ga_date: "2024-04-01"
homepage: "https://www.microsoft.com/en-us/security/business/ai-machine-learning/microsoft-security-copilot"
related:
  - "[[microsoft]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-secure-agentic-ai-end-to-end]]"
  - "[[vasu-jakkal]]"
  - "[[agentic-soc-state-of-the-field]]"
  - "[[crowdstrike]]"
sources:
  - "https://www.microsoft.com/en-us/security/business/ai-machine-learning/microsoft-security-copilot"
  - "[[microsoft-secure-agentic-ai-end-to-end]]"
---

# Microsoft Security Copilot

**Sources:** [Homepage](https://www.microsoft.com/en-us/security/business/ai-machine-learning/microsoft-security-copilot) · [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI End-to-End (Vasu Jakkal, Mar 2026)]]

> [!gap] Stub page — created 2026-05-13
> Page seeded as part of the wiki scope-expansion punch-list (see [[scope-expansion-2026-05|Scope Expansion Punch-List (2026-05)]]). Substantive product description, agent-by-agent capability coverage, customer case studies, and crosswalk to the [[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) and [[agentic-soc-state-of-the-field|Agentic SOC]] thesis are deferred to the next ingest pass.

## Overview

Microsoft Security Copilot is the AI-augmented security operations product within the Microsoft Security portfolio, anchoring the **"Defend with agents and experts"** pillar of Microsoft's three-pillar agentic AI security framing (see [[microsoft-secure-agentic-ai-end-to-end|Microsoft Secure Agentic AI End-to-End]]). It is distributed in M365 E5/E7 and surfaces a fleet of role-specialized defender agents plus a Security Store of partner agents.

## Component Agents (as of March 2026)

Five Microsoft-built role-specialized agents are publicly named in Microsoft's pre-RSAC 2026 product roadmap:

- **Security Analyst Agent** — incident summarization, investigation narration, recommendation synthesis.
- **Alert Triage Agent** — first-pass triage of SIEM/XDR alerts with disposition recommendation.
- **Conditional Access Optimization Agent** — policy-drift detection and recommendation for [[microsoft-entra-agent-id|Entra]] Conditional Access.
- **Data Security Posture Agent** — credential scanning, sensitive-data exposure detection across the Microsoft data plane.
- **Data Security Triage Agent** — disposition recommendation for data-loss events.

Plus **15 partner agents** available through the Security Store (as of March 2026 — specific partner list to be captured on next ingest).

## CMM / RA Mapping

> [!gap] Pending crosswalk
> The Security Copilot agents map most directly to [[agentic-ai-security-cmm-2026|CMM]] domain D7 (Observability & Detection) at L4–L5 — the agent-aware SIEM playbook component. Detailed per-agent mapping deferred.

## Open Questions

- How does Security Copilot's agent governance plane compare to [[microsoft-entra-agent-id|Microsoft Entra Agent ID / Agent 365]] as the canonical control plane for defender agents? Are they integrated, or is Security Copilot a separate identity surface?
- Public benchmarks: are there independent (non-Microsoft) evaluations of Security Copilot agent quality, false-positive rates, or analyst-time savings?
- Comparison vs. [[crowdstrike|CrowdStrike Falcon AIDR]] and Google Sec-PaLM (the latter is another known gap in [[scope-expansion-2026-05|the punch-list]]).

## Notes

This page was created from existing wiki references to Security Copilot in [[microsoft-secure-agentic-ai-end-to-end|the Microsoft Secure Agentic AI paper]] and [[agentic-soc-state-of-the-field|the Agentic SOC thesis]]. It is a routing address rather than a full product page; promote to `developing` after the first sourced ingest specifically about Security Copilot lands.
