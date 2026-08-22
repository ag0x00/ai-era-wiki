---
type: framework
title: "MAAIS: Multilayer Agentic AI Security"
address: c-000006
created: 2026-05-07
updated: 2026-08-19
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
  - "[[csa-maestro|CSA MAESTRO / CSA Agentic Trust Framework]]"
  - "[[standards-review-csa-maestro-atf-2026-Q2|CSA MAESTRO and ATF Standards Review]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[nist-ai-rmf|NIST AI RMF]]"
  - "[[iso-iec-42001|ISO/IEC 42001]]"
  - "[[eu-ai-act|EU AI Act]]"
  - "[[agent-availability-threats|Agent Availability Threats]]"
sources:
  - "[[.raw/papers/maais-arora-hastings-2025-12-19.md]]"
---

# MAAIS — Multilayer Agentic AI Security Framework

A seven-layer defense-in-depth security framework for agentic AI systems, proposed in an arXiv preprint by [[sunil-arora|Sunil Arora]] and [[john-hastings|John Hastings]] (arXiv 2512.18043v1, 2025-12-19). Methodologically grounded in Design Science Research (DSR) and validated via mapping the seven layers against [[mitre-atlas|MITRE ATLAS]] adversarial tactics. The framework's most distinctive single contribution is **CIAA** — augmenting the classical CIA security triad (Confidentiality, Integrity, Availability) with Accountability as a first-class principle for agentic systems.

## CIAA — augmented security triad

The classical CIA triad is well-established as a foundation of cybersecurity. MAAIS adds **Accountability** as a fourth principle specific to autonomous decision-making:

> "In agentic AI, where decisions and actions may occur without direct human oversight, maintaining accountability is essential for security, transparency, and governance. Accountability enables identifying who is responsible for the AI Agent's decisions and outcomes."

The augmentation is a memorable shorthand for the wiki's existing emphasis on action-to-identity tracing, audit chains of custody, and decision-rights documentation. CIAA is referenced inline in [[agentic-ai-security-cmm-2026|the CMM]] D1 (Governance & Accountability) as the foundation principle.

## The seven layers

Each layer carries a defined set of controls; the layered architecture is explicitly framed as defense-in-depth + zero-trust.

| # | Layer | Representative controls |
|---|---|---|
| **1** | Infrastructure Security | Secure hardware; secure compute; secure orchestration; supply-chain security; storage protection; network segmentation; secure CI/CD deployment |
| **2** | Data Security | Data confidentiality; access control and data governance; differential privacy; data provenance and lineage; data integrity |
| **3** | Model Security | Adversarial defense techniques; model hardening and confidentiality; poisoning and backdoor detection; secure model deployment and execution |
| **4** | Agent Execution and Control | Execution sandbox; policy (behavioral constraints) enforcement; runtime and safety verification; secure API and tools integration controls |
| **5** | Accountability and Trustworthiness | Explainable AI (XAI) techniques; bias detection and mitigation; AI system documentation; accountability and provenance mechanisms; human oversight and governance |
| **6** | User and Access Management | Identity governance; access control (authorization); MFA and credential protection; privilege management and segregation of duties; continuous access monitoring and behavioral analytics |
| **7** | Monitoring and Audit | Immutable logging and audit trails; continuous monitoring and anomaly detection; agent behavioral analytics; threat intelligence and adaptive security policies; automated incident response mechanisms |

## MITRE ATLAS validation mapping

The paper's validation step maps each ATLAS adversarial tactic to the MAAIS layer(s) responsible for mitigation:

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

The mapping is at the **tactic level**, not the technique level — the paper does not enumerate against the ~84 [[mitre-atlas|MITRE ATLAS]] techniques (`AML.T####`) individually. This is appropriate scope for a preprint introducing a framework but limits the validation to coverage rather than depth.

## Cross-walk to wiki frameworks

MAAIS sits in conceptual space already occupied by several wiki pages. The crosswalk:

The [[csa-maestro|CSA MAESTRO]] layer names in the right-hand column are primary-source-verified by [[standards-review-csa-maestro-atf-2026-Q2|the 2026-Q2 standards review]] (Layer 1 is Foundation Models, Layer 2 is Data Operations).

