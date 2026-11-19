---
type: entity
entity_type: product
vendor: "[[ping-identity|Ping Identity]]"
title: "Ping Enterprise Personal Agent Access"
address: c-000349
created: 2026-09-10
updated: 2026-10-06
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
  - "[[microsoft-agent-365]]"
  - "[[ping-identity]]"
  - "[[mcp-security]]"
  - "[[shadow-automation]]"
sources:
  - "https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action"
  - "https://www.helpnetsecurity.com/2026/09/01/ping-identity-enterprise-personal-agent-access/"
  - "https://vmblog.com/news/ping-identity-secures-claude-personal-agents-from-discovery-to-action/"
  - "https://itwire.com/business-it-news/security/ping-identity-secures-claude-personal-agents-from-discovery-to-action"
verified: 2026-10-06
verified_against: []
verified_findings: 0
verified_note: "Read the live Ping press release and the Help Net Security and iTWire reprints on 2026-10-06 (vmblog returned 403). Discovery scope, credential and PAM-positioning claims absent from them were rewritten to the release text; the resource list is the enforcement scope, not discovery scope."
---

# Ping Enterprise Personal Agent Access

**Sources:** [Press release](https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action) · [Help Net Security coverage](https://www.helpnetsecurity.com/2026/09/01/ping-identity-enterprise-personal-agent-access/) · [iTWire coverage](https://itwire.com/business-it-news/security/ping-identity-secures-claude-personal-agents-from-discovery-to-action)

## Identity and role

[[ping-identity|Ping Identity]] announced Enterprise Personal Agent Access on September 1, 2026, delivering it through the existing PingOne Privilege platform rather than as a separate product. It targets personal AI agents — Claude desktop assistants and Claude Code, with the design extensible to other personal agents — and combines three capabilities: [[agent-catalog|agent discovery]], secretless privileged access, and runtime action control.

## Agent discovery

The product discovers personal AI agents, including the shadow agents that employees run without approval ([[shadow-automation|Shadow Automation]]), which Ping calls shadow AI. When an agent starts within a supported environment, Ping detects it and ties the session to the user and device behind it.[^ping-pr] The release names Claude desktop assistants and Claude Code as supported agents and does not list the environments it supports.

## Secretless privileged access

Agents commit code and reach approved resources "without ever holding a long-lived credential", which is the problem the [[credential-proxy-pattern|credential proxy pattern]] addresses, applied here to personal-agent workflows.[^ping-pr] Every commit lands under the identity that made it, the developer or the agent, and agent activity stays attributable to the agent as well as to the user who started the session. Ping grounds the need in agent speed: unlike human users, agents "can act across systems at machine speed and scale".[^ping-pr]

## Runtime action control

Policy enforcement sits in front of managed resources and decides at the moment of action what an agent can reach and do. It can allow, deny or log an action, require human approval for sensitive activity, or revoke access in real time.[^ping-pr] The resources Ping names are the ones personal agents reach: [[mcp-security|MCP]] servers, code repositories, internal services and APIs, Kubernetes clusters, databases and cloud infrastructure. The activity record gives what the agent did, when it happened and the user associated with the session.

## Availability

Ping describes Enterprise Personal Agent Access as available now, with pilot deployments under way at unnamed global enterprises as of the September 1, 2026 announcement. No pricing has been disclosed. The product implements the [[agent-identity-architecture|agent identity architecture]]'s discovery, credential, and authorization layers together in one offering, and its secretless-access and per-action-enforcement design bears on the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity and Authorization]] domain and the [[agentic-ai-security-cmm-2026|CMM]]'s broader identity criteria; [[okta-for-ai-agents|Okta]] and [[microsoft-entra-agent-id|Microsoft Entra]] occupy the same identity-for-agents category with different platform integrations.

## Notable statements

Andre Durand, founder and CEO, Ping Identity: "The security challenge is establishing a common way to see and govern all of them. The question isn't whether an agent is intelligent enough to act, but whether the enterprise can see and control its action when it does."

[^ping-pr]: Ping Identity, [*Ping Identity Secures Claude Personal Agents From Discovery to Action*](https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action), press release, 2026-09-01, retrieved 2026-10-06. Help Net Security and iTWire reproduce the release text.
