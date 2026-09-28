---
type: entity
title: "ISO/IEC Standards Bodies"
created: 2026-04-30
updated: 2026-09-25
tags:
  - entities
  - organizations
  - standards-body
  - international
status: active
entity_type: organization
org_type: advisory
homepage: "https://www.iso.org"
role: "International standards body; publisher of ISO/IEC 42001 (AI Management Systems) and ISO/IEC 27000 series (information security)"
related:
  - "[[iso-iec-42001]]"
  - "[[eu-ai-act]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "https://www.iso.org/standard/44546.html"
  - "https://webstore.iec.ch/en/publication/108460"
---

# ISO/IEC — International Organization for Standardization

**Sources:** [ISO (homepage)](https://www.iso.org) · [IEC (homepage)](https://www.iec.ch) · [ISO/IEC 42001 standard page](https://www.iso.org/standard/81230.html)

**ISO** (International Organization for Standardization) and **IEC** (International Electrotechnical Commission) collaborate on information technology standards. Joint Technical Committee 1 (JTC 1) and its Subcommittee 42 (SC 42) on Artificial Intelligence are responsible for AI-related standards.

## AI Security Role

ISO/IEC produces the only **certifiable** AI management system standard ([[iso-iec-42001|ISO/IEC 42001:2023]]), giving it unique significance in compliance-driven contexts. ISO standards require paying membership and are not freely available, limiting community access compared to NIST or OWASP.

## Q1 2026 Activity

- **ISO/IEC 42006:2025** — requirements for bodies that audit and certify AI management systems, published in July 2025, before the quarter; enables consistent third-party certification[^iso42006]
- **ISO/IEC 27090** — AI cybersecurity guidance; FDIS ballot opened March 12, 2026; publication expected mid-2026; guidance-only (not certifiable)
- ISO/IEC 42001 base standard unchanged; no amendment or revision planned for 2026

## Key Standards

| Standard | Description | Status |
|---|---|---|
| ISO/IEC 42001:2023 | AI Management System (certifiable) | Published; active |
| ISO/IEC 42006:2025 | AI AIMS audit body requirements | Published 2025-07-07[^iso42006] |
| ISO/IEC 27090 | AI cybersecurity guidance | FDIS ballot (mid-2026 publication) |
| ISO/IEC 23894 | AI risk management guidance | Active |
| ISO/IEC 27001 | Information security management | Active; no AI-specific controls |

ISO 42001 and the 27090 FDIS draft were assessed against the Agentic AI Security CMM in [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]] — citation-only and paywall-bounded, since both documents require paid membership.

## EU AI Act Relationship

ISO/IEC 42001 is positioned as a primary compliance pathway for the [[eu-ai-act|EU AI Act]], but the harmonized standards being developed by CEN/CENELEC (prEN 18282, ETSI prEN 304 223) are separate from ISO 42001 and still in development (targeting end 2026).

## Notes

[^iso42006]: [ISO — ISO/IEC 42006:2025](https://www.iso.org/standard/44546.html), *Information technology — Artificial intelligence — Requirements for bodies providing audit and certification of artificial intelligence management systems*, edition 1, ISO/IEC JTC 1/SC 42, retrieved 2026-09-25. Publication date 2025-07-07, as ISO's open-data deliverables catalogue records it for that entry and [the IEC webstore listing](https://webstore.iec.ch/en/publication/108460) states it.
