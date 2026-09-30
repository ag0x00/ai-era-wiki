---
type: framework
title: "MAAIS: Multilayer Agentic AI Security"
address: c-000006
created: 2026-05-07
updated: 2026-09-29
tags:
  - frameworks
  - agentic-ai
  - multi-layer
  - mitre-atlas
  - academic
status: developing
scope_axis:
  - sec-of-ai
authoring_organization: "Independent academic / preprint"
authors:
  - "[[sunil-arora|Sunil Arora]]"
  - "[[john-hastings|John Hastings]]"
publication_date: 2025-12-19
arxiv_id: 2512.18043v1
related:
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[csa-maestro|CSA MAESTRO]]"
  - "[[standards-review-csa-maestro-atf-2026-Q2|CSA MAESTRO and ATF Standards Review]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[nist-ai-rmf|NIST AI RMF]]"
  - "[[iso-iec-42001|ISO/IEC 42001]]"
  - "[[eu-ai-act|EU AI Act]]"
  - "[[agent-availability-threats|Agent Availability Threats]]"
sources:
  - "[[.raw/papers/maais-arora-hastings-2025-12-19.md]]"
verified: 2026-09-29
verified_against:
  - ".raw/papers/maais-arora-hastings-2025-12-19.md"
verified_findings: 0
verified_note: "Read full arXiv HTML preprint and archived paper summary; narrowed validation language to tactic-level coverage mapping."
---

# MAAIS — Multilayer Agentic AI Security Framework

A seven-layer defense-in-depth security framework for agentic AI systems, proposed in an arXiv preprint by [[sunil-arora|Sunil Arora]] and [[john-hastings|John Hastings]] (arXiv 2512.18043v1, 2025-12-19). The authors use Design Science Research and map the layers to [[mitre-atlas|MITRE ATLAS]] tactics as a preliminary coverage check. They also propose **CIAA**: the classical confidentiality, integrity and availability triad extended with accountability.

## CIAA — augmented security triad

The classical CIA triad is well-established as a foundation of cybersecurity. MAAIS adds **Accountability** as a fourth principle specific to autonomous decision-making:

> "In agentic AI, where decisions and actions may occur without direct human oversight, maintaining accountability is essential for security, transparency, and governance. Accountability enables identifying who is responsible for the AI Agent's decisions and outcomes."

The augmentation is a shorthand for action attribution, audit records and decision rights. [[agentic-ai-security-cmm-d1-governance|CMM D1]] grades those governance outcomes through its own criteria; CIAA is not a scored construct.

## The seven layers

Each layer carries controls under a defense-in-depth and zero-trust frame. The table states each layer's main concern; the paper gives its full control list.

| # | Layer | Core concern |
|---|---|---|
| **1** | Infrastructure Security | Secure deployment and operation of the compute and supply chain |
| **2** | Data Security | Confidentiality and integrity of data through its lifecycle |
| **3** | Model Security | Model hardening and resistance to adversarial modification |
| **4** | Agent Execution and Control | Confined execution and enforced action policy |
| **5** | Accountability and Trustworthiness | Documentation, provenance and human governance |
| **6** | User and Access Management | Identity and grant lifecycle |
| **7** | Monitoring and Audit | Protected action records, detection and response |

## MITRE ATLAS coverage mapping

The paper maps each ATLAS adversarial tactic to the MAAIS layer(s) intended to mitigate it:

| MITRE ATLAS tactic | MAAIS layer(s) |
|---|---|
| Reconnaissance | Monitoring and Audit |
| Initial Access | User and Access Management; Infrastructure Security |
| Execution | Agent Execution and Control |
| Persistence | Agent Execution and Control; Infrastructure Security |
| Privilege Escalation | User and Access Management; Infrastructure Security |
| Defense Evasion | Monitoring and Audit; Model Security |
| Credential Access | User and Access Management |
| Discovery | Monitoring and Audit |
| Collection | Data Security; Monitoring and Audit |
| Command and Control | Agent Execution and Control; Infrastructure Security |
| Exfiltration | Data Security; Infrastructure Security |
| Impact | Accountability and Trustworthiness; Agent Execution and Control |

The mapping is at the **tactic level**. The paper does not test controls against individual [[mitre-atlas|MITRE ATLAS]] techniques (`AML.T####`) or demonstrate effectiveness in a deployment. It establishes a proposed coverage map, not control validation.

## Cross-walk to wiki frameworks

MAAIS sits in conceptual space already occupied by several wiki pages. The crosswalk:

The [[csa-maestro|CSA MAESTRO]] layer names in the right-hand column are primary-source-verified by [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 standards review]] (Layer 1 is Foundation Models, Layer 2 is Data Operations).

