---
type: framework
title: "AIUC-1 AI Agent Certification Standard"
created: 2026-05-02
updated: 2026-06-22
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
  - "Schellman (first; ANAB-accredited Feb 3, 2026)"
  - "LRQA (pilot stage)"
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
  - "[[aiuc]]"
sources:
  - "https://aiuc-1.com"
  - "https://www.schellman.com/blog/news/schellman-becomes-the-first-accredited-auditor-for-aiuc-1"
  - "https://www.aiuc-1.com/research/quarterly-update-of-aiuc-1-q1-2026"
  - "https://www.uipath.com/newsroom/uipath-achieves-aiuc-1-certification"
  - "https://www.lrqa.com/en/latest-news/lrqa-partners-with-aiuc-1/"
---

# AIUC-1 — AI Agent Certification Standard

The first independent **security, safety, and reliability certification** for enterprise AI agents, positioned by its publisher as "SOC 2 for AI agents." Created by the Artificial Intelligence Underwriting Company ([[aiuc|AIUC]]) and audited by accredited third parties. The wiki's [[agentic-ai-security-cmm-2026|CMM]] cites AIUC-1 readiness as **D1 L4** evidence and AIUC-1 certification as **D1 L5** evidence.

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

Implication for the CMM: a D1 L5 "AIUC-1 certified" claim is implicitly *current at the most recent quarterly refresh*, not "ever certified." The CMM's L5 evidence requirement reflects this: *"AIUC-1 certified against the most recent quarterly refresh."*

## Standards crosswalks

AIUC publishes crosswalks against: [[iso-iec-42001|ISO 42001]], [[nist-ai-rmf|NIST AI RMF]], [[eu-ai-act|EU AI Act]], [[mitre-atlas|MITRE ATLAS]], [[owasp-llm-top-10|OWASP LLM Top 10]], [[owasp-aivss|OWASP AIVSS]], IBM AI Risk Atlas, Cisco AI Security & Safety, [[csa-maestro|CSA AICM]]. AIUC-1 is the only certification standard that maintains a current map across all of these, making it the **anchoring artifact** for the wiki's [[agentic-ai-security-cmm-crosswalk|standards crosswalk]] at L4+.

A dedicated bidirectional crosswalk against the [[owasp-agentic-ai-top-10|OWASP ASI Top 10]] was co-published with OWASP in May 2026; the wiki summary, including the eight observed AIUC-1 gaps and five newly validated mappings, is at [[owasp-asi-aiuc1-crosswalk|the OWASP ASI to AIUC-1 crosswalk]].

## Accreditation status (May 2026)

- **Schellman**: first ANAB-accredited AIUC-1 auditor (Feb 3, 2026). Was previously the first ANAB-accredited ISO 42001 certification body.
- **LRQA**: pilot stage as of 2026; not yet accredited.

**Two-actor audit model** (unusual): AIUC issues the certification based on technical evaluation; the accredited auditor (Schellman) provides independent evidence collection and reporting. This split is different from ISO 27001 / SOC 2, where the auditing body issues the report directly. A peer reviewer should know the model before accepting "AIUC-1 certified" as L5 evidence.

## Certified organizations (confirmed, as of 2026)

| Org | When | Notes |
|---|---|---|
| UiPath | March 2026 | First enterprise-automation cert; covered "more than 2,000 enterprise risk scenarios" |
| Intercom | 2026 | Fin agent |
| ElevenLabs | 2026 | First voice-AI certification; also a Technical Contributor to AIUC-1 |

## Direct quotes

- *"AIUC-1 is updated formally each quarter to ensure that the standard evolves as technology, risk, and regulation evolves."* (aiuc-1.com)
- *"The first security, safety, and reliability standard for AI agents."* (Schellman press release, Feb 3 2026)
- *"More than 2,000 enterprise risk scenarios."* (UiPath certification announcement)

## Use in this wiki

| Use | Where |
|---|---|
| D1 L4 evidence | [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] — "AIUC-1 readiness assessment complete" |
| D1 L5 evidence | [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] — "AIUC-1 certified against the most recent quarterly refresh" |
| Standards crosswalk anchor | [[agentic-ai-security-cmm-crosswalk\|Agentic AI Security CMM — Standards Crosswalk Matrix]] — six-pillar map |
| Validation comparator | [[agentic-cmm-vs-standards-validation\|Validation: Agentic AI Security CMM vs Widely Adopted Standards]] §2 |

## Caveats: what a peer reviewer would surface

> [!gap] Known concerns to flag
> 1. **Single accredited auditor (Schellman) is a capacity constraint.** LRQA's pilot is the only known second auditor; an enterprise that adopts the CMM L5 path is dependent on Schellman's queue.
> 2. **Two-actor audit model is unusual.** Issuer + auditor are different entities; the peer-review question is whether the accreditation regime (ANAB) and the issuer (AIUC) maintain independence.
> 3. **No single canonical safeguard count published.** "50+" is the public framing; the wiki should not invent a fixed number.
> 4. **Quarterly update cadence** means audit findings can age out fast — D1 L5 evidence has a freshness requirement that auditors and assessors need to enforce.
> 5. **AIUC is both standard-setter and certification issuer**, with Schellman as the evidence-collecting auditor. This deliberately splits a role that ISO and SOC 2 keep unified; reviewers may push on whether the split improves or weakens independence.

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
<!-- /sources -->
