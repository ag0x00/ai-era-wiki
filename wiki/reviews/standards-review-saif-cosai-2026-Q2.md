---
type: review
title: "Google SAIF and CoSAI Standards Review"
address: c-000227
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - google
  - saif
  - cosai
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[google-saif]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
related:
  - "[[google-saif]]"
  - "[[cosai]]"
  - "[[cosai-org]]"
  - "[[saif-implementation-guide]]"
  - "[[a2a-protocol]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-review-backlog]]"
sources:
  - "https://saif.google/"
  - "https://saif.google/secure-ai-framework/risks"
  - "https://saif.google/secure-ai-framework/components"
  - "https://saif.google/secure-ai-framework/controls"
  - "https://www.coalitionforsecureai.org/"
  - "https://www.coalitionforsecureai.org/resources/"
  - ".raw/papers/saif-implementation-guide.pdf"
---

# Standards Review — Google SAIF and CoSAI, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to two paired instruments from the same lineage: [[google-saif|Google SAIF (Secure AI Framework)]] and [[cosai|CoSAI (Coalition for Secure AI)]]. Google donated the SAIF Risk Map and Risk Assessment to CoSAI in 2024, so the two are treated as one review with separate coverage columns. Sources are the live SAIF site (risks, components, controls pages), the [[saif-implementation-guide|SAIF Implementation Guide]] PDF archived under `.raw/papers/`, and the CoSAI site and resources listing. The review produces a clause-level coverage matrix across the nine [[agentic-ai-security-cmm-2026|CMM]] domains, falsifiable absence claims, and an adversarial second pass.

SAIF and CoSAI sit at opposite ends of one spectrum. SAIF is a principles-and-taxonomy instrument: 15 named risks, 14 components across four areas, and 24 named controls across six categories, each described as a control *category* without a pass/fail criterion or test procedure. CoSAI is a workstream-output instrument: four workstreams that ship dated technical papers (MCP security, agentic identity, supply-chain signing, incident response), each authoritative on its own subject and silent everywhere else.

**SAIF names control categories; it does not specify controls.** The 24 SAIF controls (for example "Adversarial Training and Testing", "Agent Permissions", "Threat Detection") are one-paragraph descriptions of *what category of control to apply*, with no measurable acceptance criterion, threshold, or test procedure. This is the same descriptive-not-specification limit the [[standards-review-mitre-atlas-2026-Q2|MITRE ATLAS review]] found in ATLAS mitigations: SAIF grounds a CMM domain's threat model and control vocabulary, not its per-level evidence rubric.

## Primary documents reviewed

Neither instrument is a single versioned specification. SAIF is a living website plus a quick-start PDF; CoSAI ships dated workstream papers under no umbrella version number.

