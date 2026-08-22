---
type: framework
title: "NIST IR 8596 Cyber AI Profile"
created: 2026-06-02
updated: 2026-06-02
tags:
  - frameworks
  - nist
  - csf
  - ai-cyber-defense
status: seed
origin: aggregated
address: c-000181
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - sec-against-ai
adoption_signal: draft
last_substantive_update: 2025-12-16
published_by: "[[nist|NIST]]"
current_version: "Initial Public Draft (IPRD), December 2025"
first_published: "2025-12-16"
scope: "A NIST Cybersecurity Framework 2.0 profile covering the intersection of AI and cybersecurity across three focus areas"
audience: "Enterprise security teams, AI operators, federal agencies"
aliases:
  - "IR 8596"
  - "Cyber AI Profile"
related:
  - "[[agentic-soc-state-of-the-field]]"
  - "[[agentic-soc-autonomy-ladders]]"
sources:
  - "https://csrc.nist.gov/pubs/ir/8596/iprd"
---

# NIST IR 8596 Cyber AI Profile

NIST IR 8596 is an Initial Public Draft (published 16 December 2025; comment period closed 30 January 2026) that frames the AI–cybersecurity intersection as a profile of the NIST Cybersecurity Framework 2.0 (CSF 2.0). It organizes its guidance using the CSF 2.0 Core outcomes — Functions, Categories, and Subcategories — rather than defining a maturity model or a levels-of-autonomy ladder.

## Three focus areas

The profile partitions the field into three focus areas, each mapping cleanly onto one of the wiki's scope axes:

| Focus area | Verbatim title | Scope axis |
|---|---|---|
| Secure | "Securing AI System Components" | `sec-of-ai` |
| Defend | "Conducting AI-Enabled Cyber Defense" | `ai-in-sec-defense` |
| Thwart | "Thwarting AI-enabled Cyber Attacks" | `sec-against-ai` |

The three-way split is itself useful: it is the same defender-side taxonomy the wiki uses, arriving from a U.S. federal standards body. The **Defend** focus area is the normative anchor for the agentic SOC — AI-enabled detection, triage, and response.

## Relevance to the agentic SOC

IR 8596 is a crosswalk target, not a competing maturity model. Because it carries no maturity tiers or autonomy levels, it complements the [[agentic-soc-autonomy-ladders|autonomy-ladder prior art]] rather than overlapping it: the ladders supply the graded progression, while IR 8596 supplies CSF-anchored outcome statements an agentic-SOC capability model can map its domains against. This mirrors how the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] crosswalks to external standards rather than restating them.

## Verification note

> [!gap] Single-source extraction from the draft landing page
> This page is built from one primary source: the IR 8596 Initial Public Draft landing page. The focus-area titles, the CSF 2.0 organization, the absence of maturity or autonomy tiers, and the publication dates were read from it. Subcategory-level detail, including any per-Subcategory prioritization scheme and the specific treatment of agentic, multi-agent defense, was not extracted and must be confirmed against the full draft before it is cited.

## Relations

- Profiles the NIST Cybersecurity Framework 2.0 (CSF 2.0).
- The "Defend" focus area is the normative scaffolding referenced by [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]].
- Grounds the standards-crosswalk layer of the planned agentic-SOC maturity model in [[agentic-soc-state-of-the-field|Agentic SOC State of the Field]].

<!-- sources:auto -->
## Sources

- [csrc.nist.gov](https://csrc.nist.gov/pubs/ir/8596/iprd)
<!-- /sources -->
