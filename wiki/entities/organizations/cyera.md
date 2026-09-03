---
type: entity
entity_type: organization
title: "Cyera"
address: c-000329
created: 2026-08-31
updated: 2026-08-31
tags:
  - entities
  - organizations
  - vendor
  - dspm
  - guardian-agent
  - agent-security
status: seed
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
origin: aggregated
org_type: vendor
role: "Data security posture management (DSPM) vendor; markets Agent Guardian, a four-phase (Discover/Govern/Protect/Validate) agentic-lifecycle security product"
homepage: "https://www.cyera.com"
first_mentioned: "[[oversharing-controls|Oversharing Controls]]"
related:
  - "[[cyera-agent-guardian-release|Cyera Agent Guardian Release]]"
  - "[[oversharing-controls|Oversharing Controls for AI Search]]"
  - "[[varonis|Varonis]]"
  - "[[guardian-agent|Guardian Agent]]"
  - "[[guardian-agents-market-guide|Guardian Agents Market Guide]]"
  - "[[shadow-ai|Shadow AI]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
sources:
  - "https://www.cyera.com"
  - "https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store"
  - ".raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md"
  - ".raw/articles/gartner-market-guide-for-guardian-agents-2026-05-01.md"
---

# Cyera

**Sources:** [Cyera (homepage)](https://www.cyera.com) · [New from Cyera: AI Security for Every Agent, Assistant, and Data Store](https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store)

## Identity and role

Data security posture management (DSPM) vendor, named alongside [[varonis|Varonis]] and BigID as a DSPM incumbent extending into AI-feed monitoring ([[oversharing-controls|Oversharing Controls for AI Search]]). [[guardian-agents-market-guide|Gartner's Guardian Agents Market Guide]] places Cyera in none of its six vendor segments. It names Cyera instead among the "sample vendors in the information governance category that complement agent identity and other GA solutions", alongside Bigeye, Concentric AI, Touchdown and Collibra, and states that those vendors are expanding into agent discovery and inventory and into contextual risk mapping.

## Relevance to this wiki

Cyera announced Agent Guardian and a set of platform extensions around it in an undated release fetched 2026-08-31 ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). Cyera states the announcement's premise: an agent inherits the full access of the person who launched it and can exercise that access immediately, unlike a new hire who grows into permissions over time. The release is a vendor self-report with no independent evaluation, no benchmark, and no customer count; the full assessment, including its disagreement over tool-call monitoring with the [[owasp-ai-exchange|OWASP AI Exchange]] detection-layer ranking that [[agentic-ai-security-reference-architecture|the AAI-S reference architecture]] carries, is on the release page.

## Products

Cyera's product catalog, per that release:

- **Agent Guardian** — Cyera states it secures the agent lifecycle in four phases, named Discover, Govern, Protect, and Validate: a live inventory of agents across cloud, SaaS, endpoint, and Shadow AI; posture and intent-drift monitoring; runtime blocking of a risky tool call or field-stripping of a sensitive response; and continuous adversarial red teaming with audit-ready evidence.
- **Cyera Endpoint** — extends Agent Guardian to employee laptops. Cyera states it covers sanctioned and unsanctioned agents and their use of local `bash`, `run-code`, and MCP calls, naming Claude Code, Cowork, and ChatGPT Desktop as covered desktop targets.
- **Incident Materiality Assessment** — a data-impact service, generally available as of this release, that Cyera states pairs data scanning and classification with expert-led analysis to produce a disclosure-ready record of what was exposed, when, and to whom.
- **Cy** — Cyera's data and AI security assistant. Cyera states it answers natural-language questions over alert, exposure, and sensitive-data context, and that it supports Saved Prompts and project-scoped responses. Cy is itself an agentic assistant, so [[agentic-ai-security-reference-architecture|the reference architecture]] governs it as an object under this wiki's scope.
- **Projects** — role-scoped views over datastores, alerts, identities, and API token scope within a single Cyera tenant.
- **Access Trail** — an audit-event feed, extended in this release to Microsoft Exchange Online mailboxes and NetApp.

## See Also

- [[cyera-agent-guardian-release|Cyera Agent Guardian Release]]
- [[oversharing-controls|Oversharing Controls for AI Search]]
- [[varonis|Varonis]]
- [[guardian-agent|Guardian Agent]]
