---
type: entity
entity_type: organization
org_type: vendor
title: "Solo.io"
address: c-000346
created: 2026-09-10
updated: 2026-09-10
tags:
  - entities
  - organizations
  - vendor
  - mcp
  - open-source
  - egress
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
homepage: "https://www.solo.io"
related:
  - "[[agentgateway]]"
  - "[[agentdesktop]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[mcp-security]]"
sources:
  - "https://www.solo.io"
  - "https://www.solo.io/blog/introducing-agentdesktop"
  - "https://www.globenewswire.com/news-release/2026/09/03/3355961/0/en/solo-io-extends-agentic-governance-to-the-desktop-with-agentdesktop.html"
  - "https://www.linuxfoundation.org/press/linux-foundation-welcomes-agentgateway-project-to-accelerate-ai-agent-adoption-while-maintaining-security-observability-and-governance"
  - "https://www.globenewswire.com/news-release/2026/09/10/3359872/0/en/solo-io-extends-agentic-governance-to-any-environment-with-solo-enterprise-for-agentgateway.html"
---

# Solo.io

**Sources:** [Solo.io (homepage)](https://www.solo.io) · [Introducing agentdesktop](https://www.solo.io/blog/introducing-agentdesktop) · [agentgateway joins the Linux Foundation](https://www.linuxfoundation.org/press/linux-foundation-welcomes-agentgateway-project-to-accelerate-ai-agent-adoption-while-maintaining-security-observability-and-governance)

## Identity and role

Solo.io is a cloud-native networking and API infrastructure vendor, built on contributions to Istio and its own Gloo Edge API gateway. The company carries that networking and policy-enforcement background into agentic-AI infrastructure, positioning its products as the connectivity and governance layer beneath AI agent deployments rather than as a model- or prompt-layer control.

## Agentic AI product line

Solo.io maintains two complementary products for agentic-AI traffic. [[agentgateway|agentgateway]] is a Rust-based open-source data plane for [[mcp-security|MCP]] and A2A traffic, serving as a network-layer control point for agent-to-tool and agent-to-agent communication; the Linux Foundation announced Solo.io's donation of the project on August 25, 2025.[^lf] [[agentdesktop|agentdesktop]], released September 3, 2026 under an Apache 2.0 license, extends the same governance model to the endpoint: it discovers and governs the AI agent harnesses (Claude Code, Codex, VS Code agent extensions) running on developer desktops, and hands short-lived credentials carrying user, device, and tool context to agentgateway for enforcement at the traffic layer. The two products divide the stack: agentdesktop supplies inventory, device and user context, and declares and enforces policy locally on the endpoint (sandbox and MCP-connection restrictions translated into each harness's native configuration); agentgateway enforces that same policy in-path against model, MCP, and inter-agent traffic once it leaves the endpoint. Solo.io announced a commercial distribution, Solo Enterprise for agentgateway, on September 10, 2026, extending the same enforcement to any deployment environment rather than the mesh alone.[^enterprise]

## Relevance to this wiki

The [[agentic-ai-security-cmm-2026|CMM]]'s [[agentic-ai-security-cmm-d5-egress-network|D5 Egress & Network]] domain lists agentgateway as its sole open-source example of a mesh-deployed agent-aware proxy. Solo.io positions agentdesktop as extending agent governance from cloud and Kubernetes environments to the developer's machine, reaching the agent-harness layer that mobile device management leaves uncovered because MDM inventories the device rather than the harness running on it.

[^lf]: [Linux Foundation — Linux Foundation Welcomes agentgateway Project](https://www.linuxfoundation.org/press/linux-foundation-welcomes-agentgateway-project-to-accelerate-ai-agent-adoption-while-maintaining-security-observability-and-governance), August 25, 2025.
[^enterprise]: [GlobeNewswire — Solo.io Extends Agentic Governance to Any Environment with Solo Enterprise for agentgateway](https://www.globenewswire.com/news-release/2026/09/10/3359872/0/en/solo-io-extends-agentic-governance-to-any-environment-with-solo-enterprise-for-agentgateway.html), September 10, 2026.
