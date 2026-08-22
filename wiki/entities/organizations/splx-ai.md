---
type: entity
title: "SplxAI (now part of Zscaler)"
created: 2026-05-03
updated: 2026-05-23
tags:
  - entities
  - organization
  - vendor
  - agentic-ai-security
  - red-teaming
  - acquired
status: stub
entity_type: organization
org_type: vendor
role: "End-to-end AI security platform — automated adversarial testing for agentic AI systems; acquired by Zscaler"
related:
  - "[[general-analysis]]"
  - "[[mindgard-cart]]"
  - "[[promptfoo]]"
  - "[[garak]]"
homepage: "https://splx.ai"
sources:
  - "https://splx.ai/blog/splxai-closes-7m-seed-funding-round-to-help-organizations-secure-agentic-ai-systems"
  - "https://www.businesswire.com/news/home/20250326713814/en/SplxAI-Closes-$7M-Seed-Funding-Round-to-Help-Organizations-Secure-Agentic-AI-Systems"
  - "https://therecursive.com/zscaler-acquires-splxai-for-advanced-llm-security-and-ai-governance/"
---

# SplxAI (now part of Zscaler)

**Sources:** [Homepage](https://splx.ai) · [Seed funding announcement (Splx blog)](https://splx.ai/blog/splxai-closes-7m-seed-funding-round-to-help-organizations-secure-agentic-ai-systems) · [Zscaler acquisition coverage (The Recursive)](https://therecursive.com/zscaler-acquires-splxai-for-advanced-llm-security-and-ai-governance/)

## Description

Croatian-founded agentic-AI red-teaming and security testing company. Automated security testing of AI agents and customer-facing AI applications via simulated adversarial scenarios. Acquired by **Zscaler** during the same year for advanced LLM security and AI governance — the first publicly disclosed exit in the agentic-AI-security seed cohort tracked here.

## Funding

**\$7M seed round, March 26, 2025** — led by **LAUNCHub Ventures** with Rain Capital, Inovo, Runtime Ventures, DNV Ventures, South Central Ventures.

**Acquired by [Zscaler](https://www.zscaler.com)** later in 2025 (exact date not in primary sources reviewed).

## Relevance

Maps to the [[agentic-ai-security-cmm-2026|CMM]] **D7 (Observability & Detection)** continuous-red-team / CART evidence slot. Closest peers: [[mindgard-cart|Mindgard CART]], [[promptfoo|Promptfoo]] (now part of OpenAI), [[garak|Garak]] (NVIDIA), [[general-analysis|General Analysis]] (the next-cohort seed).

The **Zscaler acquisition** is a category signal: the offensive/CART side of agentic AI security is consolidating into existing CNAPP / SSE platforms, not staying independent. Comparable trajectory to **Promptfoo → OpenAI** (also part of a larger platform now). Implication for the seed-stage cohort: pure-play CART startups may have a shorter independent runway than gateway / runtime / identity startups.

## Product

Capabilities advertised at seed:
- Automated security testing of internal AI agents and customer-facing AI applications
- Simulates **sophisticated adversarial scenarios that mimic the tactics of highly skilled attackers**
- Detects and mitigates prompt injection, off-topic responses, hallucinations
- Continuous monitoring and dynamic remediation

Founding team included AI red teamers and CTF winners (Wiz Capture-The-Flag, Black Hat) — pedigree-grade research staff.

## Notable

The SplxAI exit happened **before the next seed wave** (Trent AI, General Analysis, Capsule Security in April 2026). This timing is informative for the broader synthesis: investors funded Helmet, Runlayer, Capsule, etc. **knowing the CART category had already absorbed**, suggesting the seed cohort views the gateway / runtime / identity / lifecycle plays as more durable independent businesses than the testing-only category.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[general-analysis|General Analysis]] — closest peer at the next seed wave
- [[mindgard-cart|Mindgard CART]]
