---
type: maturity-model-companion
title: "US Financial Institution Crosswalk"
address: c-000134
created: 2026-05-26
updated: 2026-09-29
tags:
  - maturity-models
  - crosswalk
  - united-states
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
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[cmm-known-limitations]]"
sources:
  - "https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm"
  - "https://www.occ.gov/news-issuances/bulletins/2026/bulletin-2026-13.html"
  - "https://www.fdic.gov/news/financial-institution-letters/2026/agencies-revise-interagency-model-risk-management-guidance"
  - "https://ncua.gov/regulation-supervision/regulatory-compliance-resources/artificial-intelligence-ai"
  - "https://www.dfs.ny.gov/system/files/documents/2023/12/rf23_nycrr_part_500_amend02_20231101.pdf"
  - "https://www.ftc.gov/business-guidance/resources/ftc-safeguards-rule-what-your-business-needs-know"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Live primary sources checked; FDIC page returned 403, joint Fed/OCC text corroborated; no archive opened."
---

# US Financial Institution Crosswalk

For a defined agent deployment, identify the applicable US requirements and supervisory guidance before reusing evidence from the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]. The CMM is an internal assessment method. A domain level neither proves regulatory compliance nor substitutes for an institution's legal and supervisory scope decision. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] defines the assessment record.

## Regulator and scope selector

Select the institution's charter, primary federal supervisor, deposit or share insurance status, relevant activities, and state authorizations before using the evidence map. Subsidiaries and service providers may have different coverage from the institution they serve.

| Institution or authorization | Starting point for security requirements | Scope check |
|---|---|---|
| National bank or federal savings association | OCC information-security guidelines and applicable OCC supervision.[^bank-security] | Confirm entity and activity in scope; apply FFIEC examination guidance as examiner guidance, not a separate regulation.[^ffiec] |
| State member bank or relevant bank holding company | Federal Reserve information-security guidelines.[^bank-security] | Confirm the regulated entity and applicable Federal Reserve rule. |
| FDIC-supervised state nonmember bank | FDIC information-security guidelines.[^bank-security] | Confirm FDIC supervisory status rather than inferring it from deposit insurance alone. |
| Federally insured credit union | NCUA security-program rule, member-information guidance, and cyber-incident reporting rule.[^ncua][^incident] | A state-chartered credit union also checks its state supervisor; NCUA has issued no AI-specific regulation.[^ncua-ai] |
| Financial institution within FTC jurisdiction | FTC Safeguards Rule, 16 CFR Part 314.[^ftc] | The FTC rule covers institutions under its GLBA authority, including some non-federally insured credit unions; it is not the default rule for a federally insured bank or credit union. |
| Entity with New York DFS authorization | 23 NYCRR Part 500, subject to its own exemptions.[^nydfs] | Test whether the entity operates, or must operate, under a Banking, Insurance, or Financial Services Law authorization. Other federal regulation does not by itself remove DFS coverage; a New York customer alone does not establish it. |

For a bank subject to the Federal Reserve, OCC, or FDIC model-risk framework, check the [17 April 2026 revised interagency guidance](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm) separately. It is expected to be most relevant above **\$30 billion in total assets**. It can also matter at or below that size where model exposure is significant because of model prevalence, complexity, or activities beyond traditional community banking.[^mrm] The agencies expressly exclude **generative and agentic AI models from this guidance**, while advising banks to use their wider risk-management and governance practices for tools outside it. Its non-prescriptive principles do not create an agentic-AI security requirement or an exemption from otherwise applicable obligations.[^mrm]

## Primary anchors

