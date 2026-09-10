---
type: entity
entity_type: product
vendor: "[[crowdstrike|CrowdStrike]]"
title: "CrowdStrike Agentic Identity Provider"
address: c-000348
created: 2026-09-10
updated: 2026-09-10
tags:
  - products
  - identity
  - nhi
  - credential-proxy
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
homepage: "https://www.crowdstrike.com/en-us/press-releases/crowdstrike-agentic-identity-provider-foundation-for-ai-agent-identity-security/"
ga_date: ""
related:
  - "[[agent-identity-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[non-human-identity]]"
  - "[[credential-proxy-pattern]]"
  - "[[crowdstrike]]"
  - "[[falcon-guardian]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[agentcordon]]"
sources:
  - "https://www.crowdstrike.com/en-us/press-releases/crowdstrike-agentic-identity-provider-foundation-for-ai-agent-identity-security/"
  - "https://www.crowdstrike.com/en-us/blog/crowdstrike-announces-agentic-identity-provider/"
  - "https://siliconangle.com/2026/09/02/crowdstrike-gives-ai-agents-an-identity-provider-parallel-soc-investigations-and-package-blocking/"
  - "https://www.msspalert.com/brief/crowdstrike-introduces-agentic-identity-provider-for-ai-agents"
---

# CrowdStrike Agentic Identity Provider

**Sources:** [Press release](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-agentic-identity-provider-foundation-for-ai-agent-identity-security/) · [Product blog](https://www.crowdstrike.com/en-us/blog/crowdstrike-announces-agentic-identity-provider/) · [SiliconANGLE coverage](https://siliconangle.com/2026/09/02/crowdstrike-gives-ai-agents-an-identity-provider-parallel-soc-investigations-and-package-blocking/)

[[crowdstrike|CrowdStrike]]'s Agentic Identity Provider (Agentic IdP) is a purpose-built identity and access system for AI agents, announced September 2, 2026 at Fal.Con 2026, one day after [[falcon-guardian|Falcon Guardian]]. It is a named product in CrowdStrike's Falcon Next-Gen Identity Security platform and is unrelated to "Agentic Identity Provider" used as a generic category label for other products in this space, such as the one [[agentcordon|AgentCordon]] carries; this page covers CrowdStrike's specific, branded offering.

## Core mechanisms

Every agent [[falcon-guardian|Falcon Guardian]] discovers is registered automatically in a centralized directory and issued a cryptographically verifiable identity — a design CrowdStrike states cannot be spoofed or shared between agents. The Agentic IdP then brokers access through short-lived, task-scoped tokens rather than standing credentials: each token is scoped to the minimum access a task requires and held for the minimum time required, eliminating the model of a persistent credential sitting in an agent's context. This is CrowdStrike's implementation of the same problem the [[credential-proxy-pattern|credential proxy pattern]] addresses — an agent never holds a long-lived secret, only a broker-issued, task-bound token. Every action an agent takes is cryptographically bound to the human user who delegated it or the workload it acts on behalf of, so activity traces back to an accountable party, and each agent identity carries enriched risk context, including compromise indicators and privilege level.

## Relationship to Falcon Guardian and Continuous Identity

The Agentic IdP answers "who are you" for an agent; CrowdStrike's separately announced Continuous Identity for AI Agents (June 15, 2026, at Identiverse) answers "what should you access," layering real-time, risk-aware authorization on top of the identity the IdP establishes. [[falcon-guardian|Falcon Guardian]] triggers the registration step by discovering an agent; the Agentic IdP then issues and manages its identity. Together the three products span the [[agent-identity-architecture|agent identity architecture]]'s discovery, identity, and authorization layers, and the identity-issuance and standing-privilege-elimination design bears directly on the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity and Authorization]] domain and the [[agentic-ai-security-cmm-2026|CMM]]'s broader identity criteria, including its treatment of [[non-human-identity|non-human identity]] lifecycle.

## Positioning alongside existing identity providers

CrowdStrike frames the product as necessary because traditional identity providers, built for human authentication and fixed service-account and workload-identity models, do not extend cleanly to agents that delegate tasks to sub-agents and act without a human present at each step. CrowdStrike positions the Agentic IdP as complementary to existing identity infrastructure. The announcement blog places both of its named coexistence points in AWS: CrowdStrike shipped modern privileged access for AWS through [[okta-for-ai-agents|Okta]] earlier in 2026, and now extends that AWS privileged-access support to tenants using [[microsoft-entra-agent-id|Microsoft Entra]].

## Availability

The announcement carries CrowdStrike's standing caveat that "any unreleased services or features referenced here are still in development and subject to change," and names no shipped component of the Agentic IdP that the caveat exempts. No general availability date or pricing has been disclosed, and no technical detail on the underlying cryptographic mechanisms — token format, key derivation, or revocation model — has been published.

## Notable statements

Scott Kriz, General Manager of Continuous Identity at CrowdStrike: "Securing AI agents demands solutions built for how they operate. Traditional identity providers break the moment an agent acts on its own. Agentic IdP is the identity provider for AI agents."
