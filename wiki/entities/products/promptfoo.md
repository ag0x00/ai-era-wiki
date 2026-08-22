---
type: entity
entity_type: product
title: "Promptfoo"
created: 2026-05-02
updated: 2026-05-02
tags:
  - entities
  - products
  - red-team
  - eval
  - regression
  - open-source
status: developing
scope_axis:
  - sec-of-ai
publisher: "Promptfoo (now part of OpenAI, 2026)"
license: "MIT"
canonical_site: "https://www.promptfoo.dev/"
red_team_docs: "https://www.promptfoo.dev/docs/red-team/"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[pyrit]]"
  - "[[garak]]"
  - "[[mindgard-cart]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[owasp-llm-top-10]]"
  - "[[nist-ai-rmf]]"
sources:
  - "https://www.promptfoo.dev/"
  - "https://www.promptfoo.dev/blog/model-upgrades-break-agent-safety/"
  - "https://www.promptfoo.dev/docs/red-team/"
---

# Promptfoo — LLM evaluation and red-teaming framework

Open-source LLM evaluation and red-teaming framework that runs **YAML-defined test suites in CI** to catch prompt regressions, vulnerability findings, and behavioral drift. The wiki's [[agentic-ai-security-cmm-2026|CMM]] cites Promptfoo as the **"regression suite"** attack category in the D7 L4 four-quadrant red-team coverage requirement, and as the empirical source for the model-version-degradation finding in [[agentic-ai-threat-classes-2026|Threat Classes Class 4]].

> [!note] Acquisition
> The Promptfoo public site banner reads *"Promptfoo is now part of OpenAI"* (2026). The project remains MIT-licensed and the framework continues to ship; the wiki's vendor-neutrality framing in [[agentic-ai-security-cmm-2026|CMM]] D7 L4 should note the new organizational home.

## Function

| Capability | Detail |
|---|---|
| **YAML-defined evals** | Assertion-driven test cases; model + RAG comparisons; factuality scoring; hallucination tests |
| **Red-team plugins** | 50+ vulnerability types — direct + indirect prompt injection, guardrail-tailored jailbreaks, PII/data leaks, business-rule violations, insecure tool use, BOLA/BFLA authorization tests, competitor-endorsement, harmful-content |
| **Standards mapping** | Plugins published with [[owasp-llm-top-10\|OWASP LLM Top 10]] and [[nist-ai-rmf\|NIST AI RMF]] mappings |
| **CI integration** | GitHub / GitLab / Jenkins; output diffs surface as PR comments |

The **regression-suite framing** is correct: the same YAML eval set re-run on a new model version or prompt change is the regression check. This is what distinguishes Promptfoo from [[pyrit|PyRIT]] (orchestration) and [[garak|Garak]] (probe library).

## The model-upgrade-regression research

Promptfoo's blog post **["Your model upgrade just broke your agent's safety"](https://www.promptfoo.dev/blog/model-upgrades-break-agent-safety/)** (Guangshuo Zang, Dec 8 2025) is the wiki's primary empirical source for [[agentic-ai-threat-classes-2026|Threat Class 4 (model-version-degradation)]]. Headline findings, all directly quoted from the post:

| Claim | Number |
|---|---|
| GPT-4o → GPT-4.1 prompt-injection resistance | dropped 94% → 71% |
| Anthropic Constitutional Classifiers, jailbreak success | reduced 86% → 4.4% |
| GPT-4o-mini AgentHarm | 62.5% harm with only 22% refusal |
| Gemini 1.5 Pro refusal under jailbreak | dropped 78.4% → 3.5% |
| Crescendo vs single-turn jailbreaks | beats by 29–61% on GPT-4, 49–71% on Gemini-Pro |
| BadLlama vs Llama 3 8B safety | strips it in ~1–5 minutes |

Recommended fixes from the post: pin model IDs (no `latest`), re-run prompt-injection + tool-abuse tests on every upgrade, add app-layer guardrails, least-privilege execution credentials, monitor injection signals. *"Treat model upgrades as security changes, not just quality upgrades."*

## Direct quotes

- *"Your model upgrade just broke your agent's safety."* — verbatim post title
- *"Treat model upgrades as security changes, not just quality upgrades."* — same post

## Use in this wiki

- [[agentic-ai-security-cmm-2026|CMM]] D7 L4 — regression-suite red-team category
- [[agentic-ai-threat-classes-2026|Threat Classes 2026]] §Class 4 — primary empirical anchor for model-version-degradation
- [[agentic-cmm-vs-standards-validation|Validation page]] §5 #7 — flagged the "single-tool coverage is not L4" rule that depends on Promptfoo + Garak + PyRIT + Mindgard being treated as distinct attack categories

## Caveats

- **No version number on the landing page**; pin via npm/PyPI in any wiki citation.
- **Acquisition by OpenAI** changes the vendor-neutrality posture — peer reviewers may push on whether OpenAI-owned tooling can be used for objective evaluation of OpenAI models.
- **BOLA/BFLA plugins are the most explicit agentic-tool red-team primitives in the open-source set**, but agentic / multi-step coverage still trails commercial CART offerings.

## See Also

- [[pyrit|PyRIT]] — orchestration counterpart
- [[garak|Garak]] — probe library counterpart
- [[mindgard-cart|Mindgard CART]] — continuous managed counterpart
- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes 2026]] §Class 4 — primary citation
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — D7 L4 evidence anchor
