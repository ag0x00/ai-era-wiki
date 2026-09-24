---
type: maturity-model-companion
title: "CMM: Canadian Regulated-Finance Crosswalk"
address: c-000133
created: 2026-05-26
updated: 2026-09-24
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
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[cmm-known-limitations]]"
  - "[[securing-agentic-coding]]"
sources:
  - "[[osfi-b-13]]"
  - "[[osfi-e-23-2027]]"
---

# Agentic AI Security CMM — Canadian Regulated-Finance Crosswalk

This crosswalk maps the jurisdiction-neutral [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-reference-architecture|RA]] to the expectations a **Canadian federally regulated financial institution (FRFI)** is examined against. It re-presents existing CMM evidence through one jurisdiction's lens and adds no requirement of its own.

**The CMM is jurisdiction-neutral; this is a Canadian lens.** The CMM and RA prescribe no jurisdiction's standards. Regulatory anchors are *options for re-presenting evidence*, applicable when a given regulator examines the institution. A Canadian FRFI is examined by **OSFI** (prudential, technology/cyber, model risk), **FCAC** (market conduct), and **OPC and provincial privacy regulators**, **not** by US bodies. Nothing in the CMM, and nothing here, imports FFIEC, GLBA, or NIST as a Canadian mandate. Those belong to the separate [[agentic-ai-security-cmm-crosswalk-us-fi|US crosswalk]] and bind only US-regulated entities. A multinational maps to each home regulator on its own terms.

**Two facts that frame the Canadian picture (2026).** First, **OSFI E-23 (Model Risk Management) is the load-bearing AI anchor**. It was finalized September 2025, takes effect **1 May 2027**, and explicitly covers AI/ML and generative-AI models and third-party/vendor models across the full lifecycle.[^e23] Second, **there is no in-force federal AI statute.** AIDA died with Bill C-27 at prorogation in January 2025 and is not returning in its old form, so it must not be cited as a mandate.[^aida] The Canadian regime governs the *frame* (governance, model risk, privacy, consumer protection) and is silent on agentic-AI-specific technical controls.

## The Canadian regulatory landscape (2026)

| Instrument | Regulator | Status | What it expects |
|---|---|---|---|
| **[[osfi-b-13\|Guideline B-13]]** — Technology & Cyber Risk Management | OSFI | in force Jan 1 2024[^b13] | Board/senior accountability for tech & cyber risk; tech risk framework; asset/config management; secure SDLC; operational resilience and recovery; cyber defense; third-party/cloud technology risk |
| **Guideline B-10** — Third-Party Risk Management | OSFI | effective May 1 2024[^b10] | Governance of third-party arrangements including cloud: due diligence, concentration risk, audit and data rights, exit planning. The instrument a cloud or AI-vendor relationship is examined against |
| **[[osfi-e-23-2027\|Guideline E-23]]** — Model Risk Management | OSFI | final Sep 2025; **effective 1 May 2027**[^e23] | Enterprise model-risk management for **all models including AI/ML and generative AI, internal or third-party**; risk-proportional lifecycle (design → independent review → deployment → monitoring → decommission); explainability and alternative controls for black-box methods |
| **Integrity and Security Guideline** | OSFI | in force Jan 31 2025[^intsec] | Protection against foreign interference, undue influence, and malicious activity; personnel background-check expectations; incident reporting to OSFI and law enforcement |
| **FIFAI / EDGE / AGILE; OSFI–FCAC AI Risk Report** | OSFI + FCAC | reports / principles (non-binding)[^fifai] | Responsible-AI principles (Explainability, Data, Governance, Ethics); catalogue of AI risks at FRFIs incl. generative AI, third-party concentration, and AI-enabled fraud |
| **PIPEDA** + OPC generative-AI principles | OPC | in force; OPC guidance[^pipeda] | Meaningful consent, accountability, transparency, limiting collection/use, safeguards over personal data used in AI |
| **Law 25** (Quebec) | CAI (Quebec) | ADM obligations in force since Sep 22 2023[^law25] | For decisions based *exclusively* on automated processing: inform the individual, disclose the personal information and principal factors used, and provide a right to human review |
| **Financial Consumer Protection Framework** | FCAC | in force[^fcac] | Fair treatment of consumers; prohibition of unfair/deceptive/abusive practices; complaint handling; appropriateness — the hook for consumer-facing AI |
| **CPCSC** (ITSP.10.171) | Cyber Centre / PSPC | phasing into defence procurement from 2026[^cpcsc] | NIST SP 800-171-based organizational cyber controls — **a defence-procurement certification, not an FRFI requirement** (relevant only if the entity is also a DND supplier) |

