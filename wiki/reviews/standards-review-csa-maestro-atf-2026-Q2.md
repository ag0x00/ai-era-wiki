---
type: review
title: "CSA MAESTRO and ATF Standards Review"
address: c-000224
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - csa
  - maestro
  - agentic-trust-framework
  - zero-trust
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[csa-maestro]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
related:
  - "[[csa-maestro]]"
  - "[[csa]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[standards-review-backlog]]"
  - "[[agentic-cmm-vs-standards-validation]]"
sources:
  - "https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro"
  - "https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents"
  - "https://github.com/massivescale-ai/agentic-trust-framework"
---

# Standards Review — CSA MAESTRO and ATF, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to two Cloud Security Alliance agentic-security publications that share one wiki page: **MAESTRO** (Multi-Agent Environment, Security, Threat, Risk, and Outcome), a seven-layer threat-modeling framework published February 6, 2025,[^maestro] and the **Agentic Trust Framework (ATF) v1.0**, a Zero Trust governance model for AI agents published February 2, 2026.[^atf] It produces primary-source citations, a domain-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|CMM]] domains, falsifiable absence claims, and an adversarial second pass.

The two instruments do different jobs. MAESTRO is a **layered threat-modeling methodology**: it partitions an agentic system into seven layers and enumerates threats per layer and across layers. It is not a control catalogue and not a maturity model. ATF is a **Zero Trust governance framework**: five core elements answer five trust questions, four maturity levels (Intern -> Junior -> Senior -> Principal) grade earned autonomy, and five promotion gates govern advancement. Neither provides graded, evidence-bearing controls; the CMM supplies those.

**The wiki page misnamed the ATF's promotion gates.** The page's "Five Promotion Gates" section listed *Identity establishment, Capability attestation, Behavioral baseline, Monitored autonomy, Full autonomy* — none of which appear in the primary source. The verified gates are **Performance, Security Validation, Business Value, Incident Record, Governance Sign-off**,[^atf] and they are advancement gates between maturity levels, not per-domain control gates. The crosswalk's domain-anchored gate citations (`ATF Gate 1 Identity binding`, `ATF Gate 3 runtime guardrails`, `ATF Gate 5 memory integrity`) inherited the same error and conflate the gates with the five **elements** (Identity, Behavior, Data Governance, Segmentation, Incident Response), which are the constructs that actually anchor domains.

## Primary documents reviewed

CSA publishes MAESTRO and ATF as blog-hosted guidance with a public reference implementation; neither is a single specification PDF.

