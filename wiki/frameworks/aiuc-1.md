---
type: framework
title: "AIUC-1 AI Agent Certification Standard"
created: 2026-05-02
updated: 2026-09-25
origin: aggregated
tags:
  - frameworks
  - certification
  - aiuc-1
  - agentic-ai
  - standards
status: developing
scope_axis:
  - sec-of-ai
domain: ai-governance
publisher: "Artificial Intelligence Underwriting Company (AIUC)"
first_published: 2025
update_cadence: quarterly
accredited_auditors:
  - "Schellman (accredited by AIUC from 2025-11-01)"
  - "Coalfire, BDO, Grant Thornton, Mastermind, Sensiba and A-LIGN (provisional, from May to August 2026)"
aliases:
  - "AIUC-1"
  - "AIUC standard"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[iso-iec-42001]]"
  - "[[nist-ai-rmf]]"
  - "[[eu-ai-act]]"
  - "[[mitre-atlas]]"
  - "[[owasp-aivss]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-asi-aiuc1-crosswalk]]"
  - "[[owasp-genai-crosswalk]]"
  - "[[aisi-uk]]"
  - "[[aiuc]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
sources:
  - "https://aiuc-1.com"
  - "https://www.schellman.com/blog/news/schellman-becomes-the-first-accredited-auditor-for-aiuc-1"
  - "https://www.aiuc-1.com/research/quarterly-update-of-aiuc-1-q1-2026"
  - "https://www.uipath.com/newsroom/uipath-achieves-aiuc-1-certification"
  - "https://www.lrqa.com/en/latest-news/lrqa-partners-with-aiuc-1/"
  - "https://standard.aiuc-1.com/accredited-auditors"
  - "https://standard.aiuc-1.com/re-certification-maintaining-aiuc-1"
  - "https://www.prnewswire.com/news-releases/the-artificial-intelligence-underwriting-company-launches-with-15m-to-help-enterprises-deploy-ai-with-confidence-302512447.html"
  - "https://www.intercom.com/blog/intercom-achieves-aiuc-1-certification/"
  - "https://www.aiuc-1.com/research/elevenlabs-achieves-aiuc-1-certification"
verified: 2026-09-24
verified_against: []
verified_findings: 0
verified_note: "D8-split read: the UiPath quotation against the live announcement, the D1 L4 and L5 statements against the core page's current D1 levels (stale quoted CMM wording replaced) and the eight-gap sentence against the crosswalk page; other claims not reread."
---

# AIUC-1 — AI Agent Certification Standard

