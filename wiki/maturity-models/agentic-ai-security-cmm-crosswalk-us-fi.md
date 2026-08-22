---
type: maturity-model-companion
title: "CMM: US Regulated-Finance Crosswalk (FFIEC and GLBA)"
address: c-000134
created: 2026-05-26
updated: 2026-06-23
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
sources:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
---

# Agentic AI Security CMM — US Regulated-Finance Crosswalk (FFIEC / GLBA)

This crosswalk maps the jurisdiction-neutral [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-reference-architecture|RA]] to the expectations a **US-regulated financial institution** is examined against. It is one jurisdictional lens, not a new requirement set.

**These are US standards for US-regulated entities — not a global mandate.** FFIEC guidance, GLBA implementations, and the interagency model-risk guidance bind only institutions regulated by the relevant US agency. They are **not** requirements for a Canadian, EU, or other non-US institution; that institution maps to its own home regulator (see the [[agentic-ai-security-cmm-crosswalk-canada-fi|Canadian crosswalk]]). NIST [[nist-ai-rmf|AI RMF]] is **voluntary** even in the US. The CMM treats none of these as a universal mandate.

**The decisive 2026 fact: model-risk guidance now excludes agentic AI.** **SR 11-7 was rescinded on 17 April 2026** and replaced by revised interagency Model Risk Management guidance (Fed **SR 26-02**, OCC **Bulletin 2026-13**, FDIC concurrent). The new guidance **explicitly carves generative *and agentic* AI out of scope** and defers them to a forthcoming, not-yet-issued RFI.[^mrm] The model-risk frame that historically reached AI/ML now *deliberately* leaves agentic AI empty. Until that RFI issues, the CMM is the de-facto control layer; the regulators supply the surrounding governance, information-security, and consumer frame.

## The US regulatory landscape (2026)

| Instrument | Authority | Status | What it expects |
|---|---|---|---|
| **FFIEC IT Examination Handbook** — Information Security; Architecture, Infrastructure & Operations; Development, Acquisition & Maintenance (retitled Sep 2024) | FFIEC agencies | examiner guidance, in force[^ffiec] | Risk-based info-security program; access control; change management with test/rollback/audit trail; secure SDLC and third-party-SDLC verification; architecture, operations, resilience |
| **FFIEC Authentication and Access** guidance (2021) | FFIEC agencies | in force[^auth] | Risk-based authentication and access for all user types including third parties and **"other systems" (machine-to-machine)**; layered security; single-factor weakness flagged |
| **GLBA — FTC Safeguards Rule** (16 CFR 314) | FTC | in force (2023 amendments)[^ftc] | Written info-security program; qualified individual; risk assessment; access controls; encryption in transit and at rest; MFA; secure development; monitoring; vendor oversight; breach notice |
| **GLBA — Interagency Guidelines** (12 CFR 30 App. B / 364 App. B / Reg H, Y) | OCC / FDIC / Fed | in force[^interagency] | Bank-side §501(b) info-security program: access controls, encryption, change controls, dual control + background checks, monitoring, response programs, third-party oversight, board governance |
| **NCUA Part 748 App. A & B** | NCUA | in force[^ncua] | The **credit-union** GLBA implementation: written security program, risk assessment, access controls, encryption, monitoring, third-party oversight, board involvement; response programs + member notice |
| **Model Risk Management** — SR 26-02 / OCC 2026-13 (replacing SR 11-7) | Fed / OCC / FDIC | replaced SR 11-7 on 2026-04-17[^mrm] | Risk-based, non-prescriptive model governance/validation/monitoring; **generative and agentic AI explicitly out of scope**, deferred to a future RFI |
| **Treasury — Managing AI-Specific Cybersecurity Risks** | US Treasury | report (non-binding), Mar 2024[^treasury] | Flags the AI capability/data divide, fraud-data sharing, provider concentration, competency gaps; references NIST AI RMF |
| **NIST AI RMF + Generative AI Profile** | NIST | **voluntary**[^nist] | Govern / Map / Measure / Manage; referenced by examiners but carries no regulatory force |
| **23 NYCRR 500** | NY DFS | in force (2023 amendment) | State cybersecurity rule for DFS-licensed entities; **generally not applicable to a federally chartered/insured credit union** |

## Domain crosswalk (CMM domain → US anchor)

| CMM Domain | Primary US anchor(s) | Note |
|---|---|---|
| **D1 Governance** | GLBA program governance (FTC / Interagency / NCUA Part 748); SR 26-02 model governance; FFIEC InfoSec governance | Board-level info-security and model governance are well covered |
| **D2 Identity & Authorization** | FFIEC Authentication & Access (2021); GLBA access controls | FFIEC's "other systems" / machine-to-machine hook is the closest, but predates and does not contemplate **per-agent identity** |
| **D3 Control & Least-Agency** | FFIEC Dev/Acq/Maintenance (change control); InfoSec least-privilege | Change-management covers code, not **agent action permissions** |
| **D4 Runtime & Guardrails** | FFIEC InfoSec (layered controls) | No regulator names [[prompt-injection\|prompt injection]], jailbreak, or runtime LLM guardrails |
| **D5 Egress & Network** | FFIEC AIO (remote access, network); InfoSec boundary controls | Human/VPN-framed; nothing addresses agent egress or MCP |
| **D6 Data, Memory & RAG** | GLBA safeguarding of customer/member info (encryption, access); SR 26-02 (model data inputs) | Protects customer data generically; silent on RAG oversharing and memory poisoning |
| **D7 Observability & Detection** | FFIEC InfoSec monitoring/logging; SR 26-02 ongoing monitoring; NCUA ACET (voluntary) | Monitoring and model-outcome analysis map cleanly |
| **D8 Supply Chain & AI-BOM** | SR 26-02 vendor-model validation; FFIEC third-party SDLC verification; GLBA service-provider oversight; Treasury (provider concentration, flagged not required) | Vendor-model validation is the closest hook; no AI-BOM mandate |
| **D9 Operations & Human Factors** | GLBA response programs + breach notice; FFIEC resilience; SR 26-02 governance/human oversight; NCUA Part 748 App. B | Incident response and member-notification expectations land here |

