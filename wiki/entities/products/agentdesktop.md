---
type: entity
entity_type: product
vendor: "[[solo-io|Solo.io]]"
title: "agentdesktop"
address: c-000350
created: 2026-09-10
updated: 2026-09-10
tags:
  - products
  - open-source
  - endpoint
  - mcp
  - nhi
  - credential-proxy
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
license: "Apache-2.0"
homepage: "https://github.com/agentdesktop-dev/agentdesktop"
related:
  - "[[agentgateway]]"
  - "[[solo-io]]"
  - "[[non-human-identity]]"
  - "[[agent-catalog]]"
  - "[[mcp-security]]"
  - "[[shadow-automation]]"
  - "[[guardian-agent]]"
  - "[[nhi-governance-for-agents]]"
  - "[[credential-proxy-pattern]]"
  - "[[agentic-ai-security-cmm-2026]]"
sources:
  - "https://www.solo.io/blog/introducing-agentdesktop"
  - "https://www.globenewswire.com/news-release/2026/09/03/3355961/0/en/solo-io-extends-agentic-governance-to-the-desktop-with-agentdesktop.html"
  - "https://techstrong.ai/articles/solo-io-pushes-agentic-ai-governance-to-the-desktop-with-open-source-agentdesktop/"
  - "https://github.com/agentdesktop-dev/agentdesktop"
---

# agentdesktop

**Sources:** [GitHub repository](https://github.com/agentdesktop-dev/agentdesktop) · [Introducing agentdesktop (Solo.io blog)](https://www.solo.io/blog/introducing-agentdesktop) · [Techstrong.ai coverage](https://techstrong.ai/articles/solo-io-pushes-agentic-ai-governance-to-the-desktop-with-open-source-agentdesktop/)

## Identity and role

[[solo-io|Solo.io]] released agentdesktop on September 3, 2026 under an Apache 2.0 license, at github.com/agentdesktop-dev/agentdesktop. It is an endpoint governance layer for the AI agent harnesses developers run directly on their machines — Claude Code, Codex, and VS Code agent extensions — and for those harnesses' [[mcp-security|MCP]] client configurations. It functions as the endpoint control plane complementing [[agentgateway|agentgateway]], Solo.io's server-side data plane: agentdesktop governs the harness on the desktop, agentgateway enforces policy on traffic in the cloud.

## Governance mechanisms

agentdesktop discovers every agent harness and every MCP server registered in a harness's configuration, building an endpoint-level [[agent-catalog|inventory]] that surfaces [[shadow-automation|shadow]] agent usage a fleet-wide security team otherwise cannot see. It translates centrally declared policy into each harness's own native configuration format — sandbox filesystem and network restrictions expressed the way Claude Code or Codex natively understands them, not as OS-level rules — which distinguishes it from conventional mobile device management. It treats MCP servers as first-class policy objects, distinguishing authorized sources from unauthorized ones and enforcing that distinction at the point a harness would otherwise connect to one.

## Identity, credentials, and observability

Each agent instance on a desktop is bound to device and user identity context, giving it a [[non-human-identity|non-human identity]] distinct from the developer running it, and that binding drives least-privilege, just-in-time credential issuance: agentdesktop replaces distributed, long-lived API keys with short-lived credentials scoped to the specific agent, user, and device, injected at runtime through [[agentgateway|agentgateway]] rather than stored on the workstation. This is a desktop-scoped application of the [[credential-proxy-pattern|credential proxy pattern]] and the practical mechanism behind [[nhi-governance-for-agents|NHI governance for AI agents]] at the endpoint — the harness itself never holds a standing secret. Session and tool-use telemetry is available on an opt-in basis, so an organization can monitor agent behavior and resource consumption without mandatory always-on collection.

## Deployment modes

agentdesktop runs in two modes. Standalone mode provides local governance on a single machine. Fleet mode distributes the endpoint daemon through existing mobile device management, and a central controller integrates with the organization's identity provider, public key infrastructure, and agentgateway for coordinated, organization-wide enforcement. The product functions as an endpoint-resident instance of the [[guardian-agent|guardian agent]] pattern — software that supervises what other agents on the same machine are permitted to do — and its policy-enforcement and credential-issuance design bears on the [[agentic-ai-security-cmm-2026|CMM]]'s identity and egress domains.

## Notable statements

Idit Levine, founder and CEO, Solo.io, on the sprawl the product addresses: "Every organization is struggling with AI agents and tooling sprawl. We at Solo.io are no different." On where agents now run: "Laptops and desktops are the first real production environment for AI agents."
