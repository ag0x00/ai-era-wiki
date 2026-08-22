---
type: framework
title: "ISO/IEC 42001: AI Management Systems"
created: 2026-04-30
updated: 2026-08-17
tags:
  - frameworks
  - iso
  - governance
  - certification
  - ai-management-system
status: developing
source_url: "https://www.iso.org/standard/42001"
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2023-12-01
published_by: "[[iso|ISO/IEC]]"
current_version: "42001:2023 (unchanged through Q1 2026)"
first_published: "2023"
scope: "Certifiable AI Management System (AIMS) standard — governance framework for responsible AI development, deployment, and use"
audience: "Organizations developing or using AI; enterprise compliance; auditors"
aliases:
  - "ISO 42001"
  - "AIMS"
  - "AI Management System"
related:
  - "[[owasp-ai-exchange]]"
  - "[[iso|ISO/IEC]]"
  - "[[nist-ai-rmf]]"
  - "[[eu-ai-act]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# ISO/IEC 42001 — AI Management Systems

**ISO/IEC 42001:2023** is the first (and only) certifiable AI Management System standard. It provides a governance framework for organizations developing or using AI, with 38 Annex A controls covering data governance, bias mitigation, and third-party management. The standard is certified by accredited certification bodies against defined requirements.

## Structure

ISO/IEC 42001 follows the ISO High-Level Structure (HLS) used by ISO 27001 and ISO 9001, enabling integration with existing management systems. Management clauses 4-10 carry the Plan-Do-Check-Act conformance spine (4 Context, 5 Leadership, 6 Planning, 7 Support, 8 Operation, 9 Performance evaluation, 10 Improvement). Annex A provides ~38 AI-specific controls organized under nine control objectives: **A.2 Policies, A.3 Internal organization, A.4 Resources, A.5 AI-system impact assessment, A.6 AI system life cycle, A.7 Data, A.8 Information for interested parties, A.9 Use, A.10 Third-party relationships**. Annex B gives implementation guidance for the Annex A controls; Annex C lists potential AI-related organizational objectives and risk sources; Annex D covers applying the AIMS across domains and sectors. The standard does not include technical security controls; those are delegated to ISO 27001.

The emerging **"triple stack"**: ISO 42001 + ISO 27001 + ISO 27701 is increasingly positioned as a comprehensive governance package.

> [!gap] Citation-only / paywall-bounded structure
> The primary normative text of ISO/IEC 42001:2023 is paywalled and was not read. The clause titles and the Annex A objective scheme above are taken from the public ISO ToC metadata and attributed secondary summaries; the ~38 individual control titles remain summary-sourced, not verified against the primary Annex. The [[standards-review-iso-42001-27090-2026-Q2|2026-Q2 ISO/IEC 42001 + 27090 review]] maps the CMM against this public structure only and records its absence claims as paywall-bounded.

## Q1 2026 Developments

**Base standard: unchanged.** No amendment or revision published or planned for 2026.

Key companion developments:
- **ISO/IEC 42006:2025** — Published (date confirmed Q1 2026); establishes formal requirements for AI management system audit and certification bodies, including auditor competence, audit time calculation, and certification documentation. This resolves the critical gap in certification body consistency.
- **ISO/IEC 27090** — AI cybersecurity guidance; registered FDIS on March 12, 2026; entered the 8-week approval ballot; publication expected mid-2026. This will be the first ISO AI cybersecurity standard, though guidance-only (not certifiable).
- **Schellman** became the first ANAB-accredited ISO 42001 certification body (January 2026); market demand described as "surging"
- **Microsoft** expanded ISO 42001 certification scope to cover Microsoft 365 Copilot; plans for Copilot Studio, Dragon Copilot, Security Copilot, GitHub Copilot, and Microsoft Foundry
- 100+ organizations certified within 18 months of publication, including AWS, Anthropic, and KPMG

## Strengths

