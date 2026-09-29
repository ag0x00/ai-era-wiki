---
type: entity
entity_type: organization
org_type: government
title: "ENISA (European Union Agency for Cybersecurity)"
created: 2026-05-02
updated: 2026-09-28
tags:
  - entities
  - organizations
  - government
  - eu
  - threat-landscape
status: stub
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - sec-against-ai
website: "https://www.enisa.europa.eu/"
focus: "Annual EU Threat Landscape report; AI threat assessments; EU regulatory technical guidance"
aliases:
  - "ENISA"
  - "European Union Agency for Cybersecurity"
related:
  - "[[source-triangulation-audit-2026-05-02]]"
  - "[[eu-ai-act]]"
  - "[[wef]]"
  - "[[openai-daybreak]]"
sources:
  - "https://www.enisa.europa.eu/publications/enisa-threat-landscape-2025"
  - "https://www.enisa.europa.eu/"
  - "https://openai.com/index/daybreak-securing-the-world/"
verified: 2026-09-29
verified_against:
  - ".raw/articles/openai-daybreak-securing-the-world-2026-09-28.md"
verified_findings: 0
verified_note: "verify2, diff-scoped to the retargeted June-post footnotes; footnote descriptions match the post; no findings"
---

# ENISA (European Union Agency for Cybersecurity)

EU government cybersecurity body. The wiki's primary **EU-side government source** on AI threat landscape and incident statistics — the European counterpart to NIST and CISA in the US.

## Notable outputs

- **ENISA Threat Landscape 2025** ([enisa.europa.eu/publications/enisa-threat-landscape-2025](https://www.enisa.europa.eu/publications/enisa-threat-landscape-2025)) — Reports 4,875 EU incidents (Jul 2024 – Jun 2025); confirms identity- and credential-themed risk acceleration; positions 2025 as the inflection year for AI-shaped threats.
- Adjacent guidance on AI cybersecurity, NIS2 implementation, and EU AI Act technical aspects.

## Relevance to this corpus

ENISA is the EU government data source the wiki was previously missing. For [[eu-ai-act|EU AI Act]] compliance discussions, ENISA's threat landscape and technical reports are the authoritative complement to Annex IV evidence requirements. Useful for D1 (Governance & Accountability) CMM evidence in EU-deployed contexts and as a non-US triangulation for [[source-triangulation-audit-2026-05-02|breach-cost claims]].

OpenAI's June 2026 announcement expanding [[openai-daybreak|OpenAI Daybreak]] names EU institutions including ENISA among the Trusted Access for Cyber partnerships OpenAI established in the preceding month, alongside Australia, Canada, France, Germany, Japan and the Republic of Korea.[^daybreak-gov] The announcement gives the aim of OpenAI's work with governments and institutions, to uplift their defensive cybersecurity capabilities and protect critical infrastructure, and does not describe the terms of those partnerships.

## See Also

- [[source-triangulation-audit-2026-05-02|Source Triangulation Audit 2026-05-02]] — Claim 1 (NHI), Claim 6 (cost)
- [[eu-ai-act|EU AI Act]] — primary regulatory regime
- [[wef|World Economic Forum]] — international-leader-survey counterpart

[^daybreak-gov]: [OpenAI — Daybreak, "Protecting critical infrastructure and sensitive systems"](https://openai.com/index/daybreak-securing-the-world/#protecting-critical-infrastructure-and-sensitive-systems), 2026-06-22: the Trusted Access for Cyber government partnerships and the stated aim of that work. Local copy: `.raw/articles/openai-daybreak-securing-the-world-2026-09-28.md`. Summarized at [[openai-daybreak|OpenAI Daybreak]].
