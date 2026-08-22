---
type: review
title: "OWASP Agentic AI Standards Review"
address: c-000218
origin: produced
created: 2026-06-21
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - owasp
  - agentic-ai
  - vulnerability-taxonomy
  - vulnerability-scoring
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[owasp-agentic-ai-top-10]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-21"
review_completed: "2026-06-21"
adversarial_pass: "completed 2026-06-21"
primary_documents:
  - title: "OWASP Top 10 for Agentic Applications 2026 (ASI Top 10)"
    url: "https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/"
    version: "2026"
    published: "2025-12-09"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/owasp-asi-top-10-2026-12-09.pdf"
    scope_in_wiki: "All ten categories ASI01–ASI10 (Description / Common Examples / Attack Scenarios / Prevention and Mitigation Guidelines); Leaders' Letter (Least-Agency); Appendix A (LLM Top 10 + T-code + AIVSS map), Appendix C (NHI Top 10 map)"
  - title: "AIVSS Scoring System for OWASP Agentic AI Core Security Risks"
    url: "https://aivss.owasp.org/"
    version: "0.8"
    published: "2026-03-19"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/owasp-aivss-v0.8.pdf"
    scope_in_wiki: "§2 ten Agentic Risk Amplification Factors + 0/0.5/1.0 rubric; §3 AARS Risk-Gap formula, Threat Multiplier, Mitigation Factor; worked examples"
related:
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-aivss]]"
  - "[[owasp-llm-top-10]]"
  - "[[owasp]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-review-backlog]]"
  - "[[standards-review-nist-ai-rmf-2026-Q2]]"
sources:
  - "[[.raw/papers/owasp-asi-top-10-2026-12-09.pdf]]"
  - "[[.raw/papers/owasp-aivss-v0.8.pdf]]"
---

# Standards Review — OWASP Agentic AI Top 10 + AIVSS, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to the OWASP agentic pair: the [[owasp-agentic-ai-top-10|Top 10 for Agentic Applications 2026]] (the ASI Top 10) and the [[owasp-aivss|AIVSS]] scoring system. The two are reviewed as one P1 unit because the [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-cmm-crosswalk|crosswalk]] cite them as a single anchor (`OWASP ASI / AIVSS / LLM`). Inputs are the primary PDFs archived under `.raw/papers/` — the 57-page ASI Top 10 (CreationDate 2025-12-09) and the AIVSS v0.8 specification (2026-03-19) — read category by category and clause by clause. The output is a coverage matrix against the nine CMM domains, a set of falsifiable absence claims, and an adversarial second pass. It supersedes the OWASP row in [[agentic-cmm-vs-standards-validation|the 2026-04-30 first-pass validation]], which worked from wiki summaries.

The ASI Top 10 is the structural inverse of the [[standards-review-nist-ai-rmf-2026-Q2|NIST AI RMF stack]]. Where NIST is governance- and process-rich but silent on agentic controls, the ASI Top 10 is control-rich on exactly the agentic planes NIST cannot reach — non-human identity (D2), least-agency and tool gating (D3), runtime prompt-injection *defense* (D4), per-agent egress and inter-agent channels (D5), agent-memory poisoning (D6), and the live agentic supply chain (D8) — and is thin on the governance management-system layer (D1). Its limiting characteristic is not coverage but **enforceability**: it is an awareness framework, not a compliance standard.

**The ASI Top 10 supplies the agentic controls NIST lacks, but states them as recommendations, not as auditable requirements.** Every category ends in a "Prevention and Mitigation Guidelines" list written in the imperative ("Enforce…", "Define…", "Require…"). The document contains no `shall` statements, no conformance language, no certification scheme, no evidence criteria, and no maturity tiers. Its self-description is a "compass and go-to reference," not a yardstick. The CMM's per-level evidence rubric can lift these mitigations as control *candidates*, but cannot anchor its *grading* to them — the document supplies no graded criterion to grade against. AIVSS adds the measurement layer the Top 10 omits, but measures vulnerability *severity*, not control maturity, and explicitly delegates the control catalog to [[aiuc-1|AIUC-1]].

## Primary documents reviewed

