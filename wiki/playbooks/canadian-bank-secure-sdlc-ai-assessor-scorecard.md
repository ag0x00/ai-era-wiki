---
type: playbook
title: "Assessor's Quick Scorecard: Secure-SDLC and AI"
address: c-000050
created: 2026-05-14
updated: 2026-09-29
tags:
  - playbook
  - assessor-guide
  - scorecard
  - secure-sdlc
  - financial-services
  - canadian-banking
status: developing
origin: produced
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
audience: "external advisor assessing a named release pipeline and AI deployment at a federally regulated Canadian bank"
length: "~5 pages, 32 checks"
regulatory_anchors:
  - "OSFI Guideline B-13 — Technology and Cyber Risk Management (published 2022-07-31)"
  - "OSFI Guideline B-10 — Third-Party Risk Management (effective 2024-05-01)"
  - "OSFI Guideline E-23 — Model Risk Management (effective 2027-05-01)"
  - "PIPEDA section 10.1 — breach reporting and notification where applicable"
  - "OSFI Technology and Cyber Security Incident Reporting Advisory (effective 2021-08-13)"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[nist-ssdf]]"
  - "[[nist-sp-800-218a]]"
  - "[[frontier-ai-for-vuln-discovery]]"
sources: []
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Checked live official OSFI B-13, B-10, E-23, incident advisory, PIPEDA s10.1, and NIST SSDF; archived NIST excerpts also consulted, but page provenance is external-only."
---

# Assessor's Quick Scorecard: Secure-SDLC and AI

This scorecard screens a bank's **named release pipeline and AI deployment** for consequential security gaps. The assessor records evidence and a verdict for each check, then gives the bank a prioritized decision list. Route an agentic deployment to a separate [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] assessment for its nine-domain current and target profile. These screening results establish neither a CMM level nor regulatory compliance.

## Scope and preparation

The advisor and bank sponsor record the assessment boundary before interviews:

- Legal entity, business workflow, users, data classes, jurisdiction, and production status.
- The release pipeline, repository, artifact, deployed version, environment, and change window being sampled.
- The AI deployment shape, including model provider, agent authority, retrieval sources, external actions, and suppliers.
- Control owners, applicable internal policy, population of releases and routes, and the selected evidence sample.

Use a separate record for another deployment shape or materially different pipeline. An enterprise policy can be inherited as evidence, but a control's operation is tested on the selected path. The bank's legal and compliance functions determine which obligations apply to the entity and activity. Supplier operation alone does not make a check inapplicable; limited supplier evidence may make it unanswerable.

## Evidence and verdict rule

For each check, record the scoped population, sample, evidence identifier, version and date, observed result, verdict, reason, and owner. Examine a document, interview its owner, and test a live or staged path where the check concerns refusal, containment, or operating history. A policy or vendor description alone does not prove that a deployed path behaves as claimed. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] gives the deeper evidence method for a CMM assessment.

| Verdict | Rule |
|---|---|
| Met | Current evidence establishes the outcome for the defined scope; record sample depth and any untested part. |
| Partly met | The outcome operates on a known subset; identify the uncovered releases, users, or routes. |
| Not met | A current counterexample defeats the outcome, or the required control is known to be absent. |
| Unanswerable | The outcome applies, but available evidence cannot establish or refute it; name the missing fact and who can supply it. |
| Not applicable | The governed activity or object is absent from the defined scope; show the absence evidence. |

An untested known route prevents a met verdict on a check that says “each.” The screening verdict is not a CMM criterion verdict. In the CMM, a single counterexample can defeat a criterion that requires every route or action. Keep that counterexample in the handoff record.

## Checks

The listed evidence is the minimum starting point. The assessor selects additional samples and negative tests according to the deployment's risk. “Practice” means a recommended test drawn from the CMM or NIST guidance; it is not attributed to OSFI or PIPEDA as a prescribed mechanism.

### A. Release and application security — engineering owner

| ID | Check | Evidence or test | Basis |
|---|---|---|---|
| A1 | Does the selected pipeline enforce the bank's security requirements at its designated release gates? | Gate configuration and one blocked or excepted release | B-13 §2.4.2 |
| A2 | Does a material design change receive a threat and trust-boundary review before implementation? | Threat-review record | B-13 §§1.3, 3.1; Practice |
| A3 | Does a sampled first-party code change receive risk-selected security checks before promotion? | Release-linked CI result | B-13 §§2.4.5, 3.2.9; SSDF |
| A4 | Does the release gate refuse a finding the bank designated as blocking unless an authorized exception is recorded? | Refusal test and exception record | B-13 §§2.4.2, 2.5; Practice |
| A5 | Does a newly introduced third-party component receive a recorded risk decision before use? | Dependency-intake decision | B-13 §§2.4.4–2.4.5; Practice |
| A6 | Are sampled high-priority vulnerabilities remediated or accepted within the bank's risk-based deadline? | Time-stamped finding record | B-13 §3.2.6 |
| A7 | Does the bank execute its risk-selected security test plan against the current application release? | Release-linked test report | B-13 §§3.1.1, 3.2.9 |
| A8 | Does the test program measure missed detections with known-vulnerable cases? | Ground-truth evaluation | Practice |
| A9 | Does the test program review suppressed false alarms against known-benign cases? | Suppression validation | B-13 §3.2.4; Practice |

