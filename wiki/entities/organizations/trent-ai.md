---
type: entity
title: "Trent AI"
created: 2026-05-03
updated: 2026-08-21
tags:
  - entities
  - organization
  - vendor
  - agentic-ai-security
  - seed-funded
status: seed
entity_type: organization
org_type: vendor
role: "Agentic AI security platform — layered, lifecycle-spanning protection built using coordinated security agents"
related:
  - "[[lumia-security]]"
  - "[[onyx-security]]"
  - "[[sentinels-and-operatives]]"
  - "[[guardian-agent]]"
homepage: "https://trent.ai"
sources:
  - "https://www.businesswire.com/news/home/20260407796810/en/Trent-AI-Raises-$13M-to-Secure-the-Agentic-Age"
  - "https://www.securityweek.com/trent-ai-emerges-from-stealth-with-13-million-in-funding/"
  - "https://www.eu-startups.com/2026/04/with-74-of-businesses-planning-agentic-ai-deployment-trent-ai-secures-e11-million-in-seed-funding/"
  - "https://tech.eu/2026/04/07/openai-and-spotify-leaders-back-london-based-ai-security-agent-startup-in-13m-round/"
---

# Trent AI

**Sources:** [Trent AI (homepage)](https://trent.ai) · [Funding announcement (BusinessWire)](https://www.businesswire.com/news/home/20260407796810/en/Trent-AI-Raises-\$13M-to-Secure-the-Agentic-Age) · [SecurityWeek coverage](https://www.securityweek.com/trent-ai-emerges-from-stealth-with-13-million-in-funding/) · [Tech.eu coverage](https://tech.eu/2026/04/07/openai-and-spotify-leaders-back-london-based-ai-security-agent-startup-in-13m-round/)

> Homepage URL above is unverified — Trent AI's primary domain has not been disclosed in coverage as of 2026-05-03; re-validate before external citation.

## Description

London-based agentic AI security startup founded in 2025 by **former AWS engineering leaders**, led by CEO **Eno Thereska**. Builds a layered platform meant to secure AI agents throughout their entire lifecycle — pitched at developers and organizations building AI agents and autonomous software systems (Source: [SecurityWeek](https://www.securityweek.com/trent-ai-emerges-from-stealth-with-13-million-in-funding/)).

## Funding

**\$13M (€11M) seed round, April 7, 2026** — led by **LocalGlobe** and **Cambridge Innovation Capital**, with backing from leaders at **OpenAI, Spotify, Databricks, and AWS** as angels.

Second-largest agentic-AI-security seed round in the May 2025–May 2026 window, behind only [[lumia-security|Lumia]].

## Relevance

Cross-plane coverage: the **multiple-coordinated-agents** architecture matches the [[sentinels-and-operatives|Sentinels-and-Operatives]] split, and the lifecycle scope ("scans models, dependencies, infrastructure, runtime behavior") spans CMM **D4 (Runtime & Guardrails)**, **D7 (Observability & Detection)**, and **D8 (Supply Chain & AI-BOM)**, with **D1 (Governance & Accountability)** as the wrapping layer.

Functionally a competitor to [[onyx-platform|Onyx Platform]] and [[lumia-security|Lumia]] in the [[guardian-agent|Guardian Agent]] / [[ai-agent-management-platform|AI Agent Management Platform]] category. Differentiator: explicit **multi-agent security architecture** ("agents securing agents"), an instance of the [[guardian-agent-metagovernance|Guardian Agent Metagovernance]] question — who guards the guardians.

## Product

Per the launch announcement, the platform:

- Continuously scans models to observe **code, dependencies, infrastructure, and runtime behavior**
- Analyzes **risks and business impact**
- **Patches vulnerabilities** and **modifies configurations**
- **Validates fixes** (loop closure)
- Embeds the security layer into developer workflows

Multiple coordinated security agents work together continuously to understand the environment and secure it (Source: [Trent AI press release](https://www.businesswire.com/news/home/20260407796810/en/Trent-AI-Raises-\$13M-to-Secure-the-Agentic-Age)).

> [!gap]
> No technical disclosures as of May 2026 on whether the coordinated agents are LLM-driven (vulnerable to [[recursive-prompt-injection|recursive prompt injection]] like any LLM-as-judge layer) or rule-based. Re-validate when more material lands.

## Notable Statements

- Thereska: "Organizations are deploying AI agents and autonomous workflows faster than their security can adapt" — frames the gap that the [[ai-agent-layered-council|AI Agent Layered Council]] and [[shadow-automation|Shadow Automation]] concepts already track.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[guardian-agent-metagovernance|Guardian Agent Metagovernance]]
- [[guardian-agent|Guardian Agent]]