| Document | ID / version | Published | Type | Scope used |
|---|---|---|---|---|
| [[owasp-agentic-ai-top-10\|OWASP Top 10 for Agentic Applications]] | ASI Top 10, "Version 2026" | 2025-12-09 | Awareness framework (CC BY-SA 4.0) | All ten categories ASI01–ASI10; Leaders' Letter; Appendices A and C |
| [[owasp-aivss\|AIVSS Scoring System for Agentic AI Core Security Risks]] | AIVSS, v0.8 | 2026-03-19 | Vulnerability-scoring methodology (pre-1.0) | §2 ten amplification factors; §3 AARS formula, ThM, Mitigation Factor |

Both are public and free. The ASI Top 10 was produced by the [[owasp|OWASP]] GenAI Security Project's Agentic Security Initiative (100+ contributors; an Expert Review Board including NIST's adversarial-AI lead and Microsoft AI Red Team members). AIVSS v0.8 is a **live community-review document** that incorporated public comment across prior drafts; it self-describes its mathematical framework as a "balanced and stable starting point," and v1.0 is not yet ratified. For crosswalk purposes the ASI Top 10 is a control-candidate and requirements source; AIVSS is a measurement instrument, not a control standard.

> [!check] Primary-source correction to the wiki's category list
> The wiki's [[owasp-agentic-ai-top-10|ASI Top 10 framework page]] and the OWASP column of the [[agentic-ai-security-cmm-crosswalk|crosswalk]] carried labels from an **earlier ASI draft**, not the published 2026 edition. Two category titles were materially wrong: `ASI05` was listed as "Sensitive Data Disclosure" (the published title is **Unexpected Code Execution (RCE)**) and `ASI09` as "Missing Guardrails" (the published title is **Human-Agent Trust Exploitation**). "Sensitive Data Disclosure" and "Missing Guardrails" are not categories in the 2026 release. Both are corrected as an effect of this review (see below). The verbatim published list is in the next section.

## The published ASI Top 10 (2026)

| ID | Published title | Maps to (T-code) | AIVSS Core Risk |
|---|---|---|---|
| **ASI01** | Agent Goal Hijack | T6, T7 | Agent Goal & Instruction Manipulation |
| **ASI02** | Tool Misuse and Exploitation | T2, T4, T16 | Agentic AI Tool Misuse |
| **ASI03** | Identity and Privilege Abuse | T3 | Agent Access Control Violation |
| **ASI04** | Agentic Supply Chain Vulnerabilities | T17 (+T2/T11/T12/T13/T16) | Agent Supply Chain & Dependency Attacks |
| **ASI05** | Unexpected Code Execution (RCE) | T11 | Insecure Agent Critical Systems Interaction |
| **ASI06** | Memory & Context Poisoning | T1 (+T4/T6/T12) | Agent Memory & Context Manipulation |
| **ASI07** | Insecure Inter-Agent Communication | T12, T16 | Agent Memory & Context Manipulation |
| **ASI08** | Cascading Failures | T5, T8 | Agent Cascading Failures |
| **ASI09** | Human-Agent Trust Exploitation | T7, T8, T10 | Agent Untraceability / Human Manipulation |
| **ASI10** | Rogue Agents | T13, T14, T15 | Behavioral Integrity / Operational Security / Compliance |

Each category follows a fixed structure: Description / Common Examples of the Vulnerability / Example Attack Scenarios / **Prevention and Mitigation Guidelines** / References. The mitigation lists are the load-bearing content for a control crosswalk and are cited per domain below. The T-codes refer to the OWASP *Agentic AI Threats & Mitigations* guide (T1–T17), named throughout as the foundational taxonomy the Top 10 relies on.

## AIVSS v0.8 scoring model

AIVSS scores how much an agent's capabilities amplify a conventional vulnerability. It consumes a CVSS v4.0 base score unchanged and adds an agentic uplift:

