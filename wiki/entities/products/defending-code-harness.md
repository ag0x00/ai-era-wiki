---
type: entity
entity_type: product
title: "defending-code-harness"
address: c-000332
created: 2026-08-31
updated: 2026-09-01
tags:
  - entities
  - products
  - exploitgen
  - vuln-discovery
  - memory-safety
  - sandboxing
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
  - sec-of-ai
status: seed
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
  - sec-of-ai
origin: aggregated
vendor: "Anthropic (original, unmaintained), Semgrep (active fork)"
license: "Apache 2.0"
role: "Agentic, execution-verified harness for C/C++ memory-safety bugs. Agents craft crashing inputs; a find counts only once it reproduces under AddressSanitizer, and a patch agent produces a fix verified compile-to-survives-re-attack."
homepage: "https://github.com/semgrep/defending-code-harness"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[anthropic]]"
  - "[[semgrep]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# defending-code-harness

**Sources:** [GitHub — semgrep/defending-code-harness (active fork)](https://github.com/semgrep/defending-code-harness) · [GitHub — anthropics/defending-code-reference-harness (original, unmaintained)](https://github.com/anthropics/defending-code-reference-harness) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

defending-code-harness is the sole project [[semgrep|Semgrep]]'s July 2026 survey places in the LLM-led exploitgen category: a harness that drives to a crashing end-state as an oracle for vulnerabilities, what Semgrep frames as a new form of fuzzing. [[anthropic|Anthropic]] published the original repository, `defending-code-reference-harness`; the survey states that original is unmaintained and points to a Semgrep-maintained fork, `semgrep/defending-code-harness`, as the active version. Both are Apache 2.0. Semgrep records the combined project at about 6K GitHub stars, tied with [[trail-of-bits-skills|trailofbits/skills]] for the highest count of the nine projects compared.[^stars]

## Mechanism

Semgrep's LLM-generated summary describes an agentic, execution-verified harness scoped narrowly to C/C++ memory-safety bugs.[^llm] Agents craft malicious inputs against the target; a "find" counts only once it crashes under AddressSanitizer and reproduces three times out of three inside an isolated container. An independent grader then re-confirms the crash in a fresh container, an LLM judge deduplicates findings semantically, and a patch agent produces a fix verified against a four-rung ladder: the patch compiles, the original proof of concept stops reproducing, the test suite passes, and the fix survives a renewed attack attempt. Semgrep credits the narrow scope with producing verifiable findings: every one is a reproduced crash.

## Isolation and containment

The harness runs inside a gVisor sandbox with an egress allowlist restricting outbound network access.[^llm] That boundary is load-bearing: the agents generating and executing exploit code run inside it, and the fresh-container re-confirmation step exists to keep a crash that reproduced by accident, in a compromised or flaky container, from being reported as a finding. Semgrep's capability matrix lists gVisor plus egress control as the harness's isolation mechanism, one of only two entries in the comparison (with [[raptor|RAPTOR]]) where the tool both executes untrusted code and states an explicit sandbox for doing so.[^llm]

## Capability matrix

Semgrep's capability matrix scopes the harness to C/C++ memory safety, with SARIF and patch output, and lists Claude Opus and Sonnet as the default models.[^llm] In the comparison's "finding" table, a defending-code-harness finding is a reproducible crash under ASan; in the execution table, it is one of two tools, alongside RAPTOR, that execute code, produce a proof of concept, and produce an execution-verified patch.

## Positioning

Model guardrails against generating working exploits make exploitgen the hardest of Semgrep's three categories to use outside a trusted-access or cyber-verification exemption, and defending-code-harness is the survey's only member of that category. Semgrep recommends it to a C/C++ maintainer who wants high-confidence, exploitable findings with verified patches.

## See Also

- [[anthropic|Anthropic]] — publisher of the original, unmaintained repository.
- [[semgrep|Semgrep]] — publisher of the maintained fork and of the source survey.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure given for the combined project rather than split between the original and the fork.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
