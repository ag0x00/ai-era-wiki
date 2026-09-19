---
type: entity
entity_type: product
title: "Microsoft Agent 365"
created: 2026-09-18
updated: 2026-09-18
tags:
  - products
  - identity
  - agent-lifecycle
  - ai-control-plane
  - cots
  - microsoft
  - agent-platform
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
vendor: "Microsoft"
homepage: "https://www.microsoft.com/en-us/microsoft-agent-365"
ga_date: "2026-05-01"
related:
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft]]"
  - "[[onyx-platform]]"
  - "[[okta-for-ai-agents]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[agent-identity-architecture]]"
  - "[[nhi-governance-for-agents]]"
  - "[[shadow-automation]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[microsoft-rai]]"
  - "[[maturity-model-spread-axis-mismatch]]"
  - "[[non-human-identity]]"
  - "[[vasu-jakkal]]"
  - "[[microsoft-security-copilot]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[microsoft-entra-agent-id-security-governance]]"
  - "[[microsoft-secure-agentic-ai-end-to-end]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
sources:
  - "https://www.microsoft.com/en-us/microsoft-agent-365"
  - "https://learn.microsoft.com/en-us/microsoft-agent-365/overview"
  - ".raw/articles/microsoft-agent-365-product-page-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-service-description-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-general-availability-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-sdk-overview-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-design-patterns-2026-09-18.md"
  - ".raw/articles/microsoft-windows-365-cloud-pc-agent-pools-2026-09-18.md"
  - ".raw/articles/datadog-entra-agent-id-blueprint-blast-radius-2026-09-18.md"
  - ".raw/articles/ragnar-heil-agent-365-limitations-2026-09-18.md"
  - ".raw/articles/techtarget-m365-e7-ai-governance-prices-critiques-2026-09-18.md"
  - ".raw/articles/cordum-2026-agentic-control-plane-buying-guide-2026-09-18.md"
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
  - ".raw/articles/appsecsanta-onyx-security-review-2026-2026-09-18.md"
  - ".raw/articles/microsoft-agent-registry-admin-center-2026-09-18.md"
verified: 2026-09-18
verified_against:
  - ".raw/articles/appsecsanta-onyx-security-review-2026-2026-09-18.md"
  - ".raw/articles/cordum-2026-agentic-control-plane-buying-guide-2026-09-18.md"
  - ".raw/articles/datadog-entra-agent-id-blueprint-blast-radius-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-general-availability-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-learn-overview-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-product-page-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-sdk-overview-2026-09-18.md"
  - ".raw/articles/microsoft-agent-365-service-description-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-design-patterns-2026-09-18.md"
  - ".raw/articles/microsoft-windows-365-cloud-pc-agent-pools-2026-09-18.md"
  - ".raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md"
  - ".raw/articles/ragnar-heil-agent-365-limitations-2026-09-18.md"
  - ".raw/articles/techtarget-m365-e7-ai-governance-prices-critiques-2026-09-18.md"
verified_findings: 0
verified_note: "GA agent-type coverage rewritten: Heil and Microsoft's announcement disagree and both now stand on the page. Cloud PC quotation made verbatim; See also folded into body prose. The agent-registry Learn page was verified live at the time and has since been clipped to .raw/articles/microsoft-agent-registry-admin-center-2026-09-18.md."
---

# Microsoft Agent 365

**Sources:** [Microsoft Agent 365 (product page)](https://www.microsoft.com/en-us/microsoft-agent-365) · [Microsoft Learn — Agent 365 overview](https://learn.microsoft.com/en-us/microsoft-agent-365/overview) · [Microsoft Learn — Agent 365 service description](https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-agent-365/microsoft-agent-365)

Microsoft Agent 365 is a licensed Microsoft 365 service that extends five existing Microsoft cloud services to AI agents. Microsoft's service description calls it "a centralized control plane for AI agents" that lets an organization "discover, manage, govern, and secure agents across their environments", and the product page repeats the framing in its title and its FAQ, which adds that the service enables "organizations to extend their existing infrastructure for users to agents".[^a365-control-plane] [[standards-review-microsoft-rai-agent-365-2026-Q2|The wiki's RAI and Agent 365 standards review]] reads it as a management plane over controls that other Microsoft products enforce, whose own contribution is the registry and the administrative aggregation.

## Product

### Services the product extends

Microsoft names five services under Capabilities on the product page, and the service description lists the same five in its Management Console column.[^a365-five]

