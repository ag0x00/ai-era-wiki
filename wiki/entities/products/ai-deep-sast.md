---
type: entity
entity_type: product
title: "ai-deep-sast"
address: c-000330
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
vendor: "Cisco (cisco-open)"
license: "Apache 2.0"
role: "Fast SAST scanner combining rule-based Semgrep detection with LLM triage behind an evidence gate; an optional fully local mode runs a security-tuned 8B model so no code leaves the machine."
homepage: "https://github.com/cisco-open/ai-deep-sast"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[cisco]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# ai-deep-sast

**Sources:** [GitHub — cisco-open/ai-deep-sast](https://github.com/cisco-open/ai-deep-sast) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

ai-deep-sast is an open-source SAST+LLM hybrid vulnerability scanner published under [[cisco|Cisco]]'s Cisco Open program. In [[semgrep|Semgrep]]'s July 2026 survey of open-source AI code-security harnesses, the project held about 50 GitHub stars and an Apache 2.0 licence, the smallest following of the nine projects compared.[^stars] Semgrep places it in the SAST+LLM hybrid category: tools that use deterministic program analysis to narrow an LLM's search space before the model reasons over what remains.

## Mechanism

Semgrep's LLM-generated summary of the repository describes a hybrid SAST tool written in Python.[^llm] The Semgrep engine performs fast, rule-based detection; a large language model then triages each finding behind an "evidence gate" meant to surface only true positives. A fast-scan mode can run entirely on a local, security-tuned 8B-parameter model, Foundation-Sec-8B, so no code leaves the machine. A deep-scan mode instead hands function-level context to a frontier model, unnamed in the source. The same summary calls the optional fully local mode a genuine privacy differentiator and notes the project is brand new, at v1.0.0.

## Output and capability matrix

Findings export as Markdown, JSON, or JUnit, with severity gates usable in CI.[^llm] Semgrep's capability-matrix table, also LLM-generated, scopes the tool to the OWASP/CWE Top 25, secrets, and AI/ML-specific findings across 30 or more languages.[^llm] The matrix records ai-deep-sast's isolation posture as "n/a (static)": the tool performs no dynamic execution, so no execution sandbox applies, unlike the pipelines in the comparison that compile or run the code they analyze.[^llm]

## Positioning

Semgrep names ai-deep-sast as the harness to reach for when the requirement is running fully local and offline. Of the nine projects compared, only ai-deep-sast can run entirely on-device, a choice Semgrep frames as trading depth and proof for breadth and speed. In the comparison's "finding" table, an ai-deep-sast finding is a triaged static match; in the execution table, the tool neither executes code nor produces a proof of concept or a patch, offering advice only.

## See Also

- [[cisco|Cisco]] — publisher.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.
- [[semgrep|Semgrep]] — publisher of the source; ai-deep-sast's static-detection layer is Semgrep's own engine.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
