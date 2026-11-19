---
type: concept
title: "Shadow Automation"
created: 2026-04-30
updated: 2026-10-06
tags:
  - concepts
  - governance
  - shadow-it
  - agentic-ai
  - coding-agents
  - identity
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
complexity: basic
domain: agent-security
coined_by:
  - "[[knostic]]"
aliases:
  - "Shadow Agents"
related:
  - "[[ai-coding-agent-governance]]"
  - "[[scaling-agentic-ai-cios-talk]]"
  - "[[shadow-ai]]"
  - "[[shadow-agent-admission-control]]"
  - "[[non-human-identity]]"
  - "[[nhi-governance-for-agents]]"
  - "[[decision-rights]]"
  - "[[citizen-coders]]"
  - "[[ai-agent-layered-council]]"
  - "[[agent-catalog]]"
  - "[[spiffe]]"
  - "[[a2a-protocol]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[nist-ai-rmf]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[microsoft-entra-agent-id-security-governance]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-agent-365]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[falcon-guardian]]"
  - "[[agentdesktop]]"
  - "[[ai-spm]]"
  - "[[endor-labs-ai-code-governance]]"
  - "[[knostic]]"
sources:
  - "[[ai-coding-agent-governance]]"
  - "[[.raw/articles/knostic-ai-coding-agent-governance-2026-04-30.md]]"
  - "[[.raw/articles/csa-survey-unknown-ai-agents-2026-10-06.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-security-for-ai-2026-05-25.md]]"
  - "[[.raw/articles/ragnar-heil-agent-365-limitations-2026-09-18.md]]"
  - "[[.raw/articles/spiffe-spire-concepts-2026-10-06.md]]"
  - "[[.raw/articles/a2a-protocol-agent-discovery-2026-10-06.md]]"
  - "[[.raw/talks/scaling-agentic-ai-cios-2026-05-01.md]]"
  - "[[.raw/papers/nist-ai-100-1-rmf-2023-01-26.pdf]]"
  - "https://www.knostic.ai/blog/ai-coding-agent-governance"
  - "https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments"
  - "https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview"
  - "https://ragnarheil.de/microsoft-agent-365-what-it-cant-do-yet-limitations-you-need-to-know/"
  - "https://spiffe.io/docs/latest/spire-about/spire-concepts/"
  - "https://a2a-protocol.org/latest/topics/agent-discovery/"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf"
verified: 2026-10-06
verified_against:
  - ".raw/articles/a2a-protocol-agent-discovery-2026-10-06.md"
  - ".raw/articles/csa-survey-unknown-ai-agents-2026-10-06.md"
  - ".raw/articles/knostic-ai-coding-agent-governance-2026-04-30.md"
  - ".raw/articles/microsoft-entra-agent-id-security-for-ai-2026-05-25.md"
  - ".raw/articles/ragnar-heil-agent-365-limitations-2026-09-18.md"
  - ".raw/articles/spiffe-spire-concepts-2026-10-06.md"
  - ".raw/papers/nist-ai-100-1-rmf-2023-01-26.pdf"
  - ".raw/talks/scaling-agentic-ai-cios-2026-05-01.md"
verified_findings: 0
verified_note: "1 low fixed (GOVERN 1.6 'requires' -> 'sets the outcome'); CSA figures and funding caveat match; CMM coordinates match D1 and D2 deep dives; Knostic, Microsoft, Heil and the Gartner transcript read at the cited passages"
---

# Shadow Automation

Shadow automation is the deployment of AI agents that hold credentials and act on code, production systems or business data without registration or governance review. [[knostic|Knostic]] named it for coding agents that teams adopt faster than governance can review them, until no one can say "which agent performed what action, under what context, and with what permissions".[^knostic] Containment registers each agent with a named owner, through discovery sources that observe agents and identity issuers that admit only registered ones.

## Scope and prevalence

### Boundary with shadow IT and shadow AI

In shadow IT and [[shadow-ai|Shadow AI]] a person uses an unsanctioned service or AI tool; in shadow automation an agent acts with credentials of its own.

