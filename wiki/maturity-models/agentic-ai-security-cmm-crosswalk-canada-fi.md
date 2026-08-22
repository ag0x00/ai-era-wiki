---
type: maturity-model-companion
title: "CMM: Canadian Regulated-Finance Crosswalk"
address: c-000133
created: 2026-05-26
updated: 2026-06-23
tags:
  - maturity-models
  - crosswalk
  - canada
  - financial-services
  - regulatory
  - sec-of-ai
status: developing
origin: produced
target: "[[agentic-ai-security-cmm-2026]]"
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[osfi-b-13]]"
  - "[[osfi-e-23-2027]]"
sources:
  - "[[osfi-b-13]]"
  - "[[osfi-e-23-2027]]"
---

# Agentic AI Security CMM — Canadian Regulated-Finance Crosswalk

This crosswalk maps the jurisdiction-neutral [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-reference-architecture|RA]] to the expectations a **Canadian federally regulated financial institution (FRFI)** is examined against. It is one jurisdictional lens, not a new requirement set.

**The CMM is jurisdiction-neutral; this is a Canadian lens.** The CMM and RA prescribe no jurisdiction's standards. Regulatory anchors are *options for re-presenting evidence*, applicable when a given regulator examines the institution. A Canadian FRFI is examined by **OSFI** (prudential, technology/cyber, model risk), **FCAC** (market conduct), and **OPC and provincial privacy regulators**, **not** by US bodies. Nothing in the CMM, and nothing here, imports FFIEC, GLBA, or NIST as a Canadian mandate. Those belong to the separate [[agentic-ai-security-cmm-crosswalk-us-fi|US crosswalk]] and bind only US-regulated entities. A multinational maps to each home regulator on its own terms.

**Two facts that frame the Canadian picture (2026).** First, **OSFI E-23 (Model Risk Management) is the load-bearing AI anchor**. It was finalized September 2025, takes effect **1 May 2027**, and explicitly covers AI/ML and generative-AI models and third-party/vendor models across the full lifecycle.[^e23] Second, **there is no in-force federal AI statute.** AIDA died with Bill C-27 at prorogation in January 2025 and is not returning in its old form, so it must not be cited as a mandate.[^aida] The Canadian regime governs the *frame* (governance, model risk, privacy, consumer protection) and is silent on agentic-AI-specific technical controls.

## The Canadian regulatory landscape (2026)

| Instrument | Regulator | Status | What it expects |
|---|---|---|---|
| **[[osfi-b-13\|Guideline B-13]]** — Technology & Cyber Risk Management | OSFI | in force Jan 1 2024[^b13] | Board/senior accountability for tech & cyber risk; tech risk framework; asset/config management; secure SDLC; operational resilience and recovery; cyber defense; third-party/cloud technology risk |
| **[[osfi-e-23-2027\|Guideline E-23]]** — Model Risk Management | OSFI | final Sep 2025; **effective 1 May 2027**[^e23] | Enterprise model-risk management for **all models including AI/ML and generative AI, internal or third-party**; risk-proportional lifecycle (design → independent review → deployment → monitoring → decommission); explainability and alternative controls for black-box methods |
| **Integrity and Security Guideline** | OSFI | in force Jan 31 2025[^intsec] | Protection against foreign interference, undue influence, and malicious activity; personnel background-check expectations; incident reporting to OSFI and law enforcement |
| **FIFAI / EDGE / AGILE; OSFI–FCAC AI Risk Report** | OSFI + FCAC | reports / principles (non-binding)[^fifai] | Responsible-AI principles (Explainability, Data, Governance, Ethics); catalogue of AI risks at FRFIs incl. generative AI, third-party concentration, and AI-enabled fraud |
| **PIPEDA** + OPC generative-AI principles | OPC | in force; OPC guidance[^pipeda] | Meaningful consent, accountability, transparency, limiting collection/use, safeguards over personal data used in AI |
| **Law 25** (Quebec) | CAI (Quebec) | ADM obligations in force since Sep 22 2023[^law25] | For decisions based *exclusively* on automated processing: inform the individual, disclose the personal information and principal factors used, and provide a right to human review |
| **Financial Consumer Protection Framework** | FCAC | in force[^fcac] | Fair treatment of consumers; prohibition of unfair/deceptive/abusive practices; complaint handling; appropriateness — the hook for consumer-facing AI |
| **CPCSC** (ITSP.10.171) | Cyber Centre / PSPC | phasing into defence procurement from 2026[^cpcsc] | NIST SP 800-171-based organizational cyber controls — **a defence-procurement certification, not an FRFI requirement** (relevant only if the entity is also a DND supplier) |

