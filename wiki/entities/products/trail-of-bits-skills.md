---
type: entity
entity_type: product
title: "trailofbits/skills"
address: c-000335
created: 2026-08-31
updated: 2026-09-01
tags:
  - entities
  - products
  - skill-boosting
  - vuln-discovery
  - claude-code
  - cryptography
  - ai-in-sec-defense
status: seed
scope_axis:
  - ai-in-sec-defense
origin: aggregated
vendor: "Trail of Bits"
license: "CC-BY-SA 4.0"
role: "Marketplace of about 40 plugins, roughly 15 security-focused, for Claude Code and Codex. Distills specialist security-consultant knowledge into composable reviewer skills."
homepage: "https://github.com/trailofbits/skills"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[trail-of-bits]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# trailofbits/skills

**Sources:** [GitHub — trailofbits/skills](https://github.com/trailofbits/skills) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

trailofbits/skills is a marketplace of Claude Code and Codex plugins published by [[trail-of-bits|Trail of Bits]], placed by [[semgrep|Semgrep]]'s July 2026 survey in the LLM-skill-boosting vulnerability-research category. The survey records about 6K GitHub stars under a CC-BY-SA licence, tied with defending-code-harness for the highest count of the nine projects compared.[^stars]

## Contents

Semgrep's LLM-generated summary counts roughly 40 plugins, about 15 of them security-focused.[^llm] Two are named as standouts: `c-review` and `rust-review` use a deterministic Python planner to orchestrate parallel Claude workers running haiku, sonnet, and opus models with shared prompt caching, then run sequential deduplication and false-positive judges, emitting SARIF 2.1.0. `static-analysis` wraps Semgrep and CodeQL. `constant-time-analysis` and `zeroize-audit` drop to assembly or LLVM IR to catch cryptographic timing leaks and compiler-eliminated secret wiping. `fp-check` and `variant-analysis` are triage and expansion methodologies rather than scanners. Semgrep notes the collection benches deep on native code, cryptography, and smart contracts across six blockchains, and describes the marketplace's strength as breadth and domain depth: a toolbox of composable reviewers, distinct from the standalone pipelines elsewhere in the comparison.

## Isolation

Most of the plugins are read-only, inheriting whatever sandbox the host Claude Code or Codex agent provides.[^llm] Semgrep's footnote on the capability matrix names an exception: certain skills, including `constant-time-analysis` and `zeroize-audit`, compile or inspect assembly rather than only reading source.[^llm]

## Positioning

Trail of Bits' `fp-check` is one of four named instances of adversarial validation in Semgrep's comparison, alongside Cloudflare's Phase 3, VVAH's S6, and defending-code-harness's fresh-container grader. Semgrep recommends the marketplace to a team that wants a toolbox of specialist reviewers, spanning native code, cryptography, and smart contracts, to compose into its own workflow inside Claude Code or Codex.

## See Also

- [[trail-of-bits|Trail of Bits]] — publisher.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.
- [[semgrep|Semgrep]] — publisher of the source; `static-analysis` wraps Semgrep's own engine.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
