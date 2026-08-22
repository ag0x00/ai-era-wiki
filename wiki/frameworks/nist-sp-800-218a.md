---
type: framework
title: "NIST SP 800-218A: SSDF GenAI Profile"
address: c-000048
created: 2026-04-30
updated: 2026-08-21
tags:
  - frameworks
  - nist
  - secure-development
  - ai-supply-chain
  - ssdf
  - ssdf-community-profile
  - generative-ai
  - dual-use-foundation-models
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
framework_class: secure-sdlc-ai-profile
issuer: "[[nist|NIST]]"
homepage: https://csrc.nist.gov/pubs/sp/800/218/a/final
doi: 10.6028/NIST.SP.800-218A
current_version: "final (July 2024)"
primary_documents:
  - title: "NIST SP 800-218A — Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile"
    url: "https://doi.org/10.6028/NIST.SP.800-218A"
    version: "final"
    published: "2024-07-25"
    retrieved: "2026-06-22"
    archived_copy: ".raw/papers/nist-sp-800-218A.pdf"
    scope_in_wiki: "Table 1 (Profile practices/tasks/items, pp. 8–19); §1–3; Appendix A glossary"
first_published: 2024-04-29
last_substantive_update: 2024-07-25
authority: "Executive Order 14110 § 4.1.a"
authors:
  - "Harold Booth"
  - "Murugiah Souppaya"
  - "Apostol Vassilev"
  - "Michael Ogata"
  - "Martin Stanley (CISA)"
  - "Karen Scarfone"
adoption_signal: federal-mandated
aliases:
  - "SP 800-218A"
  - "SSDF AI Profile"
  - "SSDF Community Profile for AI"
related:
  - "[[standards-review-nist-sp-800-218a-2026-Q2]]"
  - "[[nist-ssdf]]"
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-600-1]]"
  - "[[nist|NIST]]"
  - "[[apostol-vassilev]]"
  - "[[microsoft-sdl]]"
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[owasp-llm-top-10]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
  - "[[model-layer-attacks]]"
  - "[[memory-poisoning]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[securing-agentic-coding]]"
sources:
  - "[[.raw/papers/nist-sp-800-218A.pdf]]"
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "https://csrc.nist.gov/pubs/sp/800/218/a/final"
  - "https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218A.pdf"
---

# NIST SP 800-218A — SSDF Community Profile for Generative AI and Dual-Use Foundation Models

**NIST SP 800-218A** (*Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile*) is NIST's AI-specific extension of [[nist-ssdf|SP 800-218 (SSDF v1.1)]], published July 2024 by Booth, Souppaya, Vassilev, Ogata, Stanley (CISA), and Scarfone. The publication answers Section 4.1.a of **Executive Order 14110** ("Safe, Secure, and Trustworthy Development and Use of Artificial Intelligence," October 2023), which directed NIST to "develop a companion resource to the Secure Software Development Framework to incorporate secure development practices for generative AI and for dual-use foundation models."

SP 800-218A is the wiki's **federal-anchor citation for secure development practices specific to AI models**. It is **not standalone**: *"This Profile supplements what SSDF version 1.1 already includes and is intended to be used in conjunction with it, not on its own."*

## Definition of an SSDF Community Profile

NIST formalizes a **Community Profile** as a baseline of SSDF practices and tasks that have been enhanced to address a particular use case. The 218A Profile augments the base SSDF with five additions:

- **Recommendations** (`R`): something the organization should do, AI-specific.
- **Considerations** (`C`): something the organization should consider doing.
- **Notes** (`N`): additional contextual information.
- **Priorities**: `High` / `Medium` / `Low` indicating relative importance within the AI Profile scope (not present in base SSDF).
- **Informative References**: pointers into NIST AI RMF 1.0, OWASP Top 10 for LLM Applications v1.1, and the NIST AI 100-2e2023 *Adversarial ML Taxonomy* (Vassilev et al.).

Each addition gets a unique identifier appended to the task ID (e.g., `PO.1.2.R1` is the first recommendation for task `PO.1.2`).

## Scope and Audience

**In-scope**: AI model development, covering data sourcing, designing, training, fine-tuning, and evaluating AI models, plus incorporating AI models into other software.

