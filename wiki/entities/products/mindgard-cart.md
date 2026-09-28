---
type: entity
entity_type: product
title: "Mindgard CART"
created: 2026-05-02
updated: 2026-06-21
homepage: "https://mindgard.ai"
tags:
  - entities
  - products
  - red-team
  - commercial
  - cart
  - mindgard
status: developing
scope_axis:
  - sec-of-ai
publisher: "Mindgard"
deployment: "SaaS (cloud-native; on-prem not publicly documented)"
pricing: "Not public — book-a-demo / contact-sales"
canonical_site: "https://mindgard.ai/"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[pyrit]]"
  - "[[garak]]"
  - "[[promptfoo]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[red-teaming-for-ai-synthesis]]"
sources:
  - "https://mindgard.ai/"
  - "https://mindgard.ai/blog/continuous-automated-red-teaming"
  - "https://mindgard.ai/blog/ai-red-teaming-statistics"
  - "https://techcrunch.com/2024/12/20/british-university-spinoff-mindgard-protects-companies-from-ai-threats/"
---

# Mindgard CART — Continuous Automated Red Teaming

**Sources:** [Mindgard (homepage)](https://mindgard.ai) · [CART explainer (Mindgard blog)](https://mindgard.ai/blog/continuous-automated-red-teaming) · [Mindgard spinout coverage (TechCrunch)](https://techcrunch.com/2024/12/20/british-university-spinoff-mindgard-protects-companies-from-ai-threats/)

Commercial **Continuous Automated Red Teaming (CART)** product line that simulates adversarial attacks against AI systems on a 24/7 cadence. [[red-teaming-for-ai-synthesis|Red Teaming for AI: Synthesis]] places Mindgard CART in the **continuous adversarial testing** category, one of the four from which the wiki's [[agentic-ai-security-cmm-2026|CMM]] asks for tools in two at L4, under D7-EVAL-TOOLS, and no D7 criterion requires this category.

## Definition of CART

> *"Continuous automated red teaming (CART) is a dynamic method that simulates threats 24/7."* — Mindgard blog
> *"Unlike traditional red teaming, which occurs periodically, CART operates 24/7, reducing human error, enabling scalability, and allowing immediate threat mitigation."* — Mindgard blog

> [!note] Term provenance
> **Mindgard is the marketing originator of the "CART" acronym.** The wiki treats it as a vendor term until/unless an analyst or framework adopts it independently. Analogous concept (managed continuous attack simulation) exists at other vendors and in academic literature; the specific three-letter acronym is Mindgard's.

## Product line

Three products under the Mindgard brand, with CART as the methodology framing:

| Product | Role |
|---|---|
| **AI Recon** | Discovery: what AI assets exist, what attack surface they expose |
| **AI Assessment** | Find/fix: periodic and continuous attack simulation |
| **AI Runtime Protection** | Real-time defense: runtime intervention |

## Coverage scope (advertised)

- AI chatbots, AI applications, AI infrastructure, agentic workflows
- LLMs and image-generation AI explicitly named
- Attack categories named in marketing: prompt injection, hallucination testing, misuse prevention; plus traditional "phishing, lateral movement, data exfiltration" framing
- Ecosystem integrations: OpenAI, [[anthropic|Anthropic]], AWS, Docker

## Undisclosed material

A peer reviewer should know what the wiki cannot verify:

- **Specific attack-library inventory**: gated behind sales
- **Control / probe counts**: not publicly enumerated
- **Pricing tiers**: book-a-demo only
- **On-prem option**: not documented (SaaS appears to be the default)

The wiki should not invent these numbers.

## Company

- **Founders**: Dr. Peter Garraghan (CEO/CTO), Dr. Neeraj Suri (CSO), Steve Street (CRO/COO)
- **Origin**: spinout from Lancaster University (UK), 2022
- **Funding (Dec 2024)**: \$8M round led by .406 Ventures with Atlantic Bridge, WillowTree, IQ Capital, Lakestar
- **Recognition (2025–2026)**: named in Gartner Emerging Tech 2026 — Top-Funded Startups in AI TRiSM (Agentic AI); 2025 Cybersecurity Excellence Award (Best AI Security Solution); Garraghan named 2025 Cybersecurity Innovator of the Year

## Use in this wiki

- [[agentic-ai-security-cmm-2026|CMM]] [[agentic-ai-security-cmm-d7-observability|D7]] L4: continuous adversarial testing category
- [[agentic-ai-security-cmm-measurement-protocol|Measurement Protocol]]: named among the four tools the D7 evaluation question asks about
- Closes the **24/7 / SaaS-managed** seam that PyRIT (DIY orchestration), Garak (point-in-time scan) and Promptfoo (CI-trigger-only) don't cover by default

## Caveats

- **Vendor-published "best AI red-team tools" comparisons**: useful but partisan; cross-check with independent sources.
- **CART acronym is vendor-coined**: the wiki should keep flagging this until a framework body picks it up (similar discipline to [[insight-partners|Insight Partners]]'s "UEBA for Agents").
- **D7-EVAL-TOOLS counts testing categories.** Continuous adversarial testing is one of the four, and Mindgard is one instance of it. The protocol's evaluation question names Mindgard CART beside PyRIT, Garak and Promptfoo. Equivalents to evaluate: AI-Driven Pen Testing services from large security platforms, Lakera Red, [[knostic|Knostic]] for coding-agent specific, HiddenLayer.

## See Also

- [[pyrit|PyRIT]]: DIY orchestration counterpart
- [[garak|Garak]]: point-in-time probe library
- [[promptfoo|Promptfoo]]: CI regression suite
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]: D7 L4 evidence anchor