| Document | URL | Scope used in this review |
|---|---|---|
| MAESTRO threat-modeling framework | [CSA blog (2025-02-06)](https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro) | Acronym; seven layers L1-L7; cross-layer threats |
| MAESTRO labs landing | [labs.cloudsecurityalliance.org/maestro](https://labs.cloudsecurityalliance.org/maestro/) | Acronym confirmation; layer-by-layer approach (layer names not enumerated on this page) |
| Agentic Trust Framework v1.0 | [CSA blog (2026-02-02)](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents) | 5 elements; 5 gates; 4 maturity levels; ZT basis |
| ATF reference implementation | [massivescale-ai/agentic-trust-framework](https://github.com/massivescale-ai/agentic-trust-framework) | Element definitions; maturity autonomy/oversight table; demotion rule; NIST 800-207 basis |

> [!contradiction] The wiki page carried fabricated ATF gate names and no MAESTRO layer content
> The [[csa-maestro|CSA page]] listed five ATF gates that no primary source supports and contained no MAESTRO seven-layer section despite the page title naming MAESTRO. Both are corrected by this review (verified gates and the L1-L7 table added).

## Structure-to-domain grounding

- **MAESTRO maps to the technical-stack domains (D4-D8).** Its layers describe *where* threats live (models, data ops, frameworks, infra, observability, security/compliance, ecosystem), so it anchors a domain's threat model, not its level criteria. It has no governance-program or human-factors content of its own; Layer 6 (Security and Compliance) is a cross-cutting threat surface, not a governance method.
- **ATF maps to the autonomy-governance domains (D1, D3, D9).** Its five elements and four maturity levels give D3 a named progressive-autonomy model and D1 a sign-off construct, but the gates name criteria categories without thresholds.
- **Neither provides measurable controls.** MAESTRO enumerates threats; ATF names elements and gate categories. The CMM's per-level evidence rubric is what neither supplies.

## Clause-level coverage matrix (CMM x Standard)

Names are verbatim from the fetched sources. MAESTRO column cites layer names; ATF column cites elements/gates/levels.

| CMM domain | MAESTRO layer(s) | ATF element / gate / level | Coverage |
|---|---|---|---|
| D1 Governance & Accountability | Layer 6 Security and Compliance (threat surface only) | Gate 5 Governance Sign-off; Incident Response element | Partial — ATF gives a sign-off construct; no risk-management program. MAESTRO has none |
| D2 Identity & Authorization | none (identity threats noted across layers, not a named layer) | Identity element ("Who are you?"); maturity-level scoped autonomy | Partial — ATF Identity element is a question, not a control set |
| D3 Control & Least-Agency | Layer 3 Agent Frameworks; Layer 4 Deployment and Infrastructure (threat surface) | Segmentation element ("Where can you go?"); 4 maturity levels (Intern->Junior->Senior->Principal); 5 promotion gates | Best ATF coverage — named progressive-autonomy model; criteria categories only, no thresholds |
| D4 Runtime & Guardrails | Layer 1 Foundation Models; Layer 3 Agent Frameworks | Behavior element ("What are you doing?") | Partial — threats per layer; Behavior element names monitoring, not guardrail specs |
| D5 Egress & Network | Layer 4 Deployment and Infrastructure; Layer 7 Agent Ecosystem (cross-layer leakage) | Segmentation element (blast radius) | Partial — MAESTRO names inter-layer data leakage; ATF segmentation is a question |
| D6 Data, Memory & RAG | Layer 2 Data Operations (RAG pipelines, storage) | Data Governance element ("What are you eating? What are you serving?") | MAESTRO strong on data-layer threats; ATF Data Governance covers input/output validation |
| D7 Observability & Detection | Layer 5 Evaluation and Observability | Behavior element (monitoring, anomaly detection); continuous verification | Partial — MAESTRO names an observability layer; ATF requires continuous monitoring, no detection rules |
| D8 Supply Chain & AI-BOM | Layer 3 Agent Frameworks (named, not addressed); cross-layer supply-chain threat | none | Thin — MAESTRO names supply-chain compromise as a cross-layer threat; no AI-BOM. ATF silent |
| D9 Operations & Human Factors | none | Incident Response element ("What if you go rogue?" — kill switch, circuit breaker); demotion-to-Intern rule | Partial — ATF gives containment and demotion; no runbook/IR lifecycle |

The matrix confirms the assessment: MAESTRO supplies a layered threat model (D4-D8 surface), ATF supplies an autonomy-governance overlay (D1, D3, D9). Neither carries pass/fail control specifications, so each grounds a CMM domain's threat model or governance shape, not its level criteria.

## Falsifiable absence claims found

1. **MAESTRO specifies no controls — it is a threat-modeling taxonomy.** The seven layers enumerate threats; no layer carries a control specification, pass/fail criterion, or test procedure. *Searched:* MAESTRO blog [^maestro] and labs landing, all layer and cross-layer text. *Terms:* "control", "shall", "criteria", "test", "threshold", "metric", "requirement", "baseline". *Verdict:* confirmed. *Refuting evidence:* a MAESTRO layer carrying a measurable control or acceptance criterion. *Reviewed 2026-06-22.* This is the gap the CMM's per-level rubric fills against the MAESTRO threat surface.

2. **ATF promotion gates name criteria categories without measurable thresholds.** The five gates (Performance, Security Validation, Business Value, Incident Record, Governance Sign-off) name *what* must be demonstrated, not *how much*. *Searched:* ATF blog [^atf] and GitHub reference [^atfgh]. *Terms:* "threshold", "minimum", "metric", "score", "percent", "criteria", "SLA". *Verdict:* confirmed — gates are qualitative. The GitHub reference gives an autonomy/oversight table per level but no numeric promotion thresholds. *Refuting evidence:* a published gate threshold or scored rubric. *Reviewed 2026-06-22.* The CMM's D3 supplies the org-authored thresholds the ATF leaves abstract.

3. **Neither MAESTRO nor ATF addresses the AI-BOM / software supply-chain artifact.** MAESTRO names supply-chain compromise as a cross-layer threat against Layer 3 (Agent Frameworks); ATF has no supply-chain element. *Searched:* both ATF documents and the MAESTRO blog. *Terms:* "AI-BOM", "SBOM", "bill of materials", "provenance", "artifact signing", "dependency". *Verdict:* confirmed — supply chain is a threat in MAESTRO, not a control; absent from ATF's five elements. *Refuting evidence:* a MAESTRO layer or ATF element prescribing AI-BOM generation or artifact provenance. *Reviewed 2026-06-22.* Matches the CMM D8 gap; MITRE ATLAS `AML.M0023` remains the wiki's supply-chain anchor.

4. **ATF specifies no non-human-identity (NHI) credential controls.** The Identity element asks "Who are you?" (credentials, authentication, ownership) but prescribes no token format, rotation cadence, or workload-identity binding. *Searched:* ATF blog and GitHub. *Terms:* "credential rotation", "workload identity", "SPIFFE", "token", "key management", "non-human identity". *Verdict:* confirmed — element is a governance question, not an identity control set. *Refuting evidence:* an ATF Identity-element control specifying credential mechanics. *Reviewed 2026-06-22.* The CMM D2 fills this from NIST SP 800-207 / 800-63 anchors.

5. **ATF gates and elements are distinct constructs; neither maps one-to-one onto a CMM domain.** The wiki crosswalk previously anchored CMM domains to numbered "ATF Gate N" labels; the gates are level-advancement gates, not domain controls. *Searched:* ATF blog and GitHub structural description. *Terms:* "gate", "element", "pillar", "domain", "control mapping". *Verdict:* confirmed — five elements answer trust questions, five gates govern advancement, four levels grade autonomy; none is a per-domain control. *Refuting evidence:* a primary-source mapping of a numbered gate to a security domain. *Reviewed 2026-06-22.* This finding drives the crosswalk correction below.

## Scope exclusions

- **Per-threat enumeration within MAESTRO layers.** The matrix anchors each domain with its layer(s), not every threat MAESTRO lists per layer.
- **The CSA Foundation programs** (AI Risk Observatory, Valid-AI-ted) named on the wiki page — these are organizational initiatives, not part of MAESTRO or ATF, and were not coverage-mapped.
- **Production effectiveness.** Document-versus-document review per the methodology, not a deployment audit.
- **ATF reference-implementation code quality.** The GitHub repo was read for structure (elements, levels, gates), not audited as software.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second pass attempted a counter-example for each absence claim against the three primary sources. **All five claims survived.**

1. **MAESTRO has no controls — survives.** No layer text carries a measurable control; every layer entry is a threat surface.
2. **ATF gates lack thresholds — survives.** Gates are named qualitatively in both blog and GitHub; the maturity table describes autonomy/oversight, not numeric promotion criteria.
3. **No AI-BOM — survives.** Supply chain appears only as a MAESTRO cross-layer threat; no element or layer prescribes a BOM artifact.
4. **No NHI credential controls — survives.** Identity element is scoped to governance questions; no credential mechanics.
5. **Gates != domains — survives.** No primary source maps a numbered gate to a security domain; the five-element / five-gate / four-level separation is explicit in the GitHub reference.

## Effect on existing wiki pages

- **[[csa-maestro|CSA MAESTRO / ATF framework page]]:** added the verified MAESTRO seven-layer table (L1 Foundation Models … L7 Agent Ecosystem) and acronym expansion; replaced the fabricated "Five Promotion Gates" section with the verified ATF structure (five elements, four maturity levels, five gates); added `primary_documents` frontmatter for both CSA sources plus the GitHub reference; re-scored the OWASP ASI coverage table against the five elements (ASI06 moved to partial via the Data Governance element); footnoted every layer/element/gate name.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** corrected the CSA MAESTRO/ATF column — MAESTRO layer references re-anchored to verified layer names (Layer 1 is Foundation Models, Layer 2 is Data Operations, so the D6 cell now cites Layer 2, not "Layer 1 Data"); the `ATF Gate N` domain anchors replaced with the correct element names (Identity, Behavior, Data Governance, Segmentation, Incident Response) plus the promotion-gate set where advancement is the point (D3).
- **[[agentic-ai-security-cmm-2026|Canonical CMM]]:** the D3 progressive-autonomy paragraph reconciled from "CSA Agentic Trust Framework v0.9.1" to **v1.0 (Feb 2, 2026)**; the Intern->Junior->Senior->Principal model retained (confirmed correct); the "5 promotion gates" reference confirmed and its gate names corrected where listed.
- **[[agentic-ai-security-reference-architecture|Reference Architecture]]:** the MAESTRO 7-layer line footnoted to the primary source; "CSA ATF — 5 progressive autonomy gates" reconciled to v1.0; the fail-closed/Promotion-Gates note retained.
- **[[standards-review-backlog|Standards Review Backlog]]:** CSA MAESTRO + ATF flips to done.

## Notes

[^maestro]: [CSA — Agentic AI Threat Modeling Framework: MAESTRO](https://cloudsecurityalliance.org/blog/2025/02/06/agentic-ai-threat-modeling-framework-maestro), retrieved 2026-06-22. Acronym: Multi-Agent Environment, Security, Threat, Risk, and Outcome. Layers: L1 Foundation Models, L2 Data Operations, L3 Agent Frameworks, L4 Deployment and Infrastructure, L5 Evaluation and Observability, L6 Security and Compliance, L7 Agent Ecosystem.
[^atf]: [CSA — The Agentic Trust Framework: Zero Trust Governance for AI Agents](https://cloudsecurityalliance.org/blog/2026/02/02/the-agentic-trust-framework-zero-trust-governance-for-ai-agents), retrieved 2026-06-22. Five elements: Identity, Behavior, Data Governance, Segmentation, Incident Response. Five gates: Performance, Security Validation, Business Value, Incident Record, Governance Sign-off. Four levels: Intern, Junior, Senior, Principal.
[^atfgh]: [agentic-trust-framework — massivescale-ai (GitHub)](https://github.com/massivescale-ai/agentic-trust-framework), retrieved 2026-06-22. NIST SP 800-207 Zero Trust basis; demotion-to-Intern on critical incident.
