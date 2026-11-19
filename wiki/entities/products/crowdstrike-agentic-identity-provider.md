---
type: entity
entity_type: product
vendor: "[[crowdstrike|CrowdStrike]]"
title: "CrowdStrike Agentic Identity Provider"
address: c-000348
created: 2026-09-10
updated: 2026-10-06
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
  - "[[microsoft-agent-365]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[agentcordon]]"
sources:
  - "https://www.crowdstrike.com/en-us/press-releases/crowdstrike-agentic-identity-provider-foundation-for-ai-agent-identity-security/"
  - "https://www.crowdstrike.com/en-us/blog/crowdstrike-announces-agentic-identity-provider/"
  - "https://siliconangle.com/2026/09/02/crowdstrike-gives-ai-agents-an-identity-provider-parallel-soc-investigations-and-package-blocking/"
  - "https://www.msspalert.com/brief/crowdstrike-introduces-agentic-identity-provider-for-ai-agents"
verified: 2026-10-06
verified_against: []
verified_findings: 1
verified_note: "Diff-scoped 2026-10-06 to §Positioning alongside existing identity providers: opened the archived blog capture .raw/articles/crowdstrike-agentic-identity-provider-2026-09-18.md (the page's sources: list it by URL only) and the live press release; 1 medium fixed (Okta/Entra AWS items re-attributed to the Extending Modern Privileged Access section), 1 low fixed ('complementary' positioning in neither source, replaced with the press release's 'authority for AI agents'). Open low: the Okta and Entra links target agent products, not the AWS privileged-access integration. Rest of page not re-verified."
---

# CrowdStrike Agentic Identity Provider

**Sources:** [Press release](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-agentic-identity-provider-foundation-for-ai-agent-identity-security/) · [Product blog](https://www.crowdstrike.com/en-us/blog/crowdstrike-announces-agentic-identity-provider/) · [SiliconANGLE coverage](https://siliconangle.com/2026/09/02/crowdstrike-gives-ai-agents-an-identity-provider-parallel-soc-investigations-and-package-blocking/)

[[crowdstrike|CrowdStrike]]'s Agentic Identity Provider (Agentic IdP) is a purpose-built identity and access system for AI agents, announced September 2, 2026 at Fal.Con 2026, one day after [[falcon-guardian|Falcon Guardian]]. It is a named product in CrowdStrike's Falcon Next-Gen Identity Security platform and is unrelated to "Agentic Identity Provider" used as a generic category label for other products in this space, such as the one [[agentcordon|AgentCordon]] carries; this page covers CrowdStrike's specific, branded offering.

## Core mechanisms

Every agent [[falcon-guardian|Falcon Guardian]] discovers is registered automatically in a centralized directory and issued a cryptographically verifiable identity — a design CrowdStrike states cannot be spoofed or shared between agents. The Agentic IdP then brokers access through short-lived, task-scoped tokens rather than standing credentials: each token is scoped to the minimum access a task requires and held for the minimum time required, eliminating the model of a persistent credential sitting in an agent's context. This is CrowdStrike's implementation of the same problem the [[credential-proxy-pattern|credential proxy pattern]] addresses — an agent never holds a long-lived secret, only a broker-issued, task-bound token. Every action an agent takes is cryptographically bound to the human user who delegated it or the workload it acts on behalf of, so activity traces back to an accountable party, and each agent identity carries enriched risk context, including compromise indicators and privilege level.

## Relationship to Falcon Guardian and Continuous Identity

The Agentic IdP answers "who are you" for an agent; CrowdStrike's separately announced Continuous Identity for AI Agents (June 15, 2026, at Identiverse) answers "what should you access," layering real-time, risk-aware authorization on top of the identity the IdP establishes. [[falcon-guardian|Falcon Guardian]] triggers the registration step by discovering an agent; the Agentic IdP then issues and manages its identity. Together the three products span the [[agent-identity-architecture|agent identity architecture]]'s discovery, identity, and authorization layers, and the identity-issuance and standing-privilege-elimination design bears directly on the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity and Authorization]] domain and the [[agentic-ai-security-cmm-2026|CMM]]'s broader identity criteria, including its treatment of [[non-human-identity|non-human identity]] lifecycle.

## Positioning alongside existing identity providers

CrowdStrike frames the product as necessary because traditional identity providers represent agents as service accounts, API keys and workload identities, which the press release calls "static models never meant for identities that take autonomous action on behalf of users and delegate to sub-agents." The press release positions the Agentic IdP as "the authority for AI agents."[^idp-pr] The [[okta-for-ai-agents|Okta]] and [[microsoft-entra-agent-id|Microsoft Entra]] items in the announcement blog belong to its separate privileged-access section, "Extending Modern Privileged Access Everywhere Work Happens": CrowdStrike announced modern privileged access for AWS through Okta earlier in 2026, and the blog extends that support to AWS environments that use Microsoft Entra.[^idp-blog]

## Availability

The announcement carries CrowdStrike's standing caveat that "any unreleased services or features referenced here are still in development and subject to change," and names no shipped component of the Agentic IdP that the caveat exempts. No general availability date or pricing has been disclosed, and no technical detail on the underlying cryptographic mechanisms — token format, key derivation, or revocation model — has been published.

## Notable statements

Scott Kriz, General Manager of Continuous Identity at CrowdStrike: "Securing AI agents demands solutions built for how they operate. Traditional identity providers break the moment an agent acts on its own. Agentic IdP is the identity provider for AI agents."

[^idp-blog]: [CrowdStrike — CrowdStrike Announces Agentic Identity Provider](https://www.crowdstrike.com/en-us/blog/crowdstrike-announces-agentic-identity-provider/), September 2, 2026, cloud infrastructure item under "Extending Modern Privileged Access Everywhere Work Happens".

[^idp-pr]: [CrowdStrike — Agentic Identity Provider press release](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-agentic-identity-provider-foundation-for-ai-agent-identity-security/), September 2, 2026.