| Service | Contribution Microsoft states |
|---|---|
| Microsoft 365 admin center | Central hub for users, agents and settings; hosts the registry |
| Microsoft Defender | Security posture and threat protection |
| Microsoft Entra | Agent identity protection and access security |
| Microsoft Intune | Policies and guardrails for agents |
| Microsoft Purview | Governance of the data agents use and create |

The registry is an admin-center surface reached at Agents > All Agents > Registry, and Microsoft names it the Agent Registry.[^registry] Its Microsoft Graph API is in preview.[^registry] The registry grades each entry against ten named risk types, the critical ones being a shadow agent with no registry entry, owner or [[microsoft-entra-agent-id|Entra Agent ID]], an agent with no owner assigned, and an agent holding excessive permissions.[^registry] Intune contributes three named controls: device compliance for agent conditional access, a policy-controlled environment for the agent runtime, and blocking of unsanctioned local endpoint agents.[^a365-five]

### Availability and licensing

Agent 365 reached general availability for commercial customers on 2026-05-01.[^ga] [[agentic-cmm-regulated-fi-stress-test|The regulated-FI CMM stress test]] reads that date as out of reach for a regulated institution, where a product that went generally available three weeks ago sits twelve to eighteen months from deployment behind vendor risk assessment, SOC-2 review and board sign-off. Three purchase routes exist: a standalone subscription at \$15.00 per user per month on annual commitment, an add-on to Microsoft E5, A5 or Business Premium (or to the Defender and Purview suites), and inclusion in Microsoft 365 E7 at \$99.00 per user per month.[^pricing][^entra-what-is] "Frontier Suite" appears as marketing prose about E7 in the product-page FAQ and names no SKU; the three SKUs on the pricing table are Agent 365, Microsoft 365 E7 and Microsoft 365 E7 (No Teams).[^pricing] The licence reaches past the registry: [[microsoft-entra-agent-id-security-governance|the Entra Agent ID security and governance summary]] records that current Microsoft documentation makes Conditional Access for agents require an Agent 365 licence for each user, which withdraws the near-zero incremental cost an E5 incumbent otherwise carried. [[microsoft-rai|The Responsible AI Standard]] states goals and defers enforcement to the ZT4AI control catalogue and to this management plane, which carries the platform lock and the licensing the goals standard does not.

The Frontier program continued after general availability as the channel for capabilities that had not reached it. Registry sync with AWS Bedrock and Google Cloud is in public preview, and shadow-AI detection of OpenClaw agents is limited to Frontier-program customers.[^ga] Coverage by agent type at general availability is reported two ways. Microsoft's announcement lists agents acting on behalf of users and agents operating with their own access as generally available, and puts only agents participating in team workflows in public preview.[^ga] Ragnar Heil, an independent Microsoft 365 governance consultant, wrote a week later that general availability reached on-behalf-of agents alone, and that autonomous agents, including agent-to-agent and agent-to-tool scenarios, stayed in Frontier Preview with their licensing still being defined.[^heil] The two accounts disagree on where the autonomous case sits, and no source read for this page reconciles them.

### Reach over agents built elsewhere

Microsoft states that Agent 365 "works with agents built on Microsoft platforms and with agents built or acquired from third-party sources", and names three mechanisms that bring such an agent under management.[^a365-five] An agent published through Microsoft 365 channels with an Entra Agent ID appears in the registry automatically, and others "require additional registration steps".[^pricing] Registry sync pulls agents and their metadata from platforms Agent 365 does not manage, through connectors for AWS Bedrock and Google Cloud that are in public preview.[^ga] The Agent 365 SDK connects an agent the organization already owns and runs.[^sdk]

The SDK names the runtimes it supports: LangChain, Microsoft Agent Framework, Semantic Kernel, the OpenAI Agents SDK, the Claude Agent SDK, and custom code, hosted on Azure, AWS, Google Cloud or on premises.[^sdk] Two conditions narrow that reach in practice. An agent reaches management only when an administrator onboards it through the SDK, which needs an Agent 365 licence as well as the agent vendor's own, and detection alone does not enrol a discovered shadow agent.[^heil] Microsoft also disclaims third-party MCP servers, which it classes as Non-Microsoft Products used at the customer's own risk.[^registry]

## Assessment

### Durability of the identity-first model

The unit of policy authorship stays long-lived in this product even where the unit of execution has become ephemeral. That is the claim the evidence read for this page supports, and it is narrower than an assertion that Microsoft governs agents only as durable directory objects in static environments.

