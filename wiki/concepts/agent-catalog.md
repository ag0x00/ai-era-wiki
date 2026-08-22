---
type: concept
title: "AI Agent Catalog"
created: 2026-05-01
updated: 2026-06-23
tags:
  - concepts
  - agent-catalog
  - guardian-agent
  - inventory
  - identity
  - procurement
status: developing
complexity: basic
domain: agent-management
aliases:
  - "Agent Catalog"
  - "AI Agent Inventory"
  - "Agent Registry"
  - "Agentic AI Catalog"
related:
  - "[[guardian-agents-market-guide]]"
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[guardian-agent]]"
  - "[[ai-agent-layered-council]]"
  - "[[non-human-identity]]"
  - "[[agent-identity-architecture]]"
  - "[[shadow-ai]]"
  - "[[shadow-automation]]"
  - "[[nist-ai-rmf]]"
sources:
  - "[[.raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
---

# AI Agent Catalog

The **AI agent catalog** is a mandatory primitive for any [[guardian-agent|guardian-agent]] deployment per Gartner. It inventories all AI agents — registered, unregistered, official, custom, third-party, shadow, or rogue — within an organization's network. It scores risks and tracks them over time. It stores **agent cards** as metadata.

The catalog is the foundation everything else builds on: you cannot govern, monitor, or enforce policy on agents you cannot enumerate. It is the agent-era instance of the asset inventory and ownership mapping the [[nist-ai-rmf|NIST AI RMF]] GOVERN function requires: GOVERN expects an organization to maintain an inventory of its AI systems with an attributed owner for each, and the owner-mapping field of an agent card is what makes that GOVERN claim auditable per agent.

## Two roles for the catalog

The catalog plays a **dual role** in the wiki — independently arrived at by two Gartner publications:

| Lens | Source | The catalog is... |
|---|---|---|
| **Security inventory primitive** | [[guardian-agents-market-guide\|Market Guide for Guardian Agents (Feb 2026)]] | The mandatory enumeration substrate for [[guardian-agent\|guardian agents]] — visibility, risk scoring, runtime policy attribution |
| **Procurement coordination primitive** | [[scaling-agentic-ai-cios-talk\|Scaling Agentic AI talk (May 2026)]] | The single source of truth that [[scaling-agentic-ai-cios-talk\|procurement]] uses to vet new agent purchases against the existing stack, prevent duplication, and insert IT requirements at "zero day" of any new RFP/RFI |

The two roles share the same artifact (the agent card) but use it differently. The procurement role makes the catalog the **chokepoint where new agentic services enter the enterprise** — a posture that complements the runtime-enforcement role from the Market Guide. Both roles are required for the [[ai-agent-layered-council|AI Agent Layered Council]] to function: Procurement uses the catalog to coordinate purchases, the [[guardian-agent|guardian agent]] / [[oversight-layer|oversight layer]] uses it to enforce policy at runtime.

## Required catalog contents

Per Gartner's mandatory feature definition:

| Field class | Examples |
|---|---|
| **Identity** | Unique agent ID; cryptographic identity ([[spiffe\|SPIFFE]] SVID, Okta agent ID, Microsoft Entra Agent ID); publisher signature |
| **Capabilities** | What tools the agent can call; what data it can access; what autonomy tier it operates at |
| **Interaction endpoints** | APIs, gateways, MCP servers it consumes or exposes |
| **Authentication requirements** | What credentials, scopes, or tokens it needs to operate |
| **Lineage** | Who created it, when, from what template; deployment history |
| **Risk score** | Computed from capabilities × data access × autonomy × usage history |
| **Owner mapping** | Human owner (responsible party) + machine owner (parent agent or platform) |
| **Status** | Active, deprecated, sandboxed, blocked, decommissioned |

This metadata bundle is what Gartner calls an **agent card** — analogous to a SaaS app's profile in a CASB inventory, but for agents.

## Discovery: registered + unregistered + shadow + rogue

The catalog must enumerate all four populations:

| Population | How they're discovered |
|---|---|
| **Registered** | The agent self-registers with the IAM / agent platform on creation |
| **Unregistered** | Discovered via network telemetry, identity provider observation, or platform-API enumeration; backfilled into the catalog |
| **Shadow** | [[shadow-ai\|Shadow AI]] / [[shadow-automation\|shadow automation]] — agents created outside sanctioned platforms (developer-side, BYOAI, ungoverned IDE extensions) |
| **Rogue** | Agents whose behavior diverges from declared intent or whose identity has been compromised |

