---
type: concept
title: "AI Agent Catalog"
created: 2026-05-01
updated: 2026-09-29
tags:
  - concepts
  - agent-catalog
  - guardian-agent
  - inventory
  - identity
  - procurement
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
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
  - "https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf"
  - "https://learn.microsoft.com/en-us/microsoft-agent-365/overview"
  - "https://www.okta.com/blog/ai/okta-for-ai-agents-general-availability/"
  - "https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/"
  - "https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action"
  - "https://www.solo.io/blog/introducing-agentdesktop"
  - "https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents"
  - "https://www.paloaltonetworks.com/prisma/prisma-ai-runtime-security"
  - "https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store"
verified: 2026-09-29
verified_against:
  - ".raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md"
  - ".raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md"
  - ".raw/talks/scaling-agentic-ai-cios-2026-05-01.md"
verified_findings: 0
verified_note: "Gartner guide, CIO talk, Cyera archive, NIST AI 100-1 and live product pages checked in the agent-catalog review; D2 coordinate summary reconciled with the 2026-09-29 CMM redesign. Okta checked against its vault page only."
---

# AI Agent Catalog

The **AI agent catalog** is one of the mandatory features in Gartner's market definition of a [[guardian-agent|Guardian Agent]].[^mg-def] It inventories every AI agent in an organization's network, whether registered, unregistered, official, custom, third-party, shadow or rogue, scores risks and tracks them over time, and stores each agent's metadata as an **agent card**.

Governing, monitoring, or enforcing policy on an agent requires enumerating it first, because none of those functions applies to an agent nobody has recorded. That makes the catalog the agent-era instance of the AI system inventory the [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] GOVERN function calls for: GOVERN 1.6 expects mechanisms to inventory AI systems, GOVERN 2.1 expects documented roles and responsibilities, and the owner-mapping field of an agent card makes both auditable per agent.[^rmf]

## Two roles for the catalog

Two Gartner sources describe the catalog in two roles.

| Lens | Source | The catalog is... |
|---|---|---|
| **Security inventory primitive** | [[guardian-agents-market-guide\|Gartner Market Guide for Guardian Agents]] | The mandatory enumeration substrate for [[guardian-agent\|Guardian Agent]] functions: visibility and traceability, risk scoring, and discovery and interoperability across platforms |
| **Procurement coordination primitive** | [[scaling-agentic-ai-cios-talk\|Scaling Agentic AI: A Leadership Guide for CIOs]] | The centralized catalog procurement vets new requests against |

The procurement role uses the catalog to do three things:[^talk]

- Vet each new agent request against the existing catalog.
- Prevent duplicate purchases.
- Insert the requirements for safe and responsible agentic AI use "almost at zero day" of a new procurement request, ahead of the RFP or RFI.

The two roles share one artifact, the agent card, and use it differently. Read with the Market Guide, the procurement role makes the catalog the point where new agentic services are vetted before they enter the enterprise, which complements the runtime-enforcement role. The [[ai-agent-layered-council|AI Agent Layered Council]] needs both roles: Procurement uses the catalog to coordinate purchases, and the [[guardian-agent|Guardian Agent]] / [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] uses it to enforce policy at runtime.

## Required catalog contents

The Market Guide's catalog definition names identity, capabilities, interaction endpoints and authentication requirements, and it assigns risk scoring to the catalog. Lineage and owner mapping come from its separate ownership-mapping feature, and neither feature names a status field.[^mg-def]

| Field class | Examples |
|---|---|
| **Identity** | Unique agent ID, cryptographic identity ([[spiffe\|SPIFFE / SPIRE]] SVID, Okta agent ID, [[microsoft-entra-agent-id\|Microsoft Entra Agent ID]]) and publisher signature |
| **Capabilities** | Callable tools, accessible data and autonomy tier |
| **Interaction endpoints** | APIs, gateways, MCP servers it consumes or exposes |
| **Authentication requirements** | What credentials, scopes, or tokens it needs to operate |
| **Lineage** | Who created it, when, from what template; deployment history |
| **Risk score** | Assessed from capabilities, data access, autonomy, and observed use; the scoring method is deployment-specific |
| **Owner mapping** | Human owner (responsible party) + machine owner (parent agent or platform) |
| **Status** | Active, deprecated, sandboxed, blocked, decommissioned |