| | Shadow IT | Shadow AI | Shadow Automation |
|---|---|---|---|
| Unsanctioned element | SaaS service | AI tool and the data sent to it | Agent and its credentials |
| Detection signal | DNS and SSO logs | Traffic to AI services | New agent identities, agent configuration files, unregistered MCP servers |
| Blast radius | Data in the service | Data sent to the tool | Code changes, deploys and credential use |
| Governance gap | Data processing terms | Data-handling policy | Owner, scope and decision rights |

Microsoft's **agent sprawl** is wider, adding temporary agents left in production and unreviewed permissions to the unsanctioned agents it calls shadow AI;[^ms-sprawl] Microsoft's control is the sponsor-and-blueprint model of [[microsoft-entra-agent-id-security-governance|Entra Agent ID: Security and Governance Model]].

### Emergence and prevalence

In a survey the Cloud Security Alliance (CSA) ran for Token Security, 68% of respondents reported high confidence in their visibility into AI agents, yet 82% had discovered previously unknown agents in the past year and 41% more than once.[^csa] Unknown agents most often emerged in four environments:

- Internal automation or scripting environments, 51%.[^csa]
- LLM platforms, including custom tools, assistants and plugins, 47%.[^csa]
- SaaS tools with built-in automation, 40%.[^csa]
- Developer-created workflows, 40%.[^csa]

Token Security, which sells agent discovery, financed the survey and co-developed its questionnaire, and the shares describe 418 respondents, not enterprises at large. Most of these environments lie outside a software deployment pipeline, so discovery has to reach them directly.

[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] reads the [[microsoft-cli-coding-agent-adoption-study|Microsoft CLI Coding Agent Adoption Study]] as evidence that coding agents spread through peers faster than a governance program that enumerates agents first; the study measured a sanctioned rollout, so the reading is an inference.

## Containment

### Discovery sources

Discovery sources observe agents whether or not their owners enrolled them, and each sees a different population:

| Source | Example | Population seen |
|---|---|---|
| Identity provider | [[okta-for-ai-agents\|Okta for AI Agents]], [[microsoft-agent-365\|Microsoft Agent 365]], [[ping-enterprise-personal-agent-access\|Ping Enterprise Personal Agent Access]] | Agents that authenticate to the directory |
| Endpoint process | [[falcon-guardian\|Falcon Guardian]] | Agents running on managed endpoints |
| Harness configuration | [[agentdesktop\|agentdesktop]] | Harnesses and the MCP servers they declare |
| Cloud posture | [[ai-spm\|AI Security Posture Management (AI-SPM)]] | AI assets in cloud accounts |
| Egress records | Outbound proxy | Calls to model providers and agent endpoints |

An agent that authenticates to no directory reaches the inventory only through the other four sources. Detection precedes enrolment: Agent 365 detects shadow agents, and an administrator onboards each one.[^heil]

### Admission at registration

Registries and identity issuers record an agent after its owner enrols it, so they gate admission and leave the finding to discovery. SPIRE issues a SPIFFE identity only to a workload that matches a registration entry,[^spire] and an A2A Agent Card reaches a client only when the agent's owner publishes it or hands it to the client.[^a2a-disc] When registration is the only route to a trusted identity, an unregistered agent becomes a refused and logged caller. [[shadow-agent-admission-control|Shadow Agent Admission Control]] builds the method on [[spiffe|SPIFFE / SPIRE]] and [[a2a-protocol|A2A Protocol (Agent-to-Agent)]], and Knostic recommends blocking unregistered actions "via CI/CD and runtime checks".[^knostic]

### Disposition of findings

Each discovered agent is registered or removed with its credentials revoked. Registration records the owner that [[nhi-governance-for-agents|NHI Governance for AI Agents]] requires and, through [[decision-rights|Decision Rights for AI Agents]], what the agent may do.

## Governance posture

### Comply-or-explain

Gartner's [[scaling-agentic-ai-cios-talk|Scaling Agentic AI: A Leadership Guide for CIOs]] makes procurement the gate against unvetted agents and proposes **comply-or-explain** in place of a "comply or die" clamp-down:[^gartner]

