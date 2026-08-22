---
type: framework
title: "Google SAIF: Secure AI Framework"
created: 2026-04-30
updated: 2026-08-21
tags:
  - frameworks
  - google
  - ai-security
  - lifecycle-security
status: developing
origin: aggregated
source_url: "https://saif.google/"
scope_axis:
  - sec-of-ai
adoption_signal: maintained
last_substantive_update: 2024-07-01
published_by: "[[google|Google]]"
current_version: "Living framework; no formal version numbering"
first_published: "2023"
scope: "AI security framework covering data, infrastructure, model, and application layers; donated SAIF data and Risk Map to CoSAI in 2024"
audience: "Enterprise AI deployers, platform teams, security architects"
aliases:
  - "SAIF"
  - "Secure AI Framework"
related:
  - "[[google|Google]]"
  - "[[cosai]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[saif-implementation-guide|SAIF Implementation Guide]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[owasp-ai-exchange]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/saif-implementation-guide.pdf]]"
---

# Google SAIF — Secure AI Framework

**Google SAIF** (Secure AI Framework) provides AI security principles and lifecycle guidance across data, infrastructure, model, and application layers. Google's primary institutional vehicle for collaborative standards is now [[cosai|CoSAI]], to which Google donated the SAIF Risk Map and Risk Assessment in 2024.

## Key Concepts

SAIF introduced **"Secure Agents" principles** covering three dimensions:
1. **Human control** — maintaining oversight over autonomous agent actions
2. **Limited powers** — agents operate with minimum necessary capabilities
3. **Observability** — comprehensive monitoring of agent behavior

These map directly to what the [[owasp-agentic-ai-top-10|ASI Top 10]] later formalized as the "Least Agency" principle.

## SAIF Map taxonomy (verified 2026-06-22)

The [[standards-review-saif-cosai-2026-Q2|Google SAIF and CoSAI standards review]] verified the SAIF Map's three enumerations against the live site on 2026-06-22: 15 risks, 24 controls in six categories, and 14 components in four areas. SAIF 2.0's "Focus on Agents" adds three Agent-specific controls (Agent User Control, Agent Permissions, Agent Observability).