**No catalog discipline = no governance.** This is the entry-level failure mode for AI agent security programs.

## Risk scoring

The catalog must score risk per agent and track over time. Inputs:

- Capability surface (tool count, sensitivity of accessible APIs)
- Data access scope (which classification levels, which sources)
- Autonomy tier (per [[least-agency-principle|Least Agency Principle]]: auto / notify / confirm / block)
- Usage frequency and patterns
- Behavioral baseline drift (agent behavioral monitoring signal)
- Recent incidents involving this agent or its dependencies (skill registry, MCP server, model)
- Owner status (active human owner vs. orphaned)

Risk scores feed back into runtime decisions: high-risk agents get tighter Operative oversight; low-risk agents get a lighter touch.

## Implementations (2026)

| Vendor / Tool | Coverage |
|---|---|
| Microsoft Agent 365 | First-party + third-party agents registered with Entra Agent ID |
| Okta for AI Agents | Cross-platform agent identity registry (GA April 30, 2026) |
| Astrix Security, Aembit | Non-human identity discovery + governance |
| CrowdStrike, SentinelOne | Endpoint-level agent discovery via process telemetry |
| Wiz AI-SPM, Palo Alto Prisma AIRS | AI asset inventory across cloud environments |
| Internal CMDB extensions | Many enterprises extend their CMDB to track AI agents |

No single tool covers all four populations comprehensively. Gartner's prediction is that **independent guardian-agent vendors will provide unified catalog discovery as a category in 2027–2028**, displacing the current hyperscaler-specific tools for cross-vendor enterprises.

## Placement in this wiki

| Wiki page | Connection |
|---|---|
| [[non-human-identity\|Non-Human Identity (NHI)]] | Catalog is the inventory layer for NHI; agent cards = NHI metadata |
| [[agent-identity-architecture\|AI Agent Identity Architecture]] | The architectural reference for how catalog identities are assigned and used |
| [[guardian-agent\|Guardian Agent]] | Catalog is mandatory feature category 1 (visibility and traceability) |
| [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] D2 Identity | Catalog L3+ requirement: comprehensive across all four populations |
| [[shadow-ai\|Shadow AI]], [[shadow-automation\|Shadow Automation]] | Catalog discovery surfaces both |

## CMM L3+ evidence requirements

For an organization to claim Level 3 on D2 Identity & Authorization in the [[agentic-ai-security-cmm-2026|CMM]], the catalog evidence must include:

1. **Comprehensive agent inventory** with documented coverage of all four populations (not just registered)
2. **Agent cards** for every cataloged agent with the field set above
3. **Risk-score methodology** documented and applied uniformly
4. **Catalog refresh cadence** (continuous discovery, not point-in-time)
5. **Owner attribution** for ≥95% of cataloged agents (orphan detection for the rest)

L4 adds: integration with runtime decisions (catalog signals feed Operative behavior); behavioral baselines per agent.

L5 adds: cross-vendor federation (catalog spans Microsoft + AWS + Google + on-prem with unified ID).

## Open issues

> [!gap] Catalog gaps
> 1. **Cross-vendor agent identity reconciliation.** No standard yet for resolving "is the Microsoft Entra Agent ID for agent-X the same agent as the Okta agent ID for agent-X?" Identity federation across vendors is unsolved.
> 2. **Shadow agent fingerprinting.** Without declared identity, fingerprinting depends on behavioral metadata (model used, tools called, output style). Gartner predicts metadata fingerprinting becomes the fallback identity in 2026–2027.
> 3. **Skill / MCP-server inventory.** Catalog of skills and MCP servers the agents consume is a separate (but related) inventory problem. See [[mcp-security|MCP Security]].

## See Also

- [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents (Feb 2026)]] — primary source (Mandatory Features → AI agent catalog)
- [[guardian-agent|Guardian Agent]] — catalog is mandatory feature category 1
- [[non-human-identity|Non-Human Identity (NHI)]] — the inventory layer
- [[agent-identity-architecture|AI Agent Identity Architecture]] — how identities are assigned
- [[shadow-ai|Shadow AI]] / [[shadow-automation|Shadow Automation]] — agents the catalog must surface

<!-- sources:auto -->
## Sources

- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
<!-- /sources -->