**Out-of-scope**: deployment and operation of AI systems with AI models (operational concerns); most parts of the data governance and management life cycle beyond training data security. SP 800-218A is a *development-time* document, not an *operation-time* one.

**Three audiences**:

- **AI model producers**: organizations developing their own generative AI or dual-use foundation models.
- **AI system producers**: organizations developing software that leverages a generative AI or dual-use foundation model.
- **AI system acquirers**: organizations acquiring a product or service that uses one or more AI systems.

A shared-responsibility model is explicit: a single AI capability typically spans all three roles, and acquirer-producer agreements must specify which party owns which practice.

## Six new tasks NOT in base SSDF

Table 1 tags **six tasks** with the explicit `[Not part of SSDF 1.1]` marker — confirmed against the source by [[standards-review-nist-sp-800-218a-2026-Q2|the 2026-Q2 standards review]]. Three add to existing practices, and three constitute a single net-new practice (PW.3, below):

| Task ID | Title | Practice | Priority |
|---|---|---|---|
| **PO.5.3** | Continuously monitor software execution performance and behavior in software development environments to identify potential suspicious activity and other issues | PO.5 (Secure Environments) | High |
| **PS.1.2** | Protect all training, testing, fine-tuning, and aligning data from unauthorized access and modification | PS.1 (Protect Code and Data) | High |
| **PS.1.3** | Protect all model weights and configuration parameter data from unauthorized access and modification | PS.1 (Protect Code and Data) | High |
| **PW.3.1** | Analyze data for signs of poisoning, bias, homogeneity, and tampering before use | PW.3 (Confirm Data Integrity) | High |
| **PW.3.2** | Track data provenance and document data of unknown provenance | PW.3 (Confirm Data Integrity) | Medium |
| **PW.3.3** | Include adversarial samples in training and testing data | PW.3 (Confirm Data Integrity) | Medium |

The first three define the wiki's federal-anchor citation surface for **(1) continuous development-environment monitoring as a secure-SDLC practice, (2) training-data integrity as a protected-asset class equivalent to code, and (3) model weights as a protected-asset class equivalent to code**. The PW.3 trio is detailed below.

## The net-new practice: PW.3

**PW.3 — Confirm the Integrity of Training, Testing, Fine-Tuning, and Aligning Data Before Use** is the Profile's largest contribution: an entirely new practice within the PW (Produce Well-Secured Software) group, comprising the three `[Not part of SSDF 1.1]` tasks above:

| Task | Description | Priority |
|---|---|---|
| **PW.3.1** | Analyze data for signs of data poisoning, bias, homogeneity, and tampering before using it for AI model training, testing, fine-tuning, or aligning purposes, and mitigate the risks as necessary | High |
| **PW.3.2** | Track the provenance, when known, of all training, testing, fine-tuning, and aligning data used for an AI model, and document which data do not have known provenance | Medium |
| **PW.3.3** | Include adversarial samples in the training and testing data to improve attack prevention | Medium |

This is the federal-anchor surface for the entire wiki concept cluster around [[memory-poisoning|data poisoning]], [[model-layer-attacks|model-layer attacks]], and adversarial-training mitigation. NIST cites the 2023-edition OWASP `LLM03` (Training Data Poisoning) and the AI 100-2e2023 *Adversarial ML Taxonomy* as informative references. In the current [[owasp-llm-top-10|OWASP LLM Top 10 2025 edition]] that category was renumbered and broadened: `LLM03:2025` is now Supply Chain and the poisoning risk moved to `LLM04:2025` Data and Model Poisoning, as verified by [[standards-review-owasp-llm-top-10-2026-Q2|the LLM Top 10 review]]. <!-- taxonomy-ok: 2023-edition label, attributed to NIST and corrected on this line -->


## High-priority AI-specific recommendations across the rest of the Profile

The Profile attaches **High-priority** AI-specific recommendations to existing SSDF tasks at multiple points. The wiki-load-bearing additions:

**Protect Software (PS): model weights and AI elements as a protected asset class.**

