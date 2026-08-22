---
type: playbook
title: "Assessor's Quick Scorecard: Secure-SDLC and AI"
address: c-000050
created: 2026-05-14
updated: 2026-05-29
tags:
  - playbook
  - assessor-guide
  - scorecard
  - secure-sdlc
  - financial-services
  - canadian-banking
  - osfi
  - pipeda
status: developing
origin: produced
scope_axis:
  - sec-of-ai
  - sec-against-ai
audience: "2nd-party advisor (consultant) conducting a secure-SDLC + AI assessment engagement at a large Ontario-based federally-regulated bank"
length: "~10 pages, ~65 questions across six sections"
regulatory_anchors:
  - "OSFI Guideline B-13 — Technology and Cyber Risk Management (effective 2022-07-31)"
  - "OSFI Guideline E-23 — Model Risk Management (2027) (published 2025-09-11; effective 2027-05-01; explicit AI/ML scope)"
  - "OSFI Guideline B-10 — Third-Party Risk Management (effective 2024-05-01)"
  - "PIPEDA — Personal Information Protection and Electronic Documents Act + Breach of Security Safeguards Regulations (§10.1, real-risk-of-significant-harm test)"
  - "Voluntary Code of Conduct on the Responsible Development and Management of Advanced Generative AI Systems (ISED, Sept 2023)"
related:
  - "[[secure-sdlc-framework-stack-2026]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[red-teaming-capability-framework]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[nist-ssdf]]"
  - "[[nist-sp-800-218a]]"
  - "[[nist-ai-rmf]]"
  - "[[ai-bom]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[plan-validate-execute]]"
  - "[[lethal-trifecta]]"
  - "[[capability-based-authorization]]"
  - "[[non-human-identity]]"
  - "[[least-agency-principle]]"
sources: []
---

# Assessor's Quick Scorecard — Secure-SDLC and AI Practices for a Large Canadian Bank

A condensed two-party-advisor assessment instrument for evaluating a large Ontario-based federally-regulated bank's secure-SDLC practices, with explicit overlays for AI applications, optional Frontier-AI vulnerability discovery in the CI/CD pipeline, and continuous penetration testing — the latter explicitly framed to reduce both false-positive findings (alert-fatigue from scanner noise) and false-negative weaknesses (vulnerabilities missed by point-in-time testing).

## 0 — How to Use

**Audience.** A 2nd-party advisor engaging a federally-regulated Canadian bank. The bank is either already building AI applications or plans to do so. The scorecard is engagement-oriented: it produces a per-section score, a maturity tier, a prioritized findings backlog, and a 90-day quick-wins list — not a compliance certification.

**Engagement flow.** Kickoff → Document Request → Interviews (Eng, Security, Risk, Model Risk) → Evidence Collection → Scoring → Findings Workshop → Report.

**Scoring rubric.** Each question takes one value:

| Value | Score | Definition |
|---|---|---|
| **Yes** | 2 | Documented, implemented, evidence available, last reviewed within cadence |
| **Partial** | 1 | In place but missing one of: documentation, full coverage, cadence, evidence |
| **No** | 0 | Not in place, or planned-only with no working implementation |
| **N/A** | excluded | Not applicable to the bank's current technology footprint (justify briefly) |

**Evidence type per question.** Each question has an expected evidence type — `D` document, `I` interview, `O` live observation, `T` telemetry / log sample. A "Yes" without the expected evidence type is downgraded to "Partial."

**Section maturity ladder.**

| Tier | Threshold | Interpretation |
|---|---|---|
| **L1** | <30% | Ad hoc / undocumented |
| **L2** | 30-50% | Defined but uneven |
| **L3** | 50-75% | Implemented and auditable *(inflection — minimum expected for a federally-regulated bank)* |
| **L4** | 75-90% | Measured and improving |
| **L5** | >90% | Continuous improvement with evidence |