## Domain crosswalk, CMM domain to Canadian anchor

| CMM Domain | Primary Canadian anchor(s) | Note |
|---|---|---|
| **D1 Governance** | E-23 (model-risk governance, board accountability); B-13 (tech-risk governance); FIFAI EDGE-Governance; FCAC (market-conduct accountability) | E-23 + B-13 are the load-bearing governance anchors |
| **D2 Identity & Authorization** | Integrity & Security Guideline (personnel vetting — **human-level only**) | **No per-agent / non-human identity anchor exists** — the closest hook is human background checks |
| **D3 Control & Least-Agency** | (no clean anchor) | E-23 gestures at "autonomous decision-making" as a risk but sets no agent-action-scoping control |
| **D4 Runtime & Guardrails** | B-13 (cyber defense — infrastructure-level) | No regulator names [[prompt-injection\|prompt injection]], jailbreak, or runtime LLM guardrails |
| **D5 Egress & Network** | B-13 (network/cyber defense, generic) | Nothing addresses agent egress, tool-call traffic, or MCP |
| **D6 Data, Memory & RAG** | PIPEDA + OPC generative-AI principles; Quebec Law 25 (ADM disclosure); E-23 (data standards at design) | Privacy law governs consent and ADM disclosure, not RAG oversharing or memory poisoning |
| **D7 Observability & Detection** | E-23 (ongoing monitoring, drift detection, explainability); B-13 (incident detection) | E-23's monitoring + explainability expectations map cleanly here |
| **D8 Supply Chain & AI-BOM** | E-23 (third-party/vendor model governance); **B-10 (third-party arrangements, cloud concentration, audit and exit rights)**; B-13 (third-party technology risk); CPCSC (defence only) | E-23's third-party-model governance is the closest AI-BOM-adjacent hook, no AI-BOM mandate exists. B-10 is the vendor instrument, where single-vendor AI control becomes a concentration finding |
| **D9 Operations & Human Factors** | E-23 (change management, decommission, human oversight); FCAC (complaint handling, human recourse); Law 25 (right to human review) | The human-review and decommission expectations land here |

## Canadian regulatory omissions the CMM fills

> [!gap] No Canadian FI regulator prescribes agentic-AI-specific technical controls
> The regime governs the frame (governance, model risk, privacy, consumer protection), not the agent internals. As of 2026 it is silent on **per-agent / non-human identity (D2)**, **least-agency and tool-permission scoping (D3)**, **runtime guardrails and prompt-injection defense (D4)**, **agent egress and MCP traffic control (D5)**, **RAG oversharing, context-window leakage, and memory poisoning (D6 specifics)**, and **AI-BOM / model-and-tool provenance (D8)**. E-23 is the nearest hook for third-party-model governance, but it addresses model risk, not an agent control plane. For these gaps the CMM and RA are the de-facto control layer; the regulators set the surrounding governance, model-risk, privacy, and consumer obligations.

## Practical guidance for a Canadian FRFI

