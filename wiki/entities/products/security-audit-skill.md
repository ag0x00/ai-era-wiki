---
type: entity
entity_type: product
title: "security-audit-skill"
address: c-000334
created: 2026-08-31
updated: 2026-09-01
tags:
  - entities
  - products
  - skill-boosting
  - vuln-discovery
  - claude-code
  - ai-in-sec-defense
status: seed
scope_axis:
  - ai-in-sec-defense
origin: aggregated
vendor: "Cloudflare"
license: "MIT"
role: "Claude Code skill running a six-phase multi-agent security audit with adversarial validation, installed via npx skills add."
homepage: "https://github.com/cloudflare/security-audit-skill"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[cloudflare]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# security-audit-skill

**Sources:** [GitHub — cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

security-audit-skill is a Claude Code skill published by [[cloudflare|Cloudflare]], placed by [[semgrep|Semgrep]]'s July 2026 survey in the LLM-skill-boosting vulnerability-research category: skills added inside an LLM to make it reason about code the way a human vulnerability researcher would. The survey records about 2K GitHub stars under an MIT licence.[^stars]

## Mechanism

Semgrep's LLM-generated summary describes roughly 1,200 lines of Markdown methodology paired with a zero-dependency Node validator, installed via `npx skills add`.[^llm] The audit runs six phases: reconnaissance; a parallel hunt across eight to twelve or more agents, each assigned an attack class; adversarial validation, where separate agents attempt to disprove each finding; a report phase; a schema-validated `findings.json` output; and an independent phase-six verification against the source code. The methodology is language-agnostic, with domain playbooks for memory-safety and binary code, AI/LLM systems, web-protocol and authentication issues, and client-side code. It may optionally build and run code to confirm a finding. Output combines a human-readable report with a SARIF-like custom JSON schema carrying payloads and reproduction steps rather than standalone exploits. Semgrep's summary states the point directly: the intelligence is whatever model hosts the skill, and the skill itself is the methodology.

## Deployment shape

As a skill rather than a pipeline, security-audit-skill deploys as a prompt pack inside Claude Code and brings no model calls of its own. It uses the host agent's model and inherits whatever sandbox the host agent has, running unattended only as far as the driving agent allows.[^llm]

## Positioning

The Phase 3 adversarial-validation step is one of four named instances of adversarial validation across Semgrep's comparison, alongside VVAH's S6, Trail of Bits' `fp-check`, and defending-code-harness's fresh-container grader. Semgrep recommends the skill to a team already working inside Claude Code that wants a rigorous, adversarially-validated audit methodology with near-zero setup and no new infrastructure.

## See Also

- [[cloudflare|Cloudflare]] — publisher.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.
- [[semgrep|Semgrep]] — publisher of the source.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
