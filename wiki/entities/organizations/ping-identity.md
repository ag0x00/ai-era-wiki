---
type: entity
entity_type: organization
org_type: vendor
title: "Ping Identity"
address: c-000345
created: 2026-09-10
updated: 2026-09-10
tags:
  - entities
  - organizations
  - vendor
  - identity
  - nhi
  - iam
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
homepage: "https://www.pingidentity.com"
related:
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[agent-identity-architecture]]"
  - "[[non-human-identity]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[crowdstrike]]"
  - "[[falcon-guardian]]"
  - "[[crowdstrike-agentic-identity-provider]]"
sources:
  - "https://www.pingidentity.com"
  - "https://www.prnewswire.com/news-releases/ping-identity-launches-identity-for-ai-solution-to-power-innovation-and-trust-in-the-agent-economy-302606897.html"
  - "https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action"
  - "https://idtechwire.com/ping-identity-launches-identity-for-ai-to-secure-and-govern-autonomous-agent-economy/"
  - "https://www.prnewswire.com/news-releases/ping-identity-extends-runtime-identity-for-ai-agents-across-aws-google-cloud-and-cloudflare-302800889.html"
---

# Ping Identity

**Sources:** [Ping Identity (homepage)](https://www.pingidentity.com) · [Identity for AI launch (PR Newswire)](https://www.prnewswire.com/news-releases/ping-identity-launches-identity-for-ai-solution-to-power-innovation-and-trust-in-the-agent-economy-302606897.html) · [Enterprise Personal Agent Access announcement](https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action)

## Identity and role

Ping Identity is an enterprise identity and access management (IAM) vendor, supplying single sign-on, multi-factor authentication, and privileged access management to large organizations. Since late 2025 the company has extended that platform to govern [[non-human-identity|non-human identities]] held by autonomous AI agents, alongside comparable identity-for-agents work at [[okta-for-ai-agents|Okta]] and [[microsoft-entra-agent-id|Microsoft Entra]].

## Identity for AI platform

Ping launched **Identity for AI** on November 6, 2025, reaching general availability in early 2026.[^launch] The platform organizes agent-identity governance into five pillars: visibility into deployed agents, onboarding and management of agents and resources, authentication and authorization under least privilege, human oversight for accountability, and threat protection against adversarial AI activity. Its [[agent-identity-architecture|identity architecture]] exposes agent identity through programmatic interfaces — MCP, a CLI, and APIs — rather than through a human-facing console alone. Ping extended the platform's Runtime Identity component across AWS, Google Cloud, and Cloudflare in June 2026.[^runtime]

## Enterprise Personal Agent Access

On September 1, 2026, Ping shipped the platform's capability for personal AI agents as a named product: [[ping-enterprise-personal-agent-access|Enterprise Personal Agent Access]], delivered through the existing PingOne Privilege platform to secure Claude desktop and Claude Code sessions from discovery through action. That page carries the product's discovery, secretless-access, and runtime-control detail. The announcement landed the same week as [[crowdstrike|CrowdStrike]]'s [[falcon-guardian|Falcon Guardian]] (September 1) and [[crowdstrike-agentic-identity-provider|Agentic Identity Provider]] (September 2), a clustering that marks identity-layer security for autonomous agents as a distinct, contested vendor category by September 2026.

## Notable statements

Andre Durand, founder and CEO, on the September 2026 announcement: "The security challenge is establishing a common way to see and govern all of them. The question isn't whether an agent is intelligent enough to act, but whether the enterprise can see and control its action when it does."

On the November 2025 platform launch: "AI agents are changing how business gets done. With Identity for AI, we give organizations the guardrails to innovate responsibly and with confidence through enterprise-grade identity management."

[^launch]: [PR Newswire — Ping Identity Launches Identity for AI](https://www.prnewswire.com/news-releases/ping-identity-launches-identity-for-ai-solution-to-power-innovation-and-trust-in-the-agent-economy-302606897.html), November 6, 2025.
[^runtime]: [PR Newswire — Ping Identity Extends Runtime Identity for AI Agents Across AWS, Google Cloud, and Cloudflare](https://www.prnewswire.com/news-releases/ping-identity-extends-runtime-identity-for-ai-agents-across-aws-google-cloud-and-cloudflare-302800889.html), June 2026.