| Document | URL / path | Scope used in this review |
|---|---|---|
| SAIF — Risks | [saif.google/secure-ai-framework/risks](https://saif.google/secure-ai-framework/risks) | The 15 named SAIF risks |
| SAIF — Components | [saif.google/secure-ai-framework/components](https://saif.google/secure-ai-framework/components) | The 14 named components across Data / Infrastructure / Model / Application |
| SAIF — Controls | [saif.google/secure-ai-framework/controls](https://saif.google/secure-ai-framework/controls) | The 24 named controls across six categories |
| SAIF — homepage / map | [saif.google](https://saif.google/) | SAIF 2.0 "Focus on Agents", Risk Self Assessment, SAIF Map structure |
| SAIF Implementation Guide (PDF) | `.raw/papers/saif-implementation-guide.pdf` | Six core elements, four-step adoption methodology |
| CoSAI — homepage | [coalitionforsecureai.org](https://www.coalitionforsecureai.org/) | The four workstreams (verbatim names), mission |
| CoSAI — resources | [coalitionforsecureai.org/resources](https://www.coalitionforsecureai.org/resources/) | The eight dated published deliverables |

### SAIF taxonomy as read (2026-06-22)

- **15 risks:** Data Poisoning; Unauthorized Training Data; Model Source Tampering; Excessive Data Handling; Model Exfiltration; Model Deployment Tampering; Denial of ML Service; Model Reverse Engineering; Insecure Integrated Component; Prompt Injection; Model Evasion; Sensitive Data Disclosure; Inferred Sensitive Data; Insecure Model Output; Rogue Actions.[^saif-risks]
- **24 controls, six categories:** *Data* — Privacy Enhancing Technologies, Training Data Management, Training Data Sanitization, User Data Management. *Infrastructure* — Model and Data Inventory Management, Model and Data Access Controls, Model and Data Integrity Management, Secure-by-Default ML Tooling. *Model* — Input Validation and Sanitization, Output Validation and Sanitization, Adversarial Training and Testing. *Application* — Application Access Management, User Transparency and Controls, Agent User Control, Agent Permissions, Agent Observability. *Assurance* — Red Teaming, Vulnerability Management, Threat Detection, Incident Response Management. *Governance* — User Policies and Education, Internal Policies and Education, Product Governance, Risk Governance.[^saif-controls]
- **14 components, four areas:** *Data* — Data Sources, Data Filtering and Processing, Training Data. *Infrastructure* — Model Frameworks and Code, Training/Tuning/Evaluation, Data and Model Storage, Model Serving. *Model* — The Model, Input Handling, Output Handling. *Application* — Application, Agent.[^saif-components]
- **Six core elements** (Implementation Guide): Expand strong security foundations; Extend detection and response; Automate defenses; Harmonize platform-level controls; Adapt controls and create faster feedback loops; Contextualize AI system risks in business processes.[^saif-guide]

### CoSAI workstreams and deliverables as read (2026-06-22)

- **Four workstreams (verbatim):** WS1 Software Supply Chain Security for AI Systems; WS2 Preparing Defenders for a Changing Security Landscape; WS3 AI Security Risk Governance; WS4 Secure Design Patterns for Agentic Systems.[^cosai-ws]
- **Eight published deliverables:** AI Shared Responsibility Framework (2026-05-28); Agentic Identity and Access Management (2026-04-17); The Future of Agentic Security: From Chatbots to Autonomous Swarms (2026-03-31); Model Context Protocol (MCP) Security (2026-01-20); AI Incident Response Framework (2025-10-30); Signing ML Artifacts: Building towards tamper-proof ML metadata records (2025-09-29); Preparing Defenders of AI Systems (2025-07-16); Establish Risks and Controls for the AI Supply Chain (2025-06-25).[^cosai-res]

> [!contradiction] Wiki figures for the MCP paper and CoSAI publication dates need reconciliation
> The [[cosai|CoSAI page]] dates the MCP Security White Paper to "January 27, 2026"; the CoSAI resources listing dates "Model Context Protocol (MCP) Security" to **2026-01-20**. The CoSAI page also names "Agentic IAM Apr 2026"; the listing title is **"Agentic Identity and Access Management" (2026-04-17)**. The "AI Incident Response Framework v1.0" is dated **2025-10-30** on the resources page. The "nearly 40 threats across 12 categories" MCP figure was not re-verifiable from the homepage/resources pages in this pass and is flagged for a deeper-source check.

## Structure-to-domain grounding

The two instruments map onto the CMM by different mechanisms.

- **SAIF — broad-but-shallow.** SAIF's six core elements and 24 controls touch every CMM domain, including governance (D1, via Product Governance and Risk Governance) and operations (D9, via Incident Response Management and the Assurance category). The breadth is real; the depth is not — each control is a category label.
- **CoSAI — deep-but-discontinuous.** CoSAI's coverage is wherever a workstream has shipped a paper: strong on supply chain (D8, WS1 + Signing ML Artifacts), agentic identity (D2, Agentic IAM), inter-agent and MCP runtime (D4/D5, MCP Security + A2A), incident response (D9, AI IR Framework), and governance (D1, AI Security Risk Governance + Shared Responsibility). It has no graded model and no coverage between papers.
- **Agents are first-class in both.** SAIF 2.0's "Focus on Agents" and three Agent-specific controls (Agent User Control, Agent Permissions, Agent Observability) plus CoSAI WS4 give both instruments explicit least-agency content — the strongest single area of the pair.

## Clause-level coverage matrix (CMM x SAIF / CoSAI)

Each cell cites the verified SAIF control/element or CoSAI workstream/deliverable that anchors the CMM domain. SAIF citations name the control or core element; CoSAI citations name the workstream (WS1-WS4) or the dated deliverable.

| CMM domain | SAIF coverage (controls / six elements) | CoSAI coverage (workstreams / deliverables) |
|---|---|---|
| D1 Governance & Accountability | Governance category: Product Governance, Risk Governance, Internal/User Policies and Education; Element 6 (Contextualize risks). Category-level only. | WS3 AI Security Risk Governance; AI Shared Responsibility Framework (2026-05-28). No maturity criteria. |
| D2 Identity & Authorization | Infrastructure: Model and Data Access Controls; Application: Application Access Management, Agent Permissions. Names categories; no NHI/agent-credential scheme. | Agentic Identity and Access Management (2026-04-17); [[a2a-protocol\|A2A]] signed Agent Cards. Strongest CoSAI identity content. |
| D3 Control & Least-Agency | Application: Agent User Control, Agent Permissions; "Secure Agents" principles (human control, limited powers, observability). | WS4 Secure Design Patterns for Agentic Systems; The Future of Agentic Security (2026-03-31). |
| D4 Runtime & Guardrails | Model: Input Validation and Sanitization, Output Validation and Sanitization, Adversarial Training and Testing; Element 3 (Automate defenses). | Model Context Protocol (MCP) Security (2026-01-20); WS4 secure design patterns. |
| D5 Egress & Network | Components Input Handling / Output Handling (filter/sanitize); no named egress-filtering or network-segmentation control. | MCP Security (2026-01-20) addresses tool/transport boundary; no agent-egress allowlist spec. |
| D6 Data, Memory & RAG | Data category: Training Data Management, Training Data Sanitization, Privacy Enhancing Technologies, User Data Management; risks Data Poisoning, Unauthorized Training Data. Training-data-centric. | WS1 supply-chain provenance touches training data; no RAG-runtime or memory-poisoning control paper. |
| D7 Observability & Detection | Assurance: Threat Detection; Application: Agent Observability; Element 2 (Extend detection and response), Element 5 (feedback loops). | WS2 Preparing Defenders; Preparing Defenders of AI Systems (2025-07-16). No detection-rule content. |
| D8 Supply Chain & AI-BOM | Infrastructure: Model and Data Inventory Management, Model and Data Integrity Management, Secure-by-Default ML Tooling; Element 1 (track supply-chain assets). | **Strongest area.** WS1 Software Supply Chain Security; Establish Risks and Controls for the AI Supply Chain (2025-06-25); Signing ML Artifacts (2025-09-29, SLSA-based). |
| D9 Operations & Human Factors | Assurance: Vulnerability Management, Incident Response Management, Red Teaming; Element 5; Implementation Guide four-step adoption. | AI Incident Response Framework (2025-10-30) — closest industry AI IR playbook; WS2 defender prep. |

The matrix confirms the pair's complementary shape: SAIF supplies a complete control *vocabulary* spanning all nine domains; CoSAI supplies *depth* on the subset its workstreams have reached (D8, D2, D4, D9). Neither supplies graded level criteria, so both ground a CMM domain's threat model and control taxonomy rather than its per-level evidence rubric.

## Falsifiable absence claims found

What the CMM scores that SAIF and CoSAI, as read on 2026-06-22, do not provide. Each is bounded to the searched scope and reversible by the stated refuting evidence.

1. **SAIF controls carry no measurable acceptance criteria.** All 24 SAIF controls are category descriptions; none states a threshold, pass/fail criterion, evidence requirement, or test procedure. *Searched:* SAIF controls page (all 24, six categories), 2026-06-22. *Terms:* "criteria", "measure", "threshold", "shall", "minimum", "test", "evidence", "metric". *Verdict:* confirmed. The closest, "Adversarial Training and Testing" and "Red Teaming", name an activity without a cadence, coverage target, or acceptance condition. *Refuting evidence:* any SAIF control page stating a measurable criterion or conformance test. *Reviewed 2026-06-22.* This is the gap the CMM per-level rubric fills; it mirrors the ATLAS-mitigation finding.

2. **No AI-BOM requirement despite strong supply-chain coverage.** Neither SAIF nor CoSAI mandates a structured AI Bill of Materials artifact. SAIF names "Model and Data Inventory Management"; CoSAI ships supply-chain and ML-artifact-signing papers. *Searched:* SAIF controls/components pages; CoSAI resources listing (WS1 deliverables), 2026-06-22. *Terms:* "AI-BOM", "AIBOM", "bill of materials", "SBOM", "ML-BOM". *Verdict:* not addressed as a named required artifact. **Near-miss disclosed:** Signing ML Artifacts (2025-09-29) builds "tamper-proof ML metadata records" — provenance metadata, not a BOM schema. *Refuting evidence:* a SAIF control or CoSAI deliverable specifying an AI-BOM artifact and its required fields. *Reviewed 2026-06-22.* Matches the AI-BOM gap the CMM D8 carries; ATLAS `AML.M0023` names the category SAIF/CoSAI do not.

3. **No cognitive-file-integrity or proof-of-guardrail control.** Neither instrument addresses integrity controls for agent rule files, identity/system-prompt files, or cryptographic proof that a guardrail executed. *Searched:* SAIF controls page (Model and Data Integrity Management), CoSAI resources (MCP Security, agentic papers), 2026-06-22. *Terms:* "system prompt integrity", "rules file", "identity file", "cognitive integrity", "proof of guardrail", "attestation". *Verdict:* not addressed. SAIF "Model and Data Integrity Management" covers model/data artifacts, not agent rule/identity files. *Refuting evidence:* a control naming integrity for agent rule/identity/system-prompt files, or guardrail-execution attestation. *Reviewed 2026-06-22.* Matches the cognitive-file-integrity gap carried in the CMM.

4. **No graded maturity model or conformance scheme.** Both instruments are non-graded and non-certified. SAIF offers a Risk Self Assessment (questionnaire), not maturity tiers; CoSAI workstream outputs are not certification-backed. *Searched:* SAIF homepage (Risk Self Assessment), controls page; CoSAI homepage, resources listing, 2026-06-22. *Terms:* "maturity", "tier", "level", "certification", "conformance", "audit", "attestation". *Verdict:* confirmed. *Refuting evidence:* a SAIF maturity rubric with level definitions, or a CoSAI conformance/certification program. *Reviewed 2026-06-22.* This is structural: the CMM exists to add the grading both instruments omit.

5. **CoSAI coverage is discontinuous between shipped papers.** CoSAI provides no content for CMM domains a workstream has not yet reached — notably non-supply-chain D6 (agent memory/RAG-runtime poisoning) and standalone D5 egress filtering. *Searched:* CoSAI resources listing (all eight deliverables), workstream names, 2026-06-22. *Terms:* "memory poisoning", "RAG", "egress", "data exfiltration", "network segmentation". *Verdict:* not addressed outside the MCP/supply-chain papers. **Near-miss disclosed:** MCP Security (2026-01-20) covers the tool/transport boundary, adjacent to but not a general agent-egress control. *Refuting evidence:* a CoSAI deliverable on agent-memory poisoning or agent-egress filtering. *Reviewed 2026-06-22.*

## Scope exclusions

- **The full SAIF Risk Map cross-references.** The matrix uses the 15 risks and 24 controls; the per-risk-to-component mapping arrows in the interactive SAIF Map were not enumerated cell by cell.
- **CoSAI deliverable internals.** Paper titles and dates were verified from the resources listing; the clause-level content of each PDF (e.g. every MCP threat) was not re-extracted in this pass. The "40 threats / 12 categories" MCP figure is retained as previously sourced and flagged for a deeper-source check.
- **A2A protocol depth.** A2A is reviewed only as it anchors D2/D3; [[a2a-protocol|the A2A page]] carries the detail.
- **Production effectiveness.** Document-versus-document review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second pass attempted a counter-example for each absence claim against the SAIF controls/components/risks pages and the CoSAI resources listing as read 2026-06-22. **All five claims survived; claims 2 and 5 were narrowed after partial refutation.**

1. **SAIF controls lack acceptance criteria — survives.** No control page surfaced a threshold, conformance test, or evidence requirement; activity-named controls (Red Teaming, Adversarial Training and Testing) specify no cadence or coverage target.
2. **No AI-BOM — partially refuted, then narrowed.** Signing ML Artifacts (2025-09-29) and Model and Data Inventory Management are real and adjacent; neither defines a BOM artifact with required fields, so the narrowed "no named AI-BOM artifact" claim holds.
3. **No cognitive-file-integrity / proof-of-guardrail — survives.** "Model and Data Integrity Management" covers model/data artifacts; no control addresses agent rule/identity files or guardrail-execution attestation.
4. **No graded maturity / conformance — survives.** Risk Self Assessment is a questionnaire, not tiers; CoSAI outputs are explicitly not certification-backed.
5. **CoSAI discontinuous — partially refuted, then narrowed.** MCP Security covers the tool/transport boundary (near-miss on egress); no deliverable covers agent-memory poisoning, so the narrowed claim holds.

## Effect on existing wiki pages

- **[[google-saif|SAIF framework page]]:** the verified taxonomy was reconciled onto the page — the 15 named risks, the 24 controls across six categories, and the 14 components across four areas were added with primary-source footnotes; SAIF 2.0 "Focus on Agents" and the three Agent controls were noted; the descriptive-not-specification limit was framed against this review.
- **[[cosai|CoSAI framework page]]:** the four workstream names were corrected to the verbatim site labels; the eight dated deliverables replaced the prior partial list; the MCP paper date discrepancy (2026-01-20 vs 2026-01-27) and the Agentic IAM title/date (2026-04-17) were reconciled, with the unverified "40 threats" figure flagged.
- **[[cosai-org|CoSAI organization page]]:** the activity list was reconciled to the verified deliverable dates.
- **[[agentic-ai-security-cmm-2026|Canonical CMM]] and the D1/D8/D9 deep dives:** the SAIF/CoSAI anchors per domain were set to the verified control/workstream names from the matrix.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the SAIF and CoSAI columns were updated to the verified control/workstream citations.
- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]]:** the SAIF/CoSAI rows gained a correction marker pointing here.
- **[[standards-review-backlog|Standards Review Backlog]]:** Google SAIF / CoSAI flips to done.

## Notes

[^saif-risks]: [SAIF — Risks](https://saif.google/secure-ai-framework/risks), retrieved 2026-06-22. 15 named risks.
[^saif-controls]: [SAIF — Controls](https://saif.google/secure-ai-framework/controls), retrieved 2026-06-22. 24 controls across Data, Infrastructure, Model, Application, Assurance, Governance.
[^saif-components]: [SAIF — Components](https://saif.google/secure-ai-framework/components), retrieved 2026-06-22. 14 components across Data, Infrastructure, Model, Application.
[^saif-guide]: Secure AI Framework Approach — A quick guide to implementing SAIF, Google, 2024; archived `.raw/papers/saif-implementation-guide.pdf` and summarized at [[saif-implementation-guide|the SAIF Implementation Guide page]]. Six core elements.
[^cosai-ws]: [CoSAI — Workstreams](https://www.coalitionforsecureai.org/workstreams/), retrieved 2026-06-22. Four workstreams.
[^cosai-res]: [CoSAI — Resources](https://www.coalitionforsecureai.org/resources/), retrieved 2026-06-22. Eight published deliverables with dates.