| MAAIS Layer | Closest wiki [[agentic-ai-security-cmm-2026\|CMM]] domain | Closest wiki [[agentic-ai-security-reference-architecture\|RA]] plane | [[csa-maestro\|CSA MAESTRO]] layer |
|---|---|---|---|
| 1 Infrastructure | D8 Supply Chain and AI-BOM (partial); cross-cutting infra | (Hosting plane — implicit; not a named RA plane) | L1 Foundation Models; L2 Data Operations |
| 2 Data Security | D6 Data, Memory & RAG | Data plane | L2 Data Operations |
| 3 Model Security | D6 Data, Memory & RAG (partial); D4 Runtime & Guardrails (model-side) | (Spans Runtime + Data planes) | L1 Foundation Models |
| 4 Agent Execution & Control | D3 Control & Least-Agency; D4 Runtime & Guardrails | Control plane; Runtime plane | L4 Deployment & Infrastructure (sandboxing); L3 Agent Frameworks |
| 5 Accountability & Trustworthiness | D1 Governance & Accountability | (Cross-cutting concern across planes) | L7 Agent Ecosystem (audit/governance) |
| 6 User & Access Management | D2 Identity & Authorization | Identity plane | (Cross-cutting) |
| 7 Monitoring & Audit | D7 Observability & Behavioral Monitoring; D1 Governance & Accountability | Observability plane | L5 Evaluation & Observability |

**MAAIS layers and CMM domains measure different things.** MAAIS organizes by **control surface** (what kind of asset/process is being protected). The CMM organizes by **operational domain** with maturity tiers per domain (how systematically each surface is secured). The crosswalk above gives the closest content overlap; it does not imply equivalence. A control may show up under the same surface in both, with MAAIS describing what control to deploy and the CMM describing how mature that deployment needs to be at each maturity level.

## Comparison with adjacent agentic-AI multi-layer frameworks

| Dimension | **MAAIS** (Arora & Hastings, 2025) | [[csa-maestro\|CSA MAESTRO]] | Wiki [[agentic-ai-security-cmm-2026\|CMM]] | [[aws-agentic-ai-security-scoping-matrix\|AWS Scoping Matrix]] |
|---|---|---|---|---|
| Primary axis | Control surface (7 layers) | Threat model (7 layers) | Operational domain × maturity (9 × 5+) | Agency × autonomy (4 scopes) |
| Validation | MITRE ATLAS tactic mapping | Threat enumeration per layer | Per-domain Maps-to (multiple frameworks) | Six security dimensions per scope |
| Maturity gating | None | None (gates from CSA ATF) | L1–L5+ per domain | None (scope = deployment characterization) |
| Publication | arXiv preprint (Dec 2025) | Standards body (CSA) | This wiki (May 2026) | Vendor blog (AWS, Nov 2025) |
| Scope | Layer-control content | Layer-threat content | Maturity progression + control content | Scope characterization + control content |

