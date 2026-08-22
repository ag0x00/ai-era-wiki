---
type: entity
title: "General Analysis"
created: 2026-05-03
updated: 2026-06-21
tags:
  - entities
  - organization
  - vendor
  - agentic-ai-security
  - seed-funded
  - red-teaming
status: seed
scope_axis:
  - sec-of-ai
  - ai-in-sec-offense
entity_type: organization
org_type: vendor
role: "Adversarial testing and empirical measurement for agentic AI — live-system red-teaming, not static rules"
related:
  - "[[splx-ai]]"
  - "[[mindgard-cart]]"
  - "[[promptfoo]]"
  - "[[garak]]"
  - "[[agentdojo]]"
  - "[[claude-stripe-coupons-imessage-injection]]"
homepage: "https://www.generalanalysis.com"
sources:
  - "https://www.businesswire.com/news/home/20260429247972/en/General-Analysis-Raises-$10M-in-Seed-Funding-to-Secure-Agentic-AI"
  - "https://www.axios.com/pro/enterprise-software-deals/2026/04/29/agentic-ai-security-startup-general-analysis"
  - "https://techstartups.com/2026/04/29/general-analysis-raises-10m-in-seed-funding-to-secure-agentic-ai-against-real-world-attacks/"
---

# General Analysis

**Sources:** [General Analysis (homepage)](https://www.generalanalysis.com) · [Funding announcement (BusinessWire)](https://www.businesswire.com/news/home/20260429247972/en/General-Analysis-Raises-\$10M-in-Seed-Funding-to-Secure-Agentic-AI) · [Axios Pro coverage](https://www.axios.com/pro/enterprise-software-deals/2026/04/29/agentic-ai-security-startup-general-analysis) · [Tech Startups coverage](https://techstartups.com/2026/04/29/general-analysis-raises-10m-in-seed-funding-to-secure-agentic-ai-against-real-world-attacks/)

> Homepage URL above is unverified — primary domain not stated in launch coverage; re-validate.

## Description

Agentic AI security research-and-product company founded in 2025 by **Rez Havaei** (CEO; previously [[anthropic|Cohere]] and NVIDIA), **Maximilian Li** (Harvard), and **Rex Liu** (Caltech). Treats AI security as an **empirical measurement problem** rather than a rules-and-guardrails problem. It runs adversarial simulations against live agent systems and quantifies how often, and how badly, agents fail (Source: [Tech Startups](https://techstartups.com/2026/04/29/general-analysis-raises-10m-in-seed-funding-to-secure-agentic-ai-against-real-world-attacks/)).

The same Havaei / Li / Liu team produced the **[[claude-stripe-coupons-imessage-injection|Claude → Stripe coupons via iMessage]]** exploit research already filed in this wiki (multi-MCP context-pollution exploit). General Analysis is the productization of that research line.

## Funding

**\$10M seed round, April 29, 2026** — led by **Altos Ventures** with **645 Ventures**, **Menlo Ventures**, **Y Combinator**, and angels. Fourth-largest agentic-AI-security seed in the 12-month window.

## Relevance

Cross-cutting in the [[agentic-ai-security-reference-architecture|RA]] (testing surface, not a runtime plane). In the [[agentic-ai-security-cmm-2026|CMM]], maps to **D7 (Observability & Detection)** at the **continuous red-team / CART** evidence slot — same row as [[mindgard-cart|Mindgard CART]], [[splx-ai|SplxAI]] (now part of Zscaler), [[promptfoo|Promptfoo]], [[garak|Garak]], [[pyrit|PyRIT]], and [[agentdojo|AgentDojo]].

Differentiator from those peers: explicitly **live-system, agent-level** rather than model-level. The research line that produced [[claude-stripe-coupons-imessage-injection|the coupon exploit]] suggests their probe library targets multi-MCP / multi-agent attack chains, not single-prompt-injection harnesses.

## Product

Methodology disclosed in launch coverage:

1. **Live-system adversarial simulations** against production agent stacks
2. **Failure-frequency and severity measurement** — rejects "prove safe" framing in favor of "drive numbers down"
3. **Risk quantification** to help defenders prioritize controls that actually reduce risk without crippling agent utility

Product specifics (probe library, harness API, integration shape) not disclosed in launch material.

## Notable Statements

- Co-founder: *"You cannot prove an agent is safe. You can only measure how often it fails, and how badly, and drive both numbers down."* — matches the spirit of [[evidence-centered-benchmark-design|Evidence-Centered Benchmark Design]] and the [[agentic-ai-security-cmm-2026|CMM]]'s **D7 L4 measurement-based** evidence stance.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[claude-stripe-coupons-imessage-injection|Claude → Stripe coupons exploit]] — the team's prior research output
- [[mindgard-cart|Mindgard CART]] — closest commercial peer at higher stage
- [[splx-ai|SplxAI]] — closest seed-stage peer (now Zscaler)
