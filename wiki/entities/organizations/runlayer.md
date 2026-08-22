---
type: entity
title: "Runlayer"
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
role: "All-in-one MCP security gateway: threat detection, observability, permissions, custom AI automations"
related:
  - "[[helmet-security]]"
  - "[[agentgateway]]"
  - "[[mcp-security]]"
  - "[[mcp-cves-q1-2026]]"
homepage: "https://www.runlayer.com"
sources:
  - "https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/"
  - "https://www.runlayer.com/blog/runlayer-raises-11m-to-scale-enterprise-mcp-infrastructure"
  - "https://theaiinsider.tech/2025/12/02/runlayer-emerges-from-stealth-with-11m-to-secure-the-mcp-era/"
---

# Runlayer

**Sources:** [Homepage](https://www.runlayer.com) · [Funding announcement (Runlayer blog)](https://www.runlayer.com/blog/runlayer-raises-11m-to-scale-enterprise-mcp-infrastructure) · [TechCrunch launch coverage](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)

## Description

San Francisco-based MCP-security startup founded by third-time founder **Andrew Berman** (previously Nanit baby-monitor and Vowel AI video conferencing; Vowel sold to Zapier in 2024). Sells an **all-in-one MCP gateway** with threat detection, observability, custom-automation enablement, and Okta/Entra-integrated permissions. **David Soria Parra** (lead creator of [[mcp-security|MCP]]) is an angel and advisor.

## Funding

**\$11M seed round, November 17, 2025**, led by **Khosla Ventures** (Keith Rabois) and **Felicis**. Customer roster cited at launch included **eight unicorns or public companies**: Gusto, dbt Labs, Instacart, Opendoor, and four undisclosed (Source: [TechCrunch](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)).

## Relevance

Maps cleanly to the [[agentic-ai-security-reference-architecture|RA]] **Egress** plane (gateway is the canonical primitive) with strong overlap into **Observability** (full MCP request analytics) and **Identity** (permission system integrates with Okta + Entra).

Functional analog of [[agentgateway|AgentGateway]] (open-source, Linux Foundation) but commercial and MCP-specialized. Closest commercial peers: [[helmet-security|Helmet Security]] (discovery-and-monitoring side), Operant MCP Gateway, Natoma, and Cloudflare AI Gateway.

CMM evidence supports **D5 L3-L4** (egress / MCP), partial **D7 L3** (observability), partial **D2 L3** (identity integration).

## Product

Four advertised pillars (Source: [TechCrunch](https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis/)):

1. **Gateway**: inline MCP traffic mediation
2. **Threat detection**: analyzes every MCP request (mitigation surface for the [[mcp-cves-q1-2026|MCP CVE wave]]: 30+ CVEs, 82% path-traversal)
3. **Observability**: visibility across all agentic activity over MCP servers
4. **Enterprise development**: custom AI automation building blocks for IT
5. **Detailed permissions**: works with existing identity providers (Okta, Microsoft Entra)

The "MCP creator as advisor" signal is meaningful: protocol-level guidance for the gateway approach.

## Notable Statements

- Eight-unicorn customer signal at four months post-launch is unusually high for a seed-stage security company; suggests the MCP gateway market is **demand-led, not supply-led**.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]
- [[mcp-security|MCP Security]]
- [[helmet-security|Helmet Security]], discovery + monitoring counterpart
