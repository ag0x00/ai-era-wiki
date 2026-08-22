---
type: entity
entity_type: product
title: "Microsoft Agent 365 (with Entra Agent ID)"
created: 2026-05-03
updated: 2026-08-21
tags:
  - products
  - identity
  - nhi
  - agent-lifecycle
  - cots
  - microsoft
  - agent-platform
status: developing
scope_axis:
  - sec-of-ai
vendor: "Microsoft"
homepage: "https://techcommunity.microsoft.com/blog/identity/introducing-microsoft-entra-agent-id/4405875"
ga_date: "2026-05-01"
sku: "Microsoft 365 E7: The Frontier Suite"
related:
  - "[[okta-for-ai-agents]]"
  - "[[agent-identity-architecture]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[microsoft-zt4ai|Microsoft ZT4AI]]"
  - "[[microsoft-secure-agentic-ai-end-to-end|Secure Agentic AI End-to-End]]"
  - "[[microsoft-entra-agent-id-security-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[microsoft-rai|Microsoft Responsible AI Standard (RAI)]]"
  - "[[agentic-cmm-regulated-fi-stress-test|Agentic AI CMM: Regulated-FI Stress Test]]"
sources:
  - "https://techcommunity.microsoft.com/blog/identity/introducing-microsoft-entra-agent-id/4405875"
  - "[[.raw/articles/microsoft-secure-agentic-ai-end-to-end-2026-05-07.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-security-for-ai-2026-05-25.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-governance-2026-05-25.md]]"
---

# Microsoft Agent 365 (with Entra Agent ID)

**Sources:** [Introducing Microsoft Entra Agent ID (Microsoft Tech Community)](https://techcommunity.microsoft.com/blog/identity/introducing-microsoft-entra-agent-id/4405875) · [[microsoft-secure-agentic-ai-end-to-end|Secure Agentic AI End-to-End]] · [[microsoft-entra-agent-id-security-governance|Entra Agent ID security & governance]]

Microsoft's umbrella product for agentic-AI governance, positioned by Microsoft as **"the control plane for agents"** ([[microsoft-secure-agentic-ai-end-to-end|Vasu Jakkal, Microsoft Security Blog, March 2026]]). Agent 365 was launched at **Ignite 2025 (2025-11-18)** and reached general availability on **May 1, 2026** as part of the new **Microsoft 365 E7: The Frontier Suite** SKU (bundled with Microsoft 365 Copilot, Microsoft Entra Suite, and Microsoft 365 E5).

The Microsoft Learn overview frames Agent 365 around three verbs — **observe, govern, secure** — surfaced through five named capabilities: **Registry** (agent inventory, single source of truth, with Registry sync for cross-cloud agents), **Access Control** (per-agent identity, Policy Templates, risk-based Conditional Access), **Visualization** (a unified dashboard and Agent Map over agents, users, and resources), **Interoperability** (Foundry, open-source frameworks, partner clouds), and **Security** (Defender runtime protection, Purview information protection and DLP). The standards review treats Agent 365 as a **management plane** over the [[microsoft-zt4ai|ZT4AI]]-catalogued enforcement controls — its distinct contribution is the registry and admin aggregation, not a new control. See [[standards-review-microsoft-rai-agent-365-2026-Q2|the RAI / Agent 365 standards review]].

Agent 365 includes capabilities across three Microsoft Security pillars:
- **Microsoft Entra** — agent identity, conditional access, governance (this product page's primary focus).
- **Microsoft Defender** — agent runtime threat detection (covered separately on the [[microsoft|Microsoft]] org page).
- **Microsoft Purview** — data oversharing prevention for agent flows (covered separately).

**Entra Agent ID** is the identity-credential layer underneath Agent 365. The **Agent 365 Registry** is the catalog / governance layer. Together they are the Microsoft-native equivalent of what [[okta-for-ai-agents|Okta for AI Agents]] offers for Okta-anchored organizations.

## Components

**Entra Agent ID** is the identity credential layer: each agent is registered as a distinct identity in the Microsoft Entra directory (analogous to a service principal but typed as an agent). It supports:

- Scoped OAuth tokens issued per agent identity, carrying delegated permission scopes inherited from the agent identity blueprint
- Short-lived credentials enforced by the identity platform rather than by the agent
- RBAC policy assignment at the agent-identity level
- Integration with Conditional Access for risk-based step-up

The scoping above is bounded at the identity layer. [[agentic-cmm-regulated-fi-stress-test|The regulated-FI CMM stress test]] records that Agent ID issues tokens scoped to an *agent identity* through the on-behalf-of flow, which leaves per-*task* holder-bound capability grants of the [[tenuo-warrant|Tenuo Warrant]] class as an off-stack fill for an all-Microsoft buyer.

**Agent 365 Registry** is the catalog and governance layer: a centralized registry of all agents deployed in an organization's Microsoft 365 tenant. It provides:

- Agent discovery (including shadow agents not explicitly enrolled)
- Lifecycle event tracking (creation, rotation, suspension, decommission)
- Programmatic management via the Microsoft Graph API
- Owner-to-agent binding (every agent traces to a human principal in the directory)

## Security and governance model (Microsoft Learn, 2026)

Two Microsoft Learn overviews document the security and governance mechanics — see [[microsoft-entra-agent-id-security-governance|the consolidated summary]]. The load-bearing points:

- **Agent ID is available to all Microsoft Entra customers**, not gated behind Agent 365. Agent 365 is a paid integration layer (per-user license) for cross-M365 operation; this page's title reflects the bundle, but Agent ID stands alone. The security features need Microsoft 365 E5, or Entra ID P1 (Conditional Access, ID Governance) / P2 (ID Protection) / Entra Internet Access (network controls).
- **Four object types:** agent identity blueprint (template), blueprint principal (per-tenant instance), agent identity (instance), and agent user (optional 1:1 paired user account with mailbox/Teams). Controls applied at the **blueprint level** are inherited by all instances, so an entire class of agents can be disabled in one operation.
- **Three deployment patterns:** assistive (delegated permissions, acts for a user), autonomous (own identity, client-credentials flow), and agent's user account.
- **Sponsors** are the human accountability primitive: a named human owns each agent identity's lifecycle and access, and **sponsorship transfers automatically to the manager** when a sponsor leaves — the concrete mechanism behind [[agentic-ai-security-cmm-d9-operations|D9]] accountability and decommission. Access packages bind resource access time-bound with expiry and sponsor notification.
- **Zero Trust:** Conditional Access for agents (Microsoft Managed Policies block high-risk agents) and ID Protection for agents (agent identity risk → automatic remediation).
- Microsoft frames the motivating risk as **agent sprawl** — uncontrolled agent proliferation without lifecycle controls, its term for what the wiki tracks as [[shadow-automation|shadow automation]].

## ZT4AI integration

Entra Agent ID and Agent 365 Registry are core components of Microsoft's [[agentic-ai-security-reference-architecture|ZT4AI]] (Zero Trust for AI) framework, announced March 2026. Within ZT4AI, these products implement:

- **Agent Governance pillar** — enrollment, approval workflows for high-risk agents, lifecycle management
- **Action-to-identity tracing** — every agent action is attributed to the registered agent identity, creating a durable audit trail accessible via Microsoft Purview

The Anthropic Compliance API (announced March 24, 2026) provides a complementary audit capability for Claude-powered agents: API-level attribution of model-generated actions to specific deployment identities, compatible with Entra Agent ID as the authoritative identity source.

[[microsoft-rai|The Microsoft Responsible AI Standard]] sits above both catalogues: it states required outcomes and defers the enforcing control to Microsoft's Security Development Lifecycle, the Privacy Standard, the ZT4AI control catalogue, and the Agent 365 control plane. Agent 365 is therefore the product surface on which several RAI accountability and transparency goals acquire an enforcement mechanism, which carries this page's licensing and vendor-stack constraints into RAI conformance.

## Relation to the RA Identity plane

In the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]], Entra Agent ID appears in two Identity plane rows:

| Capability | Role |
|---|---|
| **Agent identity & lifecycle** | Primary enterprise COTS choice for M365/Azure-native organizations |
| **Action-to-identity tracing** | Native via Agent 365 + Purview; vendor-stack-locked |

The enterprise recommended stack pairs Entra Agent ID with **Microsoft Agent Governance Toolkit** (Apr 2026) in the Control plane for policy management.

## Comparison with Okta for AI Agents

| Dimension | [[okta-for-ai-agents\|Okta for AI Agents]] | Microsoft Entra Agent ID |
|---|---|---|
| GA date | Early Access; GA expected FY27 | May 1, 2026 (Agent ID available to all Entra customers) |
| Best fit | Okta-as-IdP organizations | M365 / Azure-native organizations |
| Registry | Okta Universal Directory + Agent Discovery | Agent 365 Registry (Graph API) |
| Audit integration | Okta System Log | Microsoft Purview |
| Third-party agent support | Broader (IdP-agnostic agents) | Optimized for Microsoft Copilot agents; third-party via OIDC |
| Action tracing | Via Okta Workflows | Via Anthropic Compliance API + Purview (Claude-specific) |

## CMM positioning

Equivalent to [[okta-for-ai-agents|Okta for AI Agents]] in CMM positioning: organizations adopting Entra Agent ID reach **L3 CMM** on the D2 identity track. The Purview integration for action tracing enables **L4** on D7 (Observability and Detection).

> [!note] Vendor-stack constraint
> Action-to-identity tracing via Purview is deeply integrated with the Microsoft stack. Organizations using non-Microsoft models or agent frameworks may find the tracing coverage incomplete; the Anthropic Compliance API integration covers Claude-on-Azure deployments specifically.
