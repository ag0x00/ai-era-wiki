---
type: review
title: "NIST SP 800-218A Standards Review"
address: c-000225
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - nist
  - ssdf
  - ssdf-community-profile
  - secure-development
  - ai-supply-chain
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[nist-sp-800-218a]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
related:
  - "[[nist-sp-800-218a]]"
  - "[[nist-ssdf]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-review-backlog]]"
  - "[[standards-review-nist-ai-rmf-2026-Q2]]"
sources:
  - ".raw/papers/nist-sp-800-218A.pdf"
  - "https://csrc.nist.gov/pubs/sp/800/218/a/final"
  - "https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218A.pdf"
  - "https://doi.org/10.6028/NIST.SP.800-218A"
---

# Standards Review — NIST SP 800-218A, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to NIST SP 800-218A (*Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile*, July 2024): primary-source citations from the publication's Table 1, a task-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains, falsifiable absence claims, and an adversarial second pass. It is the fifth completed standards review and supersedes the wiki-summary-level treatment of 218A in [[agentic-cmm-vs-standards-validation|the 2026-04-30 first-pass validation]].

The review confirmed the practice and task identifiers on the [[nist-sp-800-218a|218A framework page]] against Table 1 of the source, corrected the framework page's claim of "three net-new tasks" (the Profile adds **six** tasks tagged `[Not part of SSDF 1.1]`), and produced a task-level crosswalk across all nine CMM domains. 218A maps most strongly to the development-time domains — D6 (Data/Memory), D8 (Supply Chain), and D4 (Runtime, via threat modeling and input/output handling). It has, by its own stated scope, no coverage of deployment-time runtime guardrails, network egress, or NHI/agent identity.

**218A is a development-time document and contributes process tasks, not deployment controls.** The Profile's own scope statement excludes "the deployment and operation of AI systems with AI models," so every CMM cell it anchors is a build-time secure-SDLC obligation (protect weights, verify data integrity, threat-model the model, scan for malware) — none is a runtime guardrail, egress filter, or agent-identity control, and the Profile attaches no measurable acceptance criteria to any recommendation.

## Primary documents reviewed

The Profile is a single PDF. Practice, task, priority, and the AI-specific Recommendations/Considerations/Notes were read from Table 1 (pp. 8-19); the glossary from Appendix A (p. 22). Base SSDF practice and task definitions (PO/PS/PW/RV) are from the parent SP 800-218 v1.1.

