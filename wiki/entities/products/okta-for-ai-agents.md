---
type: entity
entity_type: product
title: "Okta for AI Agents"
homepage: "https://www.okta.com"
created: 2026-05-03
updated: 2026-09-18
tags:
  - products
  - identity
  - nhi
  - agent-lifecycle
  - cots
status: developing
scope_axis:
  - sec-of-ai
vendor: "Okta"
ga_date: 2026-04-29
related:
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-agent-365]]"
  - "[[agent-identity-architecture]]"
  - "[[nhi-governance-for-agents]]"
  - "[[credential-proxy-pattern]]"
  - "[[spiffe]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[agentic-ai-security-ra-gaps]]"
sources:
  - "https://www.okta.com/resources/whitepaper/managing-ai-agents-with-okta/"
verified: 2026-09-18
verified_against: []
verified_findings: 1
verified_note: "Read against the Okta GA announcement of 2026-04-29 cited in the body and the vault's own correction history in wiki/meta/log-archive.md; no archived document opened. Removed the residual clause naming Auth0 as the only GA product in the line. Open: agent-catalog and CMM D2 still carry the retracted Early Access / FY27 status"
---

# Okta for AI Agents

**Sources:** [Okta (homepage)](https://www.okta.com) · [Managing AI Agents with Okta (whitepaper)](https://www.okta.com/resources/whitepaper/managing-ai-agents-with-okta/)

Okta for AI Agents is Okta's identity and lifecycle management platform for non-human identities (NHIs), specifically AI agents. It reached **general availability on 2026-04-29**.[^okta-ga] Auth0 for AI Agents, the developer-platform product in the same line, reached general availability in October 2025. Okta for AI Agents is one of the first purpose-built agentic-AI identity-governance products from a major IAM vendor.

## Function

Okta for AI Agents extends Okta's existing IAM platform (Universal Directory, Workflows, Lifecycle Management) to cover AI agents as first-class identity subjects alongside human users and service accounts.

**Core capabilities:**

| Capability | Description |
|---|---|
| **Agent enrollment and registration** | Agents are registered in Okta's Universal Directory with typed metadata: agent name, owning human principal, deployment context, tool scopes |
| **OAuth 2.1 delegation** | Agents obtain scoped, short-lived tokens via standard OAuth 2.1 flows; human-authorized delegation with scope constraints |
| **Lifecycle management** | Create, suspend, rotate, and revoke agent identities programmatically; integrates with provisioning workflows |
| **Agent discovery** | Okta Agent Discovery identifies and catalogs agents running in an environment, including shadow agents not explicitly registered |
| **NHI governance** | Extends Okta's NHI security controls (credential rotation, access reviews, orphaned-identity detection) to agent identities |
| **Policy integration** | Integrates with Okta's policy engine for adaptive MFA step-up, risk-based access decisions, and least-privilege enforcement |

## Relation to the RA Identity plane

In the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]], Okta for AI Agents is the **enterprise COTS primary** for two Identity plane capabilities:

- **Agent identity & lifecycle** — the core registration + lifecycle management capability
- **Non-Human Identity governance** — the NHI posture layer (orphan detection, access review, credential rotation)

The enterprise recommended stack in the RA pairs Okta for AI Agents with **CyberArk Conjur or Aembit** for NHI governance at organizations with existing PAM infrastructure.

Gap 7 of [[agentic-ai-security-ra-gaps|Agentic AI Security RA Gaps]] names three controls over a departing owner's agents — ownership attestation, orphaned-identity detection, and automatic revocation — and records that this product answers the first two and not the third: discovery and access review name a departing employee's agents, and the deactivation switch stays operator-invoked.

## Comparison with Microsoft Entra Agent ID

| Dimension | Okta for AI Agents | [[microsoft-entra-agent-id\|Microsoft Entra Agent ID]] |
|---|---|---|
| GA status | GA 2026-04-29 | GA May 1, 2026 (Agent 365 Registry) |
| Best fit | Organizations with Okta as primary IdP | Microsoft 365 / Azure-native organizations |
| Protocol basis | OAuth 2.1 | OAuth 2.1 + Microsoft identity platform extensions |
| Agent registry | Universal Directory + Agent Discovery | Agent 365 Registry (Graph API) |
| Lifecycle automation | Okta Workflows | Microsoft Entra lifecycle workflows |
| Shadow agent detection | Okta Agent Discovery | Agent 365 discovery scope |

Both products converged on the same fundamental architecture (scoped OAuth 2.1 tokens + lifecycle governance) in the same product cycle, suggesting convergence on what agent identity management requires — and both reached general availability within three days of each other, Okta on 2026-04-29 and Entra Agent ID on 2026-05-01.

Two further entrants arrived in September 2026 on the same architecture: [[crowdstrike-agentic-identity-provider|CrowdStrike's Agentic Identity Provider]], announced September 2 from the endpoint-security market and stated to be in development, and [[ping-enterprise-personal-agent-access|Ping's Enterprise Personal Agent Access]], announced September 1 by an established IAM vendor and stated to be available. CrowdStrike positions its product to work alongside Okta, citing the modern privileged access for AWS it shipped through Okta earlier in 2026.

## CMM positioning

In the [[agentic-ai-security-cmm-2026|CMM]], Okta for AI Agents is a D2 (Identity & Authorization) domain reference implementation. Organizations adopting it reach at minimum **L3 CMM** on the identity maturity track: per-agent identity (not shared service account), programmatic lifecycle management, and access reviews for agent credentials.

Okta for AI Agents' published integration patterns focus on Okta-as-IdP deployments. Guidance for federating Okta agent identities with [[spiffe|SPIFFE]]/SPIRE for workload-level identity at the infrastructure layer, or with third-party MCP servers via agent-scoped tokens, is not yet publicly documented.

## Notes

[^okta-ga]: Okta, [*Okta for AI Agents is now generally available*](https://www.okta.com/blog/ai/okta-for-ai-agents-general-availability/) (2026-04-29), retrieved 2026-09-18. Covers discovery, onboarding, protection and governance of agent identities across agent frameworks, clouds and SaaS environments; agent deactivation is described as an operator-invoked kill switch. Agent-to-agent delegation, an Agent Gateway, threat detection and human-in-the-loop controls are named as roadmap rather than GA.