Two Microsoft pages refute the wider assertion. Windows 365 for Agents provisions Cloud PC agent pools, which are shared collections of Cloud PCs where "agents check out a Cloud PC from the pool when they need one and return it when they're finished"; against Enterprise Cloud PCs the comparison table gives their persistence as "Reset after use", their assignment as "Shared across multiple agents" and their billing as consumption-based.[^w365] Entra Agent ID documents a matching identity pattern, in which an orchestrator "creates a temporary agent identity at runtime to facilitate a specific interaction", "grants it permissions inherited from the blueprint, and deletes the identity when the session ends", which "limits the blast radius to the duration of the task".[^design-patterns] Execution and identity lifetime both reach below the long-lived object.

Scope does not follow them down. The temporary identity inherits the grant written on the blueprint, so its lifetime shortens while its permissions stay the ones authored for the class. Datadog Security Labs reports that delegated scopes granted through a blueprint "are applied when a token is issued and cannot be seen on the agent identities inheriting them", which leaves the inheriting identity unauditable for what it can reach, and records a ceiling of 250 agent identities per blueprint per tenant.[^datadog] Anything finer than an identity is assigned to the customer's application code. Microsoft's design-patterns guidance rules per-object identity out under "Patterns to avoid": creating one agent identity "per meeting, per document, or per ephemeral object at high volume isn't practical with directory-level identities today", and "for high-flux scenarios, use shared agent identities and rely on session or context identifiers at the application layer to distinguish interactions".[^design-patterns] The same page assigns the perimeter itself to the customer, because "a trust boundary is an application threat-modeling decision, not one that Microsoft Entra Agent ID defines for you".[^design-patterns]

One third party has reported the operating consequence. Gartner's First Take of 2026-03-09, quoted by TechTarget, found that Agent 365 "surfaces alerts (for example, risky agents), but administrators must act on them manually", which "may work for a small number of agents, but at scale it will require automation or agentic automation capabilities, such as guardian agents".[^techtarget]

Two questions stay open. No source read for this page measures Agent 365 at the agent counts where the model is predicted to break, so the scaling argument rests on the documented ceiling rather than on an observation. None of the Microsoft pages read here — the design-patterns guidance, the Agent 365 overview, the service description, the SDK overview and the Cloud PC agent-pool documentation — states whether an ephemeral agent identity counts against the 250-per-blueprint ceiling, or what rate limit governs creating and deleting one.

### Position among agent control planes

No source read for this page compares Agent 365 with [[onyx-platform|the Onyx Platform]] directly; the comparison below is assembled from each product's own documented enforcement surface.

The two products are mostly complementary because they enforce at different points. Agent 365 enforces on the identity, through registration, conditional access, lifecycle and posture. Onyx enforces on the action, protecting "prompts, responses, and agent actions in real time" and proxying "all MCP traffic with an inline gateway" that logs every request and response.[^onyx] Cordum, a competitor to both, states the resulting procurement pattern: "a reasonable enterprise often picks two: one control plane for the in-stack agents (Microsoft Agent 365 if the stack is Microsoft) and one for the out-of-stack or customer-managed governance surface".[^cordum]

They overlap in three places. Both discover shadow AI, though Agent 365 stops at detection because a detected agent still needs manual onboarding.[^heil] Both present an inventory and a posture dashboard. Both govern Microsoft Copilot, which an independent review of Onyx lists among that product's SaaS coverage, and which is the one workload the two contend for.[^appsecsanta]

Among the other entrants the vault carries, [[crowdstrike-agentic-identity-provider|CrowdStrike's Agentic Identity Provider]] is the closest architectural alternative, because it brokers short-lived scoped access per task in place of standing credentials and so makes the task rather than the registered agent its unit. [[okta-for-ai-agents|Okta for AI Agents]] and [[ping-enterprise-personal-agent-access|Ping's Enterprise Personal Agent Access]] compete with Agent 365 on the same identity surface, for organizations anchored on a directory other than Entra.

## Notes