**SAIF names control categories; it does not specify controls.** Each of the 24 controls is a one-paragraph description of *what category of control to apply*, with no measurable acceptance criterion, threshold, or test procedure. SAIF grounds a CMM domain's threat model and control vocabulary, not its per-level evidence rubric. This is the same descriptive-not-specification limit the [[standards-review-mitre-atlas-2026-Q2|MITRE ATLAS review]] found in ATLAS mitigations. The [[owasp-ai-exchange|OWASP AI Exchange]] supplies a same-risk comparison: against SAIF's Model Evasion risk and its Adversarial Training and Testing control, the Exchange names five controls with methods, tools, and stated costs, including Madry adversarial training with the PGD attack ([`/go/trainadversarial/`](https://owaspai.org/go/trainadversarial/)). Specification depth is the axis separating the two catalogues; neither grades maturity.

- **15 risks:** Data Poisoning; Unauthorized Training Data; Model Source Tampering; Excessive Data Handling; Model Exfiltration; Model Deployment Tampering; Denial of ML Service; Model Reverse Engineering; Insecure Integrated Component; Prompt Injection; Model Evasion; Sensitive Data Disclosure; Inferred Sensitive Data; Insecure Model Output; Rogue Actions.[^saif-risks]
- **24 controls, six categories:**[^saif-controls]
  - *Data* — Privacy Enhancing Technologies; Training Data Management; Training Data Sanitization; User Data Management.
  - *Infrastructure* — Model and Data Inventory Management; Model and Data Access Controls; Model and Data Integrity Management; Secure-by-Default ML Tooling.
  - *Model* — Input Validation and Sanitization; Output Validation and Sanitization; Adversarial Training and Testing.
  - *Application* — Application Access Management; User Transparency and Controls; Agent User Control; Agent Permissions; Agent Observability.
  - *Assurance* — Red Teaming; Vulnerability Management; Threat Detection; Incident Response Management.
  - *Governance* — User Policies and Education; Internal Policies and Education; Product Governance; Risk Governance.
- **14 components, four areas:**[^saif-components]
  - *Data* — Data Sources; Data Filtering and Processing; Training Data.
  - *Infrastructure* — Model Frameworks and Code; Training/Tuning/Evaluation; Data and Model Storage; Model Serving.
  - *Model* — The Model; Input Handling; Output Handling.
  - *Application* — Application; Agent.

## Six Core Elements (the SAIF "Approach")

The SAIF framework's practitioner-facing contribution, documented at length in [[saif-implementation-guide|the SAIF Implementation Guide]] (Google, 2024). The six elements are levers to apply collectively rather than steps to follow chronologically:

1. **Expand strong security foundations to the AI ecosystem**: review existing security controls for AI applicability; analyze gaps; track supply-chain assets / code / training data; scale data governance to AI; **retain and retrain** existing security talent rather than only hiring external AI experts.
2. **Extend detection and response to bring AI into an organization's threat universe**: develop AI-specific threat understanding; respond to attacks against AI *and* issues raised by AI output; for Gen AI, enforce **content safety policies**; adjust abuse policy + IR processes for AI-specific incident types.
3. **Automate defenses to keep pace with existing and new threats**: augment traditional defenses (encryption, access control, auditing) with AI-driven training-data and model protections; use AI to automate time-consuming defensive tasks (e.g., AI-generated YARA rules from malware analysis); keep humans in the loop on important decisions.
4. **Harmonize platform-level controls to ensure consistent security across the organization**: periodic review of AI usage and lifecycle; **prevent fragmentation** by standardizing on tooling and frameworks; right-fit existing control frameworks to AI rather than creating parallel ones.
5. **Adapt controls to adjust mitigations and create faster feedback loops for AI deployment**: Red Team exercises; track novel attacks (**prompt injection, data poisoning, evasion**); apply ML to improve detection accuracy and speed; **create a feedback loop** so Red Team findings flow back into training and protections.
6. **Contextualize AI system risks in surrounding business processes**: establish a **model risk management framework**; build inventory of AI models with risk profiles; enforce data-privacy / cyber-risk / third-party-risk policies through the ML model lifecycle; apply **shared responsibility** (developer / deployer / user splits); **match AI use cases to risk tolerances**.

The six elements are distinct from (and complementary to) the SAIF Risk Map's **four layers** (data / infrastructure / model / application) — the layers describe *what* to protect; the elements describe *how* to apply controls. Both views are part of SAIF.

## Four-Step Implementation Methodology

The implementation guide's adoption pathway:

1. **Understand the use** — characterize the business problem, the data needed, the user-interaction model, and whether you are building / fine-tuning / using a third-party model. Each combination drives different control requirements.
2. **Assemble the team** — multidisciplinary stakeholder list: Business use case owners; Security; Cloud Engineering; Risk and Audit; Privacy; Legal; Data Science; Development; Responsible AI and Ethics. Establishes that security, privacy, risk, and compliance considerations are included from the start.
3. **Level set with an AI primer** — bring all stakeholders to a baseline understanding of AI / ML / Deep Learning / Gen AI / LLMs and the model development lifecycle.
4. **Apply the six core elements** — the framework's substantive content; not chronological, treat as collectively-applied levers.

This stakeholder list operates at the **implementation-team level** — distinct from the [[ai-agent-layered-council|AI Agent Layered Council]]'s governance-level cross-functional body (CIO + CFO + COO + GC + Procurement + CHRO). SAIF's team is who *does the work*; the Layered Council is who *governs the program*.

## CMM / RA crosswalk for the six elements

| SAIF element | Wiki [[agentic-ai-security-cmm-2026\|CMM]] domain | Wiki [[agentic-ai-security-reference-architecture\|RA]] plane |
|---|---|---|
| **1** Expand security foundations | D8 (Supply Chain & AI-BOM); D6 (Data, Memory & RAG); D9 (Operations & Human Factors) | (Cross-cutting; foundational) |
| **2** Extend detection & response | D7 (Observability and Detection) | Observability plane |
| **3** Automate defenses | D4 (Runtime & Guardrails); D7 (Observability and Detection) | Runtime plane; Observability plane |
| **4** Harmonize platform-level controls | D1 (Governance & Accountability); D3 (Control & Least-Agency) | Control plane |
| **5** Adapt controls + feedback loops | D7 (Observability and Detection); D9 (Operations & Human Factors) | Observability plane; Cross-cutting |
| **6** Contextualize risks in business | D1 (Governance & Accountability) | (Cross-cutting; governance) |

## Q1 2026 Developments

No formal "SAIF 2.0" has been announced. Google's Q1 2026 AI security contributions primarily flowed through CoSAI and Google ADK:

**Google ADK Go 1.0** (March 31, 2026) shipped with built-in security features:
- **OpenTelemetry integration** — full observability out of the box
- **Human-in-the-loop confirmations** — configurable approval gates
- **`before_model_callback` hooks** — platform-level input validation (the reference implementation of platform-level enforcement)
- **Model Armor integration** — prompt injection detection at the API layer

**[[a2a-protocol|A2A Protocol]]** reached **v1.0.0 on 2026-03-12** with signed Agent Cards (§8.4) and gRPC support; under Linux Foundation governance since 2025-06-23.

> [!warning] Supply chain risk in Google's own toolchain
> A LiteLLM supply chain compromise was detected in ADK dependencies (March 24, 2026), demonstrating that even Google's AI toolchain is not immune to supply chain attacks.

**Google donated** its SAIF Risk Map and Risk Assessment to CoSAI, ensuring continuity of SAIF content under the multi-stakeholder collaborative model.

## Architecture: A2A Protocol

The **Agent-to-Agent (A2A) protocol** introduces an important security principle:

- **Opacity principle** — agents collaborate without sharing internal memory or proprietary logic; only externally visible capabilities are exchanged
- **Agent Cards** at `.well-known` endpoints for identity discovery
- **Signed Agent Cards** (§8.4 of [[a2a-protocol|A2A v1.0]], algorithm-agnostic — Ed25519 is the de facto vendor pick) enabling cryptographic verification of identity-and-capability metadata
- Authentication delegates to OpenAPI-style schemes (OAuth 2.0/OIDC, mTLS, API keys); message-level integrity, replay protection, and cross-agent delegation remain vendor-side or proposal-side

## Strengths

- ADK's `before_model_callback` is the reference implementation of **platform-level security enforcement** — the pattern the paper identifies as most critical
- A2A "opacity principle" is a sound architectural security pattern for multi-agent systems
- SAIF's comprehensive lifecycle coverage (data → infrastructure → model → application) remains one of the most holistic single-framework scopes
- CoSAI donation ensures SAIF content has multi-stakeholder governance

## Gaps and Shortcomings

- CoSAI publications remain **principled guidance rather than enforceable specifications**
- [[a2a-protocol|A2A v1.0]] has security woven through §7 + §8.4 but **no standalone security specification** — message integrity, replay protection, and cross-agent delegation remain implementer / vendor concerns (see [[multi-agent-runtime-security|Multi-Agent Runtime Security]] for the depth)
- Agent identity in A2A relies on Agent Cards at .well-known endpoints; formalized authorization schemes remain on the roadmap
- AI Incident Response Framework lacks AI-specific IoCs or forensic procedures
- No AI-BOM requirements
- Cognitive file integrity and proof-of-guardrail concepts are unaddressed

## See Also

- [[google|Google]] (publisher)
- [[cosai|CoSAI — Coalition for Secure AI]] — the collaborative successor vehicle for SAIF content
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — SAIF Foundation Govern → **D1**; Element 1 (Data) → **D6 + D8**; Element 4 (Application defense) → **D4**; Element 5 (Auto-defense / Detect) → **D7**
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — taxonomy that addresses the agentic gaps in SAIF
- [[agent-observability|Agent Observability]] — ADK's OTel and before_model_callback validate the glass-box approach
- [[standards-review-saif-cosai-2026-Q2|Google SAIF and CoSAI standards review]] — verified the 15 risks / 24 controls / 14 components and framed the descriptive-not-specification limit

[^saif-risks]: [SAIF — Risks](https://saif.google/secure-ai-framework/risks), retrieved 2026-06-22. 15 named risks.
[^saif-controls]: [SAIF — Controls](https://saif.google/secure-ai-framework/controls), retrieved 2026-06-22. 24 controls across Data, Infrastructure, Model, Application, Assurance, Governance.
[^saif-components]: [SAIF — Components](https://saif.google/secure-ai-framework/components), retrieved 2026-06-22. 14 components across Data, Infrastructure, Model, Application.

<!-- sources:auto -->
## Sources

- [Google SAIF: Secure AI Framework](https://saif.google/)
<!-- /sources -->