- **Treat E-23 as the primary framework, and start now.** It takes effect 1 May 2027 and covers AI/ML, generative AI, and third-party models. Its lifecycle runs design, independent review, deployment, monitoring and decommission, and maps onto CMM D1/D6/D7/D9 and D8. Building the CMM evidence now produces the E-23 documentation later.
- **Map B-13 to the technical planes.** B-13's tech-risk, resilience, and cyber-defense expectations re-present cleanly as CMM D1/D4/D5/D7/D8/D9 evidence.
- **If any member is a Quebec resident, Law 25's ADM disclosure applies.** A member-facing bot that makes or materially drives a decision based exclusively on automated processing must inform the member and offer human review. This is a D6/D1/D9 obligation regardless of where the FRFI is headquartered.
- **Consumer-facing AI is FCAC territory.** Fair treatment, non-deceptive behaviour, and accessible complaint handling are market-conduct expectations (D1/D9).
- **Do not adopt US frameworks as Canadian requirements.** FFIEC/GLBA/NIST are not Canadian mandates; cite them only if the entity is *also* US-regulated. CPCSC applies only to DND suppliers, not to FRFIs as such.
- **AIDA is not law.** Plan against OSFI, OPC, and provincial expectations, not the lapsed bill.
- **Data residency on Google's stack is a B-10 vendor fact, and it resolves differently for each deployment shape.** B-10 governs third-party arrangements and expects the institution to know where a provider processes its data; it names no jurisdiction, so a processing location outside Canada is a fact the third-party file records and justifies. No single Canadian region carries both Gemini model serving and Model Armor screening, so a Canadian-resident deployment spans Montréal and Toronto by construction, and the guardrail trades filter coverage for residency in Toronto. See [[#Google Cloud data residency]] below for the region-by-region detail and the coding harness's Canadian position.

## Google Cloud data residency

Google Cloud operates two Canadian regions, Montréal `northamerica-northeast1` and Toronto `northamerica-northeast2`.[^gcpregions] The whole-tenant assistant cannot meet a Canadian requirement: Workspace data regions cover Gemini prompts and responses both at rest and during processing, and the locations they offer are the United States or Europe.[^wsdatareg] A Gemini deployment on Google Cloud can meet a Canadian requirement, across both regions and on part of the model line: seven of the twenty-seven Google-model rows on the Agent Platform residency table carry a Montréal commitment, those for Gemini 3.5 Flash, Gemini 2.5 Flash at 128k and 1M, Gemini 2.5 Pro at 64k and 1M, `text-embedding-004` and `text-multilingual-embedding-002`. The table holds no 3.x flagship above 3.5 Flash and no tuning row, and the deployments-and-endpoints page lists Montréal under Americas without listing Toronto, so a Canadian model call resolves to Montréal.[^gcpmodels] Model Armor screens in Toronto and not in Montréal, at limited feature support: a Toronto template with data-residency compliance enabled keeps the Responsible AI, Sensitive Data Protection and prompt-injection-and-jailbreak filters and drops malicious-URL detection, multi-language detection, CSAM screening, image support and antivirus scanning, while floor settings restore every filter and stop enforcing residency for data in use and in transit. At-rest residency in Toronto holds under both.[^maregion] No single Canadian region therefore carries both the model and the guardrail, so a Canadian-resident deployment runs the agent runtime and Agent Gateway in Toronto beside Model Armor and calls a model served from Montréal.[^agentloc] The coding harness has no Canadian option to configure: partner models sit on a separate residency table with no Canada column, so no Anthropic model carries a Canadian ML-processing commitment, and `us-east5` is Claude Code's documented default region, as [[securing-agentic-coding|Securing Agentic Coding]] records.[^gcpmodels][^ccvertex]

## Open questions and watch items