- **Information security.** The selected agency's GLBA security rule establishes the institution's baseline. For institutions supervised by a FFIEC member, the [Information Security](https://ithandbook.ffiec.gov/it-booklets/information-security), [Architecture, Infrastructure, and Operations](https://ithandbook.ffiec.gov/it-booklets/architecture-infrastructure-and-operations), and [Development, Acquisition, and Maintenance](https://ithandbook.ffiec.gov/it-booklets/development-acquisition-and-maintenance/) booklets describe examination considerations for governance, architecture, development, operations, and third parties.[^ffiec]
- **Access.** [FFIEC Authentication and Access to Financial Institution Services and Systems (2021)](https://www.ffiec.gov/sites/default/files/media/press-releases/2021/authentication-and-access-to-financial-institution-services-and-systems.pdf) supports risk-based authentication and layered access controls. Its discussion of machine-to-machine access can inform an agent assessment; it does not define CMM per-agent criteria.[^auth]
- **Credit unions.** [NCUA's AI resource](https://ncua.gov/regulation-supervision/regulatory-compliance-resources/artificial-intelligence-ai) says technology-neutral rules remain applicable to AI, and identifies internal controls, monitoring, and third-party due diligence as examination concerns. Part 748 Appendix A is guidance for meeting security-program obligations, not an independently enforceable AI control catalogue.[^ncua-ai][^ncua]
- **New York.** For a covered entity, Part 500 sets its own risk assessment, cybersecurity program, access, monitoring, third-party, and response duties. DFS's [AI cybersecurity guidance of October 2024](https://www.dfs.ny.gov/industry-guidance/industry-letters/il20241016-cyber-risks-ai-and-strategies-combat-related-risks) describes AI risks within that existing rule; it does not make every CMM criterion a DFS requirement.[^nydfs-ai]

## Nine-domain evidence map

The table identifies **candidate evidence reuse**, not equivalence. The cited source may cover a general security outcome while the CMM tests an agent-specific implementation. Apply a row only after the selector above establishes the relevant source.

| CMM domain | Evidence to reuse | External anchor and limit |
|---|---|---|
| [[agentic-ai-security-cmm-d1-governance\|CMM D1: Governance and Accountability]] | Deployment register and production approval (D1-REGISTER, D1-GATE). | Information-security governance and risk assessment under the institution's GLBA implementation; Part 500 for a covered entity. The CMM tier is internal.[^bank-security][^ncua][^ftc][^nydfs] |
| [[agentic-ai-security-cmm-d2-identity\|CMM D2: Identity and Authorization]] | Agent identity and authorization policy (D2-IDENTITY, D2-AUTHZ). | FFIEC access guidance and applicable security-program access controls. Neither automatically establishes a regulatory per-agent identity requirement.[^auth][^bank-security] |
| [[agentic-ai-security-cmm-d3-control-least-agency\|CMM D3: Control and Least-Agency]] | Allowed actions and tool-call enforcement, with denied-call evidence (D3-ALLOW, D3-MEDIATE). | Least-privilege and change-control evidence can support FFIEC examination. The CMM's action tiers and mediation tests are more specific.[^ffiec] |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|CMM D4: Runtime and Guardrails]] | Prompt-injection tests and isolation results (D4-INJECT-INDIRECT, D4-SANDBOX). | General application-security and layered-control expectations; DFS AI guidance for covered entities. A prompt-injection test is CMM evidence, not a named FFIEC test.[^ffiec][^nydfs-ai] |
| [[agentic-ai-security-cmm-d5-egress-network\|CMM D5: Egress and Network]] | Egress policy, destinations and denied flows (D5-ALLOW, D5-REACH). | Network and architectural controls under FFIEC examination guidance. Agent-to-tool egress decisions require deployment-specific evidence.[^ffiec] |
| [[agentic-ai-security-cmm-d6-data-rag\|CMM D6: Data, Memory and RAG]] | Data entitlements, retrieval scope and memory-store access (D6-ENTITLE, D6-STORE-ACCESS). | Customer or member information safeguards and Part 500 nonpublic-information controls where applicable. Retrieval and memory tests supply the agent-specific detail.[^bank-security][^ncua][^ftc][^nydfs] |
| [[agentic-ai-security-cmm-d7-observability\|CMM D7: Observability and Detection]] | Event logs and trace reconstruction (D7-LOG, D7-SPANS). | FFIEC security monitoring and Part 500 monitoring where applicable. Trace coverage does not alone show that required events were detected or acted on.[^ffiec][^nydfs] |
| [[agentic-ai-security-cmm-d8-supply-chain\|CMM D8: Engineering and Supply Assurance]] | Release assurance package (D8-DESIGN, D8-TEST, D8-RELEASE) and supplier assessment (D8-SUPPLIER). | FFIEC development and third-party examination guidance and applicable service-provider oversight. A CMM AI bill of materials is not an asserted federal mandate.[^ffiec][^bank-security][^ncua] |
| [[agentic-ai-security-cmm-d9-operations\|CMM D9: Operations and Human Factors]] | Incident playbook and notification route (D9-IR, D9-IR-NOTIFY). | Incident response, resilience and applicable regulator-notification duties. Determine each rule's trigger and clock; a CMM exercise does not satisfy a filing duty.[^incident] |

## Assessment use and limits

In the assessment record, name the entity, deployment shape, workflow, data, external actions, federal and state supervisors, and each source selected above. For each applicable CMM domain, report current level, target level, evidence confidence, consequential gaps, and the effort and exposure behind a proposed investment. Keep a separate obligation register with each external requirement, responsible owner, trigger, evidence, and conclusion. There is no aggregate CMM score or CMM certification. The [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]] defines the separate CMM result.

Where one artifact supports both records, cite it in each with the distinct test it answers. For example, a release-gate log may show that the institution reviewed changes, while D8-RELEASE tests the agent deployment's own gate. Neither claim follows from the artifact's title alone.

## Open questions

- **NCUA Appendix A and B placement.** NCUA proposed moving these guidance appendices out of the CFR and publishing their content separately. Its [deregulation project page](https://ncua.gov/news/deregulation-project) still lists the proposals; verify disposition and current citation before using an appendix in a filing. The Part 748 security-program and incident-reporting rules remain separate anchors.[^ncua][^incident]
- **Institution-specific supervision.** Record which banking or credit-union entity owns the deployment and whether another state authorization or affiliate creates an additional obligation. The federal selector does not settle that legal inventory.[^nydfs][^bank-security]
- **Agentic AI under model-risk guidance.** The April 2026 interagency guidance excludes generative and agentic AI models from its scope. Its agencies described further consideration of AI; assess current issued material when the institution revisits this mapping, rather than assuming a future instrument's content.[^mrm]

## Sources and limits

[^bank-security]: [Federal Reserve information-technology guidance](https://www.federalreserve.gov/supervisionreg/topics/information-technology-guidance.htm) identifies Regulation H, 12 CFR 208 Appendix D-2, for state member banks and Regulation Y, 12 CFR 225 Appendix F, for bank holding companies; [OCC, 12 CFR Part 30, Appendix B](https://www.ecfr.gov/current/title-12/chapter-I/part-30/appendix-Appendix%20B%20to%20Part%2030); [FDIC, 12 CFR Part 364, Appendix B](https://www.ecfr.gov/current/title-12/chapter-III/subchapter-B/part-364/appendix-Appendix%20B%20to%20Part%20364). Select the institution's actual implementation.
[^ffiec]: [FFIEC IT Examination Handbook, Information Security](https://ithandbook.ffiec.gov/it-booklets/information-security), with the Architecture, Infrastructure, and Operations and Development, Acquisition, and Maintenance booklets linked above. Examination guidance must be read with the applicable agency's rules and supervision.
[^auth]: [FFIEC, Authentication and Access to Financial Institution Services and Systems (2021)](https://www.ffiec.gov/sites/default/files/media/press-releases/2021/authentication-and-access-to-financial-institution-services-and-systems.pdf).
[^ncua]: [NCUA, Regulations and Guidance](https://ncua.gov/regulation-supervision/regulatory-compliance-resources/cybersecurity-resources/ncuas-regulations-and-guidance); [NCUA, Credit Union Policy Reviews](https://ncua.gov/regulation-supervision/examination-program/credit-union-policy-reviews) distinguishes mandatory Part 748 security-program obligations from Appendix A guidance.
[^ncua-ai]: [NCUA, Artificial Intelligence](https://ncua.gov/regulation-supervision/regulatory-compliance-resources/artificial-intelligence-ai), last checked 2026-09-29.
[^ftc]: [FTC, Safeguards Rule: What Your Business Needs to Know](https://www.ftc.gov/business-guidance/resources/ftc-safeguards-rule-what-your-business-needs-know), “Who's covered by the Safeguards Rule?”
[^nydfs]: [New York DFS, Second Amendment to 23 NYCRR Part 500](https://www.dfs.ny.gov/system/files/documents/2023/12/rf23_nycrr_part_500_amend02_20231101.pdf), effective 1 November 2023, §§ 500.1(e), 500.19; [DFS Cybersecurity Resource Center](https://www.dfs.ny.gov/industry_guidance/cybersecurity) for current filing guidance. The DFS-hosted amendment copy says it is not an official version. Coverage turns on authorization under the named New York laws, including when another agency also regulates the entity.
[^nydfs-ai]: [New York DFS, Cybersecurity Risks Arising from Artificial Intelligence and Strategies to Combat Related Risks](https://www.dfs.ny.gov/industry-guidance/industry-letters/il20241016-cyber-risks-ai-and-strategies-combat-related-risks), 16 October 2024.
[^incident]: [Joint banking-agency computer-security incident notification final rule](https://www.occ.gov/news-issuances/news-releases/2021/2021-119a.pdf); [NCUA Cyber Incident Reporting Guide](https://ncua.gov/regulation-supervision/regulatory-compliance-resources/cybersecurity-resources/cyber-incident-reporting-guide); [FTC Safeguards Rule breach-notification guidance](https://www.ftc.gov/business-guidance/resources/ftc-safeguards-rule-what-your-business-needs-know); [DFS Part 500](https://www.dfs.ny.gov/system/files/documents/2023/12/rf23_nycrr_part_500_amend02_20231101.pdf), § 500.17. Each has its own coverage, trigger, clock, and recipient.
[^mrm]: [Federal Reserve SR 26-2 and attached revised interagency guidance](https://www.federalreserve.gov/supervisionreg/srletters/SR2602a1.pdf); [OCC Bulletin 2026-13](https://www.occ.gov/news-issuances/bulletins/2026/bulletin-2026-13.html); [FDIC FIL-15-2026](https://www.fdic.gov/news/financial-institution-letters/2026/agencies-revise-interagency-model-risk-management-guidance), all 17 April 2026. The revised guidance is non-prescriptive, generally most relevant above \$30 billion, and excludes generative and agentic AI models; the agencies allow risk-based relevance below that threshold for significant model exposure.