- **Ten Agentic Risk Amplification Factors**, each scored `0.0` (none), `0.5` (partial), or `1.0` (full): Execution Autonomy, External Tool Control Surface, Natural Language Interface, Contextual Awareness, Behavioral Non-Determinism, Opacity & Reflexivity, Persistent State Retention, Dynamic Identity, Multi-Agent Interactions, Self-Modification. `Factor_Sum` is their unweighted total (max 10).
- **Agentic Risk Gap uplift:** `AARS = (10 − CVSS_Base) × (Factor_Sum / 10) × ThM`. The `(10 − CVSS_Base)` term is the "Agentic Risk Gap" — the headroom agentic capability can add on top of the technical flaw.
- **Threat Multiplier (`ThM`):** `1.0` Attacked, `0.97` Proof-of-Concept (default), `0.50` Unreported.
- **Final score:** `AIVSS = (CVSS_Base + AARS) × Mitigation_Factor`, where `Mitigation_Factor` is `1.0` No/Weak (default), `0.83` Partial, `0.67` Strong; output bounded 0–10.

This is a measurement model, not a control set. Its only control-adjacent dimension is the coarse three-level `Mitigation_Factor`, which scales the score but enumerates no controls; the control catalog is delegated to [[aiuc-1|AIUC-1]] via the published AIUC-1 ↔ AIVSS crosswalk.

> [!contradiction] AIVSS formula and ThM drifted between versions — cite v0.8
> The v0.5 spec used a simple-average form, `AIVSS = ((CVSS_Base + AARS) / 2) × ThM`, with `ThM = 0.91` for Unreported. v0.8 replaced this with the **Risk-Gap uplift** form above and set `ThM = 0.50` for Unreported. Third-party write-ups quote both; the v0.8 PDF (§3.3–§3.4) is authoritative and is what the [[owasp-aivss|AIVSS page]] now reflects. A personal-repo variant circulating with per-factor weights (`w₁=0.3 / w₂=0.5 / w₃=0.2`) is **not** the OWASP-canonical equation.

## Clause-level coverage matrix (CMM × OWASP pair)

Each row cites the ASI category IDs and the specific mitigation bullets that anchor a CMM domain, plus the AIVSS factor that scores the domain's amplification. Mitigation references are to the per-category "Prevention and Mitigation Guidelines" lists.

