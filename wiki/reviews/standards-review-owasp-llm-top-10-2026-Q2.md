---
type: review
title: "OWASP LLM Top 10 Standards Review"
address: c-000226
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - owasp
  - llm-security
  - vulnerability-taxonomy
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[owasp-llm-top-10]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
primary_documents:
  - title: "OWASP Top 10 for LLM Applications 2025"
    url: "https://genai.owasp.org/llm-top-10/"
    version: "2025 edition"
    published: "2024-11-17"
    retrieved: "2026-06-22"
    scope_in_wiki: "All ten categories LLM01:2025-LLM10:2025 (codes and titles)"
related:
  - "[[owasp-llm-top-10]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-aivss]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[standards-review-owasp-agentic-aivss-2026-Q2]]"
  - "[[standards-review-backlog]]"
sources:
  - "https://genai.owasp.org/llm-top-10/"
  - "https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/"
---

# Standards Review — OWASP LLM Top 10, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to the OWASP Top 10 for LLM Applications 2025 edition: primary-source verification of every category code and title from the OWASP GenAI Security Project site, a category-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains, falsifiable absence claims, and an adversarial second pass. It is distinct from [[standards-review-owasp-agentic-aivss-2026-Q2|the OWASP Agentic/ASI Top 10 + AIVSS review]], which covers the agentic taxonomy; this review covers the application-level LLM list that the ASI Top 10 explicitly complements rather than replaces.

The review verified all ten codes against the source, confirmed the November 17, 2024 publication date of the "2025" edition,[^pub] mapped each category to its anchoring CMM domain, and found two taxonomy-drift errors elsewhere in the wiki where 2023-era titles or mis-numbered categories persist.

**The LLM Top 10 is an application-level awareness list, not an agentic control standard, and it under-covers exactly the three agent-specific CMM domains — D2 Identity, D3 Control & Least-Agency, and D9 Operations.** Only `LLM06:2025` Excessive Agency reaches into agent autonomy, and it does so as a single risk description with no graded control. The agentic surface that D2/D3/D9 grade is the deliberate scope of the [[owasp-agentic-ai-top-10|ASI Top 10]], not this list.

## Primary documents reviewed

The 2025 edition has no per-category page on a single canonical URL; the GenAI Security Project landing page enumerates the codes and titles, and the resource page carries the publication date and downloadable PDF. Both were read for this review.

