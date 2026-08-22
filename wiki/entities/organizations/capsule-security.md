---
type: entity
title: "Capsule Security"
created: 2026-05-03
updated: 2026-06-21
tags:
  - entities
  - organization
  - vendor
  - agentic-ai-security
  - seed-funded
  - runtime
status: seed
entity_type: organization
org_type: vendor
role: "Runtime trust layer for AI agents — monitors and blocks unsafe actions without proxies, gateways, or SDKs"
related:
  - "[[runlayer]]"
  - "[[helmet-security]]"
  - "[[lakera-guard]]"
  - "[[miggo-security]]"
  - "[[hitl]]"
homepage: "https://capsule.security"
sources:
  - "https://www.calcalistech.com/ctechnews/article/rk900cethzg"
---

# Capsule Security

**Sources:** [Capsule Security (homepage)](https://capsule.security) · [CTech launch coverage](https://www.calcalistech.com/ctechnews/article/rk900cethzg)

> Capsule's coverage at funding was thin (single primary article). Homepage URL above is unverified; re-validate when more material lands. Per the [[inline-gateway-vs-runtime-instrumentation|gateway-vs-instrumentation]] note, this is the cohort's only explicit-anti-proxy positioning, so additional sources will materially update this page.

## Description

Tel Aviv-based AI agent security startup founded in 2025 by **Naor Paz** (CEO; ex-F5, ex-Unit 8200) and **Lidan Hazout** (CTO; ex-Securedtouch, ex-Transmit Security). Builds a **runtime trust layer** that monitors and controls autonomous agents accessing data, executing workflows, and interacting with business systems, with the explicit positioning that no proxies, gateways, SDKs, or browser extensions are required (Source: [CTech](https://www.calcalistech.com/ctechnews/article/rk900cethzg)).

## Funding

**\$7M seed round, April 15, 2026**, led by **Lama Partners** and **Forgepoint Capital International**. Seventh-largest agentic-AI-security seed in the 12-month window.

## Relevance

Maps to the [[agentic-ai-security-reference-architecture|RA]] **Runtime** plane (primary, by explicit positioning) with cross-cutting **Observability** coverage. CMM evidence supports **D4 L3-L4** (runtime guardrails) and **D7 L3** (observability via runtime hooks).

**Architecturally explicit counter to gateway-style products** ([[runlayer|Runlayer]], [[helmet-security|Helmet Security]], [[agentgateway|AgentGateway]]): Capsule's whole pitch is *no gateway, no proxy, no SDK*. This is a direct instance of the [[inline-gateway-vs-runtime-instrumentation|gateway vs. runtime-instrumentation fork]] in the seed cohort.

Released **ClawGuard** as an open-source tool: adds checkpoints before agent tool execution, comparable in shape to [[lakera-guard|Lakera Guard]] but at the tool-call moment rather than at content layer. ClawGuard is conceptually a [[hitl|HITL]] confirm-tier primitive embedded into agent runtimes (Cursor, Claude Code, Copilot Studio, ServiceNow, Salesforce Agentforce are named integrations).

## Product

Per the launch announcement:

- **Runtime trust layer**: sits inside the agent runtime, not in the network path
- **Monitors**: visibility into autonomous-agent activity in production
- **Blocks unsafe actions** in real time
- **No proxy / gateway / SDK / browser-extension** deployment: the differentiator vs. the gateway camp
- Integrations: **Cursor**, **Claude Code**, **Microsoft Copilot Studio**, **ServiceNow**, **Salesforce Agentforce**

OSS spinoff: **ClawGuard**, providing pre-tool-execution checkpoints.

## Notable Statements

- Microsoft data point cited at funding: **"more than 80% of Fortune 500 companies now use active AI agents"**, which sized the market.
- The "no proxy/gateway/SDK" framing is distinctive enough to be a category claim, not just a feature claim. If Capsule's deployment model holds in production, it suggests the seed-stage market may be over-indexed on gateway-shaped products.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass covering this cohort
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]
- [[runlayer|Runlayer]], gateway counterpoint at similar stage
- [[lakera-guard|Lakera Guard]], content-layer guardrail counterpart
