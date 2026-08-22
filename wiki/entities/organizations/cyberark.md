---
type: entity
entity_type: organization
org_type: vendor
title: "CyberArk"
created: 2026-05-03
updated: 2026-06-21
homepage: "https://www.cyberark.com"
tags:
  - entities
  - organizations
  - identity-security
  - pam
  - secrets-management
  - iam
status: developing
role: "Identity Security Platform vendor; secrets management; AI agent identity governance"
related:
  - "[[cyberark-conjur]]"
  - "[[palo-alto-networks]]"
  - "[[oasis-security]]"
  - "[[nhi-governance-for-agents]]"
sources:
  - "https://www.cyberark.com/"
---

# CyberArk

**Sources:** [CyberArk (homepage)](https://www.cyberark.com) · [Conjur Secrets Manager](https://www.cyberark.com/products/secrets-manager-enterprise/) · [Secure AI Agents](https://www.cyberark.com/solutions/secure-ai-agents/)

CyberArk is a publicly-traded identity security vendor (NASDAQ: CYBR until 2026 acquisition) historically anchored on Privileged Access Management (PAM). The company has expanded its Identity Security Platform across human PAM, secrets management ([[cyberark-conjur|Conjur]]), workload identity, and — beginning in 2025 — non-human-identity governance for AI agents under the **Secure AI Agents** initiative.

## Core product lines

| Line | Role |
|---|---|
| **Privileged Access Manager** | Human PAM — session recording, vaulting, just-in-time elevation |
| **[[cyberark-conjur\|Conjur (Secrets Manager)]]** | Vault and policy engine for secrets used by non-human identities; foundation for credential proxying for AI agents |
| **Secure AI Agents** | Initiative spanning Conjur + new agent-specific tooling (AI Agent Gateway, MCP-aware enforcement, zero-standing-privileges) |
| **Agent Guard** | Product for STDIO-based MCP server flows (AWS Marketplace, 2025) |
| **Identity Security Platform** | Umbrella platform unifying human + non-human identity security |

## Palo Alto Networks acquisition (2026)

[[palo-alto-networks|Palo Alto Networks]] announced acquisition of CyberArk for approximately \$25B (closed/announced 2026). The strategic implication is convergence of CyberArk's identity-side stack (PAM, Conjur, NHI governance) with Palo Alto's runtime/network/AI-security stack ([[palo-alto-prisma-airs|Prisma AIRS]], Prisma Cloud, Prisma SASE, Cortex). This positions Palo Alto with one of the most complete commercial AI agent security portfolios in the market, spanning identity → policy → runtime → network → posture → red-team.

## Position in the agent security market

CyberArk is the **incumbent vault layer** in the credential-proxy / NHI ecosystem. Newer NHI-governance vendors ([[oasis-security|Oasis Security]], Aembit, Astrix Security, GitGuardian) typically position above the vault layer rather than as substitutes. The industry framing: Conjur is a vault; the others are governance/access layers.

## Wiki references

- [[cyberark-conjur|CyberArk Conjur (and Secure AI Agents / Agent Guard)]] — primary product page
- [[agentic-ai-security-reference-architecture|RA]] Identity plane (Non-Human Identity governance, Credential proxy)
- [[nhi-governance-for-agents|NHI Governance for AI Agents]] practice page
- [[credential-proxy-pattern|Credential Proxy Pattern]] — Conjur is the canonical commercial implementation

> [!gap]
> Detailed product roadmap post-Palo Alto acquisition has not been publicly disclosed. Track for: integration of Conjur with Prisma AIRS identity surface, unification of the AI Agent Gateway with AgentGateway / Prisma AIRS runtime, and any deprecation of overlapping Palo Alto identity offerings.