| Document | URL | Scope used in this review |
|---|---|---|
| OWASP Top 10 for LLM Applications 2025 (landing list) | [genai.owasp.org/llm-top-10](https://genai.owasp.org/llm-top-10/) | Verified codes and titles LLM01:2025-LLM10:2025 |
| OWASP Top 10 for LLM Applications 2025 (resource page) | [genai.owasp.org resource](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/) | Publication date (2024-11-17); PDF download |

### Verified category set (primary source, retrieved 2026-06-22)

All ten codes and titles below were read directly from [genai.owasp.org/llm-top-10](https://genai.owasp.org/llm-top-10/); none are recited from memory.

| Code | Title |
|---|---|
| `LLM01:2025` | Prompt Injection |
| `LLM02:2025` | Sensitive Information Disclosure |
| `LLM03:2025` | Supply Chain |
| `LLM04:2025` | Data and Model Poisoning |
| `LLM05:2025` | Improper Output Handling |
| `LLM06:2025` | Excessive Agency |
| `LLM07:2025` | System Prompt Leakage |
| `LLM08:2025` | Vector and Embedding Weaknesses |
| `LLM09:2025` | Misinformation |
| `LLM10:2025` | Unbounded Consumption |

## Structure-to-domain grounding

The 2025 list is a ranked vulnerability-awareness taxonomy for LLM-based applications. Each entry is a risk description with example attack scenarios and prevention guidance; none carries a measurable acceptance criterion, audit procedure, or pass/fail threshold. It maps onto the CMM unevenly:

- **Application-runtime and data surface (strong): D4, D5, D6.** Prompt injection, improper output handling, system-prompt leakage, vector/embedding weaknesses, data/model poisoning, and sensitive-information disclosure cover the runtime, egress, and data domains in detail.
- **Supply chain (strong, application-scoped): D8.** `LLM03:2025` Supply Chain is a dedicated category, though scoped to model/dataset/plugin provenance rather than an AI-BOM control program.
- **Identity, control, operations (thin or absent): D2, D3, D9.** Only `LLM06:2025` Excessive Agency touches agent autonomy and identity scoping, and only as a risk class. No category addresses non-human identity, least-agency tool gating, or operational human factors as graded controls — these are the ASI Top 10's domain.
- **Governance (absent): D1.** The list carries no governance, accountability, or risk-framework content by design.

## Clause-level coverage matrix (CMM x Standard)

Each row cites the verified `LLMxx:2025` categories that anchor a CMM domain. Categories are application-level risk descriptions; they ground a domain's *threat model*, not its level criteria.

| CMM domain | Anchoring LLM Top 10 categories (2025) | Coverage |
|---|---|---|
| D1 Governance & Accountability | none | None — no governance, accountability, or risk-management content |
| D2 Identity & Authorization | `LLM06:2025` Excessive Agency (excessive permissions / autonomy facet only) | Thin — agency framed as a risk; no non-human-identity or authorization controls |
| D3 Control & Least-Agency | `LLM06:2025` Excessive Agency | Partial — names excessive functionality/permissions/autonomy; no graded least-agency control or tool-gating spec |
| D4 Runtime & Guardrails | `LLM01:2025` Prompt Injection; `LLM05:2025` Improper Output Handling; `LLM09:2025` Misinformation | Strong (application-level) — the best-covered domain; risk descriptions, not control baselines |
| D5 Egress & Network | `LLM02:2025` Sensitive Information Disclosure; `LLM07:2025` System Prompt Leakage; `LLM10:2025` Unbounded Consumption (resource/cost exfil facet) | Partial — data-egress risks named; no network/egress-filtering control |
| D6 Data, Memory & RAG | `LLM04:2025` Data and Model Poisoning; `LLM08:2025` Vector and Embedding Weaknesses; `LLM02:2025` Sensitive Information Disclosure; `LLM07:2025` System Prompt Leakage | Strong — RAG/embedding and poisoning surface covered; no agent-memory poisoning model |
| D7 Observability & Detection | `LLM09:2025` Misinformation (output-monitoring facet); `LLM10:2025` Unbounded Consumption (rate/cost monitoring facet) | Thin — implies monitoring; no detection-rule or telemetry guidance |
| D8 Supply Chain & AI-BOM | `LLM03:2025` Supply Chain; `LLM04:2025` Data and Model Poisoning | Partial — dedicated supply-chain category; no AI-BOM artifact or provenance-verification spec |
| D9 Operations & Human Factors | `LLM07:2025` System Prompt Leakage (confidentiality test cases); `LLM10:2025` Unbounded Consumption (cost-budget facet) | Thin — supplies a system-prompt-leakage test surface; no IR runbook, lifecycle, or human-factors content |

The matrix confirms the list's design premise: it enumerates application-level LLM risks (what can go wrong) but not a graded defensive program. Its prevention sections name control categories without specifications, so they ground a CMM domain's threat model, not its level criteria — the same shape as MITRE ATLAS mitigations, and the gap the CMM's per-level evidence rubric fills.

## Falsifiable absence claims found

What the CMM scores that the LLM Top 10 2025, as published, does not provide. Each is bounded to the verified category set and reversible by the stated refuting evidence.

1. **No non-human-identity or authorization controls (D2).** The list has no category for agent/non-human identity, credential scoping, or authorization. *Searched:* the ten verified 2025 categories and titles at [genai.owasp.org/llm-top-10](https://genai.owasp.org/llm-top-10/). *Terms:* "identity", "non-human", "credential", "authorization", "least privilege". *Verdict:* not addressed. The closest, `LLM06:2025` Excessive Agency, names excessive permissions as a *consequence* of over-granted agency, not an identity-control program. *Refuting evidence:* a category specifying non-human-identity or authorization controls. *Reviewed 2026-06-22.* The ASI Top 10's `ASI03` (Identity and Privilege Abuse) is where this lives.

2. **Excessive Agency is a single risk class, not a graded least-agency control (D3).** `LLM06:2025` describes the risk of excessive functionality, permissions, and autonomy, but specifies no tool-gating, intent-mediation, or progressive-autonomy control with acceptance criteria. *Searched:* the verified category set and the `LLM06:2025` description. *Terms:* "least agency", "tool gating", "intent", "policy decision point", "autonomy promotion", "graded". *Verdict:* confirmed — risk-level only. *Refuting evidence:* a measurable least-agency control or autonomy-promotion gate inside the list. *Reviewed 2026-06-22.* Matches the CMM `D3` control gap the ASI Top 10 (`ASI02` Tool Misuse) and CSA ATF gates fill.

3. **No agentic multi-agent, orchestration, or cascading-failure content.** The list is application-level; no category models multi-agent orchestration, agent-to-agent trust, or cascade failure. *Searched:* the verified category set. *Terms:* "multi-agent", "orchestration", "cascade", "agent-to-agent", "rogue agent". *Verdict:* confirmed — definitional exclusion. The wiki's own [[owasp-llm-top-10|framework page]] and [[owasp-agentic-ai-top-10|ASI Top 10]] both state the LLM Top 10 was not designed for these classes. *Refuting evidence:* a category naming multi-agent or cascade risk. *Reviewed 2026-06-22.*

4. **Awareness list, not a conformance or audit standard.** No category carries a control specification, certification scheme, audit procedure, or evidence/pass-fail criterion. *Searched:* the verified category set and per-category prevention guidance structure. *Terms:* "shall", "audit", "certification", "evidence", "criteria", "conformance", "test procedure". *Verdict:* confirmed. *Refuting evidence:* any category with measurable conformance criteria or a certification mapping. *Reviewed 2026-06-22.* This is the same awareness-not-conformance finding the [[standards-review-owasp-agentic-aivss-2026-Q2|ASI/AIVSS review]] records for the OWASP agentic family.

5. **No operational, lifecycle, or human-factors program (D9).** No category addresses incident response, model-deprecation lifecycle, decommissioning, or human-oversight fatigue. *Searched:* the verified category set. *Terms:* "incident response", "deprecation", "decommission", "lifecycle", "human oversight", "operations". *Verdict:* confirmed. `LLM07:2025` supplies a system-prompt-confidentiality test surface and `LLM10:2025` a cost-budget facet, but neither is an operations program. *Refuting evidence:* a category naming IR, lifecycle, or human-factors controls. *Reviewed 2026-06-22.*

## Scope exclusions

- **Per-category prevention-guidance enumeration.** The matrix anchors each domain with its load-bearing categories, not every prevention bullet under each category.
- **The downloadable PDF's full narrative.** Verification used the codes and titles published on the GenAI Security Project landing and resource pages; the per-category example scenarios in the PDF were not transcribed clause-by-clause.
- **The dormant ML Security Top 10 and the 2023 edition,** except where 2023-era labels surface as drift on existing wiki pages (recorded below).
- **Production effectiveness.** Document-versus-document review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second pass attempted a counter-example for each absence claim against the verified 2025 category set. **All five claims survive.**

1. **No NHI/authorization controls (D2) — survives.** `LLM06:2025` names excessive permissions as a consequence; it specifies no identity or authorization control.
2. **Excessive Agency is risk-only, not graded control (D3) — survives.** The category describes the risk and lists generic mitigations (limit functionality/permissions/autonomy) without acceptance criteria or a promotion gate.
3. **No multi-agent/cascade content — survives.** No category in the verified set names multi-agent, orchestration, or cascade risk; corroborated by OWASP's own positioning of the ASI Top 10 as the complement.
4. **Awareness not conformance — survives.** No category carries certification, audit, or measurable-criteria language.
5. **No D9 operations/lifecycle program — survives.** `LLM07:2025` and `LLM10:2025` supply test surfaces, not an operations program.

## Effect on existing wiki pages

- **[[owasp-llm-top-10|LLM Top 10 framework page]]:** the verified ten-category table (codes + exact titles) and the November 17, 2024 publication date were confirmed against the source; the `LLM08:2025` title was reconciled to the full "Vector and Embedding Weaknesses"; `primary_documents` frontmatter was added per Step 1; the new review is linked from `related:` and See Also.
- **[[rag-hardening|RAG hardening]]:** the mislabeled line citing `OWASP LLM07` as "Vector and Embedding Weaknesses" was corrected — `LLM07:2025` is **System Prompt Leakage**; the vector/embedding category is **`LLM08:2025`**. Both entries were re-cited with correct codes and `:2025` suffixes.
- **[[nist-sp-800-218a|NIST SP 800-218A]]:** the "OWASP LLM03 (Training Data Poisoning)" reference was annotated as a 2023-edition label — in the 2025 edition `LLM03:2025` is **Supply Chain** and poisoning is **`LLM04:2025` Data and Model Poisoning**; the line was reframed to name the edition.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the LLM Top 10 cells were verified against the source and confirmed correct; the D2 row's `LLM06:2025` anchor was annotated as agency-facet-only per absence claim 1.
- **[[agentic-ai-security-cmm-2026|Canonical CMM]]:** the `LLM01:2025`-`LLM10:2025` reference range was verified complete and correct.
- **[[standards-review-backlog|Standards Review Backlog]]:** OWASP LLM Top 10 (P3) flips to done.

## Notes

[^pub]: [OWASP Top 10 for LLM Applications 2025 — resource page](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/), publication date November 17, 2024 (the "2025" edition), retrieved 2026-06-22. Codes and titles verified from [genai.owasp.org/llm-top-10](https://genai.owasp.org/llm-top-10/), retrieved 2026-06-22.