## US regulatory omissions the CMM fills

> [!gap] US financial regulators supply a model-risk + information-security + consumer frame, not agentic-AI controls
> Confirmed silences as of 2026: **per-agent / non-human identity (D2)**, where FFIEC Authentication gestures at "other systems" but does not contemplate per-agent identity; **least-agency / action authorization (D3)**; **runtime guardrails and prompt-injection defense (D4)**; **agent egress / MCP / tool-call network control (D5)**; **RAG oversharing, vector-store scoping, and memory poisoning (D6 specifics)**; and **AI-BOM / model-and-tool provenance (D8)**, where SR 26-02 requires vendor-model validation but mandates no model bill-of-materials. The April 2026 carve-out of agentic AI from model-risk guidance makes this gap explicit and intentional: the agencies have left the agentic frame empty pending a future RFI. In the interim the CMM and RA are the de-facto control layer.

## Practical guidance for a US-regulated institution

- **Identify your actual GLBA implementation.** A credit union is governed by **NCUA Part 748**, not the FTC Safeguards Rule or the banking-agency Interagency Guidelines; a national bank by the OCC's Part 30 App. B; a non-bank financial institution by the FTC rule. Map CMM evidence to *your* regulator's version.
- **Re-present CMM evidence through FFIEC booklets.** Examiners read technical controls through the InfoSec, AIO, and Dev/Acq/Maintenance booklets; CMM D1/D3/D4/D5/D7/D8/D9 evidence maps to them.
- **Do not over-rely on the model-risk frame for AI.** SR 26-02 explicitly excludes agentic AI, so treating model-risk validation as sufficient AI-security governance is now a documented gap. Govern the agent with the CMM and watch for the agencies' AI RFI.
- **Treat NIST AI RMF as a voluntary scaffold, not a requirement.** It is useful structure and examiner-referenced, but not a mandate; NCUA ACET (also voluntary) maps to the FFIEC handbook and NIST CSF.
- **NYDFS 500 applies only if a NY-DFS-regulated entity is in scope.** It generally does not bind a federally insured credit union.

## Open questions and watch items

- **The interagency AI RFI** referenced in SR 26-02 had not issued as of this writing. It is the most important item to track, since it will populate the currently-empty agentic-AI frame. Verify its status before relying on this section.
- Whether examiners will, in practice, expect agentic-AI controls under existing FFIEC InfoSec authority before the RFI issues is unsettled.
- The exact per-agency CFR citations for the banking-side Interagency Guidelines (Fed Reg H App. F / Reg Y App. D) should be confirmed against eCFR before use in a filing.

## Notes

[^ffiec]: [FFIEC IT Examination Handbook — Information Security](https://ithandbook.ffiec.gov/it-booklets/information-security); [Architecture, Infrastructure, and Operations](https://ithandbook.ffiec.gov/it-booklets/architecture-infrastructure-and-operations); [Development, Acquisition, and Maintenance](https://ithandbook.ffiec.gov/it-booklets/development-acquisition-and-maintenance/) (retitled Sep 2024).
[^auth]: [FFIEC — Authentication and Access to Financial Institution Services and Systems (2021)](https://www.ffiec.gov/sites/default/files/media/press-releases/2021/authentication-and-access-to-financial-institution-services-and-systems.pdf). Covers third parties and machine-to-machine ("other systems").
[^ftc]: [FTC Safeguards Rule (16 CFR Part 314)](https://www.ftc.gov/legal-library/browse/rules/safeguards-rule); [2023 final amendments](https://www.federalregister.gov/documents/2023/11/13/2023-24412/standards-for-safeguarding-customer-information).
[^interagency]: [Federal Reserve — Interagency Guidelines Establishing Information Security Standards (GLBA §501(b))](https://www.federalreserve.gov/supervisionreg/interagencyguidelines.htm). Banking-agency implementations at 12 CFR 30 App. B (OCC), 364 App. B (FDIC), Reg H / Reg Y (Fed).
[^ncua]: [NCUA — Part 748 (Guidelines for Safeguarding Member Information)](https://www.ncua.gov/regulation-supervision/Pages/rules/reg-history/part-748.aspx); [IT Security Compliance Guide](https://ncua.gov/regulation-supervision/letters-credit-unions-other-guidance/it-security-compliance-guide-credit-unions). The credit-union GLBA implementation.
[^mrm]: [Federal Reserve SR 26-02 — Model Risk Management](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.pdf) and [OCC Bulletin 2026-13](https://www.occ.gov/news-issuances/bulletins/2026/bulletin-2026-13.html), 2026-04-17, replacing SR 11-7 / OCC 2011-12. Generative and agentic AI explicitly out of scope, deferred to a future RFI.
[^treasury]: [US Treasury — Managing AI-Specific Cybersecurity Risks in the Financial Services Sector](https://home.treasury.gov/news/press-releases/jy2212), Mar 2024. Non-binding report.
[^nist]: [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) and [Generative AI Profile (NIST AI 600-1)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf). Voluntary.
