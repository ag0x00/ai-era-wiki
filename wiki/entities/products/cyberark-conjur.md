---
type: entity
entity_type: product
title: "CyberArk Conjur (and Secure AI Agents / Agent Guard)"
created: 2026-05-03
updated: 2026-08-21
tags:
  - products
  - secrets-management
  - nhi
  - credential-proxy
  - identity-plane
  - cots
status: developing
vendor: "CyberArk"
homepage: "https://www.cyberark.com/products/secrets-manager-saas/"
related:
  - "[[cyberark]]"
  - "[[credential-proxy-pattern]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agent-identity-architecture]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
sources:
  - "https://www.cyberark.com/products/secrets-manager-saas/"
  - "https://www.cyberark.com/resources/product-insights-blog/cyberark-secure-ai-agents"
  - "https://aws.amazon.com/marketplace/pp/prodview-d4xna3hosphyw"
---

# CyberArk Conjur (and Secure AI Agents / Agent Guard)

**Sources:** [CyberArk Secrets Manager SaaS](https://www.cyberark.com/products/secrets-manager-saas/) · [CyberArk Secure AI Agents](https://www.cyberark.com/resources/product-insights-blog/cyberark-secure-ai-agents) · [Agent Guard (AWS Marketplace)](https://aws.amazon.com/marketplace/pp/prodview-d4xna3hosphyw)

CyberArk Conjur is the secrets management product line within [[cyberark|CyberArk]]'s Identity Security Platform — a centralized vault and policy engine for storing, rotating, and brokering credentials (API keys, database passwords, TLS certificates, cloud tokens) used by non-human identities. Originally built for service accounts, containers, microservices, and CI/CD pipelines, Conjur has been extended for AI agent workloads through the **Secure AI Agents** initiative and the **Agent Guard** product (AWS Marketplace listing, 2025).

## Components

| Component | Role |
|---|---|
| **Conjur Open Source** | Self-hosted free version of the Conjur vault and policy engine |
| **Secrets Manager Self-Hosted** (formerly Conjur Enterprise) | On-premises / private-cloud distribution; enterprise SLAs |
| **Secrets Manager SaaS** (formerly Conjur Cloud) | Multi-tenant managed vault |
| **Secure AI Agents** | Initiative spanning Conjur + new agent-specific tooling: AI Agent Gateway, MCP-aware enforcement, zero-standing-privileges policy |
| **Agent Guard** | Product for STDIO-based MCP server flows; secret retrieval and observability for agent-to-tool communications; AWS Marketplace |

## Role in the RA Identity plane

In the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]], CyberArk Conjur appears in two Identity plane rows:

| Capability | Role |
|---|---|
| **Non-Human Identity governance** | Primary enterprise COTS for organizations with existing PAM infrastructure |
| **Credential proxy** | Conjur is the canonical commercial implementation of the [[credential-proxy-pattern\|Credential Proxy Pattern for AI Agents]] — agents never see long-lived secrets; Conjur mints ephemeral per-task credentials |

The enterprise recommended stack pairs CyberArk Conjur with [[okta-for-ai-agents|Okta for AI Agents]] or [[microsoft-entra-agent-id|Microsoft Entra Agent ID]] as the agent-identity layer, with Conjur as the credential layer beneath.

## Discovery for AI agents

The Secure AI Agents discovery dashboard covers AWS Bedrock and Microsoft Copilot Studio agents, providing visibility into which agents exist, what credentials they hold, and what tools they invoke. This is the catalog primitive that feeds CMM D7 (Observability and Detection).

## Comparison with Aembit / Astrix / Oasis Security

These three NHI-governance vendors are often compared with Conjur, but the positioning is complementary rather than substitutive:

| Vendor | Primary role | Relation to Conjur |
|---|---|---|
| **CyberArk Conjur** | Vault: store, rotate, broker credentials | Incumbent vault layer |
| **Aembit** | Workload-to-workload access policy with on-demand short-lived tokens | Explicitly integrates with CyberArk; sits *above* the vault as the policy/access layer |
| **Astrix Security** | NHI discovery, lifecycle, and posture across SaaS | Discovery and risk layer; consumes vault inventory |
| **Oasis Security** | NHI lifecycle and governance at enterprise scale | Discovery and lifecycle layer; pairs with vaults |
| **GitGuardian** | NHI governance with secret-scanning lineage | Lifecycle integrations into Conjur |

The industry framing: Conjur is the **vault** (where credentials live and rotate); Aembit / Astrix / Oasis / GitGuardian are **governance/access layers** that sit above the vault and enforce policy on how agents request and use credentials.

## Palo Alto acquisition (2026)

[[palo-alto-networks|Palo Alto Networks]] announced acquisition of CyberArk for approximately \$25B (closed/announced 2026). Conjur is a likely future component of Palo Alto's identity-security platform pillar, positioning CyberArk's secrets-management capability alongside [[palo-alto-prisma-airs|Prisma AIRS]] (runtime AI security) under a unified portfolio. Strategic implication: CyberArk's PAM/Conjur and Palo Alto's network/runtime stacks are converging into a single AI-agent-aware platform.

## CMM positioning

In the [[agentic-ai-security-cmm-2026|CMM]]:
- **D2 (Identity & Authorization) L4**: Conjur with rotation policies + Aembit/Astrix for governance achieves L4 evidence (per-agent credentials, programmatic rotation, no shared credentials)
- **D3 (Control & Least-Agency) L3**: Agent Guard + MCP-aware policy enforcement supports tool authorization tracking
- **D7 (Observability & Detection) L3**: Discovery dashboard + audit logs for agent credential use

> [!note] Vault vs governance layer distinction
> When evaluating CyberArk Conjur, assess separately: (a) vault capability — meets table-stakes for any enterprise; (b) governance capability for AI agents — currently developing (Secure AI Agents, Agent Guard). The agent-specific governance layer is newer than the vault and is still maturing as of Q2 2026; for the most demanding agent workloads, pair with a dedicated NHI governance product (Aembit, Astrix, Oasis).

## OSS alternative for self-hosted deployments

[[agentcordon|AgentCordon]] (Rust, GPL-3.0) is an early-stage open-source alternative for organizations that want a self-hosted credential broker without a CyberArk dependency. It collapses vault + Cedar PDP + MCP gateway + Agentic IDP into a single deployment with a three-tier (CLI / broker / server) split. Treat as a credible reference design rather than a battle-tested production option as of 2026-05-04 — the project has limited public deployment evidence at this stage.
