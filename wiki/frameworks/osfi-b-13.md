---
type: framework
title: "OSFI Guideline B-13: Technology and Cyber Risk"
address: c-000051
created: 2026-05-14
updated: 2026-05-14
tags:
  - framework
  - osfi
  - canadian-banking
  - technology-risk
  - cyber-risk
  - secure-sdlc
  - regulatory
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
framework_class: regulatory-guideline
issuer: "[[osfi|OSFI — Office of the Superintendent of Financial Institutions (Canada)]]"
homepage: https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management
current_version: "Final (July 2022)"
first_published: 2022-07-31
last_substantive_update: 2022-07-31
authority: "Office of the Superintendent of Financial Institutions Act"
applicable_sectors: "Federally-regulated financial institutions (FRFIs) — banks, foreign bank branches, life insurance and fraternal companies, property and casualty companies, trust and loan companies"
aliases:
  - "Guideline B-13"
  - "B-13"
  - "OSFI B-13"
related:
  - "[[osfi]]"
  - "[[osfi-e-23-2027]]"
  - "[[canadian-bank-secure-sdlc-ai-assessor-scorecard]]"
  - "[[nist-ssdf]]"
  - "[[nist-sp-800-218a]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[red-teaming-capability-framework]]"
sources:
  - "https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management"
---

# OSFI Guideline B-13 — Technology and Cyber Risk Management

**OSFI Guideline B-13** — *Technology and Cyber Risk Management* — is Canada's federal regulatory expectations document for technology and cyber risk at federally-regulated financial institutions (FRFIs), issued by the [[osfi|Office of the Superintendent of Financial Institutions]] and effective **July 31, 2022**. The guideline is the **single most load-bearing OSFI instrument for any wiki claim about Canadian-bank secure-development or cyber-security expectations**.

B-13 is structured as **three domains** containing **17 high-level expectations**, each operationalized by named outcome statements with numbered sub-section identifiers. It is principles-based rather than prescriptive — OSFI defines outcomes, not specific implementations — and is intended to be applied proportionate to institution size, strategy, risk profile, and operational complexity.

## Scope and Applicability

B-13 applies to all federally-regulated financial institutions:

- Banks (Schedule I, II, III)
- Foreign bank branches
- Federally-regulated life insurance and fraternal companies
- Federally-regulated property and casualty insurance companies
- Federally-regulated trust and loan companies

It does NOT apply to provincially-regulated entities (provincially-chartered credit unions, FSRA-regulated Ontario entities), though provincial regulators frequently reference B-13 in their own guidance.

## The Three Domains

### Domain 1: Governance and Risk Management

| Sub-section | Title | Key expectations |
|---|---|---|
| **1.1** | Accountability and Organizational Structure | Senior Management assigns responsibility for technology and cyber risks to senior officers (1.1.1); structures established with clear roles, adequate people and financial resources (1.1.2); Senior Management has sufficient understanding of technology and cyber risks (1.1.2). |
| **1.2** | Technology and Cyber Strategy | Strategic plans anticipate and evolve with internal/external technology and cyber environment changes (1.2.1); clearly outline drivers, opportunities, vulnerabilities, threats and measures; include defined, measured, monitored, reported risk indicators. |
| **1.3** | Risk Management Framework | Framework aligned with enterprise risk management framework, regularly reviewed and refreshed (1.3.1); risk appetite captured with limits, thresholds, tolerance levels (1.3.2); processes for identifying, assessing, managing, monitoring and reporting technology and cyber risks (1.3.2). |

### Domain 2: Technology Operations and Resilience

Domain 2 contains nine sub-sections (2.1-2.9). The wiki-load-bearing ones for secure-development assessment:

| Sub-section | Title | Key expectations |
|---|---|---|
| **2.4** | **System Development Life Cycle** | **The direct OSFI hook for secure-SDLC expectations.** SDLC framework outlines processes and controls in each phase to achieve security and functionality (2.4.1); security requirements and expectations embedded in each SDLC phase (2.4.2); for Agile methods, incorporate SDLC and security-by-design principles throughout (2.4.2); acquired systems and software assessed for risk before implementation (2.4.4); coding principles and best practices defined including secure coding and third-party / open-source code handling (2.4.5). |
| **2.5** | Change and Release Management | Changes documented, assessed, tested, approved, implemented and verified in a controlled manner (2.5.1); segregation of duties prevents same person from developing, authorizing, executing, and moving code (2.5.2); traceability and integrity of change record (2.5.3). |
| **2.6** | Patch Management | Controlled and timely application of patches (2.6.1); clear stakeholder roles and responsibilities; patches tested before production deployment. |
| **2.7** | Incident and Problem Management | Timely identification and escalation of incidents (2.7.1); early warning indicators (2.7.2); incident classification by priority based on business-service impact; **periodic testing and exercises using plausible scenarios** (2.7.2); problem management with post-incident reviews, root cause and impact diagnostics (2.7.3). |

The remaining sub-sections (2.1 Architecture, 2.2 Asset Management, 2.3 Project Management, 2.8 Service Measurement and Monitoring, 2.9 Disaster Recovery) are standard technology-operations expectations and are not transcribed here.

### Domain 3: Cyber Security

| Sub-section | Title | Key expectations |
|---|---|---|
| **3.1** | Identify | Practices, capabilities, processes, and tools to identify and assess cyber security weaknesses (3.1.1); **intelligence-led threat assessments to test cyber security processes and controls** (3.1.2); **penetration testing and red teaming** with defined scope and risk controls (3.1.2); regular vulnerability assessments with defined frequency (3.1.3); continuous situational awareness of external cyber threat landscape (3.1.5); threat modelling and manual threat-hunting (3.1.6); regular employee testing for cyber-threat awareness (3.1.7). |
| **3.2** | Defend | Secure-by-design practices with preventive controls where feasible (3.2.1); strong cryptographic technologies with encryption-key protection (3.2.2); enhanced controls on critical and external-facing assets (3.2.3); multiple layers of cyber security controls defending at every stage of the attack life cycle (3.2.4); intrusion prevention and detection on network perimeter (3.2.4); timely risk-based patching with timelines and exception tracking (3.2.6); **Multi-Factor Authentication across external-facing channels and privileged accounts** (3.2.7); security configuration baselines with managed deviations (3.2.8). |
| **3.3** | Detect | Continuous security logging for technology assets and defence-tool layers (3.3.1); central tools for aggregating, correlating, managing security event logs (3.3.1); SIEM capabilities for continuous detection (3.3.2); defined roles for triage of high-risk cyber security alerts to rapidly contain threats (3.3.3). |
| **3.4** | Respond, Recover and Learn | Alignment and integration between cyber security, technology, crisis management, and communication protocols (3.4.1); **cyber incident taxonomy** including severity, category, type, root cause (3.4.2); cyber incident management process and playbooks (3.4.3); **forensic investigation for incidents where technology assets may have been materially exposed** (3.4.5). |

## Key Cross-References

B-13 does not stand alone. It is part of a broader OSFI guideline family that operationalizes the technology, cyber, and operational-risk surface for FRFIs:

- **OSFI Guideline B-10** (Third-Party Risk Management; previously titled "Outsourcing"; effective 2024-05-01) — handles vendor and third-party governance, including SaaS, IaaS, and AI vendor relationships. Referenced from B-13 §A.4.
- **OSFI Guideline E-21** (Operational Risk Management) — referenced from B-13 §A.4 and from incident/problem management expectations.
- **OSFI Guideline E-23 (2027)** ([[osfi-e-23-2027|Model Risk Management]]) — explicitly references B-13 in its Deployment section as the cybersecurity/infrastructure risk-assessment anchor. The integration point: a model going into production triggers a B-13-aligned technology/cyber risk assessment.