- E-23's examination expectations for *generative and agentic* AI specifically are not yet detailed in supervisory practice. The guideline names AI/ML, but the agent-control specifics remain CMM-filled.
- FIFAI II's AGILE framework and any successor OSFI guidance may add agentic-AI expectations. Watch for an OSFI AI-specific guideline or letter.
- The OPC's PIPEDA-reform proposals (right to explanation, algorithmic impact assessments) lapsed with C-27. A future privacy reform could reintroduce them.
- Provincial privacy regimes beyond Quebec (for example, forthcoming Alberta and BC updates) may add ADM obligations.
- B-13's in-force date is unconfirmed against the sources this crosswalk cites; see [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] Part 3 and [[cmm-known-limitations|CMM Known Limitations]] item 11.
- B-10's effective date carries the same gap: the cited guideline page states a publication date and no effective or in-force date, so 2024-05-01 above is unconfirmed against the source and held as a watch item.
- Two facts behind the residency position above stay unresolved at source. Google gates Workspace data-regions coverage *during processing* by Workspace edition and points at a comparison table this crosswalk has not read, so which editions carry the processing half of that coverage is unconfirmed.[^wsdatareg] Model Armor is in scope for the Canada Data Boundary and Canada Data Boundary and Support control packages and absent from Data Boundary for Canada Protected B, which leaves open whether a Protected B workload runs an agent with no guardrail plane or places that plane outside the boundary.[^awcanada] The residency position names B-10 as the instrument that examines this evidence and asserts no date for it, since B-10's own effective date stays unconfirmed against its source.

## Notes

