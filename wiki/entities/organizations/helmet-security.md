---
type: entity
title: "Helmet Security"
created: 2026-05-03
updated: 2026-05-23
tags:
  - entities
  - organization
  - vendor
  - mcp-security
  - agentic-ai-security
  - seed-funded
status: seed
entity_type: organization
org_type: vendor
role: "End-to-end platform for securing agentic AI communications: MCP server discovery, monitoring, and control"
related:
  - "[[runlayer]]"
  - "[[agentgateway]]"
  - "[[mcp-security]]"
  - "[[ai-bom]]"
  - "[[mcp-cves-q1-2026]]"
homepage: "https://www.helmet.sh"
sources:
  - "https://www.businesswire.com/news/home/20251204673180/en/Helmet-Security-Secures-the-Future-of-Agentic-AI-Communications-Emerges-with-$9M"
  - "https://www.helmet.sh/blog/helmet-announces-raise/"
  - "https://siliconangle.com/2025/12/04/helmet-security-lands-9m-protect-enterprises-mcp-based-ai-security-risks/"
  - "https://www.securityweek.com/helmet-security-emerges-from-stealth-mode-with-9-million-in-funding/"
---

# Helmet Security

**Sources:** [Homepage](https://www.helmet.sh) · [Funding announcement (Helmet blog)](https://www.helmet.sh/blog/helmet-announces-raise/) · [SiliconANGLE coverage](https://siliconangle.com/2025/12/04/helmet-security-lands-9m-protect-enterprises-mcp-based-ai-security-risks/) · [SecurityWeek coverage](https://www.securityweek.com/helmet-security-emerges-from-stealth-mode-with-9-million-in-funding/)

## Description

Washington DC-area startup co-founded by **Fred Kneip** (founder of CyberGRX, which raised \$100M and was acquired into Marlin Equity / ProcessUnity) and **Kaushik Shanadi**. Builds an end-to-end platform that **discovers, monitors, and enforces controls on MCP servers** across the enterprise, pitched as the agentic-communications counterpart to traditional EDR/network-monitoring layers (Source: [SiliconANGLE](https://siliconangle.com/2025/12/04/helmet-security-lands-9m-protect-enterprises-mcp-based-ai-security-risks/)).

## Funding

**\$9M seed round, December 4, 2025**, led by **SYN Ventures** with **WhiteRabbit Ventures**. Fifth-largest agentic-AI-security seed in the 12-month window.

## Relevance

Maps to the [[agentic-ai-security-reference-architecture|RA]] **Egress** plane (MCP communications) and the **Observability** plane (continuous monitoring), with the **AI-BOM / supply chain** plane in scope through the discovery surface.

CMM evidence: **D5 L3-L4** (egress / MCP), **D7 L3-L4** (continuous monitoring), **D8 L3** ([[ai-bom|AI-BOM]] via runtime MCP discovery).

**Architectural distinction from [[runlayer|Runlayer]]**: where Runlayer is an inline gateway (sits in the data path), Helmet positions as a **discovery-monitoring-and-posture** layer that integrates with existing EDR, closer to [[wiz-ai-spm|AI-SPM]] in shape than to a gateway. This pairing is a direct instance of the [[inline-gateway-vs-runtime-instrumentation|gateway vs. instrumentation architectural fork]].

Cited contextual data: **"over 17,000 MCP servers deployed since [the protocol's] launch in November 2024, most unmonitored"**, the same scale-and-novelty argument that sells the category.

## Product

Three advertised functions:

1. **Identify**: find MCP servers as they appear in the environment
2. **Monitor**: continuous visibility into MCP communications and connections
3. **Enforce**: controls on what MCP servers do, in what context

Integrates with existing endpoint-detection-and-response stacks rather than replacing them. Identifies **new MCP communication paths as they appear** and immediately brings them under management, a pattern matching the [[shadow-automation|Shadow Automation]] discovery problem.

## Notable Statements

- Kneip's CyberGRX background colors the positioning: third-party-risk-style continuous discovery, applied to MCP-mediated agent-to-agent and agent-to-tool communications (the **AI-to-AI links** framing in [Fintech Global](https://fintech.global/2025/12/08/helmet-security-bags-9m-to-secure-ai-to-ai-links/)).

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]
- [[runlayer|Runlayer]], gateway peer at the same seed cohort
- [[mcp-security|MCP Security]]