B-13 itself contains no explicit reference to E-23 — the integration is unidirectional (E-23 → B-13).

## Position in the Wiki's Framework Stack

B-13 is the **Canadian regulatory anchor** for any wiki secure-SDLC or cyber-defense content. Its relationship to peer instruments:

| Framework | Class | Relationship to B-13 |
|---|---|---|
| [[nist-ssdf\|NIST SSDF (SP 800-218)]] | US standard | Convergent secure-SDLC outcomes; B-13 §2.4 is the Canadian regulatory hook, SSDF is the US one. |
| [[nist-sp-800-218a\|NIST SP 800-218A]] | US AI extension | Convergent AI-specific scope; B-13 itself predates the AI-extension wave but its principles-based framing accommodates it. |
| [[microsoft-sdl\|Microsoft SDL]] | Vendor practice | One implementation pattern that satisfies B-13 §2.4 expectations. |
| **B-13** | **Canadian regulatory expectation** | **The instrument FRFIs must self-attest against in OSFI supervisory cycles.** |
| FFIEC IT Examination Handbook | US regulatory expectation | Cross-border peer for FRFIs with US operations. |
| EU DORA | EU regulation | Cross-border peer for FRFIs with EU operations; mandates threat-led pentesting (TLPT) which exceeds B-13 §3.1.2 expectations. |

For an assessor working a Canadian-bank engagement, B-13 is the regulatory floor — every secure-SDLC and cyber-defense control claim should map back to a B-13 sub-section, and gaps to B-13 expectations are at minimum **Major** findings in any [[canadian-bank-secure-sdlc-ai-assessor-scorecard|scorecard]].

## In the RA / CMM

B-13 maps onto the wiki's [[agentic-ai-security-cmm-2026|CMM]] across multiple domains:

| CMM domain | B-13 anchor |
|---|---|
| D1 — Governance & Accountability | Domain 1 (1.1 / 1.2 / 1.3) |
| D2 — Risk Management | 1.3 |
| D4 — Threat Modeling & Adversarial Defense | 3.1.6 (threat modelling); 3.1.2 (red teaming and pentest) |
| D6 — Supply Chain & Component Governance | 2.4.4 (acquired system risk assessment); 2.4.5 (third-party / open-source code) |
| D7 — Observability and Detection | 3.3.1, 3.3.2 (logging, SIEM); 2.8 |
| D8 — Cyber Defense | 3.2.x (Defend) |
| D9 — Incident Response & Recovery | 2.7, 3.4 |

## Placement in this wiki

- **First dedicated Canadian-financial-regulatory framework page.** Closes a long-standing gap noted in `[[canadian-bank-secure-sdlc-ai-assessor-scorecard|the scorecard]]`'s adjacent-gaps log.
- **Direct anchor for [[canadian-bank-secure-sdlc-ai-assessor-scorecard|the Canadian-bank assessor scorecard]]** — every Section A and Section D question in that scorecard maps to a B-13 sub-section identifier.
- **Cross-border peer citation** for any wiki claim about secure-SDLC regulatory anchors (alongside NIST SSDF for the US, EU DORA for the EU).

## See also

- [[osfi|Office of the Superintendent of Financial Institutions (Canada)]] — issuing organization
- [[osfi-e-23-2027|OSFI Guideline E-23 (2027) — Model Risk Management]] — sibling OSFI guideline, integration point at E-23 Deployment
- [[canadian-bank-secure-sdlc-ai-assessor-scorecard|Assessor's Quick Scorecard — Secure-SDLC and AI Practices for a Large Canadian Bank]] — operational instrument anchored on B-13
- [[nist-ssdf|NIST SSDF — Secure Software Development Framework]] — US peer secure-SDLC framework
- [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] — thesis on how secure-SDLC frameworks compose

<!-- sources:auto -->
## Sources

- [OSFI Guideline B-13: Technology and Cyber Risk](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management)
<!-- /sources -->
