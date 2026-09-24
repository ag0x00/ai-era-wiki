---
type: concept
title: "AI Agent Catalog"
created: 2026-05-01
updated: 2026-09-23
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
  - "[[cyera-agent-guardian-release]]"
  - "[[falcon-guardian]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[agentdesktop]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
sources:
  - "[[.raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
---

# AI Agent Catalog

The **AI agent catalog** is a mandatory primitive for any [[guardian-agent|guardian-agent]] deployment per Gartner. It inventories all AI agents — registered, unregistered, official, custom, third-party, shadow, or rogue — within an organization's network, scores their risk, tracks that score over time, and stores the result as an **agent card**.

Governing, monitoring, or enforcing policy on an agent requires enumerating it first, because none of those functions applies to an agent nobody has recorded. That makes the catalog the agent-era instance of the asset inventory and ownership mapping the [[nist-ai-rmf|NIST AI RMF]] GOVERN function requires: GOVERN expects an organization to maintain an inventory of its AI systems with an attributed owner for each, and the owner-mapping field of an agent card makes that GOVERN claim auditable per agent.

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

Gartner calls this metadata bundle an **agent card**, analogous to a SaaS app's profile in a CASB inventory but for agents.

## Discovery: registered + unregistered + shadow + rogue

The catalog must enumerate all four populations:

| Population | How they're discovered |
|---|---|
| **Registered** | The agent self-registers with the IAM / agent platform on creation |
| **Unregistered** | Discovered via network telemetry, identity provider observation, or platform-API enumeration; backfilled into the catalog |
| **Shadow** | [[shadow-ai\|Shadow AI]] / [[shadow-automation\|shadow automation]] — agents created outside sanctioned platforms (developer-side, BYOAI, ungoverned IDE extensions and desktop agent harnesses) |
| **Rogue** | Agents whose behavior diverges from declared intent or whose identity has been compromised |

**Catalog discipline is a precondition for governance.** Its absence is the entry-level failure mode for AI agent security programs.

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
| Okta for AI Agents | Cross-platform agent identity registry (GA 2026-04-29) |
| Astrix Security, Aembit | Non-human identity discovery + governance |
| [[falcon-guardian\|CrowdStrike Falcon Guardian]] | Endpoint-resident discovery of known and shadow agents on Windows, macOS and Linux, recording deployment source, user identity and security status |
| [[ping-enterprise-personal-agent-access\|Ping Enterprise Personal Agent Access]] | Personal-agent discovery including shadow AI, each session linked to the user and the device that started it |
| [[agentdesktop\|agentdesktop]] (Solo.io, Apache 2.0) | Desktop inventory of agent harnesses and the MCP servers registered in each one's configuration |
| Wiz AI-SPM, Palo Alto Prisma AIRS | AI asset inventory across cloud environments |
| Internal CMDB extensions | Many enterprises extend their CMDB to track AI agents |
| Cyera Agent Guardian | Cloud, SaaS, endpoint and Shadow AI agent discovery (vendor-stated; no published coverage figure) |

No single tool covers all four populations comprehensively. Gartner's prediction is that **independent guardian-agent vendors will provide unified catalog discovery as a category in 2027–2028**, displacing the current hyperscaler-specific tools for cross-vendor enterprises.

Cyera is marketing that unified position ahead of the predicted window, and from outside the population the prediction covers: the same Market Guide places this data security posture management incumbent in none of its six vendor segments, naming it under information governance instead ([[guardian-agents-market-guide|Guardian Agents Market Guide]]). An incumbent outside the guardian-agent category reaching for the unified catalog is a stronger timing signal than an independent vendor doing so, because it prices the category as worth entering before Gartner's window opens. Cyera's Agent Guardian release, fetched 2026-08-31, claims a live inventory spanning cloud platforms, SaaS tools, endpoints and Shadow AI in one product, alongside an endpoint agent covering desktop harnesses ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). The claim is a vendor self-report with no published coverage figure and no third-party evaluation, so it bears on when the category forms rather than on whether any tool has closed the four-population gap.