## Domain crosswalk (CMM domain → Canadian anchor)

| CMM Domain | Primary Canadian anchor(s) | Note |
|---|---|---|
| **D1 Governance** | E-23 (model-risk governance, board accountability); B-13 (tech-risk governance); FIFAI EDGE-Governance; FCAC (market-conduct accountability) | E-23 + B-13 are the load-bearing governance anchors |
| **D2 Identity & Authorization** | Integrity & Security Guideline (personnel vetting — **human-level only**) | **No per-agent / non-human identity anchor exists** — the closest hook is human background checks |
| **D3 Control & Least-Agency** | (no clean anchor) | E-23 gestures at "autonomous decision-making" as a risk but sets no agent-action-scoping control |
| **D4 Runtime & Guardrails** | B-13 (cyber defense — infrastructure-level) | No regulator names [[prompt-injection\|prompt injection]], jailbreak, or runtime LLM guardrails |
| **D5 Egress & Network** | B-13 (network/cyber defense, generic) | Nothing addresses agent egress, tool-call traffic, or MCP |
| **D6 Data, Memory & RAG** | PIPEDA + OPC generative-AI principles; Quebec Law 25 (ADM disclosure); E-23 (data standards at design) | Privacy law governs consent and ADM disclosure, not RAG oversharing or memory poisoning |
| **D7 Observability & Detection** | E-23 (ongoing monitoring, drift detection, explainability); B-13 (incident detection) | E-23's monitoring + explainability expectations map cleanly here |
| **D8 Supply Chain & AI-BOM** | E-23 (third-party/vendor model governance); B-13 (third-party technology risk); CPCSC (defence only) | E-23's third-party-model governance is the closest AI-BOM-adjacent hook; no AI-BOM mandate exists |
| **D9 Operations & Human Factors** | E-23 (change management, decommission, human oversight); FCAC (complaint handling, human recourse); Law 25 (right to human review) | The human-review and decommission expectations land here |

## Canadian regulatory omissions the CMM fills

> [!gap] No Canadian FI regulator prescribes agentic-AI-specific technical controls
> The regime governs the frame (governance, model risk, privacy, consumer protection), not the agent internals. As of 2026 it is silent on **per-agent / non-human identity (D2)**, **least-agency and tool-permission scoping (D3)**, **runtime guardrails and prompt-injection defense (D4)**, **agent egress and MCP traffic control (D5)**, **RAG oversharing, context-window leakage, and memory poisoning (D6 specifics)**, and **AI-BOM / model-and-tool provenance (D8)**. E-23 is the nearest hook for third-party-model governance, but it addresses model risk, not an agent control plane. For these gaps the CMM and RA are the de-facto control layer; the regulators set the surrounding governance, model-risk, privacy, and consumer obligations.

## Practical guidance for a Canadian FRFI

