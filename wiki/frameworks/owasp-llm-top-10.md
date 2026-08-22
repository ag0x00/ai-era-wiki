---
type: framework
title: "OWASP Top 10 for LLM Applications"
address: c-000309
created: 2026-04-30
updated: 2026-08-19
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

The [[owasp-ai-exchange|OWASP AI Exchange]] covers the same threat as `LLM10:2025` Unbounded Consumption under the name AI resource exhaustion, and gives it two threat-specific controls, `DOS INPUT VALIDATION` and `LIMIT RESOURCES` ([`/go/airesourceexhaustion/`](https://owaspai.org/go/airesourceexhaustion/)). The correspondence stated here is this wiki's mapping. The Exchange cites this list by identifier elsewhere in the same document, and the wiki records three of those identifiers as unreconciled: `LLM10:2026` for Improper Output Handling, `LLM08` for a category that is live as Vector and Embedding Weaknesses, and a 2026 edition year for Sensitive Information Disclosure. Each is attributed to the Exchange on the page that carries the mapping.

## Current Status (Q1 2026)

As of April 2026, the LLM Top 10 2025 is **unchanged**. A 2026 community questionnaire suggests a future update is under development, but no timeline has been announced.

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
- [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] — LLM Top 10 IDs anchor: `LLM01:2025` Prompt Injection → **D4 Runtime**; `LLM04:2025` Data and Model Poisoning → **D6 Data**; `LLM06:2025` Excessive Agency → **D3**; `LLM07:2025` System Prompt Leakage → **D6 + D9**; `LLM08:2025` Vector and Embedding Weaknesses → **D6**; `LLM10:2025` Unbounded Consumption → **D4 + D5**
- [[standards-review-owasp-llm-top-10-2026-Q2|Standards Review — OWASP LLM Top 10]] — primary-source verification of all ten codes and the CMM coverage matrix
- [[stride-ai-2026|STRIDE-AI Threat Modeling Framework]] — academic threat-modeling method that proposes the LLM Top 10 as the technical taxonomy bridged to NIST AI RMF governance

<!-- sources:auto -->
## Sources

- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)
<!-- /sources -->