Gartner calls this metadata bundle an **agent card**. It is analogous to a SaaS app's profile in a CASB inventory, applied to agents.

## Discovery states and risk labels

Gartner names registered, unregistered, shadow, and rogue agents as catalog concerns. These labels overlap: an unregistered agent may be shadow, and a registered agent may become rogue. Record discovery source and current status separately.

| Label | How to identify it |
|---|---|
| **Registered** | Compare the platform or identity-provider registry with the deployment inventory. |
| **Unregistered** | Find an agent in endpoint, network, or platform telemetry that the registry omits. |
| **Shadow** | Find an unsanctioned agent in developer configurations, endpoints, or procurement records; see [[shadow-ai\|Shadow AI]] and [[shadow-automation\|shadow automation]]. |
| **Rogue** | Investigate behavior that diverges from declared intent or evidence that its identity was compromised. |

The Market Guide lists the catalog as a mandatory feature, so a program without one lacks the enumeration the other guardian-agent functions depend on.[^mg-def]

## Risk scoring

The catalog must score risk per agent and track the score over time.[^mg-def] The inputs a score can draw on include:

- Capability surface (tool count, sensitivity of accessible APIs)
- Data access scope (which classification levels, which sources)
- Autonomy tier (per [[least-agency-principle|Least Agency Principle]]: auto / notify / confirm / block)
- Usage frequency and patterns
- Behavioral baseline drift (agent behavioral monitoring signal)
- Recent incidents involving this agent or its dependencies (skill registry, MCP server, model)
- Owner status (active human owner vs. orphaned)

Risk scores feed back into runtime decisions: high-risk agents get tighter runtime oversight, and low-risk agents get lighter oversight.

## Implementations

None of the tools below is documented, in its cited source, as covering all four populations. The broadest claim, Cyera's live inventory of every agent across cloud, SaaS, endpoint and Shadow AI, gives no coverage figure.

| Vendor / Tool | Coverage |
|---|---|
| [[microsoft-agent-365\|Microsoft Agent 365]] | A single, centralized registry of an organization's agents, including partner agents deployed from the Microsoft 365 admin center[^ms] |
| [[okta-for-ai-agents\|Okta for AI Agents]] | Cross-platform agent identity registry, generally available since April 2026[^okta] |
| Astrix Security | Agent identity and access governance, listed among the Market Guide's agent identity vendors[^mg-vendors] |
| [[falcon-guardian\|Falcon Guardian]] | Endpoint-resident discovery of known and shadow agents on Windows and macOS, recording who deployed each and its security status[^falcon] |
| [[ping-enterprise-personal-agent-access\|Ping Enterprise Personal Agent Access]] | Personal-agent discovery including shadow AI, each session linked to the user and the device that started it[^ping] |
| [[agentdesktop\|agentdesktop]] ([[solo-io\|Solo.io]], Apache 2.0) | Desktop inventory of agent harnesses and the MCP servers registered in each one's configuration[^agentdesktop] |
| [[wiz-ai-spm\|Wiz AI-SPM]] | Agentless discovery of AI services, models and integrations across cloud environments[^wiz] |
| [[palo-alto-prisma-airs\|Palo Alto Prisma AIRS (AI Runtime Security)]] | Visibility into AI agents, apps and models and how they connect[^prisma] |
| [[cyera-agent-guardian-release\|Cyera Agent Guardian Release]] | Cloud, SaaS, endpoint and Shadow AI agent discovery (vendor-stated; the release gives no coverage figure)[^cyera] |

### Incumbent entrants

The Market Guide expects enterprise-owned, independent guardian agents to provide unified oversight across multicloud, IAM and data environments, and predicts that by 2029 they will remove the need for almost half of today's incumbent AI-agent security systems in over 70% of organizations.[^mg-dir] Incumbents from adjacent markets have begun shipping agent discovery in the meantime:

- Data security: [[cyera|Cyera]], which the Market Guide names as a sample information-governance vendor, claims one live inventory across cloud, SaaS, endpoint and Shadow AI.[^mg-vendors][^cyera]
- Endpoint detection: CrowdStrike's [[falcon-guardian|Falcon Guardian]].[^falcon]
- Identity: Ping Identity's [[ping-enterprise-personal-agent-access|Ping Enterprise Personal Agent Access]].[^ping]
- Infrastructure: Solo.io's open-source [[agentdesktop|agentdesktop]].[^agentdesktop]

The last three arrived in the first week of September 2026, and each discovers agents on one surface. The Cyera release is a vendor self-report that states no coverage figure and cites no third-party evaluation. None of the four releases gives comparable coverage across identity platforms, endpoints, and agent hosts. An assessor must compare each discovery source with the deployment's actual surfaces rather than infer completeness from a product claim.

## Placement in this wiki

The catalog is the inventory layer for the identity pages and a mandatory feature of the guardian-agent page.

| Wiki page | Connection |
|---|---|
| [[non-human-identity\|Non-Human Identity (NHI)]] | The catalog is the inventory layer for NHI; agent cards are NHI metadata |
| [[agent-identity-architecture\|AI Agent Identity Architecture]] | The architectural reference for how catalog identities are assigned and used |
| [[guardian-agent\|Guardian Agent]] | The catalog is mandatory feature category 1 (visibility and traceability) |
| [[agentic-ai-security-cmm-d2-identity\|CMM D2: Identity and Authorization]] | The catalog evidences D2-INVENTORY at L2, D2-OWNER at L3 and D2-REGISTRY at L5 |
| [[shadow-ai\|Shadow AI]], [[shadow-automation\|Shadow Automation]] | Catalog discovery surfaces both |

## CMM D2 criteria the catalog evidences

[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]], the identity domain of the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]], grades one deployment at a time, and the catalog is the artifact behind the following D2 criteria:

- **D2-INVENTORY, at L2.** An inventory records each agent in the deployment and every non-human identity it holds, and each identity's entry names its agent. An inventory kept by hand meets it.
- **D2-OWNER, at L3.** Every agent and every non-human identity in the deployment names a human owner whom the personnel record shows as current. The bar is every one, with no percentage threshold, because the deployment's own design enumerates its identities and one unowned identity inside it is the failure the criterion grades.
- **D2-COUPLING, at L3, and D7-IDENTITY-BASELINE, at L4.** The inventory classes each credential as coupled or decoupled. [[agentic-ai-security-cmm-d7-observability|D7 Observability and Detection]] separately grades a running detector against each identity's normal resource and origin activity.
- **D2-REGISTRY and D2-DISCOVER, at L5.** A registry the deployment pipeline writes through a governed interface holds each agent's identity graph, and scheduled discovery reports every agent the registry does not hold until each is registered or removed.
- **Cross-platform identity graph, at D2 L5 where applicable.** D2-REGISTRY includes the identities and grants from every identity platform the deployment actually uses. Federation and a separate automatic reconciliation service are implementation choices, not scored criteria.

D2's levels grade neither a risk-score methodology nor discovery of agents outside the assessed deployment. The [[guardian-agents-market-guide|Market Guide]] describes broader catalog ambitions, while D2 tests the identities and discovery paths in the assessed deployment.

## Open issues

1. **Cross-platform discovery coverage.** A platform export can reveal an agent identity missing from the D2-REGISTRY graph. The catalog needs a comparison procedure for each identity platform the deployment uses; federation alone does not prove the graph is complete.
2. **Shadow agent fingerprinting.** The Market Guide expects organizations to use metadata when agents declare no identity.[^mg-iam] Model, tool, and output metadata can support investigation, but a fingerprint is not a verified identity.
3. **Skill and MCP-server inventory.** The components an agent consumes are a separate inventory problem. See [[mcp-security|MCP Security]].