| CMM domain | ASI Top 10 (2026) | AIVSS factor | Net coverage |
|---|---|---|---|
| D1 Governance & Accountability | **Partial** — Least-Agency principle (Leaders' Letter); insider-threat-program integration (`ASI01`); immutable signed audit logs and behavioral manifests (`ASI10`); but no management-system process, no roles/accountability matrix, no risk-classification *process* | n/a (scoring, not governance) | **Partial** — governance named as awareness, not as a managed process |
| D2 Identity & Authorization | **Strong** — `ASI03` dedicated: per-agent identities and mTLS/scoped tokens, task-scoped time-bound permissions, OAuth-token-to-signed-intent binding, centralized per-action policy engine, re-auth on context switch; Appendix C maps the full NHI Top 10 | Dynamic Identity | **Strong** |
| D3 Control & Least-Agency | **Strong** — Least-Agency coined here; `ASI02` per-tool least-privilege profiles, "Intent Gate" PEP/PDP over untrusted planner output, action-level auth, adaptive tool budgeting, JIT/ephemeral access; `ASI08` blast-radius guardrails, circuit breakers, planner/executor separation | Execution Autonomy; External Tool Control Surface | **Strong** |
| D4 Runtime & Guardrails | **Strong** — `ASI01` treat all NL input as untrusted + prompt-injection safeguards (a *defensive* control, not red-team-only); `ASI05` ban `eval`, safe interpreters, output encoding (LLM05), code/exec separation; `ASI09` plan-divergence detection; "Semantic Firewalls" (`ASI02`) | Natural Language Interface; Non-Determinism | **Strong** — fills the prompt-injection-defense gap NIST left open |
| D5 Egress & Network | **Strong** — `ASI02` outbound allowlists / deny non-approved destinations / execution sandboxes; `ASI07` end-to-end encryption, mutual auth, anti-replay, protocol-downgrade rejection; `ASI08` network segmentation | Multi-Agent Interactions | **Strong** — per-agent egress and inter-agent channel both addressed |
| D6 Data, Memory & RAG | **Strong** — `ASI06` dedicated: validate all memory writes before commit, memory segmentation, source-attribution/provenance, expire unverified memory, per-tenant namespaces + trust-weighted retrieval, prevent bootstrap re-ingestion of own outputs | Persistent State Retention; Contextual Awareness | **Strong** — closes the agent-memory-poisoning gap NIST/ATLAS only partly held |
| D7 Observability & Detection | **Strong** — pervasive: behavioral baselines + stable goal identifiers (`ASI01`), immutable tamper-evident logs bound to cryptographic agent identity (`ASI08`/`ASI10`), drift and plan-divergence detection (`ASI09`), watchdog/governance agents (`ASI10`); AIVSS supplies severity scoring | Opacity & Reflexivity | **Strong** |
| D8 Supply Chain & AI-BOM | **Strong** — `ASI04` dedicated: sign/attest manifests/prompts/tool definitions, require SBOMs and **AIBOMs** with periodic attestation, dependency allowlist + pin by content hash + typosquat scanning, supply-chain kill switch; Appendix B relates CycloneDX SBOM / ML-BOM / AIBOM | Self-Modification | **Strong** — AI-BOM named as artifact, unlike the NIST stack |
| D9 Operations & Human Factors | **Strong** — `ASI09` Human-Agent Trust Exploitation (over-reliance, automation/authority bias, UI risk-differentiation, oversight-personnel training); `ASI08` circuit breakers and human gates; `ASI10` kill switches, quarantine, recovery/reintegration with fresh attestation | n/a | **Strong** |

Net per-domain verdict: **Strong** — D2, D3, D4, D5, D6, D7, D8, D9. **Partial** — D1. The pair is strong on every agentic-control plane and partial only on the governance management-system layer, the mirror image of the NIST stack's profile.

### RA-plane mapping

The six [[agentic-ai-security-reference-architecture|RA]] planes correspond to CMM D2–D7. The ASI Top 10 anchors all six strongly — identity (D2 / `ASI03`), control (D3 / `ASI02` + Least-Agency), runtime (D4 / `ASI01` + `ASI05`), egress (D5 / `ASI02` + `ASI07`), data/memory (D6 / `ASI06`), and observability (D7, pervasive). It is the most complete single anchor for the RA's control planes among the standards reviewed so far; what it does not supply is the evidence model that turns those control planes into gradable maturity levels.

## Falsifiable absence claims found

What the CMM scores that the OWASP pair, as published, does not provide. Each is bounded to the searched documents and reversible by the stated refuting evidence. These are not "the control is missing" claims — the ASI Top 10 is control-rich — but "the control is stated as guidance, not as an auditable or measurable requirement" claims.

1. **No conformance, certification, audit, or evidence mechanism.** The ASI Top 10 is an awareness framework. *Searched:* full 57-page PDF. *Terms:* "shall", "conformance", "certification", "audit of", "evidence", "criteria", "maturity", "tier", "requirement". *Verdict:* confirmed. Zero `shall`; the few `must` occurrences are license boilerplate or explanatory prose, not graded requirements. The four "certif" hits are cryptographic *certificates* (mTLS/PKI), not a certification program; the ten "audit" hits are recommended *controls to build* (immutable audit logs), never an audit *of conformance to the standard*. The document self-describes as a "compass," not a yardstick. *Refuting evidence:* any clause stating a `shall`/`should` conformance requirement, a certification or audit procedure, or an evidence criterion for the standard itself. *Reviewed 2026-06-21.* This is the gap the CMM's per-level evidence rubric fills; OWASP anchors the CMM's *control content*, not its *grading*.

2. **No measurable acceptance criteria or graded maturity tiers.** Every mitigation is an imperative recommendation without a threshold, target, or pass/fail condition. *Searched:* all ten "Prevention and Mitigation Guidelines" lists. *Terms:* "threshold", "target", "metric", "level", "score" (as a control criterion), "acceptance". *Verdict:* confirmed. `ASI08` explicitly concedes residual unmitigated risk the enterprise "must evaluate carefully … within the overall risk budget" — language inconsistent with pass/fail conformance. *Refuting evidence:* a mitigation expressed as a measurable acceptance criterion or assigned to a maturity tier. *Reviewed 2026-06-21.* The CMM `D1`–`D9` per-level rubrics supply this; the ASI mitigations map to the rubric as control candidates, not as graded criteria.

3. **No governance management-system layer.** The pair governs autonomy through the Least-Agency *principle* and per-category controls, not through an accountability structure, a roles-and-responsibilities matrix, or a risk-classification *process*. *Searched:* full ASI PDF; AIVSS v0.8. *Terms:* "roles and responsibilities", "accountability", "management system", "risk classification", "governance process", "policy lifecycle". *Verdict:* confirmed. Least-Agency is defined only in one Leaders'-Letter paragraph and operationalized as a tool-privilege control (`ASI02`), not as a governance process. *Refuting evidence:* a clause defining an organizational governance process, role matrix, or risk-tiering procedure. *Reviewed 2026-06-21.* This is the layer [[iso-iec-42001|ISO 42001]] and the NIST `GOVERN` function supply and the ASI Top 10 does not; the CMM `D1` anchors there, not to OWASP.

4. **The ASI Top 10 2026 contains no MITRE ATLAS mapping.** *Searched:* full PDF. *Terms:* "ATLAS", "ATT&CK", "AML.T", "mitre". *Verdict:* confirmed — the only "mitre" hit is a CWE URL in `ASI08` references. The document cross-maps to the *Agentic AI Threats & Mitigations* T-codes (T1–T17), the [[owasp-llm-top-10|LLM Top 10 2025]] (Appendix A), and the NHI Top 10 2025 (Appendix C); it does **not** map to [[mitre-atlas|ATLAS]]. *Refuting evidence:* an ATLAS technique reference inside the ASI Top 10 2026 PDF. *Reviewed 2026-06-21.* The wiki's prior claim that "MITRE ATLAS cross-mapping now covers all 10 categories" is not supported by the primary document; any such mapping is community/ATLAS-side work, not part of the OWASP publication. Corrected on the framework page (see below).

5. **AIVSS is a scoring methodology, not a control standard.** *Searched:* AIVSS v0.8 full text. *Terms:* "control", "shall", "requirement", "must implement". *Verdict:* confirmed. AIVSS quantifies vulnerability severity and explicitly delegates the control catalog: the AIUC-1 ↔ AIVSS crosswalk states AIVSS identifies and scores risk while "AIUC-1 offers a comprehensive list of controls." AIVSS is pre-1.0 (v0.8, community review), and its mathematical framework is self-described as a "starting point." *Refuting evidence:* a clause in the AIVSS spec prescribing an implementable security control rather than a score. *Reviewed 2026-06-21.* AIVSS anchors the CMM's *severity-scoring* and risk-register tagging (`D1`/`D7`), not any control rung.

6. **AIVSS's only control-adjacent dimension is a coarse three-level scalar.** The `Mitigation_Factor` (`1.0` No/Weak, `0.83` Partial, `0.67` Strong) scales the score by mitigation strength but enumerates no controls and defines no evidence for assigning a level. *Searched:* AIVSS v0.8 §3.4. *Terms:* "mitigation factor", "strong mitigation", "control list", "evidence". *Verdict:* confirmed — the rubric for "Strong" vs "Partial" is qualitative judgment, not a control checklist. *Refuting evidence:* a control catalog or evidence model attached to the `Mitigation_Factor` levels. *Reviewed 2026-06-21.* It cannot substitute for a graded control framework; it is an input to a severity score.

7. **Least-Agency lacks an operational risk-tier-to-autonomy classification.** The principle is stated ("avoid unnecessary autonomy") without the agent-classification scheme and enforcement mapping needed to apply it. *Searched:* full ASI PDF. *Terms:* "least agency", "autonomy tier", "risk tier", "classification", "autonomy level". *Verdict:* confirmed — Least-Agency appears in the Leaders' Letter and as `ASI02` mitigation #1, never as a tiered classification model. *Refuting evidence:* a clause classifying agents into autonomy/risk tiers with per-tier governance. *Reviewed 2026-06-21.* The [[owasp-agentic-ai-top-10|framework page]] already flagged this informally; it is now bounded. The CMM `D3` and the four-tier least-agency model in the crosswalk supply the operational classification.

## Scope exclusions

- **The Agentic AI Threats & Mitigations guide (T1–T17).** The ASI Top 10 references this companion as its foundational taxonomy. The T-codes were used to confirm category mappings, not reviewed as a separate standard; that guide is a distinct backlog candidate.
- **Per-mitigation enumeration.** The matrix anchors each domain with its load-bearing mitigation bullets, not every bullet under every category. The ASI Top 10 lists roughly 7–10 mitigations per category; only the domain-relevant ones are cited.
- **AIVSS worked-example arithmetic.** The v0.8 §4 worked examples were read to confirm the formula and ThM/Mitigation defaults, not reproduced.
- **The living incidents tracker (Appendix D).** Maintained out-of-band in GitHub and explicitly mutable; out of scope for a versioned review.
- **Production effectiveness.** Document-versus-document review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-21`. A second pass attempted a counter-example for each absence claim against the two PDFs. **All seven claims survived; claims 1 and 4 were narrowed to their bounded forms.**

1. **No conformance mechanism — survives, narrowed.** The Dec 9 announcement post mentions an "Agentic Adoption Challenge" with "recognition, certification, and RSAC showcases," but that machinery is an adjacent community program, not in the versioned PDF. The document itself has no conformance scheme. Narrowed to: the ASI Top 10 2026 *publication* contains no certification/audit/evidence mechanism.
2. **No measurable criteria — survives.** No threshold or pass/fail condition found in any mitigation list; `ASI08`'s residual-risk concession confirms the non-conformance posture.
3. **No governance management-system — survives.** Least-Agency and per-category controls found; no role matrix, accountability structure, or risk-classification process.
4. **No ATLAS mapping in the document — survives, narrowed.** Confirmed zero ATLAS references in the PDF. Narrowed to the document; community ATLAS↔ASI mappings may exist elsewhere and are not claimed absent.
5. **AIVSS scoring-only — survives.** Controls explicitly delegated to AIUC-1; no control clause in the AIVSS spec.
6. **Coarse Mitigation_Factor — survives.** Three qualitative levels with no attached control catalog or evidence model.
7. **Least-Agency unoperationalized — survives.** Principle present; no autonomy/risk-tier classification.

## Effect on existing wiki pages

- **[[owasp-agentic-ai-top-10|ASI Top 10 framework page]]:** gains `primary_documents` frontmatter with `archived_copy` and `scope_in_wiki` per methodology Step 1. Two category titles corrected to the published 2026 edition — `ASI05` "Sensitive Data Disclosure" → **"Unexpected Code Execution (RCE)"**, `ASI09` "Missing Guardrails" → **"Human-Agent Trust Exploitation"** — plus the minor title updates (`ASI02` "Tool Misuse and Exploitation", `ASI03` "Identity and Privilege Abuse", `ASI04` "Agentic Supply Chain Vulnerabilities", `ASI06` "Memory & Context Poisoning"). The "MITRE ATLAS cross-mapping now covers all 10 categories" claim is corrected: the 2026 document maps to the T-codes, LLM Top 10, and NHI Top 10, not to ATLAS.
- **[[owasp-aivss|AIVSS framework page]]:** gains `primary_documents` frontmatter with `archived_copy`. The amplification-factor list is corrected from the partial five-factor paraphrase to the **ten** official v0.8 factors, and the page gains the v0.8 Risk-Gap formula, the Threat Multiplier and Mitigation Factor scales, and a note that controls are delegated to AIUC-1.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the OWASP column carried the same stale draft labels:
    - **D3** cited "`ASI09` Missing Guardrails" — `ASI09` is Human-Agent Trust Exploitation and "Missing Guardrails" is not a 2026 category; replaced with `ASI02` Tool Misuse and Exploitation, the Least-Agency principle, and `ASI08` blast-radius guardrails.
    - **D6** cited "`ASI05` Sensitive Data Disclosure" — `ASI05` is Unexpected Code Execution (RCE); removed from D6 (RCE belongs to D4), retaining `ASI06` Memory & Context Poisoning.
    - **D9** cited no ASI category — added `ASI09` Human-Agent Trust Exploitation, the category most directly about human factors.
    - AIVSS amplification-factor names corrected to the v0.8 official set (e.g. "Autonomy Level" → "Execution Autonomy", "Tool Use Scope" → "External Tool Control Surface").
- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]] (OWASP row):** its wiki-summary-level verdict is superseded by this clause-level review.
- **[[standards-review-backlog|Standards Review Backlog]]:** OWASP ASI Top 10 + AIVSS flips to done (4 of 11 reviewed).
### Wiki-wide propagation pass (2026-06-22)

The first cut of this review stopped at the framework pages and the crosswalk. A follow-up pass reconciled the corrected taxonomy across the whole wiki — the renamed `ASI05` (RCE) and `ASI09` (Human-Agent Trust) had leaked into authoritative structures as the pre-release draft labels. Done:

- **[[agentic-ai-security-cmm-2026|CMM]] body — semantic re-anchoring.** `D3` cited `Maps to: OWASP ASI09` and tagged findings `ASI09`, using it to mean autonomy control; re-anchored to **`ASI02` + the Least-Agency principle** (with `ASI08` for blast-radius), since the published `ASI09` is Human-Agent Trust Exploitation. `D6` dropped `ASI05` (now RCE, not a data-domain concern), retaining `ASI06`. The AIVSS factor list and the two `Findings tag with` lines updated to the v0.8 factor names.
- **[[agentic-ai-security-reference-architecture|RA]] — plane remapping.** The threat diagram and the ASI→plane table mapped `ASI05` to Identity+Egress (data-disclosure controls) and `ASI09` to Runtime+Control (LlamaFirewall/NeMo). Re-mapped: `ASI05` (RCE) → Runtime+Control (sandboxing, `eval` ban, code/exec separation, CodeShield); `ASI09` (Human-Agent Trust) → Observability+Control (plan-divergence detection, provenance UI, oversight training).
- **Six framework comparison tables re-scored.** `nist-ai-rmf`, `iso-iec-42001`, `mitre-atlas`, `microsoft-rai`, `cosai`, `csa-maestro` each scored coverage against the draft `ASI05`/`ASI09` meanings. Both rows were relabeled **and re-evaluated** against the corrected categories (e.g. NIST `ASI09` rose to ● on the AI 600-1 Human-AI Configuration content; ATLAS `ASI05` rose to ● on its execution/escape techniques; Microsoft `ASI05`/`ASI09` fell from ● to ◐). The cross-framework matrix in [[ai-security-standards-in-q1-2026|AI Security Standards: Agentic Threats Outpace Frameworks]] was re-scored consistently, with a snapshot note that it predates the Dec-2025 release.
- **Concept + incident pages.** [[lethal-trifecta|Lethal Trifecta]] (`ASI05`→`ASI02` for the exfiltration outcome), [[least-agency-principle|Least Agency Principle]] (`ASI09`→`ASI03`), and [[jules-ai-kill-chain|Jules AI Kill Chain: Injection to Remote Control]] (three mislabels fixed: persistence→`ASI06`, the C2/RCE stage→`ASI05`, invisible-to-user→`ASI09`).
- **CMM domain pages with OWASP absence claims:** reframed where present — the ASI Top 10 supplies the control as guidance; what it lacks is the *auditable/measurable* form. The bounded statement is claim 1/2 above, not "OWASP doesn't cover it."
- **Methodology.** A [[standards-validation-methodology-2026-05|Definition of Done]] (§5.1) was added so future per-standard reviews execute this propagation as a required step rather than documenting it as optional.
- **Companion crosswalk (forward pointer).** The bidirectional ASI ↔ AIUC-1 mapping this review treats as out-of-band is now captured on the wiki: [[owasp-asi-aiuc1-crosswalk|the OWASP ASI to AIUC-1 crosswalk]] maps each ASI category to AIUC-1 requirements at the requirement level and records eight AIUC-1 coverage gaps, completing the certification-side counterpart to this review's control-candidate matrix.
