---
type: entity
title: "Visa"
address: c-000337
created: 2026-08-31
updated: 2026-09-01
tags:
  - entities
  - organizations
  - vuln-discovery
  - ai-in-sec-defense
status: seed
scope_axis:
  - ai-in-sec-defense
origin: aggregated
entity_type: organization
org_type: vendor
homepage: "https://www.visa.com"
role: "Global payments network; publisher of VVAH, an eleven-stage agentic SAST pipeline built on learnings from Anthropic's Project Glasswing."
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[vvah]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[anthropic]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# Visa

**Sources:** [Visa (homepage)](https://www.visa.com) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

Visa is a global payments-network company. It publishes `visa/visa-vulnerability-agentic-harness` on GitHub, an agentic SAST pipeline named in [[semgrep|Semgrep]]'s July 2026 survey of open-source AI code-security harnesses.

## Relevance to This Wiki

[[vvah|VVAH]] runs an eleven-stage flow, per Semgrep's LLM-generated summary of the repository, that Visa built on learnings from [[anthropic|Anthropic]]'s Project Glasswing.[^llm] The pipeline threat-models a target with STRIDE, decomposes it into taint chunks, and adversarially verifies each finding before proposing and scoring a fix, all without executing code. Semgrep records the project at about 600 GitHub stars and licensed Apache 2.0.[^stars] Its LLM-generated sections add that the repository is closed to external contributions.[^llm]

Semgrep recommends VVAH to an AppSec program optimizing time-to-reviewed-fix, describing it as broad-language SAST with real triage rigor at the cost of a frontier-model dependency and no runnable proof of concept. VVAH's default configuration uses Claude Sonnet 4.6 for detection and Claude Opus 4.8 for remediation and validation, per the same LLM-generated summary.[^llm]

## Outputs / Products

- **VVAH (visa-vulnerability-agentic-harness)** — eleven-stage agentic SAST pipeline across 42 languages and six specialist lenses; proposes and LLM-validates fixes; exports SARIF.[^llm]

## See Also

- [[vvah|VVAH]] — the product covered here.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.
- [[anthropic|Anthropic]] — VVAH draws on learnings from Anthropic's Project Glasswing.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
