---
type: concept
title: "AI Agent Management Platform (AMP)"
created: 2026-05-01
updated: 2026-05-01
tags:
  - concepts
  - amp
  - guardian-agent
  - agent-management
  - vendor-category
status: developing
no_public_url: "Gartner-coined category; canonical source is the paywalled Gartner Market Guide (held as an internal source)"
complexity: intermediate
domain: agent-management
aliases:
  - "AMP"
  - "AI Agent Management Platform"
related:
  - "[[guardian-agents-market-guide]]"
  - "[[guardian-agent]]"
  - "[[ai-trism]]"
  - "[[agent-catalog]]"
sources:
  - "[[.raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md]]"
coined_by:
  - "[[gartner]]"
---

# AI Agent Management Platform (AMP)

An **AI Agent Management Platform (AMP)** is a unified interface to securely manage, monitor, govern, acquire, organize, and generate analytics on AI agents and their supporting toolsets. Per Gartner Note 2, AMPs are "the most valuable real estate in AI" — a category of platform vendors competing to become the management layer for what Gartner argues is "the most important new piece of IT infrastructure in a generation."

A guardian agent capability is **an essential component of an AMP**. Without one, the AMP is an inventory and provisioning tool, not a governance tool.

## Components of an AMP

| Capability | What it does |
|---|---|
| **Agent acquisition / marketplace** | Discovery and procurement of agents (first-party, third-party, custom) |
| **Agent provisioning** | Deployment, configuration, lifecycle management |
| **Catalog and inventory** | The [[agent-catalog\|AI Agent Catalog]] — see that page for the canonical primitive |
| **Identity management** | Issuance and rotation of agent identities (workload, federated) |
| **Monitoring and observability** | Telemetry collection across the managed agents |
| **Policy enforcement** | Runtime gating of agent actions per organizational policy |
| **Governance and compliance** | Reporting, audit trails, regulatory alignment |
| **Analytics** | Performance, cost, and behavior reporting |
| **Guardian agent integration** | Real-time supervision of managed agents |

## Vendor landscape (Gartner-named, 2026)

AMP is a broad category spanning hyperscalers, AI data platforms, and AI orchestration startups:

| Tier | Vendors |
|---|---|
| **Hyperscalers** | Microsoft (Agent 365 + Entra + Defender + Purview); AWS (Bedrock Agents + Bedrock Guardrails); Google Cloud (Vertex AI Agent Builder + guardrails) |
| **AI data platforms** | Databricks (Mosaic AI Gateway & Guardrails); Snowflake; IBM (Watsonx) |
| **AI orchestration startups** | Airia; AgilePoint; Salesforce Agentforce |

Note: most of these vendors offer **first-party guardian agents** as part of their AMP. Per Gartner, this creates two structural issues:

1. **Lock-in risk** — the AMP vendor controls model access, building tools, AND the management layer
2. **Cross-vendor blind spots** — first-party AMPs cannot fully manage third-party agents created outside their environment

> [!warning] Gartner's specific concern
> "Gartner has yet to see evidence that hyperscaler AI governance solutions can fully manage third-party agents created outside of the environment in a comparable way to first-party agents. It is also unlikely that rival AI providers will want to outsource management of their AI agents to their competition, leading to gaps in coverage provided."

This is why Gartner argues for an **independent guardian-agent layer** alongside the AMP — see [[guardian-agent|Guardian Agent]] for the structural argument.

## Microsoft Agent 365: the canonical AMP

Microsoft Agent 365 (currently in Preview as of late 2026) is the most concrete AMP example today. The promise:

- Register your agent (first-party or third-party) with Microsoft Entra Agent ID
- Inherit the security and governance controls of the wider Microsoft platform: Purview (data governance), Entra (identity), Defender (threat detection)
- Get a unified management layer

The lock-in counterpoint: rival AI providers won't outsource agent management to Microsoft. Multi-vendor enterprises end up running AMP-A (Microsoft) and AMP-B (e.g., Google Vertex) and AMP-C (specialized) in parallel, each with its own management surface.

## Sufficiency of an AMP against an independent layer

| Situation | AMP-only is adequate | Independent guardian layer is required |
|---|---|---|
| Single-cloud, single-vendor AI deployment | ✓ | ✗ |
| Multi-cloud agent deployment | ✗ | ✓ (cross-cloud policy enforcement) |
| Multiple AI vendors (Microsoft + Anthropic + Google) | ✗ | ✓ (cross-vendor identity unification) |
| Regulated industry with cross-platform information governance requirements | ✗ | ✓ (independent policy enforcement) |
| Small, exploratory deployment | ✓ | ✗ (overkill) |

Per Gartner's prediction: by 2029, independent guardian agents will eliminate the need for ~50% of incumbent security systems used to protect AI agent activities, in 70%+ of organizations. The AMP category survives but becomes a managed-services layer that consumes (rather than competes with) the independent guardian layer.

## Placement in the RA

The wiki's [[agentic-ai-security-reference-architecture|six-plane RA]] does not name "AMP" as a plane. Instead, AMPs span the planes:

- AMP **identity-plane** capabilities = Microsoft Entra Agent ID, Okta for AI Agents
- AMP **observability-plane** capabilities = native telemetry, posture management
- AMP **runtime-plane** capabilities = first-party guardrails

This is consistent with Gartner's view: AMPs are vendor packages that deliver subsets of the planes. The architecture's value is plane-by-plane evaluation; the AMP is one possible packaging of those planes.

## See Also

- [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents (Feb 2026)]] — primary source (Note 2 + Note 7)
- [[guardian-agent|Guardian Agent]] — AMPs include first-party GAs; independent GAs sit alongside
- [[ai-trism|Gartner AI TRiSM]] — the framework AMPs operate within
- [[agent-catalog|AI Agent Catalog]] — a primitive every AMP must include
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — plane-by-plane decomposition that AMPs partially implement