| MAAIS Layer | Closest wiki [[agentic-ai-security-cmm-2026\|CMM]] domain | Closest wiki [[agentic-ai-security-reference-architecture\|RA]] boundary or component | [[csa-maestro\|CSA MAESTRO]] layer |
|---|---|---|---|
| 1 Infrastructure | D8 Engineering and Supply Assurance (release controls); D4 Runtime and Guardrails (confinement) | Release admission and confined runtime | L1 Foundation Models; L2 Data Operations |
| 2 Data Security | D6 Data, Memory and RAG | Retrieval mediator and corpus boundary | L2 Data Operations |
| 3 Model Security | D8 Engineering and Supply Assurance (model provenance); D4 Runtime and Guardrails | Model provider boundary and release admission | L1 Foundation Models |
| 4 Agent Execution & Control | D3 Control and Least Agency; D4 Runtime and Guardrails | Policy service, action gateway and confined runtime | L4 Deployment & Infrastructure (sandboxing); L3 Agent Frameworks |
| 5 Accountability & Trustworthiness | D1 Governance and Accountability; D9 Operations and Human Factors | Risk authority, approval service and evidence store | L7 Agent Ecosystem (audit/governance) |
| 6 User & Access Management | D2 Identity and Authorization | Identity and session service; credential broker | (Cross-cutting) |
| 7 Monitoring & Audit | D7 Observability and Detection; D9 Operations and Human Factors | Independent evidence store and security triage | L5 Evaluation & Observability |

**MAAIS layers and CMM domains serve different purposes.** MAAIS groups control categories by surface. The CMM tests outcomes and evidence for a defined deployment shape. The crosswalk identifies content overlap; a MAAIS category supplies neither an implementation test nor a CMM level.

## Comparison with adjacent agentic-AI multi-layer frameworks

| Dimension | **MAAIS** (Arora & Hastings, 2025) | [[csa-maestro\|CSA MAESTRO]] | Wiki [[agentic-ai-security-cmm-2026\|CMM]] | [[aws-agentic-ai-security-scoping-matrix\|AWS Scoping Matrix]] |
|---|---|---|---|---|
| Primary axis | Control surface (7 layers) | Threat model (7 layers) | Deployment domain × maturity (9 × 5) | Agency × autonomy (4 scopes) |
| Evidence method | MITRE ATLAS tactic mapping | Threat enumeration per layer | Criterion tests and evidence | Six security dimensions per scope |
| Maturity gating | None | None | Five cumulative levels per applicable domain | None (scope = deployment characterization) |
| Publication | arXiv preprint (Dec 2025) | Standards body (CSA) | This wiki (May 2026) | Vendor blog (AWS, Nov 2025) |
| Scope | Layer-control content | Layer-threat content | Maturity progression + control content | Scope characterization + control content |

## Limitations

- **Tactic-level mapping only.** The MITRE ATLAS table covers 12 tactics but does not test controls against specific `AML.T####` techniques. A threat-coverage review must inspect the relevant ATLAS techniques directly; a CMM level does not supply that coverage map.
- **No systematic threat enumeration per layer.** MAAIS lists controls and examples but provides no testable threat-to-control coverage by layer.
- **No prompt-injection / lethal-trifecta treatment.** The paper does not name [[lethal-trifecta|the Lethal Trifecta]], [[indirect-prompt-injection|indirect prompt injection]], [[mcp-security|MCP-specific risks]], [[a2a-protocol|agent-to-agent (A2A)]] orchestration risks, or [[promptware|promptware]]. These are the load-bearing threat classes in current practitioner discourse but absent from this framework.
- **No agency-vs-autonomy distinction.** Uses both terms but does not formalize the split (now anchored from the [[aws-agentic-ai-security-scoping-matrix|AWS Scoping Matrix]]).
- **No identity / NHI deep treatment.** Layer 6 (User & Access Management) lists controls but does not address the [[non-human-identity|NHI lifecycle]], [[identity-credential-coupling|identity-credential coupling]], or [[credential-proxy-pattern|credential proxy pattern]] that the wiki treats as load-bearing.

## Use cases

- **Lecture / course-material reference** — DSR-based academic paper appropriate for graduate-level AI security curricula. The seven-layer breakdown is teachable in a single lecture.
- **CIAA citation anchor** — when discussing accountability as a security principle alongside CIA, cite this paper's section IV-A as the published source of the augmentation.
- **MITRE ATLAS coverage check** — the tactic-level mapping table is a starting point for a coverage review. Inspect ATLAS techniques and the deployment's threat model for depth; use the CMM separately to assess implemented controls.

## Provenance

Authored by [[sunil-arora|Sunil Arora]] and [[john-hastings|John Hastings]]. The arXiv preprint 2512.18043v1 was submitted 2025-12-19 in cs.CR, cs.AI and cs.CY. DOI: [10.48550/arXiv.2512.18043](https://doi.org/10.48550/arXiv.2512.18043). No journal reference is listed. Source summary: [[maais-multilayer-agentic-ai-security-paper|the paper page]].

<!-- sources:auto -->
## Sources

- [Securing Agentic AI Systems -- A Multilayer Security Framework](https://arxiv.org/abs/2512.18043)
<!-- /sources -->