[^b13]: [OSFI — Guideline B-13: Technology and Cyber Risk Management](https://www.osfi-bsif.gc.ca/en/risks/technology-cyber-risk-management), in force 2024-01-01. OSFI's [guidance-library entry](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management) and this technology-and-cyber-risk page, both fetched 2026-09-15, state the publication date of 2022-07-31 and no in-force date, so the in-force date above is held as a watch item rather than a settled fact.
[^b10]: [OSFI — Guideline B-10: Third-Party Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/third-party-risk-management-guideline), effective 2024-05-01 per [[osfi|the OSFI page]] and [[canadian-bank-secure-sdlc-ai-assessor-scorecard|the scorecard's]] regulatory anchors. Fetched 2026-09-15: the guideline URL itself states only "Date: April 30, 2023" and no effective or in-force date, so the 2024-05-01 figure is held as a watch item rather than a settled fact, the same status as B-13's in-force date below.
[^e23]: [OSFI — Guideline E-23: Model Risk Management (2027)](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027), final Sep 2025, effective 2027-05-01. Defines "model" to include AI/ML; covers third-party models and the full lifecycle.
[^intsec]: [OSFI — Integrity and Security Guideline](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/integrity-security-guideline), in force 2025-01-31. Foreign-interference/insider protection; personnel background checks.
[^fifai]: [OSFI–FCAC Risk Report — AI Uses and Risks at FRFIs](https://www.osfi-bsif.gc.ca/en/about-osfi/reports-publications/osfi-fcac-risk-report-ai-uses-risks-federally-regulated-financial-institutions), 2024-09-24; and [FIFAI — A Canadian Perspective on Responsible AI (EDGE principles)](https://www.osfi-bsif.gc.ca/en/about-osfi/reports-publications/financial-industry-forum-artificial-intelligence-canadian-perspective-responsible-ai). Reports and principles, not binding guidance.
[^pipeda]: [OPC — Principles for responsible, trustworthy and privacy-protective generative AI](https://www.priv.gc.ca/en/privacy-topics/technology/artificial-intelligence/gd_principles_ai/), Dec 2023. Applies existing PIPEDA obligations to generative AI.
[^law25]: [Act respecting the protection of personal information in the private sector (Quebec, P-39.1)](https://www.legisquebec.gouv.qc.ca/en/document/cs/P-39.1) — Law 25 automated-decision obligations in force since 2023-09-22 (inform, disclose principal factors, right to human review).
[^fcac]: [FCAC — protecting financial consumers](https://www.canada.ca/en/financial-consumer-agency/corporate/about/protect.html). Financial Consumer Protection Framework; fair treatment, complaint handling.
[^cpcsc]: [Canadian Program for Cyber Security Certification (CPCSC)](https://www.canada.ca/en/public-services-procurement/services/industrial-security/security-requirements-contracting/cyber-security-certification-defence-suppliers-canada.html). NIST SP 800-171-based; defence-procurement certification, not an FRFI requirement.
[^aida]: AIDA (Part 3 of Bill C-27) died at prorogation in January 2025 and is not in force; the responsible minister confirmed in 2025 it will not return in its old form. No federal AI statute is currently in force in Canada.
[^gcpregions]: [Locations for machine learning services](https://docs.cloud.google.com/gemini-enterprise-agent-platform/machine-learning/general/locations), fetched 2026-09-16. Lists both Canadian regions under Canada: Montréal (`northamerica-northeast1`) and Toronto (`northamerica-northeast2`). Google locates three different things per region on three different pages, and those pages disagree about Toronto, so each residency claim on this page names the page that governs it: model serving on the deployments-and-endpoints page, platform features here, agent runtime and Agent Gateway on the agent-locations page.
[^gcpmodels]: [Gemini Enterprise Agent Platform — Data residency](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency) and [Deployments and endpoints](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations), both fetched 2026-09-16. The residency page's per-model ML-processing table carries one Canadian column, `Canada (northamerica-northeast1)`, marked Supported on seven of its twenty-seven Google-model rows. Partner models sit on a separate table whose columns are US multi-region, EU multi-region, Belgium, Netherlands, Singapore, Taiwan and Global, so the page states no Canadian commitment for any Anthropic model. The deployments-and-endpoints page lists Montréal under Americas and does not mention Toronto or `northamerica-northeast2`; that is an absence of a published model-serving location on that page, not a statement that Toronto is excluded from other Agent Platform services. The same page states that "Endpoints don't guarantee data residency or in-region ML processing" and that a global endpoint gives no control over which region processes a request.
[^maregion]: [Model Armor — Data residency and endpoints](https://docs.cloud.google.com/model-armor/data-residency) and [Feature availability for templates by region](https://docs.cloud.google.com/model-armor/feature-availability-by-region), both fetched 2026-09-16. Canada's residency row is `northamerica-northeast2 | Canada | Yes | Yes | Yes | Limited support`, and `northamerica-northeast1` appears on neither page. With data-residency compliance enabled, the Toronto row's supported-filters cell names Responsible AI, Sensitive Data Protection, and prompt injection and jailbreak, omits the malicious-URL filter the `us` multi-region row carries, and reads No against multi-language detection, CSAM support, image support and antivirus scanning, "because they rely on services that might process data outside the jurisdiction of your chosen Model Armor region". Under floor settings every feature is available and Canada's row reads `Yes | No | No` for data at rest, in use and in transit.
[^agentloc]: [Supported locations for agents in Agent Platform](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations), fetched 2026-09-16. Both Canadian rows read "v1 is supported for GA features. v1beta1 is supported for Preview features." The page marks Agent Gateway unsupported in asia-east2, asia-northeast3 and asia-southeast2 and in no other region, so Agent Gateway is available in both Canadian regions.
[^wsdatareg]: [Data covered by data regions](https://support.google.com/a/answer/9223653), fetched 2026-09-16. "With data regions, you can choose to store your covered data in a specific geographic location (the United States or Europe)." The covered-data table marks Gemini App and Google Workspace with Gemini, both for prompts and responses, covered at rest and during processing, and footnotes the processing column "Data covered during processing varies by Google Workspace edition." The string "Canada" occurs zero times in the page's raw HTML, so the policy states no Canadian location.
[^ccvertex]: [Claude Code on Google Cloud's Agent Platform](https://code.claude.com/docs/en/google-vertex-ai), fetched 2026-09-16. `CLOUD_ML_REGION` falls back to `us-east5` when unset, and the setup instructions export `CLOUD_ML_REGION=global`. The page names no Canadian region and states no retention, logging or training position.
[^awcanada]: [Supported products by control package](https://docs.cloud.google.com/assured-workloads/docs/supported-products), fetched 2026-09-16. Model Armor is listed in scope for the Canada Data Boundary and Canada Data Boundary and Support packages and is absent from the product set of Data Boundary for Canada Protected B.
