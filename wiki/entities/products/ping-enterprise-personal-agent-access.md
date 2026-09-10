---
type: entity
entity_type: product
vendor: "[[ping-identity|Ping Identity]]"
title: "Ping Enterprise Personal Agent Access"
address: c-000349
created: 2026-09-10
updated: 2026-09-10
tags:
  - products
  - identity
  - nhi
  - credential-proxy
  - mcp
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
homepage: "https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action"
ga_date: "2026-09-01"
related:
  - "[[agent-identity-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[non-human-identity]]"
  - "[[agent-catalog]]"
  - "[[credential-proxy-pattern]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[ping-identity]]"
  - "[[mcp-security]]"
sources:
  - "https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action"
  - "https://www.helpnetsecurity.com/2026/09/01/ping-identity-enterprise-personal-agent-access/"
  - "https://vmblog.com/news/ping-identity-secures-claude-personal-agents-from-discovery-to-action/"
  - "https://itwire.com/business-it-news/security/ping-identity-secures-claude-personal-agents-from-discovery-to-action"
---

# Ping Enterprise Personal Agent Access

**Sources:** [Press release](https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action) · [Help Net Security coverage](https://www.helpnetsecurity.com/2026/09/01/ping-identity-enterprise-personal-agent-access/) · [iTWire coverage](https://itwire.com/business-it-news/security/ping-identity-secures-claude-personal-agents-from-discovery-to-action)

## Identity and role

[[ping-identity|Ping Identity]] announced Enterprise Personal Agent Access on September 1, 2026, delivering it through the existing PingOne Privilege platform rather than as a separate product. It targets personal AI agents — Claude desktop assistants and Claude Code, with the design extensible to other personal agents — and combines three capabilities: [[agent-catalog|agent discovery]], secretless privileged access, and runtime action control.

## Agent discovery

The product identifies every personal AI agent running in an enterprise environment, including [[non-human-identity|shadow]] agents operating without approval, and ties each agent session to the specific user and device that started it. Discovery covers [[mcp-security|MCP]] servers, code repositories, internal services, APIs, Kubernetes clusters, databases, and cloud infrastructure.

## Secretless privileged access

Agents receive temporary, task-scoped credentials rather than permanent API keys or passwords, so no long-lived secret sits in an agent's reach — the same problem the [[credential-proxy-pattern|credential proxy pattern]] addresses, applied here to personal-agent workflows. Access is limited to the specific resources and operations a task requires, developers can authorize an agent to commit code or reach an approved service without handing it a static credential, and every action is attributed to both the agent's identity and the user who delegated to it. Ping positions this against traditional privileged access management, which assumes periodic credential rotation for a human or service account rather than the continuous, high-frequency access pattern an autonomous agent generates.

## Runtime action control

Authorization runs continuously, at each point an agent attempts an action as well as at session start: allow/deny decisions execute in real time, sensitive operations can require human approval before execution, and access can be revoked immediately without waiting for credential rotation or a session refresh. Every action is logged with agent identity, user identity, timestamp, and outcome, across the same enforcement scope as discovery — MCP servers, code repositories, internal services, APIs, Kubernetes clusters, databases, and cloud infrastructure.

## Availability

Ping describes Enterprise Personal Agent Access as available now, with pilot deployments under way at unnamed global enterprises as of the September 1, 2026 announcement. No pricing has been disclosed. The product implements the [[agent-identity-architecture|agent identity architecture]]'s discovery, credential, and authorization layers together in one offering, and its ephemeral-token and per-action-enforcement design bears on the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity and Authorization]] domain and the [[agentic-ai-security-cmm-2026|CMM]]'s broader identity criteria; [[okta-for-ai-agents|Okta]] and [[microsoft-entra-agent-id|Microsoft Entra]] occupy the same identity-for-agents category with different platform integrations.

## Notable statements

Andre Durand, founder and CEO, Ping Identity: "The security challenge is establishing a common way to see and govern all of them. The question isn't whether an agent is intelligent enough to act, but whether the enterprise can see and control its action when it does."
