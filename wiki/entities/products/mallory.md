---
type: entity
title: "Mallory"
address: c-000080
created: 2026-05-15
updated: 2026-05-15
tags:
  - entities
  - product
  - tool
  - vulnops
  - agentic-soc
  - threat-intel
status: seed
scope_axis: [ai-in-sec-defense]
entity_type: product
role: "Startup building an intelligence-driven security operations platform that fuses threat intelligence and vulnerability management into a single agent-augmented function. Founder: Jonathan Cran. Customers are the source of the *VulnOps* term as the article reports it; positioned to complement L1-SOC-automation vendors by focusing on threat-intelligence contextualization and reasoning for Level 2+ analysts."
homepage: ""
no_public_url: "Mallory is pre-GA per the source article; no public product URL captured. Founder targets Black Hat for full demo."
maintainer: "[[jonathan-cran|Jonathan Cran]]"
first_mentioned: "[[vulnops-l1-soc-extinction|CYBR.SEC.Media VulnOps / L1 SOC extinction article]]"
related:
  - "[[jonathan-cran|Jonathan Cran]]"
  - "[[vulnops-l1-soc-extinction|Source article]]"
  - "[[vulnops|VulnOps]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC — State of the Field]]"
  - "[[guardian-agent|Guardian Agent]]"
  - "[[hitl|HITL]]"
sources:
  - "[[vulnops-l1-soc-extinction|CYBR.SEC.Media VulnOps article]]"
---

# Mallory

**Sources:** [CYBR.SEC.Media — *From Threat Intel to VulnOps*](https://www.cybrsecmedia.com/from-threat-intel-to-vulnops-why-level-1-soc-as-we-know-it-is-heading-to-extinction/) (May 2026). Founder targets Black Hat for full demonstration.

## Description

Startup building what its founder [[jonathan-cran|Jonathan Cran]] describes as an *"intelligence-driven security operations platform."* Architecturally:

- **Continuous ingestion of ~3,000 sources**: social feeds, ISAC data, vendor advisories, GitHub security disclosures, structured government feeds.
- **Threat graph** for enrichment and mapping.
- **Automatic mapping** of global intelligence to the organization's specific assets, cloud environments, code repositories, and infrastructure-as-code configurations.
- **Action-on-finding**: *"produces detections, routes tickets to the appropriate teams, and in some cases takes direct action, subject to whatever policy guardrails the organization has configured."*
- **User-customizable skill files** that define the right action per environment.
- **Threads** replace conventional case management: every investigation is a collaborative analyst-agent thread rather than artifacts-in / reports-out case management.

## Relevance to This Wiki

Mallory is the wiki's first sourced product entity whose unit of analysis is explicitly **VulnOps as the fusion of threat intelligence and vulnerability management**, distinct from but complementary to the [[vulnops|VulnOps]] concept page's other anchoring (the Mythos-ready briefing's vulnerability-research-and-remediation framing). Together the two sourcings give the VulnOps concept independent corroboration from a vendor-trade-press source in addition to the community-consensus strategic briefing.

Position relative to the existing wiki landscape:

- **Complementary to** L1-SOC automation vendors (alert triage / initial investigation / ticket routing): Mallory is positioned to handle *Level 2+ or strategic / ongoing analysis*. Cran's framing of "monitor mode" SOC has Mallory and similar tools as the layer above L1 automation.
- **Adjacent to** [[guardian-agent|Guardian Agent]] vendors (per Gartner's Feb 2026 market guide); Mallory occupies the contextualization-and-reasoning slot rather than the gating slot.
- **Adjacent to** [[ai-spm|AI-SPM]] and [[ai-bom|AI-BOM]] practices: the asset-correlation layer Mallory is building (per Cran, *"still maturing"*) is structurally similar to AI-SPM's asset-inventory primitive applied to vulnerability-management ground truth.

## Adjacent / Open

- **Repository / homepage**: not captured in the source article; product is pre-GA. Full demo targeted at Black Hat. Worth following up.
- **Asset correlation layer and user-customizable skill files**: explicitly *"still maturing"* per Cran; their final shape will determine whether Mallory's architectural distinctness holds at GA.
- **Quantitative metrics**: none published (no recall, FP rate, time-to-detection, cost-per-investigation numbers in the source article).
- **Competitive landscape**: the article frames Mallory as complementary to existing SOC-automation vendors that focus on L1 tasks; named L1-SOC-automation vendors are not enumerated.
- **Funding / company status**: not captured in the source.