- `PS.1.1.R1` — *"Secure code storage should include AI models, model weights, pipelines, reward models, and any other AI model elements that need their confidentiality, integrity, and/or availability protected."*
- `PS.1.1.R3` — *"Store reward models separately from AI models and data."*
- `PS.1.1.R4` — *"Permit indirect access only to model weights."*
- `PS.1.3.R4` — *"Specify and implement additional risk-proportionate cybersecurity practices around model weights, such as encryption, cryptographic hashes, digital signatures, multi-party authorization, and air-gapped environments."*

**Protect Software (PS): provenance for AI models.**

- `PS.3.2.R1` — *"Track the provenance of an AI model and its components and derivatives, including the training libraries, frameworks, and pipelines used to build the model."*
- `PS.3.2.R2` — *"Track AI models that were trained on sensitive data ... determine if access to the models should be restricted to individuals who already have access to the sensitive data used for training."* (This is the model-weight-as-PII/sensitive-data inference.)

**Produce Well-Secured Software (PW): AI-specific threat modeling.**

- `PW.1.1.R1` — *"Incorporate relevant AI model-specific vulnerability and threat types in risk modeling. Examples include poisoning of training data, malicious code or other unwanted content in inputs and outputs, denial-of-service conditions arising from adversarial prompts, supply chain attacks, unauthorized information disclosure, theft of AI model weights, and misconfiguration of data pipelines."*
- `PW.1.1.C2` — *"During risk modeling, consider checking that the AI model is not in a critical path to make significant security decisions without a human in the loop."* (Directly anchors the wiki's [[human-parity-line|human-parity-line]] argument.)

**Produce Well-Secured Software (PW): input/output handling.**

- `PW.5.2` — *"Code the handling of inputs (including prompts and user data) and outputs carefully. All inputs and outputs should be logged, analyzed, and validated within the context of the AI model, and those with issues should be sanitized or dropped."* (Federal anchor for prompt-injection-input-validation.)
- `PW.5.3` — *"Encode inputs and outputs to prevent the execution of unauthorized code."*

**Produce Well-Secured Software (PW): AI red-teaming named explicitly.**

- `PW.8.1.R1` — *"Several forms of code testing can be used for AI models, including unit testing, integration testing, penetration testing, **red teaming**, use case testing, and **adversarial testing**."* (Federal-anchor citation for AI red-teaming as a recommended SDLC practice.)

**Respond to Vulnerabilities (RV): AI shutdown mechanism.**

- `RV.2.2.R2` — *"Establish and implement criteria and processes for when to stop using an AI model and when to roll back to a previous version and its components."*
- `RV.2.2.C1` — *"Consider being prepared to stop using an AI model at any time and to continue operations through other means until the AI model's risks are sufficiently addressed."*

These two surface the [[distributed-kill-switch|distributed kill switch]] concept at federal-policy level, and are the exact analog of one of [[microsoft-sdl-evolving-security-practices|Microsoft SDL's six AI focus areas]] (AI shutdown mechanisms).

## Glossary contributions

Appendix A defines several terms that the wiki should treat as canonical anchors for federal-level definitions:

- **artificial intelligence red-teaming**: *"A structured testing effort to find flaws and vulnerabilities in an AI system, often in a controlled environment and in collaboration with developers of AI."*
- **dual-use foundation model**: broad-data training, self-supervised, ≥tens of billions of parameters, applicable across wide range of contexts, exhibits or could be modified to exhibit high performance on tasks that pose serious risk to national security including (i) lowering CBRN-weapon barrier of entry for non-experts, (ii) enabling powerful offensive cyber operations through automated vulnerability discovery and exploitation, or (iii) permitting evasion of human control or oversight through deception/obfuscation.
- **generative artificial intelligence**: class of AI models that emulate the structure and characteristics of input data to generate derived synthetic content.
- **model weight**: a numerical parameter within an AI model that helps determine the model's outputs in response to inputs.
- **provenance**: metadata pertaining to the origination or source of specified data.

The **dual-use foundation model** definition is particularly wiki-relevant. Criterion (ii), *"enabling powerful offensive cyber operations through automated vulnerability discovery and exploitation,"* is the federal-policy frame for the wiki's [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] thesis. The Glasswing concrete-vulnerability disclosures (27-year-old OpenBSD, 16-year-old FFmpeg, autonomous Linux kernel privesc chain) are exactly what this definition anticipated.

## Sources of Expertise

The Profile leverages and integrates:

- NIST research and publications on trustworthy AI: [[nist-ai-rmf|AI RMF 1.0]], **NIST AI 100-2e2023** *Adversarial Machine Learning: A Taxonomy and Terminology* (Vassilev et al.), NIST SP 1270 (Bias in AI), Dioptra testbed.
- NIST SSDF v1.1 itself.
- NIST CSF 2.0, NIST SP 800-53 Rev 5, NIST SP 800-161r1 (Cyber Supply Chain Risk Management Practices).
- [[nist-ai-600-1|NIST AI 600-1]] (AI RMF Generative AI Profile).
- OWASP Top 10 for LLM Applications v1.1.
- Industry/government experts via NIST's January 2024 workshop.

## Stated Limitations

NIST is explicit about what 218A does not address:

- *"A limitation of the SSDF and this Profile is that they only address cybersecurity risk management."* Other AI risk types (data privacy, intellectual property, bias) need separate frameworks (the Profile points to AI RMF, AI 600-1, NIST SP 1270, Privacy Framework, IR 8286).
- **Model weights and training process tracking is structurally hard.** *"Source code for defining the model architecture and building model binaries is amenable to secure software engineering practices for versioning, lineage, and reproducibility. However, the final model weights are defined only after the model is trained and fine-tuned; this is where limitations in tracking all aspects of collection, processing, and training data arise."* This is the wiki's federal-anchored articulation of *why model weights need different controls than source code*: a load-bearing point for [[model-layer-attacks|model-layer attacks]] and the [[supply-chain-security-for-agents|supply chain]] discussion.
- **Future work: Implementation Examples.** The Profile leaves this column empty. NIST signals it "is considering adding a column for Implementation Examples in a future version."

## Adoption Signal

SP 800-218A is **federal-mandated** under EO 14110. Although the EO 14110 was an executive instrument (subject to revocation), its NIST deliverables (including 218A) remain published as NIST guidance and are referenced in subsequent federal AI procurement guidance. Organizations selling AI models or AI-augmented software to the US federal government are functionally required to demonstrate 218A-aligned practices under the SSDF self-attestation regime (OMB M-22-18 + CISA Secure Software Development Attestation Form).

**218A is the federal-anchor analog of [[microsoft-sdl-evolving-security-practices|Microsoft SDL for AI]] — but with regulatory teeth.** Both documents extend a classical secure-SDLC framework to AI workloads with strikingly convergent scope: training-data integrity, model weights as protected assets, agent identity, observability, [[threat-modeling-for-ai|threat modeling for AI]], and shutdown/rollback mechanisms. The difference is **enforcement**: Microsoft SDL is a vendor framework; SSDF + 218A is the basis for US federal procurement self-attestation. A vendor publishing to both ecosystems can use Microsoft SDL as the engineering instrument and 218A as the federal-attestation surface.

## In the RA / CMM

SP 800-218A is the federal-anchor citation across multiple [[agentic-ai-security-cmm-2026|CMM]] domains. The task-level coverage matrix below is reproduced from [[standards-review-nist-sp-800-218a-2026-Q2|the 2026-Q2 standards review]], which audited all nine domains against Table 1. 218A is a development-time document, so its anchors are build-time secure-SDLC obligations; D2 (Identity & Authorization) and D5 (Egress & Network) have no 218A anchor by the Profile's stated scope.

| CMM domain | 218A anchor |
|---|---|
| D1 Governance & Accountability | `PO.1.1.R1`/`R2`, `PO.1.2.R1` (AI in security requirements, org policy); `PO.2.x` (AI roles, training, leadership commitment) — partial |
| D2 Identity & Authorization | none specific — `PS.1.1`/`PS.1.3.R3` least-privilege is asset protection, not identity |
| D3 Control & Least-Agency | `PW.1.1.C2` (human-in-the-loop in critical security path); `PO.4.1.C1` — design-time consideration, thin |
| D4 Runtime & Guardrails | `PW.1.1.R1` (AI threat modeling); `PW.5.2`/`PW.5.3` (input/output handling); `PO.4.1.R1` (guardrails) — build-time only |
| D5 Egress & Network | none specific — `PO.5.1.R1` is dev-environment hardening, not runtime egress |
| D6 Data, Memory & RAG | **PW.3** (`PW.3.1`/`PW.3.2`/`PW.3.3` data integrity); `PS.1.2` (training data); `PS.3.2.R1`/`R2` (model provenance) — strongest domain |
| D7 Observability & Detection | `PO.5.3` (continuous dev-environment monitoring); `RV.1.1.R1` (log/monitor/analyze I/O); `RV.2.1.N1` — partial |
| D8 Supply Chain & AI-BOM | `PS.3.2` (provenance via SBOM/SLSA); `PW.4.4.R1`/`R2` (verify acquired models); `PS.1.3.R4` (weight protection); `PS.2.1.R1`/`R2` (hashes/signatures) — strong, no AI-BOM schema |
| D9 Operations & Human Factors | `RV.2.2.R2`/`C1` (stop-using-model criteria and rollback); `RV.1.3.R1`/`R2` (disclosure policy) — partial |

The [[agentic-ai-security-cmm-2026|CMM]]'s D8 cites 218A as the supply-chain reference; this page and the [[agentic-ai-security-cmm-crosswalk|CMM crosswalk]] now carry the task-level anchors per the standards review.

## Placement in this wiki

- **Federal-anchor citation surface for at least 5 CMM domains.** Replaces the previous stub that only flagged the gap.
- **First wiki page citing AI red-teaming as a NIST-recommended SDLC practice** (`PW.8.1.R1`). Closes a gap where wiki red-teaming concept pages cited industry sources but not a federal-policy anchor.
- **Documentary support for the wiki's "model weights ≠ source code" argument.** NIST's explicit acknowledgment that classical secure-development primitives (versioning, lineage, reproducibility) do not cleanly cover model weights is the cleanest federal articulation of the gap.
- **Federal-policy anchor for the [[frontier-ai-for-vuln-discovery|Frontier-AI-for-Vuln-Discovery thesis]]** via the dual-use-foundation-model glossary criterion (ii).
- **Anchor for [[secure-sdlc-framework-stack-2026|the 2026 framework-stack thesis]]'s "AI overlay" layer.** 218A is precisely what that thesis prescribes as the SSDF-side AI extension.

The profile's subject is the generative AI system under development. A conventional application built *by* a coding agent falls outside that subject, even though the agent introduces credentials, untrusted-input handling, and autonomous execution into the development environment. That scope boundary is recorded as [[secure-sdlc-framework-stack-2026|Gap 4]] of the framework-stack thesis, and the vendor-side controls available for it are graded in [[securing-agentic-coding|Securing Agentic Coding]].

## See also

- [[nist-ssdf|NIST SSDF — Secure Software Development Framework (SP 800-218 v1.1)]] — the base framework this Profile extends
- [[nist|NIST]] — issuing organization
- [[apostol-vassilev|Apostol Vassilev]] — co-author; NIST adversarial-ML lead
- [[nist-ai-rmf|NIST AI RMF 1.0]] — peer AI-governance framework (informative reference throughout the 218A column)
- [[nist-ai-600-1|NIST AI 600-1 — Generative AI Profile of the AI RMF]] — sibling Profile (RMF-side rather than SSDF-side)
- [[microsoft-sdl|Microsoft SDL]] — vendor-side peer with convergent AI scope
- [[microsoft-sdl-evolving-security-practices|Microsoft SDL: Evolving Security Practices for an AI-Powered World]] — vendor analog with convergent scope
- [[owasp-llm-top-10|OWASP Top 10 for LLM Applications]] — second informative-reference source in the 218A column
- [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026]] — thesis on AI-overlay composition
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — adjacent thesis

<!-- sources:auto -->
## Sources

- [NIST SP 800-218A: SSDF GenAI Profile](https://csrc.nist.gov/pubs/sp/800/218/a/final)
- [NIST SP 800-218A — Secure Software Development Practices for Generative AI and Dual-Use Foundation Models: An SSDF Community Profile](https://doi.org/10.6028/NIST.SP.800-218A)
- [nvlpubs.nist.gov](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218A.pdf)
<!-- /sources -->