- The **only certifiable AI management system standard** — unlike NIST AI RMF or OWASP, it enables third-party verification
- ISO 42006 resolves the certification body competence gap — credible, consistent audits now possible
- Major vendors pursuing certification lends credibility and market signal
- HLS structure enables integration with ISO 27001 (security) and ISO 27701 (privacy)
- Aligns with EU AI Act compliance trajectories — positioned as a primary certifiable compliance pathway, though distinct from the CEN/CENELEC harmonized standards still in development; the [[standards-review-eu-ai-act-2026-Q2|2026-Q2 EU AI Act review]] confirms the Act delegates its control specifications to those harmonized standards rather than to ISO 42001

## Gaps and Shortcomings

- **Governance framework that delegates all technical security controls to ISO 27001**, which itself has no AI-specific controls — a structural limitation
- No guidance on agentic AI, MCP/A2A security, plugin supply chains, agent identity, or runtime enforcement
- ISO/IEC 27090, once published, will be guidance-only rather than certifiable, and is a formally separate instrument from the EU AI Act harmonized standards under development (prEN 18282, ETSI prEN 304 223). Separate instruments do not imply independent content: the [[owasp-ai-exchange|OWASP AI Exchange]] states it contributed 70 pages to ISO/IEC 27090 and 70 pages to prEN 18282 through official liaison partnership, and that its founder [[rob-van-der-veer|Rob van der Veer]] works in both and was elected co-editor for the AI Act security work at CEN/CENELEC by the EU member states.[^aix-liaison] An organization treating the two as independent corroboration of one another is double-counting a single source.
- No AI-BOM requirements
- 38 Annex A controls are governance-oriented; no testable technical assertions — organizations cannot verify whether a certified entity has functional defenses against prompt injection or supply chain attacks
- Implementation cost remains prohibitive for smaller organizations
- Does **not** cover agentic AI-specific risk categories (ASI06–ASI08 have zero coverage)

## Coverage Against OWASP ASI Top 10

| ASI Category | Coverage |
|---|---|
| ASI01: Agent Goal Hijack | ○ None |
| ASI02: Tool Misuse | ○ None |
| ASI03: Identity & Privilege | ○ None |
| ASI04: Supply Chain | ○ None |
| ASI05: Unexpected Code Execution (RCE) | ○ None |
| ASI06: Memory Poisoning | ○ None |
| ASI07: Insecure Inter-Agent | ○ None |
| ASI08: Cascading Failures | ○ None |
| ASI09: Human-Agent Trust Exploitation | ○ None |
| ASI10: Rogue Agents | ○ None |

## Watch Items (2026)

- **ISO/IEC 27090 publication** (mid-2026) — first ISO AI cybersecurity standard; will influence EU AI Act harmonized standards
- **EU AI Act harmonized standards** (CEN/CENELEC, targeting end of 2026) — relationship between ISO 42001 and EU compliance pathways
- Whether ISO 42001 issues an AI-safety or agentic AI technical specification

## See Also

- [[iso|ISO/IEC]] (publisher)
- [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]] — voluntary governance complement; no certification but broader practitioner adoption in the U.S.
- [[eu-ai-act|EU AI Act]] — regulatory framework that ISO 42001 is a primary compliance pathway for
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — ISO 42001 alignment is **D1 L4** evidence; certification is **D1 L5**; Annex A 38-control crosswalk in [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]]
- [[standards-review-iso-42001-27090-2026-Q2|ISO/IEC 42001 and 27090 Standards Review]] — citation-only, paywall-bounded CMM coverage map; corrects the crosswalk's Annex A numbering and confirms ISO 27090's FDIS-unpublished status

## Notes

[^aix-liaison]: OWASP AI Exchange, ["About the AI Exchange"](https://owaspai.org/go/about/), retrieved 2026-08-17. The Exchange states 70 pages contributed to prEN 18282 and 70 pages to ISO/IEC 27090 through official liaison partnership, plus contribution to ISO/IEC 27091. These are the source's own claims and are not independently verified here.

<!-- sources:auto -->
## Sources

- [ISO/IEC 42001: AI Management Systems](https://www.iso.org/standard/42001)
<!-- /sources -->
