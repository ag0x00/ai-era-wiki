---
type: entity
title: "AIUC"
address: c-000223
created: 2026-06-22
updated: 2026-09-25
tags:
  - entities
  - organizations
  - certification
  - aiuc-1
status: active
entity_type: organization
org_type: vendor
homepage: "https://aiuc-1.com"
role: "Artificial Intelligence Underwriting Company; publisher and certification issuer of the AIUC-1 AI agent certification standard"
scope_axis:
  - sec-of-ai
related:
  - "[[aiuc-1]]"
  - "[[owasp-asi-aiuc1-crosswalk]]"
  - "[[aiuc-1-critical-evaluation]]"
sources:
  - "[[.raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf]]"
verified: 2026-09-25
verified_against:
  - ".raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf"
verified_findings: 0
verified_note: "Read whole against the crosswalk PDF (cover, introduction, acknowledgements, supporters; full-text search), Schellman's announcement, AIUC's auditors page, Q1 update and 2025-07-23 launch release: co-publication claim re-attributed to OWASP with AIUC reviewers, SOC-2 quote sourced; none open"
---

# AIUC — Artificial Intelligence Underwriting Company

AIUC (Artificial Intelligence Underwriting Company) publishes and issues the [[aiuc-1|AIUC-1]] certification standard for enterprise AI agents, positioned by its publisher as "SOC-2 for AI agents." AIUC operates a two-actor audit model: AIUC issues the certification based on technical evaluation, while an auditor AIUC accredits, Schellman first among them, performs independent evidence collection and reporting. This split differs from ISO 27001 and SOC 2, where the auditing body issues the report directly.[^aiuc]

The [[owasp|OWASP]] GenAI Security Project's Agentic Security Initiative published the [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 crosswalk]] in May 2026, and the document credits AIUC's founding standard lead and a second AIUC reviewer among its expert reviewers.[^xwalk]

## Role in the wiki

| Use | Where |
|---|---|
| Publisher of the AIUC-1 standard | [[aiuc-1\|AIUC-1 AI Agent Certification Standard]] |
| Certification issuer (two-actor model) | [[aiuc-1\|AIUC-1 AI Agent Certification Standard]] |
| Reviewer of the ASI crosswalk | [[owasp-asi-aiuc1-crosswalk\|OWASP ASI to AIUC-1 Crosswalk]] |
| Subject of standing assessment | [[aiuc-1-critical-evaluation\|AIUC-1 Critical Evaluation]] |

## Relations

- [[aiuc-1|AIUC-1 AI Agent Certification Standard]] — the standard AIUC publishes
- [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 Crosswalk]] — published by OWASP, with AIUC reviewers
- [[aiuc-1-critical-evaluation|AIUC-1 Critical Evaluation]] — concentration and freshness caveats on AIUC as a single issuer
- [[owasp|OWASP]] — crosswalk publisher

[^aiuc]: AIUC's launch announcement, [PR Newswire](https://www.prnewswire.com/news-releases/the-artificial-intelligence-underwriting-company-launches-with-15m-to-help-enterprises-deploy-ai-with-confidence-302512447.html), 2025-07-23, for "SOC-2 for AI agents"; Schellman's announcement as the first accredited AIUC-1 auditor, after it became the first ANAB-accredited ISO/IEC 42001 certification body, [schellman.com](https://www.schellman.com/blog/news/schellman-becomes-the-first-accredited-auditor-for-aiuc-1); AIUC's statement that it issues every certificate and alone accredits AIUC-1 auditors, [AIUC — Accredited AIUC-1 auditors](https://standard.aiuc-1.com/accredited-auditors), retrieved 2026-09-24; AIUC quarterly update detail, [aiuc-1.com/research](https://www.aiuc-1.com/research/quarterly-update-of-aiuc-1-q1-2026).
[^xwalk]: *AIUC-1: Crosswalks OWASP Top 10 For Agentic Applications*, v1.0, May 2026, OWASP GenAI Security Project, acknowledgements. Archived at `.raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf`.