## See Also

- [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents]] — primary source (Mandatory Features category: AI agent catalog)
- [[guardian-agent|Guardian Agent]] — catalog is mandatory feature category 1
- [[non-human-identity|Non-Human Identity (NHI)]] — the inventory layer
- [[agent-identity-architecture|AI Agent Identity Architecture]] — how identities are assigned
- [[shadow-ai|Shadow AI]] / [[shadow-automation|Shadow Automation]] — agents the catalog must surface

[^mg-def]: Gartner, [Market Guide for Guardian Agents](https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb), 2026-02-24, Market Definition, Mandatory Features: "The AI agent catalog" and "Ownership mapping" entries.
[^mg-dir]: Gartner, [Market Guide for Guardian Agents](https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb), 2026-02-24, Market Direction: "Enterprise adoption of independent guardian agents" and "Independent guardian agents disrupt legacy security by 2029".
[^mg-iam]: Gartner, [Market Guide for Guardian Agents](https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb), 2026-02-24, Market Direction: "Deep integration of guardian agents with identity and access management capabilities".
[^mg-vendors]: Gartner, [Market Guide for Guardian Agents](https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb), 2026-02-24, Representative Vendors table (Astrix Security) and Note 9, Identity Management and Information Governance (Cyera as a sample information-governance vendor).
[^rmf]: NIST, [Artificial Intelligence Risk Management Framework (AI RMF 1.0), NIST AI 100-1](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf), 2023-01, Table 1, GOVERN 1.6 and GOVERN 2.1.
[^talk]: Gartner webinar, *Scaling Agentic AI: A Leadership Guide for CIOs*, procurement segment. The recording has no public landing page; the transcript is archived in the vault.
[^ms]: Microsoft, [Microsoft Agent 365 overview](https://learn.microsoft.com/en-us/microsoft-agent-365/overview). The Market Guide's Note 7 describes Agent 365 as in preview at its publication and ties its controls to agents, native or third party, registered with Microsoft Entra ID.
[^okta]: Okta, [Okta for AI Agents is now generally available](https://www.okta.com/blog/ai/okta-for-ai-agents-general-availability/), 2026-04-29.
[^falcon]: CrowdStrike, [CrowdStrike Unveils Falcon Guardian to Secure AI Agents Where They Execute](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/), 2026-09-01.
[^ping]: Ping Identity, [Ping Identity Secures Claude Personal Agents From Discovery to Action](https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action), 2026-09-01.
[^agentdesktop]: Solo.io, [Introducing agentdesktop](https://www.solo.io/blog/introducing-agentdesktop), 2026-09-03.
[^wiz]: Wiz, [Securing AI Agents with Wiz AI-SPM](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents), 2025-11-04.
[^prisma]: Palo Alto Networks, [Prisma AIRS](https://www.paloaltonetworks.com/prisma/prisma-ai-runtime-security) product page.
[^cyera]: Cyera, [New from Cyera: AI Security for Every Agent, Assistant, and Data Store](https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store), archived 2026-08-31; the post carries no publication date.

<!-- sources:auto -->
## Sources

- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
- [gartner.com](https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb)
- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- [learn.microsoft.com](https://learn.microsoft.com/en-us/microsoft-agent-365/overview)
- [okta.com](https://www.okta.com/blog/ai/okta-for-ai-agents-general-availability/)
- [crowdstrike.com](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-unveils-falcon-guardian-ai-agent-security/)
- [press.pingidentity.com](https://press.pingidentity.com/2026-09-01-Ping-Identity-Secures-Claude-Personal-Agents-From-Discovery-to-Action)
- [solo.io](https://www.solo.io/blog/introducing-agentdesktop)
- [wiz.io](https://www.wiz.io/blog/wiz-ai-spm-secures-ai-agents)
- [paloaltonetworks.com](https://www.paloaltonetworks.com/prisma/prisma-ai-runtime-security)
- [cyera.com](https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store)
<!-- /sources -->
