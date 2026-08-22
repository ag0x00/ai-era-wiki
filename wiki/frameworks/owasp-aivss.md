---
type: framework
title: "OWASP AI Vulnerability Scoring System (AIVSS)"
created: 2026-04-30
updated: 2026-06-22
tags:
  - frameworks
  - owasp
  - vulnerability-scoring
  - agentic-ai
  - measurement
status: developing
source_url: "https://aivss.owasp.org/"
primary_documents:
  - title: "AIVSS Scoring System for OWASP Agentic AI Core Security Risks"
    url: "https://aivss.owasp.org/"
    version: "0.8"
    published: "2026-03-19"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/owasp-aivss-v0.8.pdf"
    scope_in_wiki: "§2 ten Agentic Risk Amplification Factors + 0/0.5/1.0 rubric; §3 AARS Risk-Gap formula, Threat Multiplier, Mitigation Factor; worked examples"
adoption_signal: active
last_substantive_update: 2026-03-19
published_by: "[[owasp|OWASP]]"
current_version: "v0.8 (March 19, 2026)"
first_published: "2026-03-19"
scope: "Quantitative vulnerability scoring system for AI systems; extends CVSS 4.0 with agentic amplification factors"
audience: "Security teams, vulnerability researchers, AI platform operators, risk managers"
aliases:
  - "AIVSS"
  - "AI Vulnerability Scoring System"
related:
  - "[[owasp|OWASP]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-llm-top-10]]"
  - "[[agentic-ai-security-cmm-2026]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# OWASP AI Vulnerability Scoring System (AIVSS)

**OWASP AIVSS v0.8** (March 19, 2026) is the first AI-specific vulnerability scoring system. It extends **CVSS 4.0** with agentic amplification factors that CVSS was not designed to capture, producing contextual severity scores on a 0-10 scale.

This fills a critical measurement gap: CVSS 4.0 treats a vulnerability as a static property of code, but AI vulnerabilities are contextually dependent on how the AI system is deployed, what capabilities it has, and how autonomously it operates.

## Agentic Risk Amplification Factors

AIVSS v0.8 scores **ten** Agentic Risk Amplification Factors, each on a 3-point scale — `0.0` (none / not present), `0.5` (partial / limited), `1.0` (full / unconstrained). Their unweighted total is `Factor_Sum` (max 10). The factors below are the official v0.8 set; an earlier wiki revision listed a paraphrased subset of five.

| Factor | Description |
|---|---|
| **Execution Autonomy** | Ability to execute actions without human verification |
| **External Tool Control Surface** | Breadth and privilege of external APIs / tools the agent can access |
| **Natural Language Interface** | Reliance on unstructured natural language for goal formulation and instruction |
| **Contextual Awareness** | Use of environmental sensors or broad data context to drive decisions |
| **Behavioral Non-Determinism** | Variance in output or action for identical inputs |
| **Opacity & Reflexivity** | Lack of internal visibility or ability to audit decision logic |
| **Persistent State Retention** | Ability to retain memory or state across sessions |
| **Dynamic Identity** | Ability to assume different user roles or permissions at runtime |
| **Multi-Agent Interactions** | Coordination with or dependency on other autonomous agents |
| **Self-Modification** | Ability to alter its own code, prompts, or tool configurations |

## Scoring Formula

AIVSS v0.8 consumes a CVSS v4.0 base score unchanged and adds an agentic uplift over the "Agentic Risk Gap" — the headroom between the base score and 10:

```
AARS  = (10 − CVSS_Base) × (Factor_Sum / 10) × ThM
AIVSS = (CVSS_Base + AARS) × Mitigation_Factor          (bounded 0–10)
```

- **Threat Multiplier (`ThM`):** `1.0` Attacked, `0.97` Proof-of-Concept (default), `0.50` Unreported.
- **Mitigation Factor:** `1.0` No/Weak (default), `0.83` Partial, `0.67` Strong — a coarse scalar that scales the score by mitigation strength; it enumerates no controls.

The v0.5 spec used a different simple-average form (`((CVSS_Base + AARS) / 2) × ThM`, with `ThM = 0.91` for Unreported); cite the v0.8 Risk-Gap form above. AIVSS scores severity and delegates the control catalog to [[aiuc-1|AIUC-1]] via the published AIUC-1 ↔ AIVSS crosswalk. See [[standards-review-owasp-agentic-aivss-2026-Q2|the 2026-Q2 standards review]].

## Status and Roadmap

- v0.8 published March 19, 2026 — community review phase
- Next community review period opens: **April 16, 2026**
- OWASP AIBOM Generator is a companion tool (CycloneDX format) for inventory
- A ratified v1.0 would be the first standardized AI vulnerability scoring methodology

## Significance

The absence of an AI-specific scoring system means:
- CVE reporters default to CVSS 4.0, which may systematically underweight agentic vulnerabilities
- Risk prioritization across organizations is inconsistent
- No common language for comparing AI vulnerability severity

AIVSS provides the scoring methodology needed to make the [[owasp-agentic-ai-top-10|ASI Top 10]] quantitatively actionable.

## Strengths

- First and only AI-specific vulnerability scoring extension
- Addresses the fundamental limitation of CVSS for agentic systems
- OWASP community governance ensures vendor-neutral development
- Direct integration with CVSS 4.0 eases adoption for security teams already using CVSS

## Gaps and Shortcomings

- Still v0.8 — in community review; has not achieved the adoption level of CVSS
- Formal validation of amplification factor weights is ongoing
- No CVSS integration specification yet (how AIVSS scores relate to NVD/CVE records)
- Implementation tooling limited to community review documentation

## See Also

- [[owasp|OWASP]] (publisher)
- [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications (ASI Top 10)]] — ASI Top 10 provides the risk taxonomy that AIVSS scores
- [[owasp-llm-top-10|OWASP Top 10 for LLM Applications]] — AIVSS also applies to LLM vulnerabilities

<!-- sources:auto -->
## Sources

- [OWASP AI Vulnerability Scoring System (AIVSS)](https://aivss.owasp.org/)
<!-- /sources -->