- Business units use the sanctioned [[agent-catalog|AI Agent Catalog]] and stack by default.
- A unit whose need the catalog misses explains the need, and the explanation shapes what the stack adds.
- Procurement vets each new request against the catalog.

The catalog becomes the **procurement chokepoint** through which a new agent enters the inventory, at lower political cost to the [[ai-agent-layered-council|AI Agent Layered Council]] than blocking.

### Inventory under the NIST AI RMF

GOVERN 1.6 of the [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] sets the outcome "Mechanisms are in place to inventory AI systems".[^rmf] A shadow agent is an AI system outside that inventory, and the controls above are those mechanisms.

## CMM coordinates

The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] grades containment in two domains:

| Control | Coordinate | Baseline |
|---|---|---|
| Inventory of agents and identities | [[agentic-ai-security-cmm-d2-identity\|D2]] L2, D2-INVENTORY | Identity provider's list |
| Scheduled estate-wide discovery, each shadow agent registered or removed in a set time | [[agentic-ai-security-cmm-d1-governance\|D1]] L3, D1-SHADOW and D1-SHADOW-REAP | AI register |
| Scheduled discovery per deployment, each finding closed | D2 L5, D2-DISCOVER | Identity registry (D2-REGISTRY, also L5) |

Discovery appears twice because D1 compares the estate with the AI register, while D2 compares one deployment with an identity registry required only at L5.

## Related concepts

- [[citizen-coders|Citizen Coders]]: builders whose agents reach production at developer pace.
- [[non-human-identity|Non-Human Identity (NHI)]]: the identity class a registered agent receives.
- [[ai-coding-agent-governance|AI Coding Agent Governance]]: the Knostic post that named the term.

## Notes

[^knostic]: Knostic, [*AI Coding Agent Governance*](https://www.knostic.ai/blog/ai-coding-agent-governance), Organizational Risk Gap and FAQ, retrieved 2026-04-30.
[^ms-sprawl]: Microsoft, [*Microsoft Entra security for AI overview*, Agent sprawl](https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview), retrieved 2026-05-25.
[^csa]: Cloud Security Alliance, [*New CSA Survey Reveals 82% of Enterprises Have Unknown AI Agents*](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments), 2026-04-21. Shares of 418 online respondents, January 2026, in a survey Token Security commissioned, financed and co-developed.
[^heil]: Ragnar Heil, [*Microsoft Agent 365: what it can't do yet*](https://ragnarheil.de/microsoft-agent-365-what-it-cant-do-yet-limitations-you-need-to-know/), 2026-05-08.
[^spire]: SPIFFE project, [*SPIRE Concepts*, Workload Registration](https://spiffe.io/docs/latest/spire-about/spire-concepts/), retrieved 2026-10-06.
[^a2a-disc]: A2A Project, [*Agent Discovery in A2A*](https://a2a-protocol.org/latest/topics/agent-discovery/), retrieved 2026-10-06.
[^gartner]: Gartner, [*Scaling Agentic AI: A Leadership Guide for CIOs*](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8), webinar, transcript segment [00:25].
[^rmf]: NIST, [*AI RMF 1.0*, NIST AI 100-1, Table 1, p. 23](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf#page=28), 2023-01-26.

<!-- sources:auto -->
## Sources

- [New Cloud Security Alliance Survey Reveals 82% of Enterprises Have Unknown AI Agents in Their Environments](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments)
- [learn.microsoft.com](https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview)
- [Microsoft Agent 365: What It Can’t Do (Yet) — Limitations You Need to Know](https://ragnarheil.de/microsoft-agent-365-what-it-cant-do-yet-limitations-you-need-to-know/)
- [SPIRE Concepts | SPIFFE](https://spiffe.io/docs/latest/spire-about/spire-concepts/)
- [Agent Discovery in A2A](https://a2a-protocol.org/latest/topics/agent-discovery/)
- [Scaling Agentic AI: A Leadership Guide for CIOs](https://stream.stream-ext.bizzabo.com/U00liQQ00l2Th5l5Bkc302pY02k01IzHU8P3OqHaCDwYzvxw.m3u8)
- [knostic.ai](https://www.knostic.ai/blog/ai-coding-agent-governance)
- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
<!-- /sources -->
