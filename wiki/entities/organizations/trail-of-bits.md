---
type: entity
title: "Trail of Bits"
address: c-000339
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
homepage: "https://www.trailofbits.com"
role: "Security research and consulting firm; publisher of trailofbits/skills, a marketplace of Claude Code and Codex security plugins."
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[trail-of-bits-skills]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# Trail of Bits

**Sources:** [Trail of Bits (homepage)](https://www.trailofbits.com) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

Trail of Bits is a security research and consulting firm. It publishes `trailofbits/skills` on GitHub, a marketplace of Claude Code and Codex plugins named in [[semgrep|Semgrep]]'s July 2026 survey of open-source AI code-security harnesses.

## Relevance to This Wiki

[[trail-of-bits-skills|trailofbits/skills]] packages roughly 40 plugins, about 15 of them security-focused, under a CC-BY-SA licence, per Semgrep's LLM-generated summary of the repository.[^llm] Semgrep records the project at about 6K GitHub stars, tied with defending-code-harness for the highest count among the nine projects compared.[^stars] Standouts named in the summary include `c-review` and `rust-review`, which orchestrate parallel Claude workers behind a deterministic Python planner and emit SARIF 2.1.0, and `constant-time-analysis` and `zeroize-audit`, which drop to assembly or LLVM IR to catch cryptographic timing leaks and compiler-eliminated secret wiping. Semgrep describes the collection's strength as breadth and domain depth across native code, cryptography, and smart contracts spanning six blockchains: a toolbox of composable reviewers, distinct from the standalone pipelines elsewhere in the comparison.

Trail of Bits' `fp-check` skill is one of four named instances of adversarial validation in Semgrep's comparison, alongside Cloudflare's Phase 3, VVAH's S6, and defending-code-harness's fresh-container grader.

## Outputs / Products

- **trailofbits/skills** — plugin marketplace for Claude Code and Codex; standouts span native-code review, constant-time and secret-wiping analysis, and triage/expansion methodologies.[^llm]

## See Also

- [[trail-of-bits-skills|trailofbits/skills]] — the product covered here.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
