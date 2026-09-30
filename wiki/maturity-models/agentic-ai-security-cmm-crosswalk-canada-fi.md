---
type: maturity-model-companion
title: "CMM: Canadian Regulated-Finance Crosswalk"
address: c-000133
created: 2026-05-26
updated: 2026-09-29
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
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[osfi-b-13]]"
  - "[[osfi-e-23-2027]]"
  - "[[google-cloud-agentic-security-profile]]"
sources:
  - "https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management"
  - "https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/third-party-risk-management-guideline"
  - "https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027"
  - "https://laws-lois.justice.gc.ca/eng/acts/P-8.6/FullText.html"
  - "https://www.legisquebec.gouv.qc.ca/en/document/cs/P-39.1"
  - "https://www.canada.ca/en/financial-consumer-agency/services/banking/rights-new-protections.html"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Read current official OSFI B-13, B-10 and E-23 guidance, branch amendment, PIPEDA, Québec Act, OPC, FCAC and Google locations; no archived source reached. Corrected B-10 branch date and FCAC citation."
---

# CMM: Canadian Regulated-Finance Crosswalk

A security architect at a Canadian federally regulated financial institution (FRFI) can use this crosswalk to reuse evidence from one agentic deployment assessment in the institution's technology, third-party, model-risk and privacy files. The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] is an internal evidence method. Its domain levels are not regulatory findings, certifications or substitutes for the institution's interpretation of an applicable requirement. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] defines the deployment boundary and evidence verdicts.

## Applicability

Select the obligations before mapping evidence. The institution's charter, business activity, supplier arrangement, personal information and decision workflow determine which anchors below matter.

- **OSFI supervision.** Guidelines B-13 and B-10 address technology and third-party risk at FRFIs, including branches subject to the qualifications in each guideline. E-23 applies from 1 May 2027. It defines models broadly, including AI/ML, but calls for full model inventory and lifecycle governance for models the institution identifies as carrying non-negligible inherent model risk.[^b13][^b10][^e23]
- **Federal privacy.** The Personal Information Protection and Electronic Documents Act (PIPEDA) section 4 covers personal information used in commercial activity and employee information connected to a federal work, undertaking or business. Federally regulated businesses remain subject to PIPEDA in provinces with substantially similar private-sector laws.[^pipeda][^opc-scope]
- **Québec privacy.** The Québec private-sector Act section 1 concerns personal information handled in carrying on an enterprise. Section 12.1 adds duties when the enterprise uses personal information to render a decision **based exclusively on automated processing**: notice by the time the decision is communicated, specified information on request, and an opportunity to present observations to staff able to review the decision. A recommendation followed by a substantive human decision needs a different trigger analysis. Customer residence alone does not decide the Act's territorial reach or its application to a federally regulated institution; the institution records counsel's analysis of the actual activity and workflow.[^qc]
- **Market conduct.** The Financial Consumer Protection Framework applies to banks, authorized foreign banks and federal credit unions. Other FRFIs must select their own applicable consumer provisions. Where the framework applies, customer communications and complaint handling need evidence distinct from the CMM score.[^fcac]

## Primary regulatory anchors

| Instrument | Applicable source and date | Evidence decision |
|---|---|---|
| OSFI B-13, Technology and Cyber Risk Management | Guideline, effective 1 January 2024[^b13] | Govern assets, secure development and change, identity, defence, logging and response under the institution's risk-based framework. |
| OSFI B-10, Third-Party Risk Management | Guideline, effective 1 May 2024; branches to adhere by 31 March 2025[^b10] | Assess each supplier arrangement, data location and subcontracting risk; retain the institution's accountability for outsourced activity. |
| OSFI E-23, Model Risk Management | Final guideline, effective 1 May 2027[^e23] | Identify and risk-rate models, then apply inventory, independent review, deployment and monitoring requirements in proportion to non-negligible model risk. |
| PIPEDA | Act, section 4 and Schedule 1[^pipeda] | Establish the purposes, permitted use and safeguards for personal information on the assessed data paths. |
| Québec private-sector Act | Sections 1 and 12.1, with section 12.1 in force since 22 September 2023[^qc] | Decide whether the enterprise and exclusively automated decision trigger are in scope; preserve notice and review evidence if they are. |
| FCAC Financial Consumer Protection Framework | In force since 30 June 2022 for the named banking entities[^fcac] | Map product communications and complaints to the institution's applicable market-conduct duties. |