The first independent **security, safety, and reliability certification** for enterprise AI agents, positioned by its publisher as "SOC-2 for AI agents" ([AIUC launch announcement, PR Newswire](https://www.prnewswire.com/news-releases/the-artificial-intelligence-underwriting-company-launches-with-15m-to-help-enterprises-deploy-ai-with-confidence-302512447.html), 2025-07-23). Created by the Artificial Intelligence Underwriting Company ([[aiuc|AIUC]]) and audited by accredited third parties. The wiki's [[agentic-ai-security-cmm-2026|CMM]] cites AIUC-1 readiness as **[[agentic-ai-security-cmm-d1-governance|D1]] L4** evidence and AIUC-1 certification as **D1 L5** evidence.

## Definition

AIUC-1 is structured as **six pillars** with 50+ underlying safeguards (the standard publishes individual safeguards but not a single canonical total, so the wiki should not invent one):

| Pillar | Focus |
|---|---|
| A. Data & Privacy | Lawful basis, data minimization, retention, cross-border |
| B. Security | Authentication, secrets, network, infrastructure |
| C. Safety | Harm prevention, refusal, content boundaries |
| D. Reliability | Failure modes, degradation behavior, observability |
| E. Accountability | Logging, traceability, incident response, audit trail |
| F. Society | Catastrophic-misuse / national-security externalities |

The Society pillar is **the one the wiki's CMM does not have an analogue for**, per the [[agentic-cmm-vs-standards-validation|validation page]] §2 AIUC-1 row.

## Update cadence: a moving target

AIUC-1 is updated formally each quarter. The Q1-2026 update modified 26 requirements and added evidence-category labels (legal / technical / operational / third-party) plus a capability-specific scoping questionnaire. The **Q2-2026 update** is themed *"Strengthening MCP security, agent permissions & third-party risk,"* directly relevant to the wiki's [[mcp-security|MCP Security]] and [[non-human-identity|NHI]] coverage.

Implication for the CMM: an AIUC-1 certificate is valid for one year, and the organization keeps it by submitting its agents for red-teaming each quarter ([AIUC — Re-certification](https://standard.aiuc-1.com/re-certification-maintaining-aiuc-1), retrieved 2026-09-24). D1-ASSURE, the assurance criterion at [[agentic-ai-security-cmm-d1-governance|D1]] L5, accepts a certificate within its one-year validity with each quarterly round completed, with ISO/IEC 42001 under active surveillance preferred, each evidenced at the cadence its own scheme runs.

## Standards crosswalks

AIUC publishes crosswalks against: [[iso-iec-42001|ISO 42001]], [[nist-ai-rmf|NIST AI RMF]], [[eu-ai-act|EU AI Act]], [[mitre-atlas|MITRE ATLAS]], [[owasp-llm-top-10|OWASP LLM Top 10]], [[owasp-aivss|OWASP AIVSS]], IBM AI Risk Atlas, Cisco AI Security & Safety, [[csa-maestro|CSA AICM]]. AIUC-1 maintains a current map across all of these; the wiki's [[agentic-ai-security-cmm-crosswalk|standards crosswalk]] uses AIUC-1 as its **anchoring artifact** at L4+.

OWASP's Agentic Security Initiative published a dedicated bidirectional crosswalk against the [[owasp-agentic-ai-top-10|OWASP ASI Top 10]] in May 2026, with two AIUC-1 reviewers, and [[owasp-asi-aiuc1-crosswalk|the OWASP ASI to AIUC-1 crosswalk]] summarizes it, including the eight observed AIUC-1 gaps and five newly validated mappings. Three of the eight, inter-agent authentication, runtime monitoring and cascading-failure containment, overlap the adversary classes in [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes]].

AIUC-1's entry in one third-party registry attributes it to the wrong publisher. The [[owasp-genai-crosswalk|GenAI Crosswalk]] framework registry records AIUC-1 as published by the [[aisi-uk|UK AI Safety Institute]] under Open Government Licence v3.0. The standard is instead written by the Artificial Intelligence Underwriting Company, a private firm that also issues the certification, with an auditor AIUC accredits collecting the evidence; the wiki's attribution above is correct. That registry record carries 147 mappings and, like every other row in the dataset, was never reviewed by a named person.

## Accreditation status (September 2026)

AIUC accredits every AIUC-1 auditor, issues every certificate, alone runs the quarterly technical testing, and conducts some audits itself, such as for new agent capabilities ([AIUC — Accredited AIUC-1 auditors](https://standard.aiuc-1.com/accredited-auditors), retrieved 2026-09-24).

- **Schellman**: accredited from 2025-11-01, the first AIUC-1 auditor, announced on 2026-02-03. Schellman was earlier the first ANAB-accredited ISO/IEC 42001 certification body.
- **Provisionally accredited**, from May to August 2026: Coalfire, BDO, Grant Thornton, Mastermind, Sensiba and A-LIGN.
- **LRQA** announced a partnership to pilot AIUC-1 ([LRQA](https://www.lrqa.com/en/latest-news/lrqa-partners-with-aiuc-1/)), and it does not appear on AIUC's list.

**Two-actor audit model** (unusual): AIUC issues the certification based on technical evaluation; an auditor AIUC accredits provides independent evidence collection and reporting. This split is different from ISO 27001 / SOC 2, where the auditing body issues the report directly. A peer reviewer should know the model before accepting "AIUC-1 certified" as L5 evidence.

## Certified organizations (confirmed, as of 2026)

| Org | When | Notes |
|---|---|---|
| UiPath | March 2026 | First enterprise-automation cert; covered "more than 2,000 enterprise risk scenarios" |
| Intercom | December 2025 | Fin, its AI agent; a founding technical contributor ([Intercom](https://www.intercom.com/blog/intercom-achieves-aiuc-1-certification/), 2025-12-08) |
| ElevenLabs | February 2026 | The "first AI voice company to achieve AIUC-1 certification"; a Technical Contributor ([AIUC](https://www.aiuc-1.com/research/elevenlabs-achieves-aiuc-1-certification), 2026-02-18) |

## Direct quotes

- *"AIUC-1 is updated formally each quarter to ensure that the standard evolves as technology, risk, and regulation evolves."* (aiuc-1.com)
- *"The first security, safety, and reliability standard for AI agents."* (Schellman press release, Feb 3 2026)
- *"More than 2,000 enterprise risk scenarios."* ([UiPath certification announcement](https://www.uipath.com/newsroom/uipath-achieves-aiuc-1-certification), 2026-03-09)

## Use in this wiki

| Use | Where |
|---|---|
| D1 L4 evidence | [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] — a readiness assessment against a recognized assurance scheme |
| D1 L5 evidence | [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] — D1-ASSURE: current third-party assurance, an AIUC-1 certificate within its one-year validity accepted |
| Standards crosswalk anchor | [[agentic-ai-security-cmm-crosswalk\|Agentic AI Security CMM — Standards Crosswalk Matrix]] — six-pillar map |
| Validation comparator | [[agentic-cmm-vs-standards-validation\|Validation: Agentic AI Security CMM vs Widely Adopted Standards]] §2 |

## Caveats: what a peer reviewer would surface

> [!gap] Known concerns to flag
> 1. **Testing capacity runs through AIUC alone.** Six firms gained provisional accreditation as AIUC-1 auditors beside Schellman during 2026, and only AIUC runs the quarterly technical testing a certificate needs, so an enterprise on the AIUC-1 path depends on AIUC's testing queue.
> 2. **Two-actor audit model is unusual.** Issuer and auditor are different entities in most audits, AIUC both issues the certificate and accredits the auditor, and AIUC conducts some audits itself, so the peer-review question is whether that arrangement keeps the audit independent.
> 3. **No single canonical safeguard count published.** "50+" is the public framing; the wiki should not invent a fixed number.
> 4. **Quarterly update cadence** means audit findings can age out fast — D1 L5 evidence has a freshness requirement that auditors and assessors need to enforce.
> 5. **AIUC is both standard-setter and certification issuer**, with an auditor it accredits collecting the evidence. This deliberately splits a role that ISO and SOC 2 keep unified; reviewers may push on whether the split improves or weakens independence.

## See Also

- [[aiuc|AIUC]] — publisher and certification issuer
- [[owasp-asi-aiuc1-crosswalk|OWASP ASI to AIUC-1 Crosswalk]] — bidirectional map to the OWASP ASI Top 10, with the eight observed AIUC-1 gaps
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — D1 L4/L5 evidence anchor
- [[agentic-ai-security-cmm-crosswalk|Agentic AI Security CMM — Standards Crosswalk Matrix]] — six-pillar mapping
- [[agentic-cmm-vs-standards-validation|Validation: Agentic AI CMM vs Widely Adopted Standards]] — §2 AIUC-1 row
- [[iso-iec-42001|ISO/IEC 42001]] — paired certification target

<!-- sources:auto -->
## Sources

- [aiuc-1.com](https://aiuc-1.com)
- [schellman.com](https://www.schellman.com/blog/news/schellman-becomes-the-first-accredited-auditor-for-aiuc-1)
- [aiuc-1.com](https://www.aiuc-1.com/research/quarterly-update-of-aiuc-1-q1-2026)
- [uipath.com](https://www.uipath.com/newsroom/uipath-achieves-aiuc-1-certification)
- [lrqa.com](https://www.lrqa.com/en/latest-news/lrqa-partners-with-aiuc-1/)
- [standard.aiuc-1.com](https://standard.aiuc-1.com/accredited-auditors)
- [standard.aiuc-1.com](https://standard.aiuc-1.com/re-certification-maintaining-aiuc-1)
- [prnewswire.com](https://www.prnewswire.com/news-releases/the-artificial-intelligence-underwriting-company-launches-with-15m-to-help-enterprises-deploy-ai-with-confidence-302512447.html)
- [intercom.com](https://www.intercom.com/blog/intercom-achieves-aiuc-1-certification/)
- [aiuc-1.com](https://www.aiuc-1.com/research/elevenlabs-achieves-aiuc-1-certification)
<!-- /sources -->
