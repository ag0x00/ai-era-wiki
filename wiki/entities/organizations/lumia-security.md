---
type: entity
title: "Lumia Security"
created: 2026-05-03
updated: 2026-05-23
tags:
  - entities
  - organization
  - vendor
  - agentic-ai-security
  - seed-funded
status: seed
entity_type: organization
org_type: vendor
role: "Agentic AI security platform — visibility, governance, and control over enterprise AI agents"
related:
  - "[[onyx-security]]"
  - "[[trent-ai]]"
  - "[[ai-agent-management-platform]]"
  - "[[guardian-agent]]"
homepage: "https://www.lumia.security"
sources:
  - "https://www.businesswire.com/news/home/20251204673180/en/Lumia-Security-Raises-18M"
  - "https://www.lumia.security/blog/lumia-agentic-ai-security-platform-secures-18m-seed-round"
  - "https://siliconangle.com/2025/12/04/lumia-security-raises-18m-bring-governance-control-enterprise-ai-agents/"
  - "https://team8.vc/the-next-generation-of-securing-ai-why-we-invested-in-lumia/"
  - "https://www.calcalistech.com/ctechnews/article/rjk9mbyzwl"
---

# Lumia Security

**Sources:** [Homepage](https://www.lumia.security) · [Funding announcement (Lumia blog)](https://www.lumia.security/blog/lumia-agentic-ai-security-platform-secures-18m-seed-round) · [Team8 investment thesis](https://team8.vc/the-next-generation-of-securing-ai-why-we-invested-in-lumia/) · [SiliconANGLE coverage](https://siliconangle.com/2025/12/04/lumia-security-raises-18m-bring-governance-control-enterprise-ai-agents/)

## Description

Israeli agentic-AI security startup founded in 2025 by **Omri Iluz** (formerly CEO and co-founder of PerimeterX) and **Bobi Gilburd** (formerly CTO of Unit 8200). The company sells a platform that lets enterprises **see, govern, and control autonomous AI agents** in production — pitched as oversight infrastructure for the rapid rollout of agentic AI in financial services, technology, and other data-sensitive industries.

## Funding

**\$18M seed round, December 4, 2025** — led by [[team8|Team8]] with participation from New Era. Confirmed largest seed round in agentic AI security in the May 2025–May 2026 window (Source: [Lumia blog](https://www.lumia.security/blog/lumia-agentic-ai-security-platform-secures-18m-seed-round)).

Notable advisory addition: **Admiral Michael Rogers**, former Director of the NSA and Commander of US Cyber Command, joined the advisory board at funding announcement.

## Relevance

Maps primarily to the [[agentic-ai-security-reference-architecture|RA]] **Observability** plane and the **Control** plane (governance), with cross-cutting coverage of CMM **D1 (Governance & Accountability)**, **D7 (Observability & Detection)**, and **D9 (Operations & Human Factors)**.

Positioning competes directly with [[onyx-platform|Onyx Platform]] in the **[[guardian-agent|Guardian Agent]]** / [[ai-agent-management-platform|AI Agent Management Platform]] category — both pitch as cross-plane "single pane of glass" control planes for agentic AI in the enterprise. Differentiation rests on the founders' web-security pedigree (Iluz's PerimeterX exit was for visibility-and-control over bot traffic, an arguable analog of the agent-traffic problem) (Source: [Team8 investment thesis](https://team8.vc/the-next-generation-of-securing-ai-why-we-invested-in-lumia/)).

## Product

Per their announcement and Team8's investment thesis: a platform aimed at large organizations to **oversee the introduction of autonomous agents into operations**. Specific surfaces named in coverage:

- AI usage discovery and inventory across enterprise systems
- Governance + control over agentic activity (policy enforcement)
- Integration with major AI ecosystems and enterprise infrastructure partnerships

> [!gap]
> Public materials are governance-and-control-platform marketing language, not technical architecture. Whether Lumia ships an inline gateway, runtime instrumentation, or pure observability layer is not disclosed in primary sources as of May 2026. Re-validate when more technical material lands. See [[inline-gateway-vs-runtime-instrumentation|inline gateway vs. runtime instrumentation]] for why the architectural choice matters.

## Notable Statements

- Iluz / Gilburd framing: "rein in autonomous AI" — explicit governance/oversight (not pure threat-detection) framing (Source: [CTech](https://www.calcalistech.com/ctechnews/article/rjk9mbyzwl)).
- Team8's thesis: "next generation of securing AI" — places Lumia in the **Guardian Agent** category alongside [[onyx-platform|Onyx]] and Gartner's [[ai-trism|AI TRiSM]] expansion.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[guardian-agent|Guardian Agent]]
- [[ai-agent-management-platform|AI Agent Management Platform]]