B-13 and B-10 are supervisory guidelines, not agent-specific control catalogues. B-13 section 3.2.7 explicitly covers secure authentication, management and monitoring of system and service accounts; it does not prescribe a separate identity for each AI agent. B-10 section 2.2.2.3 calls for review of out-of-Canada arrangements and section 2.3.2 addresses data protection and location. It does not, by itself, impose a Canada-only processing rule.[^b13][^b10]

## Nine-domain evidence map

Each row identifies evidence that may support an external review. The cited CMM criterion remains governed by its deep dive; the external source does not adopt its pass threshold.

| CMM domain | External hook | Evidence to reuse and boundary of the match |
|---|---|---|
| [[agentic-ai-security-cmm-d1-governance\|CMM D1: Governance and Accountability]] | B-13 domain 1[^b13] | Use D1-REGISTER and D1-GATE records to locate ownership and approvals. |
| [[agentic-ai-security-cmm-d1-governance\|CMM D1: Governance and Accountability]] | B-10 principles 1–2[^b10] | Map supplier decisions into the institution's third-party risk framework. |
| [[agentic-ai-security-cmm-d1-governance\|CMM D1: Governance and Accountability]] | E-23 principles 1.1–1.2[^e23] | Map model decisions into the institution's model risk framework. |
| [[agentic-ai-security-cmm-d2-identity\|CMM D2: Identity and Authorization]] | B-13 section 3.2.7[^b13] | Use D2-IDENTITY and D2-TRACE to test agent and human attribution. B-13 names system and service accounts, but does not itself require the CMM's per-agent identity design. |
| [[agentic-ai-security-cmm-d3-control-least-agency\|CMM D3: Control and Least-Agency]] | B-13 sections 3.2.1 and 3.2.7; E-23 principle 2.3 for model-use constraints[^b13][^e23] | Use D3-ALLOW and D3-MEDIATE evidence for the deployed action path. The cited clauses do not define a tool-call approval protocol. |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|CMM D4: Runtime and Guardrails]] | B-13 sections 3.1.6, 3.2.1 and 3.2.9; E-23 principle 3.4 where model risk is in scope[^b13][^e23] | Use D4-INJECT-INDIRECT and D4-SANDBOX tests as deployment-specific cyber evidence. A model review alone does not prove a runtime refusal. |
| [[agentic-ai-security-cmm-d5-egress-network\|CMM D5: Egress and Network]] | B-13 principle 15; B-10 principle 7 for supplier-held data paths[^b13][^b10] | Use D5-ALLOW and D5-REACH tests to show reachable destinations. Supplier contracts and network tests address different parts of the route. |
| [[agentic-ai-security-cmm-d6-data-rag\|CMM D6: Data, Memory and RAG]] | PIPEDA Schedule 1 and B-13 section 3.2.5[^pipeda][^b13] | Use D6-ENTITLE and D6-STORE-ACCESS evidence for personal-data reach. E-23 model-data records and Québec automated-decision records require separate applicability decisions.[^e23][^qc] |
| [[agentic-ai-security-cmm-d7-observability\|CMM D7: Observability and Detection]] | B-13 principle 16; E-23 principle 3.6 where applicable[^b13][^e23] | Use D7-LOG and D7-SPANS to reconstruct actions. Model-performance monitoring and security-event detection need distinct tests. |
| [[agentic-ai-security-cmm-d8-supply-chain\|CMM D8: Engineering and Supply Assurance]] | B-13 principles 7–8 and section 3.2.9[^b13] | Use D8-DESIGN, D8-TEST and D8-RELEASE records for the released system. |
| [[agentic-ai-security-cmm-d8-supply-chain\|CMM D8: Engineering and Supply Assurance]] | B-10 principles 3–4[^b10] | Use the supplier review record, with its risk basis. |
| [[agentic-ai-security-cmm-d8-supply-chain\|CMM D8: Engineering and Supply Assurance]] | E-23 principles 3.4–3.5 where applicable[^e23] | Use the model validation record, with its risk basis. |
| [[agentic-ai-security-cmm-d9-operations\|CMM D9: Operations and Human Factors]] | B-13 principle 17 and B-10 principles 10–11[^b13][^b10] | Use D9-IR and D9-QUEUE-RUNBOOK evidence for response and human work. E-23 monitoring, Québec decision review and FCAC complaints retain their own tests.[^e23][^qc][^fcac] |

The D8 hooks do not mandate an AI bill of materials.[^b13][^b10][^e23]

## Assessment use

The architect records the applicability decision alongside the CMM report:

- Name the institution, deployment shape, business purpose, decision owner, data flows, suppliers and jurisdictions.
- Decide which external instruments and clauses apply. For E-23, record model identification, inherent risk rating and the resulting governance intensity. For Québec section 12.1, record whether personal information renders an exclusively automated decision and counsel's jurisdiction analysis.
- Index each reused artifact to the external clause and the CMM criterion. Mark missing supplier evidence, untested routes and obligations with no CMM match. An unanswerable CMM criterion remains unanswerable even if a supplier contract promises a control.
- Report domain current and target levels, confidence, gaps and a risk-and-effort case under the Handbook. Report regulatory conclusions in the institution's own compliance process; a domain level neither proves nor disproves compliance.

For a cloud deployment, B-10 requires an arrangement-level account of supplier operations, subcontractors, access, data location and exit. Check the actual model, runtime, guardrail and memory routes before asserting a processing location. The dated [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] owns product details; Google's current agent-locations page marks Toronto Agent Platform Memory Bank unsupported even though runtime and gateway are listed there.[^google-locations]

## Open questions

- **Québec application.** Sections 1 and 12.1 state the activity and decision triggers, but the sources reviewed here do not settle every territorial or federal-provincial overlap for an FRFI. Counsel must record the institution-specific conclusion and the role of any human intervention.
- **Opaque supplier model.** E-23 asks the institution to identify and risk-rate models, including third-party models, while OSFI's final-guideline response offers no general exception for a vendor that withholds documentation. The institution must decide what evidence, constraints or documented exception can support its chosen use.[^e23-letter]
- **Supplier location.** A Canada-only requirement must be traced to the institution's actual law, contract or policy. B-10 requires geographic risk assessment; a Canadian runtime region alone does not establish where every model, memory or security service processes data.[^b10][^google-locations]

## Notes

[^b13]: [OSFI, B-13 guideline](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management), sections 2.4–2.5 and 3.1–3.4; [OSFI's final-guideline letter](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/osfi-releases-final-guideline-b-13-technology-cyber-risk-management-letter-2022) states the 1 January 2024 effective date. Read 2026-09-29.
[^b10]: [OSFI, B-10 guideline](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/third-party-risk-management-guideline), sections 1–3, especially 2.2.2.3 and 2.3.2; [OSFI's consultation response](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/osfi-response-draft-guideline-b-10-consultation-feedback-third-party-risk-management) states the 1 May 2024 effective date; its [foreign-branch amendment](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/consequential-amendments-guidelines-b-10-b-13-related-foreign-branches) sets the later branch date. Read 2026-09-29.
[^e23]: [OSFI, final E-23 guideline](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027), overview, principles 2.1–2.3 and 3.1–3.6, published 2025-09-11 and effective 2027-05-01. The model inventory covers non-negligible inherent risk. Read 2026-09-29.
[^e23-letter]: [OSFI, final E-23 letter and consultation response](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027-letter), "Model definition and scope" and "Third-party vendors." Read 2026-09-29.
[^pipeda]: [Personal Information Protection and Electronic Documents Act](https://laws-lois.justice.gc.ca/eng/acts/P-8.6/FullText.html), section 4 and Schedule 1, especially principles 4.1, 4.5 and 4.7. Official consolidation current to 2026-09-21.
[^opc-scope]: [Office of the Privacy Commissioner, PIPEDA requirements in brief](https://www.priv.gc.ca/en/privacy-topics/privacy-laws-in-canada/the-personal-information-protection-and-electronic-documents-act-pipeda/pipeda_brief/), "Who is subject to PIPEDA?" Read 2026-09-29.
[^qc]: [Québec, Act respecting the protection of personal information in the private sector](https://www.legisquebec.gouv.qc.ca/en/document/cs/P-39.1), sections 1 and 12.1; [section 12.1 version history](https://www.legisquebec.gouv.qc.ca/fr/version/lc/P-39.1?code=se%3A12_1&history=20250414&langCont=en) gives 2023-09-22 as its effective date. Read 2026-09-29.
[^fcac]: [FCAC, Your banking rights and new protections](https://www.canada.ca/en/financial-consumer-agency/services/banking/rights-new-protections.html), framework scope and 2022-06-30 commencement; [FCAC, complaint-handling guideline](https://www.canada.ca/en/financial-consumer-agency/services/industry/commissioner-guidance/complaint-handling-procedures-banks/versions/2022-06-30-complaint-handling-procedures-banks.html), application to banks, authorized foreign banks and federal credit unions. Read 2026-09-29.
[^google-locations]: [Google Cloud, Supported locations for agents in Agent Platform](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations), read 2026-09-29. The Toronto row has a superscript whose note states that Memory Bank is not supported in that region; the table lists Agent Runtime and Agent Gateway among the services available in its region rows.