Three more suppliers reached for the same position in the first week of September 2026, from three different incumbencies: an EDR vendor at the endpoint ([[falcon-guardian|Falcon Guardian]]), an IAM vendor at the identity layer ([[ping-enterprise-personal-agent-access|Ping Enterprise Personal Agent Access]]), and an infrastructure vendor shipping the desktop inventory as open source ([[agentdesktop|agentdesktop]]). None of the three is an independent guardian-agent vendor of the kind the prediction names, and none publishes a coverage figure across the four populations, so the timing signal strengthens and the four-population gap stands.

## Placement in this wiki

| Wiki page | Connection |
|---|---|
| [[non-human-identity\|Non-Human Identity (NHI)]] | Catalog is the inventory layer for NHI; agent cards = NHI metadata |
| [[agent-identity-architecture\|AI Agent Identity Architecture]] | The architectural reference for how catalog identities are assigned and used |
| [[guardian-agent\|Guardian Agent]] | Catalog is mandatory feature category 1 (visibility and traceability) |
| [[agentic-ai-security-cmm-d2-identity\|CMM D2: Identity and Authorization]] | The catalog evidences D2-INVENTORY at L2, D2-OWNER at L3 and D2-REGISTRY at L5 |
| [[shadow-ai\|Shadow AI]], [[shadow-automation\|Shadow Automation]] | Catalog discovery surfaces both |

## CMM D2 criteria the catalog evidences

[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]], the identity domain of the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]], grades one deployment at a time, and the catalog is the artifact behind the following D2 criteria:

- **D2-INVENTORY, at L2.** An inventory records each agent in the deployment and every non-human identity it holds, and each identity's entry names its agent. An inventory kept by hand meets it.
- **D2-OWNER, at L3.** Every agent and every non-human identity in the deployment names a human owner whom the personnel record shows as current. The bar is every one, with no percentage threshold, because the deployment's own design enumerates its identities and one unowned identity inside it is the failure the criterion grades.
- **D2-COUPLING, at L3, and D2-BASELINE, at L4.** The inventory classes each credential as coupled or decoupled, and each non-human identity carries a behavioral baseline with a detection.
- **D2-REGISTRY and D2-DISCOVER, at L5.** A registry the deploy pipeline writes through an API holds each agent's identity graph, and scheduled discovery reports every agent the registry does not hold until each is registered or removed.
- **D2-FEDERATE and D2-FEDERATE-RECONCILE, at L5+.** Agent identities federate across identity platforms from different vendors and reconcile into one identity graph, the problem the first catalog gap below records as unsolved.

D2's levels grade no risk-score methodology and no coverage of all four populations; both are catalog capabilities the [[guardian-agents-market-guide|Market Guide]] describes.

## Open issues

> [!gap] Catalog gaps
> 1. **Cross-vendor agent identity reconciliation.** No standard yet for resolving "is the Microsoft Entra Agent ID for agent-X the same agent as the Okta agent ID for agent-X?" Identity federation across vendors is unsolved.
> 2. **Shadow agent fingerprinting.** Without declared identity, fingerprinting depends on behavioral metadata (model used, tools called, output style). Gartner predicts metadata fingerprinting becomes the fallback identity in 2026–2027.
> 3. **Skill / MCP-server inventory.** Catalog of skills and MCP servers the agents consume is a separate (but related) inventory problem. See [[mcp-security|MCP Security]].

## See Also

- [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents (Feb 2026)]] — primary source (Mandatory Features category: AI agent catalog)
- [[guardian-agent|Guardian Agent]] — catalog is mandatory feature category 1
- [[non-human-identity|Non-Human Identity (NHI)]] — the inventory layer
- [[agent-identity-architecture|AI Agent Identity Architecture]] — how identities are assigned
- [[shadow-ai|Shadow AI]] / [[shadow-automation|Shadow Automation]] — agents the catalog must surface

<!-- sources:auto -->
## Sources

- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
<!-- /sources -->