| Document | URL | Scope used in this review |
|---|---|---|
| NIST SP 800-218A (final, July 2024) | [csrc.nist.gov](https://csrc.nist.gov/pubs/sp/800/218/a/final) | Table 1 (Profile), sections 1-3, Appendix A glossary |
| 218A archived copy | `.raw/papers/nist-sp-800-218A.pdf` | Full document, 30 pp. |
| NIST SP 800-218 v1.1 (base SSDF) | [doi.org/10.6028/NIST.SP.800-218](https://doi.org/10.6028/NIST.SP.800-218) | Base PO/PS/PW/RV practice and task IDs |
| 218 archived copy | `.raw/papers/nist-sp-800-218.pdf` | Base framework reference |

## Structure-to-domain grounding

218A is an SSDF Community Profile: it does not restate the base framework but augments it. Each base SSDF task carries a Priority (`High`/`Medium`/`Low`, scoped to the Profile), zero or more AI-specific **Recommendations** (`R`), **Considerations** (`C`), and **Notes** (`N`), and **Informative References** into NIST AI RMF 1.0, OWASP Top 10 for LLM Applications, and the *Adversarial ML Taxonomy* (NIST AI 100-2e2023). Item IDs append to the task ID, e.g. `PS.1.3.R4` is the fourth recommendation on task `PS.1.3`. The Profile is explicit that it "should not be used without SP 800-218."

The Profile adds **six tasks** tagged `[Not part of SSDF 1.1]` (verified in Table 1):

| New task | Title (verbatim, abbreviated) | Practice | Priority |
|---|---|---|---|
| `PO.5.3` | Continuously monitor software execution performance and behavior in development environments | PO.5 | High |
| `PS.1.2` | Protect all training, testing, fine-tuning, and aligning data from unauthorized access and modification | PS.1 | High |
| `PS.1.3` | Protect all model weights and configuration parameter data from unauthorized access and modification | PS.1 | High |
| `PW.3.1` | (new practice PW.3) Analyze data to identify poisoning, bias, homogeneity, and tampering | PW.3 | High |
| `PW.3.2` | (new practice PW.3) Track data provenance and document data of unknown provenance | PW.3 | Medium |
| `PW.3.3` | (new practice PW.3) Include adversarial / edge-case samples | PW.3 | Medium |

> [!contradiction] The framework page undercounts the net-new tasks
> The [[nist-sp-800-218a|218A page]] states the Profile "introduces three net-new tasks" (`PO.5.3`, `PS.1.2`, `PS.1.3`) and separately lists PW.3's three tasks as a "new practice." Table 1 tags **all six** of `PO.5.3`, `PS.1.2`, `PS.1.3`, `PW.3.1`, `PW.3.2`, and `PW.3.3` with `[Not part of SSDF 1.1]` — six net-new tasks across one net-new practice (PW.3) and three additions to existing practices. The framework page's prose splits these into "three tasks" + "one practice," which understates the task count. Reconciled in the Effect section.

## Task-level coverage matrix (CMM x 218A)

Each row cites the 218A practices, tasks, and AI-specific items (R/C/N) that anchor a CMM domain. IDs and verbatim phrasing are from Table 1 (verified pp. 8-19). "n/a" rows carry the bounded-absence form.

| CMM domain | 218A tasks / items | Coverage |
|---|---|---|
| D1 Governance & Accountability | `PO.1.1.R1`/`R2`, `PO.1.2.R1` (AI in security requirements, org policy); `PO.2.1.R1`/`N1`, `PO.2.2.R1`, `PO.2.3.R1` (AI roles, role-based training, leadership commitment) | Partial — governance framed as SDLC role/policy setup; no enterprise risk-governance or accountability model (defers to AI RMF) |
| D2 Identity & Authorization | none specific | None — no NHI, agent-identity, or authorization control. `PS.1.1.R2`/`R4` and `PS.1.3.R3` invoke least privilege for model-element *access*, but address asset protection, not identity |
| D3 Control & Least-Agency | `PW.1.1.C2` (consider whether the model is "in a critical path to make significant security decisions without a human in the loop"); `PO.4.1.C1` (human-in-the-loop review beyond risk-based thresholds) | Thin — human-in-the-loop framed as a design-time consideration, not a least-agency control plane |
| D4 Runtime & Guardrails | `PW.1.1.R1` (AI threat modeling: poisoning, injection-style attacks, DoS from adversarial prompts, weight theft); `PW.5.2` (log/analyze/validate all inputs and outputs, sanitize or drop); `PW.5.3` (encode I/O to prevent unauthorized code execution); `PO.4.1.R1` (guardrails across the AI dev life cycle) | Build-time only — names guardrails and I/O handling as coding practices; deployment-time runtime defense is out of scope |
| D5 Egress & Network | none specific | None — no network-egress, data-exfiltration, or model-query-rate control. `PO.5.1.R1` (monitor/limit resource usage during *development*) is environment hardening, not runtime egress |
| D6 Data, Memory & RAG | **PW.3** (`PW.3.1` analyze data for poisoning/bias/homogeneity/tampering; `PW.3.2` track provenance + document unknown provenance; `PW.3.3` include adversarial samples); `PS.1.2` (protect training/testing/fine-tuning/aligning data); `PS.3.2.R1`/`R2` (model provenance, sensitive-data-trained models) | Strongest domain — training-data integrity is the Profile's largest contribution; no RAG-retrieval or runtime-memory content (development scope) |
| D7 Observability & Detection | `PO.5.3.R1`/`R2` (continuously monitor dev-environment execution; alert on risk-threshold-crossing model activity); `RV.1.1.R1` (log/monitor/analyze inputs and outputs); `RV.2.1.N1` (deep analysis of GenAI input/output to detect deviation from normal behavior) | Partial — monitoring scoped to development environments and vuln-response logging; no production detection-rule or SOC guidance |
| D8 Supply Chain & AI-BOM | `PS.3.2` (provenance data via SBOM/SLSA — `R1` track model provenance incl. training libraries/frameworks/pipelines); `PW.4.4.R1`/`R2` (verify integrity/provenance/security of acquired models and components; scan/test before use); `PS.1.3.R4` (encryption, hashes, signatures, multi-party auth, air-gapping for weights); `PS.2.1.R1`/`R2` (cryptographic hashes / digital signatures for AI artifacts) | Strong — federal anchor for AI provenance and acquired-model verification; "SBOM" named, no AI-BOM artifact schema |
| D9 Operations & Human Factors | `RV.2.2.R2` (criteria/processes for when to stop using a model and roll back); `RV.2.2.C1` (be prepared to stop using a model at any time); `RV.1.3.R1`/`R2` (AI vulnerabilities in disclosure/remediation policy); `RV.3.x` (root-cause analysis) | Partial — strong on stop/rollback criteria (kill-switch analog); development-lifecycle response, not operational IR runbooks |

The matrix confirms the Profile's design premise: 218A is a *development-time* secure-SDLC overlay. It supplies process obligations for the parts of the model lifecycle that classical secure engineering can cover, and explicitly documents where it cannot (the model-weights tracking limitation, section 2). Its Recommendations name *what* to do without Implementation Examples (the "how"), which NIST flags as deferred future work.

## Falsifiable absence claims found

What the CMM scores that 218A, as published (July 2024), does not provide. Each is bounded to Table 1 and Appendix A and reversible by the stated refuting evidence.

1. **No measurable acceptance criteria, thresholds, or test procedures.** Every AI-specific item is a Recommendation (`R`), Consideration (`C`), or Note (`N`) in prose; none carries a pass/fail criterion, metric, or threshold. *Searched:* Table 1, all R/C/N items (pp. 8-19). *Terms:* "criteria", "threshold", "metric", "measure" (as acceptance), "shall", "minimum", "pass/fail", "acceptance". *Verdict:* confirmed. The closest, `PS.1.3.R4`, names mechanisms (encryption, cryptographic hashes, digital signatures, multi-party authorization, air-gapping) without an acceptance condition or cadence; `PO.5.3.R2` references "a risk threshold" the organization defines, not one the Profile specifies. *Refuting evidence:* any item stating a numeric or pass/fail acceptance criterion. *Reviewed 2026-06-22.* This is the gap the CMM's per-level evidence rubric fills.

2. **No deployment-time or runtime control.** The Profile addresses model *development*; it has no control for the deployed system at inference time — no runtime guardrail enforcement, network egress, or agent-identity control. *Searched:* section 1.2 Scope, Table 1. *Terms:* "deployment", "operation", "runtime", "inference", "egress", "network", "agent identity", "non-human identity". *Verdict:* confirmed — a definitional exclusion. Section 1.2 states "practices for the deployment and operation of AI systems with AI models are out of scope." `PW.5.2` (input/output handling) and `PO.4.1.R1` (guardrails) are coding-time obligations on the produced software, not runtime enforcement. *Refuting evidence:* a task or item specifying a deployed-system runtime control. *Reviewed 2026-06-22.* The CMM's D4/D5/D2 fill this from other instruments.

3. **No AI-BOM artifact specification.** The Profile requires provenance tracking and references SBOM and SLSA, but specifies no AI-specific bill-of-materials format, field set, or model/dataset manifest. *Searched:* `PS.3.2`, `PS.3.1`, `PW.4.4` and their items. *Terms:* "AI-BOM", "ML-BOM", "model card" (as a required artifact), "manifest", "CycloneDX", "SPDX", "schema". *Verdict:* confirmed. `PS.3.2.R1` says track provenance "e.g., in a software bill of materials [SBOM], through Supply-chain Levels for Software Artifacts [SLSA]" — it points at generic SBOM/SLSA, not an AI-BOM artifact; `PO.1.2.N1` notes "data, model, and system cards" as *possible* documentation forms, not required schemas. *Refuting evidence:* an item prescribing an AI-BOM format or required field set. *Reviewed 2026-06-22.* Matches the AI-BOM gap the CMM `D8` carries and the [[agentic-ai-security-cmm-crosswalk|crosswalk]] D8 note.

4. **No multi-agent, agent-to-agent, or orchestration content.** The Profile has no task or item addressing multi-agent systems, agent-to-agent trust, tool-use authorization, or orchestration topology. *Searched:* full Table 1, Appendix A glossary. *Terms:* "agent", "agentic", "multi-agent", "orchestrat", "tool use", "tool invocation", "autonomy". *Verdict:* confirmed. The document is scoped to "AI model development" and "dual-use foundation models"; the word "agent" does not appear as a subject of any task. *Refuting evidence:* a task or glossary entry addressing agentic or multi-agent systems. *Reviewed 2026-06-22.* The CMM's agentic D2/D3/D7 content draws from MITRE ATLAS and OWASP ASI, not 218A.

5. **Cybersecurity-only; no non-adversarial AI-risk content.** The Profile addresses only cybersecurity risk; it explicitly defers data privacy, intellectual property, and bias to other frameworks. *Searched:* section 2 (limitation statement), Table 1. *Terms:* "privacy", "bias" (as a managed risk vs. `PW.3.1`'s data-quality screen), "fairness", "intellectual property", "non-adversarial". *Verdict:* confirmed — a stated limitation. Section 2 reads "A limitation of the SSDF and this Profile is that they only address cybersecurity risk management," pointing to AI RMF, AI 600-1, SP 1270, the Privacy Framework, and IR 8286. `PW.3.1` screens data for "bias" and "homogeneity" but as poisoning/tampering indicators, not as a fairness-governance obligation. *Refuting evidence:* a task managing a non-cybersecurity AI risk as subject matter. *Reviewed 2026-06-22.* The CMM's D1 fills governance of non-cyber AI risk from AI RMF / 600-1.

## Scope exclusions

- **Base SSDF v1.1 task semantics.** Tasks marked "No additions to SSDF 1.1" carry the Profile's Priority and Informative References but no AI-specific guidance; their base meaning is read from SP 800-218 and not re-audited here.
- **Informative-reference fidelity.** The OWASP `LLMxx` and AI RMF codes in the Informative References column were used to confirm cross-mapping intent, not independently verified against those source documents (the AI RMF stack is covered by [[standards-review-nist-ai-rmf-2026-Q2|its own review]]).
- **Production effectiveness.** Document-versus-document review per the methodology, not a deployment or self-attestation audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second reviewer (separate run) attempted a counter-example for each absence claim against Table 1 and Appendix A. **All five claims survived.**

1. **No measurable criteria — survives.** A sweep of all R/C/N items for acceptance criteria, thresholds, or test procedures returned none; the strongest candidate (`PS.1.3.R4`) names mechanisms without acceptance conditions.
2. **No deployment-time control — survives.** Section 1.2 scope exclusion is explicit; no Table 1 item specifies a deployed-system runtime control.
3. **No AI-BOM artifact spec — survives.** `PS.3.2.R1` points at generic SBOM/SLSA; no AI-specific format is prescribed. `PO.1.2.N1` lists documentation *forms* (data/model/system cards) without schemas.
4. **No multi-agent content — survives.** "agent" appears in no task as a subject; the document scopes to model development.
5. **Cybersecurity-only — survives.** The section 2 limitation statement is verbatim and self-confirming.

## Effect on existing wiki pages

- **[[nist-sp-800-218a|218A framework page]]:** the "three net-new tasks" prose was corrected to state that Table 1 tags six tasks `[Not part of SSDF 1.1]` (`PO.5.3`, `PS.1.2`, `PS.1.3`, `PW.3.1`, `PW.3.2`, `PW.3.3`) across one net-new practice (PW.3) and three additions to existing practices; the CMM-anchor table was extended to all nine domains with the task-level IDs above and a pointer to this review; the dual-use-foundation-model glossary entry was reconciled to the Appendix A criteria.
- **[[agentic-ai-security-cmm-2026|Canonical CMM]]:** the D8 `Maps to:` line citing 218A was extended with the specific task anchors (`PS.3.2`, `PW.4.4.R1`, `PS.1.3.R4`); a body cross-link to this review was added on the D8 and D6 rungs that cite the Profile.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the D8 218A cell was upgraded from "NIST SP 800-218A SSDF AI Profile" to the task-level anchors, with the AI-BOM-artifact absence (claim 3) cited to this review; D4 and D6 218A entries were added.
- **[[agentic-ai-security-cmm-d8-supply-chain|D8 deep dive]]:** the `PS.1.3` weight-protection reference was confirmed and joined by `PS.3.2`/`PW.4.4.R1` provenance/verification anchors; the AI-BOM-artifact gap was cited to claim 3.
- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]]:** the 218A treatment gained a correction marker pointing here; the clause-level audit supersedes the wiki-summary-level row.
- **[[standards-review-backlog|Standards Review Backlog]]:** NIST SP 800-218A flips to done.
