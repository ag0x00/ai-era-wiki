---
type: entity
entity_type: organization
org_type: vendor
parent_org: "[[google]]"
title: "Wiz"
created: 2026-04-30
updated: 2026-07-26
tags:
  - entities
  - organizations
  - cnapp
  - ai-spm
  - cloud-security
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-offense
role: "Cloud-native application protection platform (CNAPP) vendor; first-CNAPP-to-ship-AI-SPM (2024); now part of Google Cloud Security; Red Agent Opus-powered continuous pentester (2026)"
related:
  - "[[wiz-ai-spm]]"
  - "[[ai-spm]]"
  - "[[google]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[claude-partners-opus-cybersecurity]]"
  - "[[mythos]]"
  - "[[taming-shai-hulud-with-ai-talk]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - "https://www.wiz.io/"
  - "https://www.wiz.io/blog/red-agent-claude-opus"
  - "[[.raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md]]"
---

# Wiz

Wiz is a cloud-native application protection platform (CNAPP) vendor founded in 2020. The company shipped what it claims as the first AI-SPM module integrated into a CNAPP in 2024, and expanded materially through 2025. Following Google's \$32B acquisition (closed 2025), Wiz operates as part of Google Cloud Security.

## Core platform

Wiz CNAPP combines: CSPM (cloud security posture), CWPP (cloud workload protection), CIEM (cloud entitlements), DSPM (data security posture), code/IaC scanning, and now AI-SPM. The differentiating architectural primitive is the **Wiz Security Graph** — a unified data model across all CNAPP modules enabling attack-path correlation across cloud, identity, data, and code domains.

## AI-relevant offerings

| Product | Role |
|---|---|
| **[[wiz-ai-spm\|Wiz AI-SPM]]** | AI Security Posture Management — model inventory, configuration risk, attack-path correlation to AI assets, AI-BOM, runtime monitoring of agent behavior |

## Red Agent — Opus-powered offensive testing (2026)

[Wiz Red Agent](https://www.wiz.io/blog/red-agent-claude-opus) is an AI-powered attacker built on [[mythos|Claude Opus]] that reasons like a human pentester across production web applications and APIs — analyzing application logic, chaining steps, and adapting to live server responses to surface the logic-driven flaws traditional scanners miss. Running continuously across **150,000+ production assets per week**, it surfaces thousands of high- and critical-severity findings, each validated with proof of exploitability and business context from the Wiz Security Graph — reported at **zero false positives** in customer production (see [[claude-partners-opus-cybersecurity|the Opus partner ecosystem]]). VP AI & Threat Research Alon Schindel: *"Security teams are no longer limited by a lack of data, but by the ability to act on it."*

## Green Agent and AI Threat Defense (2026)

Google Cloud's [[codemender|CodeMender]] preview (2026-07-21) is the first published account of how Wiz composes with Google Cloud security products after the acquisition ([[google-cloud-codemender-preview|source summary]]). Within **AI Threat Defense**, Wiz "orchestrates agentic application security": it calls CodeMender to scan code, enriches the findings in the Wiz Security Graph with deployment context, and triggers Red Agent for AI pentesting. A **Wiz Green Agent** then directs CodeMender to generate and test patches carrying that application context.

Red Agent and Green Agent form a paired attack-and-repair loop over one asset graph. The graph is what distinguishes this from repository-only scanning: production reachability, not just source reachability, feeds prioritization. Google states CodeMender scanning under Wiz orchestration is "coming soon," so the integration is announced rather than shipped.

## Notable 2025–2026 events

- **Wizdom 2025** product launch event introduced runtime AI agent monitoring and the dedicated AI Security Dashboard
- **OpenAI Platform connector** added to AI-SPM
- **NVIDIA Enterprise AI Factory integration**
- **Google Cloud acquisition** (\$32B, 2025) — product roadmap integration with Google Cloud Security pending

## At Unprompted (March 2026)

Rami McCarthy presented [[taming-shai-hulud-with-ai-talk|Zeal of the Convert: Taming Shai-Hulud with AI]] at the [[unprompted-conference-march-2026|Unprompted Conference (March 2026)]] — a practitioner post-mortem on responding to the 2025 Shai-Hulud supply-chain campaigns by moving from single-purpose scrapers to multi-agent triage engines that parallelize victimology and automate secret-impact analysis.

## Wiki references

- [[wiz-ai-spm|Wiz AI-SPM]] — primary product page
- [[ai-spm|AI Security Posture Management]] practice page
- [[agentic-ai-security-reference-architecture|RA]] Data plane (supply-chain scanning) and Observability plane (AI-SPM)

## Position vs peers

Primary competitors in the AI-SPM segment: [[palo-alto-prisma-airs|Palo Alto Prisma AIRS]] (with separate Prisma Cloud AI-SPM from Dig Security), Orca Security AI-SPM, Reco. Wiz's strongest differentiation is graph-based attack-path correlation across the broader CNAPP, not point AI capability depth.
