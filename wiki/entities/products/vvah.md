---
type: entity
entity_type: product
title: "VVAH"
address: c-000331
created: 2026-08-31
updated: 2026-09-01
tags:
  - entities
  - products
  - sast
  - vuln-discovery
  - ai-in-sec-defense
status: seed
scope_axis:
  - ai-in-sec-defense
origin: aggregated
aliases:
  - "visa-vulnerability-agentic-harness"
vendor: "Visa"
license: "Apache 2.0 (closed to external contributions)"
role: "Eleven-stage agentic SAST pipeline across 42 languages: threat-models, verifies adversarially, proposes and validates fixes without executing code."
homepage: "https://github.com/visa/visa-vulnerability-agentic-harness"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[visa]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[anthropic]]"
  - "[[semgrep]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# VVAH

**Sources:** [GitHub — visa/visa-vulnerability-agentic-harness](https://github.com/visa/visa-vulnerability-agentic-harness) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

VVAH (visa-vulnerability-agentic-harness) is an open-source SAST+LLM hybrid vulnerability pipeline published by [[visa|Visa]]. In [[semgrep|Semgrep]]'s July 2026 survey, the project held about 600 GitHub stars under an Apache 2.0 licence.[^stars] Semgrep's LLM-generated sections add that the repository is closed to external contributions.[^llm]

## Pipeline

Semgrep's LLM-generated summary describes an eleven-stage flow, built on learnings from [[anthropic|Anthropic]]'s Project Glasswing.[^llm] The pipeline threat-models a target using STRIDE, decomposes it into taint chunks, and deep-dives each chunk with optional majority voting. It then adversarially verifies every finding — a second reviewer tries to falsify it and assigns a CVSS score — before deduplicating verified findings and chaining them into synthesized exploit paths, exported as SARIF. A final stage proposes a minimal fix and scores that fix with an agentic panel filling a security-architect, a pen-tester, and a cross-repo-analyzer role. VVAH performs no code execution at any stage. Backends are vendor-neutral; the default configuration uses Claude Sonnet 4.6 for detection and Claude Opus 4.8 for remediation and validation. Semgrep's summary states the pipeline optimizes "Mean Time to Adapt," the interval from discovery to a validated fix.

## Capability matrix

Semgrep's capability matrix, also LLM-generated, scopes VVAH to 77 or more CWEs across six specialist lenses and 42 languages, with Markdown and SARIF output.[^llm] The matrix lists VVAH's isolation as a tool sandbox with Bash access removed, the pipeline's only stated containment mechanism beyond the absence of code execution.[^llm]

## Positioning

The adversarial-verification stage above is one of four named instances of adversarial validation in Semgrep's comparison, alongside Cloudflare's Phase 3, Trail of Bits' `fp-check`, and defending-code-harness's fresh-container grader. Semgrep recommends VVAH to an AppSec program optimizing time-to-reviewed-fix, at the cost of a frontier-model dependency and no runnable proof of concept. In the comparison's execution table, VVAH does not execute code or produce a PoC, but does propose a patch that a second LLM pass validates.

## See Also

- [[visa|Visa]] — publisher.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.
- [[anthropic|Anthropic]] — VVAH is built on learnings from Anthropic's Project Glasswing.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