The [[owasp-ai-exchange|OWASP AI Exchange]] is not a layered framework, so it does not join the table above, but it reaches the same availability axis MAAIS's CIAA augmentation argues for: the Exchange carries availability of model behavior as one of six first-class impacts in its asset-and-impact matrix rather than as a layer ([`/go/aisecuritymatrix/`](https://owaspai.org/go/aisecuritymatrix/)). The Exchange's availability row is wider than the letter it matches. Its AI resource exhaustion entry names two attacker goals — depletion of funds, and unavailability of the system — and files both under model-behaviour availability ([`/go/airesourceexhaustion/`](https://owaspai.org/go/airesourceexhaustion/)). A CIAA reading that maps availability to service continuity misses the financial-depletion goal, which succeeds while the service continues to answer correctly. The threat class is enumerated on [[agent-availability-threats|Agent Availability Threats]].

## Stickiness assessment (2026-05-07, ~5 months post-publication)

- **CIAA (CIA + Accountability) framing**: **moderately sticky.** The naming convention is memorable and easy to extend. Variants of CIA augmentation appear elsewhere (e.g., Parkerian Hexad: + Possession, Authenticity, Utility), so adding Accountability is a natural extension. Whether CIAA specifically (vs alternatives like CIA + Auditability or CIA + Authenticity) becomes the canonical agentic-AI augmentation is unclear at five months out. The wiki adopts CIAA as the **inline anchor citation** when discussing accountability as a security principle alongside CIA.
- **MAAIS name**: **likely not sticky.** Multi-layer security frameworks for AI proliferate (CSA MAESTRO; the wiki's CMM; AWS Scoping Matrix; OWASP). Each preprint that proposes one tends to use a distinctive acronym; absent significant practitioner traction or follow-on citation, MAAIS is unlikely to be remembered as the canonical seven-layer.
- **Seven-layer structure**: **convergent shape, distinct content.** Multi-layer frameworks at 5–9 layers are common. MAAIS's specific seven-layer breakdown overlaps substantially with [[csa-maestro|MAESTRO]]'s seven layers but at different cuts. The convergence of "multi-layer ≈ 5-9 layers" is sticky; the specific layer breakdowns are bibliography-time choices.
- **MITRE ATLAS validation method**: **established practice.** MAAIS uses tactic-level mapping (~12 tactics). The wiki's [[agentic-ai-security-cmm-2026|CMM]] uses technique-level mapping (specific `AML.T####` IDs). Both methods are sound; MAAIS's choice is appropriate for its scope but limits depth.
- **Design Science Research methodology**: **academic-only.** DSR is a standard IS research framing; doesn't propagate into practitioner discourse.

## Limitations

- **Tactic-level validation, not technique-level.** The MITRE ATLAS mapping covers 12 tactics but does not address specific `AML.T####` techniques. For deeper validation, pair with the wiki's CMM domain Maps-to lines that cite specific techniques.
- **No threat enumeration per layer.** Unlike [[csa-maestro|MAESTRO]] which threat-models each layer explicitly, MAAIS lists layer controls without specifying which threats motivate each control.
- **No prompt-injection / lethal-trifecta treatment.** The paper does not name [[lethal-trifecta|the Lethal Trifecta]], [[indirect-prompt-injection|indirect prompt injection]], [[mcp-security|MCP-specific risks]], [[a2a-protocol|agent-to-agent (A2A)]] orchestration risks, or [[promptware|promptware]]. These are the load-bearing threat classes in current practitioner discourse but absent from this framework.
- **No agency-vs-autonomy distinction.** Uses both terms but does not formalize the split (now anchored from the [[aws-agentic-ai-security-scoping-matrix|AWS Scoping Matrix]]).
- **No identity / NHI deep treatment.** Layer 6 (User & Access Management) lists controls but does not address the [[non-human-identity|NHI lifecycle]], [[identity-credential-coupling|identity-credential coupling]], or [[credential-proxy-pattern|credential proxy pattern]] that the wiki treats as load-bearing.

## Concepts MAAIS surfaced that the wiki had under-treated

A May 2026 audit of MAAIS against the wiki identified four real coverage gaps that the wiki's threat-modeling had under-prioritized in favor of orchestration-layer (prompt-injection-class) threats. New concept pages now anchor each:

- [[differential-privacy|Differential Privacy]] — was zero-coverage; now the canonical defense citation for [[model-layer-attacks|inversion]] and membership-inference attacks; control surfaces in CMM D6 Maps-to.
- [[model-layer-attacks|Model-Layer Attacks]] — extraction / inversion / membership-inference were under-treated; the new page enumerates them as a class with shared defensive primitives; cites MITRE ATLAS techniques (`AML.T0024`, `AML.T0044`, `AML.T0048`).
- [[agent-availability-threats|Agent Availability Threats]] — runaway / recursion / resource-exhaustion as a named class. The [[lethal-trifecta|Lethal Trifecta]] is a C+I model; availability is a separate axis that the MAAIS CIAA framing surfaces and this concept page anchors.
- [[operational-xai-for-gating|Operational XAI for Action Gating]] — distinct from [[mechanistic-interpretability-for-defense|mechanistic interpretability]]; covers runtime-gated justifications for high-impact actions. MAAIS Layer 5 named "XAI Techniques" without specifying the operational vs research split; the wiki now documents both.

## Use cases

- **Lecture / course-material reference** — DSR-based academic paper appropriate for graduate-level AI security curricula. The seven-layer breakdown is teachable in a single lecture.
- **CIAA citation anchor** — when discussing accountability as a security principle alongside CIA, cite this paper's section IV-A as the published source of the augmentation.
- **MITRE ATLAS coverage check** — the tactic-level mapping table is a useful starting point if validating that a security architecture covers all twelve ATLAS tactics. Pair with technique-level Maps-to in the wiki's CMM for the depth dimension.

## Provenance

Authored by [[sunil-arora|Sunil Arora]] and [[john-hastings|John Hastings]]; arXiv preprint 2512.18043v1 submitted 2025-12-19; arXiv categories cs.CR / cs.AI / cs.CY; ACM classes K.6.5 (Computing Milieux — Management of Computing and Information Systems / Security and Protection), D.4.6 (Operating Systems — Security and Protection), I.2.11 (Artificial Intelligence — Distributed Artificial Intelligence). DOI: [10.48550/arXiv.2512.18043](https://doi.org/10.48550/arXiv.2512.18043). No journal reference (preprint). Source-summary at [[maais-multilayer-agentic-ai-security-paper|the paper page]].

<!-- sources:auto -->
## Sources

- [Securing Agentic AI Systems -- A Multilayer Security Framework](https://arxiv.org/abs/2512.18043)
<!-- /sources -->