- **Treat E-23 as the spine, and start now.** It takes effect 1 May 2027 and covers AI/ML, generative AI, and third-party models. Its design → independent-review → deployment → monitoring → decommission lifecycle maps onto CMM D1/D6/D7/D9 and D8. Building the CMM evidence now produces the E-23 documentation later.
- **Map B-13 to the technical planes.** B-13's tech-risk, resilience, and cyber-defense expectations re-present cleanly as CMM D1/D4/D5/D7/D8/D9 evidence.
- **If any member is a Quebec resident, Law 25's ADM disclosure applies.** A member-facing bot that makes or materially drives a decision based exclusively on automated processing must inform the member and offer human review. This is a D6/D1/D9 obligation regardless of where the FRFI is headquartered.
- **Consumer-facing AI is FCAC territory.** Fair treatment, non-deceptive behaviour, and accessible complaint handling are market-conduct expectations (D1/D9).
- **Do not adopt US frameworks as Canadian requirements.** FFIEC/GLBA/NIST are not Canadian mandates; cite them only if the entity is *also* US-regulated. CPCSC applies only to DND suppliers, not to FRFIs as such.
- **AIDA is not law.** Plan against OSFI, OPC, and provincial expectations, not the lapsed bill.

## Open questions and watch items

- E-23's examination expectations for *generative and agentic* AI specifically are not yet detailed in supervisory practice. The guideline names AI/ML, but the agent-control specifics remain CMM-filled.
- FIFAI II's AGILE framework and any successor OSFI guidance may add agentic-AI expectations. Watch for an OSFI AI-specific guideline or letter.
- The OPC's PIPEDA-reform proposals (right to explanation, algorithmic impact assessments) lapsed with C-27. A future privacy reform could reintroduce them.
- Provincial privacy regimes beyond Quebec (for example, forthcoming Alberta and BC updates) may add ADM obligations.

## Notes

[^b13]: [OSFI — Guideline B-13: Technology and Cyber Risk Management](https://www.osfi-bsif.gc.ca/en/risks/technology-cyber-risk-management), in force 2024-01-01.
[^e23]: [OSFI — Guideline E-23: Model Risk Management (2027)](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027), final Sep 2025, effective 2027-05-01. Defines "model" to include AI/ML; covers third-party models and the full lifecycle.
[^intsec]: [OSFI — Integrity and Security Guideline](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/integrity-security-guideline), in force 2025-01-31. Foreign-interference/insider protection; personnel background checks.
[^fifai]: [OSFI–FCAC Risk Report — AI Uses and Risks at FRFIs](https://www.osfi-bsif.gc.ca/en/about-osfi/reports-publications/osfi-fcac-risk-report-ai-uses-risks-federally-regulated-financial-institutions), 2024-09-24; and [FIFAI — A Canadian Perspective on Responsible AI (EDGE principles)](https://www.osfi-bsif.gc.ca/en/about-osfi/reports-publications/financial-industry-forum-artificial-intelligence-canadian-perspective-responsible-ai). Reports and principles, not binding guidance.
[^pipeda]: [OPC — Principles for responsible, trustworthy and privacy-protective generative AI](https://www.priv.gc.ca/en/privacy-topics/technology/artificial-intelligence/gd_principles_ai/), Dec 2023. Applies existing PIPEDA obligations to generative AI.
[^law25]: [Act respecting the protection of personal information in the private sector (Quebec, P-39.1)](https://www.legisquebec.gouv.qc.ca/en/document/cs/P-39.1) — Law 25 automated-decision obligations in force since 2023-09-22 (inform, disclose principal factors, right to human review).
[^fcac]: [FCAC — protecting financial consumers](https://www.canada.ca/en/financial-consumer-agency/corporate/about/protect.html). Financial Consumer Protection Framework; fair treatment, complaint handling.
[^cpcsc]: [Canadian Program for Cyber Security Certification (CPCSC)](https://www.canada.ca/en/public-services-procurement/services/industrial-security/security-requirements-contracting/cyber-security-certification-defence-suppliers-canada.html). NIST SP 800-171-based; defence-procurement certification, not an FRFI requirement.
[^aida]: AIDA (Part 3 of Bill C-27) died at prorogation in January 2025 and is not in force; the responsible minister confirmed in 2025 it will not return in its old form. No federal AI statute is currently in force in Canada.
