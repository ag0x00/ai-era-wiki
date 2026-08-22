---
type: paper
title: "Entra Agent ID: Security and Governance Model"
address: c-000132
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - identity
  - governance
  - microsoft
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
authors: ["Microsoft"]
publisher: "Microsoft Learn"
source_url: "https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview"
related:
  - "[[microsoft-entra-agent-id]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[shadow-automation]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources:
  - "https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview"
  - "https://learn.microsoft.com/en-us/entra/id-governance/agent-id-governance-overview"
  - "[[.raw/articles/microsoft-entra-agent-id-security-for-ai-2026-05-25.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-governance-2026-05-25.md]]"
---

# Microsoft Entra Agent ID — Security & Governance Model (Microsoft Learn, 2026)

Two Microsoft Learn overview docs define the identity-control-plane model behind [[microsoft-entra-agent-id|Microsoft Entra Agent ID]]: a *security* overview (why AI systems need identity-based security, the agent patterns, Zero Trust controls) and a *governance* overview (governing agent identities like human identities through lifecycle, sponsors, and access packages). Together they are Microsoft's authoritative statement of how an all-Microsoft enterprise secures and governs agent identities, and they corroborate and sharpen the wiki's [[agentic-ai-security-cmm-d2-identity|D2 Identity]] and [[agentic-ai-security-cmm-d9-operations|D9 Operations]] recalibrations.

**Agent ID is not gated behind Agent 365.** The governance doc states plainly that **Microsoft Entra Agent ID is available to all Microsoft Entra customers**. Agent 365 is a paid integration layer on top (per-user license) that lets agents operate across Microsoft 365, not the parent product. The security *features* require Microsoft 365 E5 or specific Entra SKUs (below). This corrects the common framing of Agent ID as a sub-component of Agent 365.

## Three agent deployment patterns

The security model distinguishes three identity shapes, each with a different control posture:

| Pattern | Identity model | Use |
|---|---|---|
| **Assistive (interactive)** | acts on behalf of a signed-in user via Entra **delegated permissions** | on-demand tasks through a chat interface (support assistant, research helper) — the [[azure-rag-chatbot-security-profile\|RAG chatbot profile]] sits here |
| **Autonomous** | the agent's **own identity** via the client-credentials flow | background work without a human in the loop (log monitoring, infra autoscaling) |
| **Agent's user account** | an optional account paired **1:1** with an agent identity, with human-user characteristics (mailbox, calendar, Teams) | only when the agent must access systems that require a user object; it does not replace the agent identity — both exist |

## Four object types

Agent ID introduces four directory objects: the **agent identity blueprint** (a template), the **blueprint principal** (the per-tenant instance for a multitenant-capable agent, analogous to a multitenant app's service principal), the **agent identity** (an individual instance), and the **agent user** (the optional paired user account). A blueprint can mint one or more agent identities, each with distinct access rights. Conditional Access rules, permissions, and governance controls applied **at the blueprint level** are inherited by all current and future instances, so an entire class of agents can be disabled in a single operation.

## Zero Trust controls

- **Conditional Access for agents** evaluates agent context and risk before granting access, across all three patterns; **Microsoft Managed Policies** provide a baseline that blocks high-risk agents; policies deploy at scale via custom security attributes.
- **ID Protection for agents** flags anomalous agent activity, derives agent identity risk (from user risk and the agent's own actions), feeds risk signals to Conditional Access, and supports automatic remediation of compromised agents.
- **Global Secure Access** ("Secure Web and AI Gateway for agents") adds network-layer controls: logging agent network activity, web categorization for APIs and MCP servers, file-type upload/download policies, threat-intelligence blocking, and prompt-injection detection — the [[agentic-ai-security-cmm-d5-egress-network|D5 egress]] network leg.

## Governance: sponsors, lifecycle, access packages

The governance model treats agent identities like human identities:

- **Sponsors** are human users accountable for an agent identity's lifecycle and access decisions. When a sponsor leaves the organization, **sponsorship transfers automatically to their manager**, so a human is always accountable; Lifecycle Workflows notify cosponsors and managers of impending changes. This is the concrete mechanism behind the [[agentic-ai-security-cmm-d9-operations|D9]] owner-accountability and decommission requirements.
- **Access packages** assign resource access (security-group membership, application OAuth / Graph permissions) to agent identities, **time-bound** with an expiry. Three request pathways exist: the agent self-requests programmatically, the sponsor requests on its behalf (human oversight), or an admin assigns directly. As expiry approaches, the sponsor is notified and either requests an extension (triggering re-approval) or lets access lapse.
- **Inventory**: a complete inventory of agent identities is maintained through centralized discovery in the Entra admin center and Microsoft Graph, tracking each agent from registration through decommissioning.

## Agent sprawl

The security doc names **agent sprawl** — the uncontrolled expansion of agents without visibility, management, or lifecycle controls — as the governance challenge that motivates the model. It emerges from business units creating agents without IT oversight (shadow AI), temporary agents that persist, and permissions that are never reviewed. This is Microsoft's framing of the same proliferation risk the wiki tracks as [[shadow-automation|shadow automation]], and the sponsor-plus-blueprint model is the proposed control.

## Licensing

The security features require Microsoft 365 E5, or these Entra SKUs individually: **Conditional Access for agents** needs Entra ID P1; **ID Protection for agents** needs Entra ID P2; **ID Governance for agents** needs Entra ID P1; **network controls** need Entra Internet Access (in the Entra Suite). Agent ID itself is available to all Entra customers; Agent 365 cross-M365 operation needs a per-user Agent 365 license. For an E5 incumbent the security and governance features carry near-zero incremental licensing — consistent with the [[agentic-ai-security-cmm-d2-identity|D2]] cost finding.

## Cross-product reach

Agent identities are provisioned across Microsoft's agent surfaces: **Microsoft Foundry** (auto-provisions a project identity; publishing creates a dedicated identity used for MCP/A2A auth), **Azure App Service / Functions**, **Microsoft Copilot Studio** (auto-assign, preview — the basis of the [[azure-rag-chatbot-security-profile|RAG chatbot profile]]'s identity plane), **Teams Developer Portal**, and **Agent 365**.

## Significance

These docs move the Entra Agent ID identity model from vendor-announcement framing to documented governance mechanics. The load-bearing additions for the wiki: the **sponsor model with automatic manager-transfer** (a concrete D9 accountability control no other vendor documents as cleanly), **blueprint-level inheritance with single-operation class disable** (a real lever against agent sprawl), and the **explicit licensing tiers** that confirm the near-zero-incremental-cost finding for an E5 incumbent. The recurring caveat from the [[azure-rag-chatbot-security-profile|RAG profile]] stands: the Copilot Studio path to Agent ID is still preview.