B-13 asks for risk-based threat assessment and testing; it does not set a universal annual penetration-test cadence for A7. The bank chooses and documents the test frequency, change triggers, and scope. A3 includes code produced by coding agents. A8 and A9 reveal whether the resulting signal is credible enough to support a release decision.

### B. Model and supplier stewardship — model-risk and third-party-risk owners

Apply B2–B4 when the selected AI system meets the bank's model definition. Apply B1 when a model's inherent risk is non-negligible. Record the triage decision. A hosted model is not automatically outside the model-risk process. [E-23's revised guideline](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027) takes effect on 2027-05-01, so findings against that revision are **readiness findings** before that date.

| ID | Check | Evidence or test | Basis |
|---|---|---|---|
| B1 | Is a model with non-negligible inherent risk present in the enterprise model inventory? | Inventory-to-use match | E-23 §C.1 |
| B2 | Does the model's inherent risk rating determine the required review intensity? | Risk-linked review plan | E-23 §§C.2–C.3 |
| B3 | Did a reviewer independent of development assess the model's fitness for its intended use? | Independent review report | E-23 model review |
| B4 | Does a breached model-monitoring threshold trigger the bank's defined escalation? | Alert disposition record | E-23 model monitoring |
| B5 | Is the selected AI supplier arrangement covered by a current risk and criticality assessment? | Dated assessment and arrangement record | B-10 §2.2 |

For B5, inspect the pre-entry decision for a new arrangement. For an established arrangement, inspect the latest periodic or material-change review and any unresolved remediation.

### C. Frontier AI for vulnerability discovery — optional security-testing owner

Apply this section when the bank uses or pilots AI to discover vulnerabilities or generate security fixes. Otherwise mark the **whole section out of scope** and record the absence of such activity. Within an in-scope section, mark a check not applicable when its specific activity is absent; C4, for example, applies when discovery output becomes a release gate. A pilot is in scope even when its findings are advisory.

| ID | Check | Evidence or test | Basis |
|---|---|---|---|
| C1 | Are AI discovery runs confined to approved targets and permissions? | Denied out-of-scope run | Practice |
| C2 | Is an AI-reported vulnerability reproduced before it enters the remediation queue? | Reproduced finding | Practice |
| C3 | Is an AI-generated security fix reviewed before promotion? | Change review linked to generated patch | SSDF; Practice |
| C4 | Is the discovery harness evaluated against held-out known cases before its output becomes a release gate? | Held-out evaluation report | Practice |

The [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] thesis describes candidate harnesses. A named vendor or coalition is not a prerequisite for any check.

### D. Agent deployment boundaries — platform and application owners

Apply each check to the named AI deployment where its governed path exists. For an agent that cannot execute code or use tools, record which action path is absent. For a deployment without retrieval, D4 may be not applicable; an opaque supplier-held retrieval path is unanswerable until its behavior can be tested or evidenced. These checks route to the CMM's identity, control, runtime, egress, data, observability, and engineering domains.

| ID | Check | Evidence or test | Basis |
|---|---|---|---|
| D1 | Can a material agent action be attributed to its distinct workload identity? | Attributed action record | CMM D2 |
| D2 | Does authorization refuse an agent action outside its active task scope? | Expired or out-of-scope action test | CMM D3 |
| D3 | Does each untrusted context route screen prompt injection before model intake? | Route-specific attack test | CMM D4 |
| D4 | Does revoking a source grant prevent the asking principal from receiving that source through the assistant? | Grant-revocation test | CMM D6 |
| D5 | Does agent-initiated network traffic receive an enforcing egress decision before leaving the deployment? | Direct-call egress test | CMM D5 |
| D6 | Can the team reconstruct the running component versions from a release-linked AI bill? | Release bill compared with runtime inventory | CMM D8 |
| D7 | Did the exact deployed assembly pass a risk-selected security test before promotion? | Release-linked assembly test | CMM D8 |
| D8 | Can an operator reconstruct a material agent action from retained event records? | Event chain for one consequential action | CMM D7 |

The D3 test includes retrieved documents and tool responses that the agent may treat as instructions. D4 tests the asking principal's **current** entitlement at answer time, including a revoked grant and any cache path. A failed D3 or D4 test needs its own finding even when a general platform document claims the control exists.

### E. Detection and response — security-operations owner

| ID | Check | Evidence or test | Basis |
|---|---|---|---|
| E1 | Is an AI-abuse or anomalous-agent alert assigned and dispositioned by a named responder? | Alert queue and disposition | B-13 §3.3; CMM D7 |
| E2 | Does an exercised containment procedure stop the sampled agent's privileged actions? | Exercise record and denied action | B-13 §3.4; CMM D9 |
| E3 | Does the incident workflow document a real-risk-of-significant-harm decision for a breach of security safeguards involving personal information? | Privacy incident decision record | PIPEDA §10.1(8); Practice |
| E4 | Does the workflow report a breach of security safeguards meeting the real-risk-of-significant-harm threshold to the Privacy Commissioner as soon as feasible? | Commissioner-report exercise | PIPEDA §10.1(1)–(2) |
| E5 | Where law permits, does the workflow notify affected individuals of a breach of security safeguards meeting the real-risk-of-significant-harm threshold as soon as feasible? | Individual-notice exercise | PIPEDA §10.1(3)–(6) |
| E6 | Does the workflow notify OSFI of a reportable AI-related technology or cyber incident within the advisory's required window? | OSFI-notification exercise | OSFI incident advisory |

E3–E5 apply where PIPEDA governs the personal information in the selected workflow. E6 uses the [OSFI incident advisory's reporting criteria and 24-hour initial notification rule](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-security-incident-reporting). The exercise tests written notice to OSFI's Technology Risk Division and Lead Supervisor within the advisory's 24-hour window, or sooner if possible. Not every AI event is reportable.

## Report and decision

Report each section as counts of **met, partly met, not met, unanswerable, and not applicable**. Show the question IDs behind each count. The applicable count is met + partly met + not met + unanswerable. If it is zero, the section is **out of scope**. If it is positive and every applicable check is unanswerable, the section is **indeterminate**. Section C remains visibly optional. Do not calculate a percentage, maturity tier, or whole-engagement score.

For every consequential partly met, not met, or unanswerable check, record the affected release or deployment, evidence and uncertainty, plausible failure path, business consequence, applicable obligation if any, existing containment, owner, and next action. Give the sponsor a short risk-and-effort decision for each work package:

- Current exposure and the consequence of leaving it open.
- Capability the proposed control or evidence request would add, and why it matters for this deployment.
- One-time engineering or supplier effort, recurring operating effort, and burden on reviewers or approvers.
- Named decision owner, funding or acceptance decision, residual risk, deadline, and reassessment trigger.

A regulatory citation is a reason to examine an obligation, not proof of a breach. The bank determines the applicable duty and legal conclusion. For an agentic deployment, carry the relevant evidence and failed paths into the [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] to produce separate current and target levels, confidence, and blockers for each applicable domain. This scorecard's section counts do not become CMM levels.

## Regulatory and practice anchors

| Source | Use in this scorecard | Boundary |
|---|---|---|
| [OSFI B-13, Technology and Cyber Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-risk-management) (published 2022-07-31) | Secure SDLC §2.4, change control §2.5, risk-based testing §§3.1–3.2, logging §3.3, response §3.4 | Outcomes and risk-based expectations; no fixed scorecard tier or required testing tool |
| [OSFI B-10, Third-Party Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/third-party-risk-management-guideline) (effective [2024-05-01](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/osfi-response-draft-guideline-b-10-consultation-feedback-third-party-risk-management)) | Supplier assessment and oversight | Intensity follows arrangement risk and criticality |
| [OSFI E-23, Model Risk Management](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/guideline-e-23-model-risk-management-2027) (effective 2027-05-01) | Model identification, risk rating, independent review, monitoring | Revised-guideline readiness before its effective date; apply the bank's model-risk triage |
| [PIPEDA §10.1](https://laws-lois.justice.gc.ca/eng/acts/P-8.6/section-10.1.html) | Real-risk-of-significant-harm determination and resulting notice duties | Applies to a breach of safeguards involving personal information under the organization's control |
| [OSFI Technology and Cyber Security Incident Reporting Advisory](https://www.osfi-bsif.gc.ca/en/guidance/guidance-library/technology-cyber-security-incident-reporting) | Reportability decision and initial notification | Apply the advisory's criteria; reportable incidents require notification within 24 hours or sooner if possible |
| [NIST SSDF v1.1](https://csrc.nist.gov/pubs/sp/800/218/final) and [SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final) | Practice selection for secure development and AI model or system production | Guidance for this instrument, not a Canadian legal obligation |

The [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] supports a clause-level review. This short screen does not replace that review or the nine-domain CMM assessment.
