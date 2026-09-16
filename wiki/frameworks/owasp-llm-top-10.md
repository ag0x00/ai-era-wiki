---
type: framework
title: "OWASP Top 10 for LLM Applications"
address: c-000309
created: 2026-04-30
updated: 2026-09-16
origin: aggregated
tags:
  - frameworks
  - owasp
  - llm-security
  - vulnerability-taxonomy
status: developing
source_url: "https://genai.owasp.org/llm-top-10/"
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2024-11-01
published_by: "[[owasp|OWASP]]"
current_version: "2025 (released November 2024)"
first_published: "2023"
scope: "Top 10 security risks for LLM-based applications; awareness framework for developers and security teams"
audience: "Application developers, security teams, AI product builders"
primary_documents:
  - title: "OWASP Top 10 for LLM Applications 2025"
    url: "https://genai.owasp.org/llm-top-10/"
    version: "2025 edition"
    published: "2024-11-17"
    retrieved: "2026-06-22"
    scope_in_wiki: "All ten categories LLM01:2025-LLM10:2025 (codes and titles)"
aliases:
  - "OWASP LLM Top 10"
  - "LLM Top 10"
related:
  - "[[owasp-ai-exchange]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-aivss]]"
  - "[[owasp-genai-crosswalk]]"
  - "[[owasp|OWASP]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
  - "[[stride-ai-2026]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# OWASP Top 10 for LLM Applications

The **OWASP Top 10 for LLM Applications** is the primary vulnerability awareness list for large language model deployments. The 2025 edition (released November 2024) ranks **Prompt Injection as the #1 risk**, with only three categories surviving unchanged from 2023, reflecting rapid threat evolution.

## 2025 Edition Changes

New additions compared to 2023:
- **System Prompt Leakage**: exposure of confidential system instructions
- **Vector and Embedding Weaknesses** (`LLM08:2025`): RAG system-specific attack surface
- **Excessive Agency**: agentic architecture risks (addressed more fully in the [[owasp-agentic-ai-top-10|ASI Top 10]])

## Verified category set (2025 edition)

Codes and titles verified against [genai.owasp.org/llm-top-10](https://genai.owasp.org/llm-top-10/) (retrieved 2026-06-22) by [[standards-review-owasp-llm-top-10-2026-Q2|the LLM Top 10 standards review]].

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

The [[owasp-ai-exchange|OWASP AI Exchange]] covers the same threat as `LLM10:2025` Unbounded Consumption under the name AI resource exhaustion, and gives it two threat-specific controls, `DOS INPUT VALIDATION` and `LIMIT RESOURCES` ([`/go/airesourceexhaustion/`](https://owaspai.org/go/airesourceexhaustion/)). The correspondence stated here is this wiki's mapping. The Exchange cites this list by identifier elsewhere in the same document, and the wiki recorded three of those identifiers as unreconciled. The [[owasp-genai-crosswalk|GenAI Crosswalk]] settles one of them: under its 2026 numbering `LLM10` is Improper Output Handling, so the Exchange and the crosswalk cite the same unpublished renumbering rather than two separate errors. The 2026 edition year for Sensitive Information Disclosure is only partly explained: the crosswalk shows a 2026 numbering in circulation, while no 2026 edition is published. The code itself is `LLM02` under both numberings, so nothing about that identifier was ever in dispute. `LLM08` for Vector and Embedding Weaknesses stays unreconciled: it matches the 2025 numbering and not the crosswalk's, where that category is `LLM09`. Each is attributed to the Exchange on the page that carries the mapping.

## Current Status

As of September 2026 the published LLM Top 10 remains the 2025 edition, and a 2026 renumbering circulates inside OWASP artifacts without a published list behind it. The [[owasp-genai-crosswalk|GenAI Crosswalk]] labels its LLM source list `LLM-Top10-2026` and reassigns seven of the ten codes:

| Category | 2025 code | 2026 crosswalk code |
|---|---|---|
| Supply Chain | `LLM03` | `LLM04` |
| Data and Model Poisoning | `LLM04` | `LLM05` |
| Improper Output Handling | `LLM05` | `LLM10` |
| Excessive Agency | `LLM06` | `LLM03` |
| Vector and Embedding Weaknesses | `LLM08` | `LLM09` |
| Misinformation | `LLM09` | `LLM07` |
| Unbounded Consumption | `LLM10` | `LLM06` |

It also carries Hidden Context Exposure at `LLM08` in place of System Prompt Leakage, which this wiki reads as a rename of the same category rather than a new one; no OWASP artifact captured here states which it is. The crosswalk's own site prose disagrees with its data layer, listing the ten titles in 2025 order while substituting the renamed title. The table above stays the wiki's citable set, because `genai.owasp.org/llm-top-10/` published no 2026 edition as of 2026-09-16. A 2026 community questionnaire suggests a future update is under development, but no timeline has been announced.

The LLM Top 10 has been complemented rather than superseded by the [[owasp-agentic-ai-top-10|Agentic Applications Top 10]] (December 2025), which handles the agentic risk classes that the LLM Top 10 was not designed to address (multi-agent orchestration, cascading failures, rogue agents).

The **ML Security Top 10** remains dormant at v0.3, creating a gap in traditional ML security coverage.

## Adoption

Translated into 10+ languages. Vendor integrations by Kong, Lakera (acquired by Check Point Q1 2026), Invicti, and others. Referenced in enterprise security policies worldwide.

## Strengths

- De facto reference list for LLM application security
- Widely adopted; translated into many languages
- Actionable awareness for development teams
- Prompt injection coverage has informed a generation of defensive tooling

## Gaps and Shortcomings

- **Awareness framework, not compliance standard**: no certification, audit procedures, or evidence criteria
- Does not address agentic-specific risk classes (handled by [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]])
- Risk descriptions, not control baselines: organizations cannot directly derive a test plan
- ML Security Top 10 (v0.3 draft) is dormant, leaving traditional ML security coverage thin
- No AI incident response playbooks or IoCs
- **OWASP states the incompleteness is deliberate.** The [[owasp-ai-exchange|OWASP AI Exchange]], the other OWASP flagship AI project, positions the list as a route to quick awareness and states that it is intentionally not complete, naming the security of prompts as one omission; it directs readers seeking full coverage to the Exchange and readers seeking verification against technical requirements to the OWASP AISVS ([`/go/aiatowasp/`](https://owaspai.org/go/aiatowasp/)). Treating the list as a coverage baseline reads it against its stated purpose.

## See Also

- [[owasp|OWASP]] (publisher)
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — the agentic complement; covers ASI01–ASI10
- [[owasp-aivss|OWASP AI Vulnerability Scoring System (AIVSS)]] — OWASP's AI vulnerability scoring system
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — LLM Top 10 IDs anchor: `LLM01:2025` Prompt Injection maps to **D4 Runtime**; `LLM04:2025` Data and Model Poisoning to **D6 Data**; `LLM06:2025` Excessive Agency to **D3**; `LLM07:2025` System Prompt Leakage to **D6 + D9**; `LLM08:2025` Vector and Embedding Weaknesses to **D6**; `LLM10:2025` Unbounded Consumption to **D4 + D5**
- [[standards-review-owasp-llm-top-10-2026-Q2|Standards Review — OWASP LLM Top 10]] — primary-source verification of all ten codes and the CMM coverage matrix
- [[stride-ai-2026|STRIDE-AI Threat Modeling Framework]] — academic threat-modeling method that proposes the LLM Top 10 as the technical taxonomy bridged to NIST AI RMF governance

<!-- sources:auto -->
## Sources

- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)
<!-- /sources -->