[^a365-control-plane]: Microsoft, [*Microsoft Agent 365 service description*](https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-agent-365/microsoft-agent-365), retrieved 2026-09-18, and [*Microsoft Agent 365*](https://www.microsoft.com/en-us/microsoft-agent-365) (product page title and FAQ), retrieved 2026-09-18. Local copies: `.raw/articles/microsoft-agent-365-service-description-2026-09-18.md`, `.raw/articles/microsoft-agent-365-product-page-2026-09-18.md`.
[^a365-five]: Microsoft, [*Microsoft Agent 365*](https://www.microsoft.com/en-us/microsoft-agent-365), Capabilities section, and [*Microsoft Agent 365 service description*](https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-agent-365/microsoft-agent-365), Management Console column and Intune feature rows, both retrieved 2026-09-18.
[^registry]: Microsoft, [*Agent registry in the Microsoft 365 admin center*](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/agent-registry), retrieved 2026-09-18. Names the Agent Registry surface, marks the Microsoft Graph API for Agent Registry and Agent Details as preview, lists the ten risk types with their severities, and states the third-party-tool disclaimer. Local copy: `.raw/articles/microsoft-agent-registry-admin-center-2026-09-18.md`.
[^ga]: Microsoft Security Blog, [*Microsoft Agent 365, now generally available, expands capabilities and integrations*](https://www.microsoft.com/security/blog/2026/05/01/microsoft-agent-365-now-generally-available-expands-capabilities-and-integrations/) (2026-05-01), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-agent-365-general-availability-2026-09-18.md`.
[^pricing]: Microsoft, [*Microsoft Agent 365*](https://www.microsoft.com/en-us/microsoft-agent-365), Plans and Pricing and FAQ sections, retrieved 2026-09-18. List prices are per user per month on annual commitment, in US dollars.
[^entra-what-is]: Microsoft, [*What is Microsoft Entra Agent ID?*](https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id), retrieved 2026-09-18, for the add-on route and the licensing gate on Entra security features.
[^heil]: Ragnar Heil, [*Microsoft Agent 365: what it can't do yet*](https://ragnarheil.de/microsoft-agent-365-what-it-cant-do-yet-limitations-you-need-to-know/) (2026-05-08), retrieved 2026-09-18. Independent Microsoft 365 governance practitioner; the limitations are the author's reading of the product at that date. Local copy: `.raw/articles/ragnar-heil-agent-365-limitations-2026-09-18.md`.
[^sdk]: Microsoft, [*Microsoft Agent 365 SDK overview*](https://learn.microsoft.com/en-us/microsoft-agent-365/developer/agent-365-sdk), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-agent-365-sdk-overview-2026-09-18.md`.
[^w365]: Microsoft, [*Cloud PC agent pools*](https://learn.microsoft.com/en-us/windows-365/agents/cloud-pc-agent-pools), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-windows-365-cloud-pc-agent-pools-2026-09-18.md`.
[^design-patterns]: Microsoft, [*Microsoft Entra Agent ID design patterns*](https://learn.microsoft.com/en-us/entra/agent-id/concept-agent-id-design-patterns), retrieved 2026-09-18. Local copy: `.raw/articles/microsoft-entra-agent-id-design-patterns-2026-09-18.md`.
[^datadog]: Datadog Security Labs, [*Agent ID blueprint blast radius*](https://securitylabs.datadoghq.com/articles/agent-id-blueprint-blast-radius/), retrieved 2026-09-18. The 250 figure is the documented Entra directory limit on agent identities associated with one blueprint per tenant. Local copy: `.raw/articles/datadog-entra-agent-id-blueprint-blast-radius-2026-09-18.md`.
[^techtarget]: TechTarget, [*Microsoft 365 E7 adds AI governance, prices draw critiques*](https://www.techtarget.com/searchitoperations/news/366639980/Microsoft-365-E7-adds-AI-governance-prices-draw-critiques), retrieved 2026-09-18, quoting a Gartner First Take report of 2026-03-09 that is paywalled at its source.
[^onyx]: Onyx Security, [*Onyx Platform*](https://onyx.security/platform), retrieved 2026-05-03, vendor marketing. Local copy: `.raw/articles/onyx-platform-secure-ai-control-plane-2026-05-03.md`.
[^cordum]: Cordum, [*2026 agentic control plane buying guide*](https://cordum.io/blog/2026-agentic-control-plane-buying-guide), retrieved 2026-09-18. Written by a vendor competing with both products named in the quotation.
[^appsecsanta]: appsecsanta, [*Onyx Security review*](https://appsecsanta.com/onyx-security), retrieved 2026-09-18, which lists Onyx's SaaS coverage as Salesforce, Glean and Microsoft Copilot and flags Onyx's own usage counters as self-reported.
