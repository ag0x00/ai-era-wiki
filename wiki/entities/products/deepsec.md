---
type: entity
entity_type: product
title: "deepsec"
address: c-000333
created: 2026-08-31
updated: 2026-09-01
tags:
  - entities
  - products
  - sast
  - vuln-discovery
  - web-security
  - sandboxing
  - ai-in-sec-defense
  - sec-of-ai
status: seed
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
origin: aggregated
vendor: "Vercel Labs"
license: "Disputed in source — see body"
role: "Web/app-stack SAST+LLM hybrid: a 198-matcher regex prefilter finds candidate sites, an AI investigation loop reasons over context and auth boundaries, and a revalidation pass cuts false positives."
homepage: "https://github.com/vercel-labs/deepsec"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[vercel]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# deepsec

**Sources:** [GitHub — vercel-labs/deepsec](https://github.com/vercel-labs/deepsec) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

deepsec is a SAST+LLM hybrid vulnerability scanner published under Vercel Labs, attributed by [[semgrep|Semgrep]]'s July 2026 survey to [[vercel|Vercel]]. The survey records about 5K GitHub stars, the second-highest count among the nine projects compared.[^stars]

> [!contradiction] The source disagrees with itself on deepsec's licence
> Semgrep's human-written category list states deepsec is Apache 2.0. The LLM-generated capability-matrix row instead reads "see repo," with a footnote directing readers to confirm the licence in the repository before relying on it.[^llm] Neither statement is preferred here; the disagreement is recorded rather than resolved.

## Mechanism

Semgrep's LLM-generated summary describes a TypeScript monorepo of roughly 37K lines of code.[^llm] A 198-matcher regex prefilter, gated by an auto-detected technology stack, finds candidate sites for free; an AI investigation loop then reasons over context and authorization boundaries; a revalidation pass cuts false positives by about 50%.[^llm] Agent tools are read-only, so the tool performs no code execution. Backends are multi-provider: the default is Codex GPT-5.5, with Claude Opus 4.8 or Pi (GLM 5.2) as alternatives.

## Isolation

deepsec sandboxes its read-only agent tools with bubblewrap or Seatbelt for local runs, and with Vercel microVMs for distributed runs.[^llm] The capability matrix lists this same bubblewrap/Seatbelt/microVM combination as its isolation entry — one of the few tools in the comparison whose sandbox choice depends on where the scan runs rather than being fixed at build time.[^llm]

## Output and positioning

deepsec covers web and application frameworks (Next.js, Django) plus infrastructure as code, across ten or more languages, with a `--diff` mode for pull-request review and per-batch token-cost reporting.[^llm] It does not yet produce SARIF; findings export as JSON or Markdown, and the tool produces no patches.[^llm] In the comparison's "finding" table, a deepsec finding is a re-validated static match; in the execution table, it does not execute code, produce a proof of concept, or produce a patch. Semgrep recommends deepsec to a Next.js or Vercel shop: cheap regex-first triage, an AI second look, and a PR-diff mode, at the cost of staying static-only.

## See Also

- [[vercel|Vercel]] — publisher, via Vercel Labs.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.
- [[semgrep|Semgrep]] — publisher of the source.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