Per-section score is `sum(yes × 2 + partial × 1) ÷ (max_possible_excluding_NA)` expressed as a percentage. Section tier is derived directly from the threshold. Aligned with [[agentic-ai-security-cmm-measurement-protocol|the wiki's CMM measurement protocol]] for cross-engagement comparability.

**Findings priority.**

| Priority | Trigger |
|---|---|
| **Critical** | Regulatory exposure ([[osfi-b-13\|OSFI B-13]] / E-23 / B-10 / PIPEDA citation) **or** safety-critical AI risk ([[lethal-trifecta\|Lethal Trifecta]] exposure in production-facing agent) |
| **Major** | Gap to industry baseline (CMM L3 expectation; NIST SP 800-218A High-priority recommendation) |
| **Moderate** | Gap to leading practice (CMM L4+; SP 800-218A Medium-priority) |
| **Informational** | Leading-edge / nice-to-have (CMM L5; SP 800-218A Low-priority or Consideration-level) |

## 1 — Section A: Secure-SDLC Foundation (12 questions)

Anchors: [[nist-ssdf|NIST SSDF v1.1]] practice groups PO/PS/PW/RV; [[osfi-b-13|OSFI B-13]] Domain 1 (Governance and Risk Management) and Domain 2 (Technology Operations and Resilience), in particular **B-13 §2.4 System Development Life Cycle** which is the direct regulatory hook for this section.

| # | Question | Evidence | Anchor |
|---|---|---|---|
| A1 | Are secure software development requirements documented in policy and reviewed at least annually? | D, I | SSDF PO.1.1; B-13 1.3 |
| A2 | Are SDLC-related roles documented including a security-champion model or equivalent embedded-security function? | D, I | SSDF PO.2.1; B-13 1.1 |
| A3 | Is role-based secure-development training delivered to engineers, with proficiency tracked and refreshed? | D, T | SSDF PO.2.2; B-13 1.1 |
| A4 | Is the build / CI/CD toolchain documented, version-controlled, security-vetted, and access-controlled with MFA and least-privilege? | D, O | SSDF PO.3.1, PO.3.2; B-13 2.4 |
| A5 | Are software security criteria (gates) defined for material releases, with documented exceptions and exception-approval authority? | D, I | SSDF PO.4.1; B-13 2.4 |
| A6 | Is source code under access control with MFA on the SCM, branch protection, and signed commits where feasible? | D, O | SSDF PS.1.1; B-13 2.4 |
| A7 | Is a software bill of materials (SBOM, CycloneDX or SPDX) generated for every material release, signed, and retained for the artifact's full support window? | D, T | SSDF PS.3.2; B-13 2.4 |
| A8 | Is threat modeling performed at design time for net-new services and material changes, with output retained and reviewed? | D, I | SSDF PW.1.1; B-13 1.3 |
| A9 | Are third-party / open-source components verified (provenance, vulnerability status, support window) and update-policy-bound? | D, T | SSDF PW.4.4; B-13 2.4; B-10 §2 |
| A10 | Are SAST, DAST, SCA, and secret-scanning in place with quality gates and tracked false-positive suppression cadence? | T, O | SSDF PW.7, PW.8; B-13 2.4 |
| A11 | Is there an inbound vulnerability disclosure channel (security.txt or equivalent) with SLAs and a responsible-disclosure policy? | D | SSDF RV.1.3; B-13 3.4 |
| A12 | Are vulnerabilities triaged with documented severity-keyed SLAs and tracked time-to-remediation reported to senior management? | D, T | SSDF RV.2.2; B-13 1.1, 3.4 |

## 2 — Section B: AI Governance and Model Risk (12 questions)

Anchors: **[[osfi-e-23-2027|OSFI E-23]] (2027)** Sections B (Enterprise-wide MRM), C (Risk-Based Approach), D (Model Lifecycle Management), and Appendix A (model inventory schema); Canada's [Voluntary AI Code of Conduct](https://ised-isde.canada.ca/) (ISED, Sept 2023); [[nist-sp-800-218a|NIST SP 800-218A]] PO/PS overlays; [[nist-ai-rmf|NIST AI RMF 1.0]]; ISO/IEC 42001. Note: [[osfi-e-23-2027|OSFI E-23]] (2027) takes effect 2027-05-01 — the bank should be preparing now even if not yet in formal compliance scope.

| # | Question | Evidence | Anchor |
|---|---|---|---|
| B1 | Is there a current enterprise-wide AI / model inventory containing all non-negligible-risk models, with the Appendix-A-aligned schema (model ID, owner, risk rating, dependencies, data sources, limitations, next review)? | D, T | E-23 C.1, Appendix A |
| B2 | Is each AI/ML system risk-rated against the bank's defined criteria (purpose, impact, data sensitivity, autonomy level) with corresponding control intensity? | D | E-23 C.2, C.3 |
| B3 | Is there an AI governance body with documented charter, escalation paths, multi-disciplinary participation (legal, compliance, ethics), and at least quarterly review cadence? | D, I | E-23 B.1; Voluntary AI Code §1 (Accountability) |
| B4 | Is independent model validation performed by reviewers separated from development, with review triggers covering new development, modifications, performance breaches, and significant data changes? | D, I | E-23 D Stage 2 (Review) |
| B5 | Is an [[ai-bom\|AI-BOM]] maintained for each deployed AI/ML system, covering training-data sources, RAG corpus, frameworks, MCP servers, reward models, and adaptation layers? | D, T | SP 800-218A PS.3.2; E-23 Appendix A |
| B6 | Is training-data provenance tracked when known, integrity-verified before use, and documented when provenance is not knowable? | D, T | SP 800-218A PW.3.1, PW.3.2 |
| B7 | Are model weights and configuration parameters protected with cryptographic hashes, digital signatures, least-privilege access, and risk-proportionate additional controls (encryption / multi-party authorization / air-gap)? | D, T, O | SP 800-218A PS.1.3, PS.1.3.R4 |
| B8 | Is there an Algorithmic Impact Assessment (or PIPEDA-aligned privacy impact assessment) for each high-risk AI system handling consumer financial data, with documented mitigations? | D | PIPEDA Principle 4 (Limiting Collection); Voluntary AI Code §2 (Safety) |
| B9 | Is the AI system designed such that no critical-path security or financial decision is taken without a human in the loop where the decision is irreversible, material, or rights-affecting? | D, O | SP 800-218A PW.1.1.C2; Voluntary AI Code §5 (Human Oversight) |
| B10 | Are documented model-shutdown / rollback criteria and procedures in place, tested at least quarterly, with named accountable owner per system? | D, T | SP 800-218A RV.2.2.R2, RV.2.2.C1; E-23 D Decommissioning |
| B11 | Are AI/ML model performance and behavior continuously monitored against defined breach thresholds, with documented contingency triggers for drift or autonomous-reparametrization events? | T, O | E-23 D Stage 5 (Monitoring); SP 800-218A PO.5.3 |
| B12 | Is PIPEDA breach-notification timing wired into the AI incident-response playbook, with the "real risk of significant harm" determination procedure documented? | D | PIPEDA §10.1 (Breach of Security Safeguards regs) |

## 3 — Section C: Frontier-AI in CI/CD (Optional Layer, 8 questions)

Anchors: [[frontier-ai-for-vuln-discovery|wiki Frontier-AI thesis]] — harness-over-model architecture; XBOW Mythos eval (42-55% FN reduction vs. Opus 4.6); MDASH (+5 percentage points from harness alone on CyberGym); Big Sleep + CodeMender (Google) production track record. This section is **optional** — applicable only if the bank uses or is piloting frontier-AI for vulnerability discovery in the development pipeline. If wholly N/A, mark the section excluded.

| # | Question | Evidence | Anchor |
|---|---|---|---|
| C1 | If frontier-AI vulnerability discovery is used in CI/CD, is the vendor or harness identified and documented (Big Sleep / CodeMender / MDASH / XBOW / Glasswing-partner / internal)? | D | Wiki Frontier-AI thesis |
| C2 | Is the rollout phased (shadow → advisory → gating) with measurable success criteria at each phase and explicit rollback authority? | D, T | MDASH 5-stage pipeline pattern |
| C3 | Are all AI-discovered findings human-reviewed before patch merge, following the CodeMender / MDASH default pattern? If auto-merge is enabled for any scope, is the auto-merge scope and rollback plan documented? | D, T | CodeMender / MDASH announcements |
| C4 | Is the harness validation step (debater / LLM-as-judge / regression check / functional-equivalence test) documented and version-controlled per the "harness over model" architecture? | D, O | Wiki Frontier-AI thesis; CodeMender multi-agent validation |
| C5 | Is false-negative reduction measured against a known-vulnerability ground-truth set (CyberGym-like, internal corpus, or third-party validation), and tracked over time? | T | XBOW eval pattern; CyberGym leaderboard |
| C6 | Is the false-positive rate per AI-discovered finding measured, with a documented SLA and periodic re-tuning cadence? | T | RTCF Tier 4 |
| C7 | Are AI-discovered findings tagged with model identity, harness version, confidence score, and evidence chain to support audit reproducibility? | T, O | SP 800-218A PS.3.2 (provenance); [[osfi-b-13\|OSFI B-13]] 1.3 |
| C8 | Is the bank participating in or evaluating coalition initiatives (Anthropic Glasswing or analogous) for shared vulnerability research, with documented data-sharing controls? | D, I | Anthropic Glasswing partner pattern |

## 4 — Section D: Continuous Pentesting and AI Red Teaming (12 questions)

Anchors: [[red-teaming-capability-framework|RTCF]] Tier 4 (continuous operations); [[red-teaming-for-ai-synthesis|red-teaming for AI synthesis]]; [[osfi-b-13|OSFI B-13]] Domain 3 (Cyber Security — Defend, Detect, Respond/Recover/Learn); NIST SP 800-218A `PW.8.1.R1` (red-teaming named as a recommended SDLC code-testing form); NIST SP 800-218A glossary definition of *AI red-teaming*.

| # | Question | Evidence | Anchor |
|---|---|---|---|
| D1 | Is penetration testing performed at least annually and on material change, with named scope, methodology, and senior-management reporting? | D | B-13 3.2, 3.3 |
| D2 | Is **continuous** security testing (BAS, CART, red-team-as-a-service, or equivalent) in place beyond annual point-in-time engagements, with results triaged on a defined cadence? | D, T | RTCF Tier 4 |
| D3 | Are per-tool false-positive rates tracked, with documented suppression rules and periodic re-tuning to prevent alert fatigue? | T | SSDF PW.7, PW.8 |
| D4 | Are false-negative rates measured via known-vulnerability injection, ground-truth corpora, or independent third-party validation, and tracked over time? | T | XBOW-style eval; RTCF Tier 4 |
| D5 | Is the pentest and red-team scope explicitly updated to include AI components — model endpoints, agent loops, MCP servers, retrieval pipelines, and adaptation layers? | D | SP 800-218A PW.8.1.R1; OWASP LLM Top 10 |
| D6 | Is there a documented AI red-teaming program with cadence, scope, methodology, and named accountable owner (per the SP 800-218A glossary definition of AI red-teaming)? | D | SP 800-218A PW.8.1.R1, Appendix A |
| D7 | Are adversarial-input corpora and prompt-injection test suites maintained, refreshed against current threats, and rotated (e.g., garak / PyRIT / AgentDojo / Promptfoo)? | D, T | RTCF Tier 4 |
| D8 | For agent-based systems, is the [[lethal-trifecta\|Lethal Trifecta]] (private data + untrusted content + external comms) explicitly assessed per system, with containment controls evidenced where exposure exists? | D, I | Wiki lethal-trifecta concept |
| D9 | Are red-team findings tracked separately from pentest findings with AI-specific severity rubrics (data exfiltration via prompt injection, model decision compromise, agent action hijack)? | T | RTCF Tier 4 |
| D10 | Are CI/CD gates configured to re-run red-team probes on material change to AI components or prompt assets? | D, T | RTCF Tier 4; SSDF PW.8 |
| D11 | Is third-party AI red-teaming evaluated or used (Mindgard CART, HiddenLayer, Protect AI, General Analysis, or equivalent), with sourcing controls applied? | D | RTCF Tier 5 |
| D12 | Is a vulnerability disclosure program (or bug bounty) explicitly scoped to AI surfaces (model endpoints, prompt-handling, agent loops), with safe-harbor language? | D | SSDF RV.1.3 |

## 5 — Section E: Identity, Least-Agency, and Supply Chain for AI (10 questions)

Anchors: [[agentic-ai-security-cmm-2026|CMM]] domains D2 (Identity & Authorization), D3 (Control & Least-Agency), D8 (Supply Chain & AI-BOM); [[agentic-ai-security-reference-architecture|RA]] Identity, Control, and Data planes; OSFI B-10 (Third-Party Risk Management).

| # | Question | Evidence | Anchor |
|---|---|---|---|
| E1 | Are non-human / agent identities provisioned per agent with documented lifecycle (provision, scope, audit, deprovision) and not shared across services? | D, T | [[non-human-identity\|NHI]]; CMM D2 |
| E2 | Are agents authorized through scoped, expiring capability tokens rather than long-lived API keys or static credentials? | D, O | [[capability-based-authorization\|capability-based authorization]]; CMM D3 |
| E3 | Is the principle of [[least-agency-principle\|least-agency]] enforced — each task grants only the authority required to complete that task — and audited per system? | D, T | CMM D3 |
| E4 | For irreversible operations (transactions, customer notifications, identity changes, sharing changes), is [[plan-validate-execute\|Plan-Validate-Execute]] or an equivalent HITL pattern enforced? | D, O | Wiki PVE concept |
| E5 | Is the AI-BOM ([[ai-bom\|model bill of materials]]) signed, retained, and machine-verifiable for each AI-augmented service? | D, T | SP 800-218A PS.3.2; CMM D8 |
| E6 | Is the coding-agent / IDE-extension supply chain governed (rules-file integrity, IDE-extension provenance, destructive-action classification, typosquat defense)? | D, T | [[ai-coding-agent-governance\|Knostic governance]] |
| E7 | For MCP servers in use, are they scanned at install time, signed, version-pinned, and tracked for CVE feed updates? | D, T | [[supply-chain-security-for-agents\|supply chain for agents]]; MCP CVEs |
| E8 | Is agent egress proxied through an inline gateway with tool authorization, response-leak scanning, and audit telemetry? | O, T | Wiki inline-gateway concept; CMM D5 |
| E9 | Are third-party AI vendors (model providers, agent-platform vendors, MCP-tool providers) governed under OSFI B-10's third-party-risk framework with documented assessments and SLAs? | D | B-10 §2, §3 |
| E10 | Is there a documented agent-containment / kill-switch procedure with named accountable owner, tested at least quarterly? | D, T | SP 800-218A RV.2.2; CMM D9 |

## 6 — Section F: Observability, Detection, and AI Incident Response (8 questions)

Anchors: [[agentic-ai-security-cmm-2026|CMM]] domains D7 (Observability & Detection), D9 (Operations & Human Factors); [[opentelemetry-gen-ai|OpenTelemetry gen_ai semantic conventions]]; [[osfi-b-13|OSFI B-13]] Domain 3 (Cyber Security — Detect, Respond/Recover/Learn); PIPEDA §10.1 Breach of Security Safeguards.

| # | Question | Evidence | Anchor |
|---|---|---|---|
| F1 | Are agent traces emitted in OTel `gen_ai.*` semantic conventions for every model call, tool call, and RAG retrieval, with retention aligned to OSFI evidence expectations? | T, O | OTel gen_ai; CMM D7; B-13 3.3 |
| F2 | Are behavioral baselines maintained per agent (action volume, unique destinations, output length distribution, tool-call profile), with alerts on deviation? | T | CMM D7 |
| F3 | Are canary tokens deployed in system prompts and high-value RAG sources to detect system-prompt leakage and prompt-injection exfiltration? | D, T | Wiki canary tokens for LLMs |
| F4 | Is there an AI-specific incident-response playbook covering data poisoning, model compromise, prompt injection in production, MCP supply-chain compromise, and agent-action hijack? | D | CMM D9; SP 800-218A RV.1.1 |
| F5 | Are [[osfi-b-13\|OSFI B-13]] material-incident reporting timing and PIPEDA breach-notification triggering wired into the AI IR playbook, including the "real risk of significant harm" determination procedure? | D | B-13 3.4; PIPEDA §10.1 |
| F6 | Is post-incident root-cause analysis required to determine whether the SDLC, model lifecycle, or governance process should be updated, with named owner for each remediation? | D | SSDF RV.3; B-13 3.4 |
| F7 | Are retention windows for AI audit logs documented and aligned with [[osfi-b-13\|OSFI B-13]] evidence-preservation expectations (typically 7 years for material financial systems)? | D, T | B-13 2.8, 3.3 |
| F8 | Is the kill-switch / model rollback path tested at least quarterly, with a named accountable owner and documented results retained? | D, T | SP 800-218A RV.2.2; CMM D9 |

## 7 — Scoring, Findings, and Reporting

**Per-section roll-up.** For each section, compute:

```
section_score_pct  =  (sum(Yes × 2 + Partial × 1)  /  (max_possible_excluding_NA)) × 100
section_maturity   =  L1 / L2 / L3 / L4 / L5  per the threshold table in §0
```

**Whole-engagement tier.** The engagement-level tier is the **minimum** of the per-section tiers — a single L1 section caps the whole engagement at L1, irrespective of strength elsewhere. This is deliberate: a federally-regulated bank cannot operate at L4 in SDLC fundamentals while at L1 in AI governance and claim L4 maturity.

**Findings priority — apply per question marked No or Partial.** Use the priority table in §0. A finding tied to an OSFI / PIPEDA citation is at minimum **Major**; safety-critical AI exposure (e.g., Lethal Trifecta in production-facing agent without containment) is **Critical** regardless of other context.

**Report deliverables.**

| Artifact | Length | Audience |
|---|---|---|
| Executive summary | 1 page | Board / Risk Committee / Executive Sponsor |
| Per-section scorecard table | 1-2 pages | CISO, Engineering leadership, Model Risk |
| Prioritized findings backlog | 2-4 pages | CISO, Eng leads, Model-risk leads |
| 90-day quick-wins list | 1 page | Eng leads, AI platform team |
| Evidence appendix | as needed | Internal Audit, future reviewers |

## 8 — References and Crosswalks

Single cross-reference table — section to anchors (wiki, regulatory, source-document).

| Section | Wiki anchor | OSFI / PIPEDA citation | Source document |
|---|---|---|---|
| A. Secure-SDLC Foundation | [[nist-ssdf\|SSDF]] PO/PS/PW/RV; CMM D1 | B-13 Domain 1, §2.4 SDLC | NIST SP 800-218 v1.1 |
| B. AI Governance & Model Risk | [[nist-sp-800-218a\|SP 800-218A]]; CMM D1, D6 | E-23 (2027) §§B/C/D + Appendix A; PIPEDA Principle 4 | NIST SP 800-218A; ISED Voluntary AI Code |
| C. Frontier-AI in CI/CD | [[frontier-ai-for-vuln-discovery\|Frontier-AI thesis]] | — *(optional layer)* | XBOW Mythos eval; Microsoft MDASH; Google Big Sleep / CodeMender |
| D. Continuous Pentesting & AI Red Team | [[red-teaming-capability-framework\|RTCF]] Tier 4; [[red-teaming-for-ai-synthesis\|red-teaming for AI]] | B-13 Domain 3 (Defend / Detect / Respond) | NIST SP 800-218A PW.8.1.R1 |
| E. Identity, Least-Agency, Supply Chain | [[agentic-ai-security-cmm-2026\|CMM]] D2/D3/D8 | B-10 §2-§3; B-13 1.3 | NIST SP 800-218A PS.1, PS.3.2 |
| F. Observability & AI IR | [[agentic-ai-security-cmm-2026\|CMM]] D7/D9 | B-13 Domain 3 (Detect, Respond/Recover/Learn); PIPEDA §10.1 | NIST SP 800-218A RV.1, RV.2; OTel gen_ai |

**Secondary regulatory cross-citations** — apply only where the bank has matching operations:

- **PCI DSS 4.0** — Sections A (secure coding), C (AI-assisted code review), D (testing) — applicable if the bank processes payment card data.
- **NYDFS Part 500 (23 NYCRR 500)** — §500.5 pentest cadence, §500.17 incident reporting — applicable if the bank is NYDFS-licensed for NY operations.
- **FFIEC IT Examination Handbook** — Information Security booklet; Development and Acquisition booklet — applicable if the bank has US federally-regulated operations.
- **DORA (EU 2022/2554)** — Articles 5-23 (ICT risk), 17-23 (incident reporting), 24-27 (TLPT — threat-led penetration testing) — applicable if the bank has EU operations.

**Out-of-scope but referenced.**

- **BSIMM** — descriptive benchmark of observed practices; optional comparator for large enterprises; not used as a primary scoring instrument here.
- **SLSA v1.0** — supply-chain provenance specification; recommended at the SBOM / AI-BOM artifact layer; not directly scored.
- **CMMC** — US Defense Industrial Base only; not Canadian-relevant.

## 9 — Notes for the Assessor

**The scorecard's first job is to expose the regulatory floor.** A federally-regulated Canadian bank operating below L3 on Section A (Secure-SDLC Foundation) or Section B (AI Governance) is exposed under [[osfi-b-13|OSFI B-13]] (effective since 2022) and pre-positioned for non-compliance under [[osfi-e-23-2027|OSFI E-23]] (2027). Findings in these sections are at minimum **Major**, frequently **Critical**. L3 — implemented and auditable — is the regulatory floor, not the target.

> [!check] Section C is optional, but increasingly common
> Several Glasswing-partner organizations (Microsoft, Google, AWS, JPMorgan, Anthropic itself) have publicly disclosed frontier-AI vulnerability-discovery in production CI/CD. For a large bank, the question is no longer *if* but *when* and *under what controls*. A bank without any Section C activity is not behind; a bank piloting it without harness-validation, FN/FP measurement, and human review is.

> [!check] Sections D, E, and F sharply differentiate at L4 and L5
> Many banks reach L3 on continuous pentesting, identity, and observability through standard enterprise security investments. The L4-L5 differentiation appears specifically in the **AI overlay**: AI-scoped pentest scope (D5), agent-specific NHI lifecycle (E1), [[lethal-trifecta\|Lethal Trifecta]] assessment (D8), and OTel `gen_ai.*` agent traces (F1). These are the questions that separate banks investing in agentic-AI security as a first-class concern from banks merely complying with general SDLC expectations.

> [!gap] [[osfi-e-23-2027|OSFI E-23]] (2027) effective date is 2027-05-01
> Although the guideline was published 2025-09-11, it is not formally effective until 2027-05-01. Findings tied to E-23 are positioned as **pre-emptive readiness** rather than current non-compliance. Banks operating well below E-23 expectations today have a runway, but the runway is finite and shrinking.

## See also

- [[secure-sdlc-framework-stack-2026|Secure-SDLC Framework Stack for 2026 — Is NIST SSDF + OWASP SAMM Enough?]] — the wiki thesis this scorecard operationalizes for a specific sector
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — full 9-domain CMM for engagements that need depth beyond the scorecard
- [[agentic-ai-security-cmm-measurement-protocol|CMM Measurement Protocol]] — formal three-stage assessment protocol if scorecard outputs warrant deeper measurement
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — six-plane RA used as the design-time blueprint underlying many scorecard questions
- [[red-teaming-capability-framework|Red-Teaming Capability Framework]] — five-tier model that anchors Section D
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — thesis underlying Section C
- [[nist-ssdf|NIST SSDF (SP 800-218 v1.1)]] and [[nist-sp-800-218a|NIST SP 800-218A (SSDF Community Profile for GenAI and Dual-Use Foundation Models)]] — the federal-anchor citation surface
