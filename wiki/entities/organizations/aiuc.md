---
type: entity
title: "AIUC"
address: c-000223
created: 2026-06-22
updated: 2026-06-22
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
---

# AIUC — Artificial Intelligence Underwriting Company

AIUC (Artificial Intelligence Underwriting Company) publishes and issues the [[aiuc-1|AIUC-1]] certification standard for enterprise AI agents, positioned by its publisher as "SOC 2 for AI agents." AIUC operates a two-actor audit model: AIUC issues the certification based on technical evaluation, while an ANAB-accredited auditor (Schellman as of February 2026) performs independent evidence collection and reporting. This split differs from ISO 27001 and SOC 2, where the auditing body issues the report directly.[^aiuc]

AIUC co-published the [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 crosswalk]] (May 2026) with the [[owasp|OWASP]] GenAI Security Project's Agentic Security Initiative. The document credits AIUC's founding standard lead and an AIUC reviewer among its expert reviewers.[^xwalk]

## Role in the wiki

| Use | Where |
|---|---|
| Publisher of the AIUC-1 standard | [[aiuc-1\|AIUC-1 AI Agent Certification Standard]] |
| Certification issuer (two-actor model) | [[aiuc-1\|AIUC-1 AI Agent Certification Standard]] |
| Co-publisher of the ASI crosswalk | [[owasp-asi-aiuc1-crosswalk\|OWASP ASI to AIUC-1 Crosswalk]] |
| Subject of standing assessment | [[aiuc-1-critical-evaluation\|AIUC-1 Critical Evaluation]] |

## Relations

- [[aiuc-1|AIUC-1 AI Agent Certification Standard]] — the standard AIUC publishes
- [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 Crosswalk]] — co-published with OWASP
- [[aiuc-1-critical-evaluation|AIUC-1 Critical Evaluation]] — concentration and freshness caveats on AIUC as a single issuer
- [[owasp|OWASP]] — crosswalk co-publisher

[^aiuc]: Schellman accreditation as first ANAB-accredited AIUC-1 auditor, [schellman.com](https://www.schellman.com/blog/news/schellman-becomes-the-first-accredited-auditor-for-aiuc-1); AIUC quarterly update detail, [aiuc-1.com/research](https://www.aiuc-1.com/research/quarterly-update-of-aiuc-1-q1-2026).
[^xwalk]: *AIUC-1: Crosswalks OWASP Top 10 For Agentic Applications*, v1.0, May 2026, OWASP GenAI Security Project, acknowledgements. Archived at `.raw/papers/owasp-agentic-top10-aiuc1-crosswalk-2026-05.pdf`.
