---
type: entity
title: "Cisco"
address: c-000336
created: 2026-08-31
updated: 2026-09-28
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
homepage: "https://www.cisco.com"
role: "Networking and cybersecurity vendor; Cisco Open publishes ai-deep-sast, a SAST+LLM hybrid vulnerability scanner."
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[ai-deep-sast]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[openai-daybreak]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
  - "https://openai.com/daybreak/partners-new/"
  - "https://openai.com/business/solutions/cybersecurity/"
  - "https://www.anthropic.com/glasswing"
verified: 2026-09-29
verified_against:
  - ".raw/articles/cybersecurity-2026-09-28.md"
verified_findings: 0
verified_note: "verify2, diff-scoped to the retargeted solutions-page footnote; strip and quote match; no findings"
---

# Cisco

**Sources:** [Cisco (homepage)](https://www.cisco.com) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

Cisco is a networking and cybersecurity vendor. Its Cisco Open program publishes `cisco-open/ai-deep-sast` on GitHub, a SAST+LLM hybrid vulnerability scanner named in [[semgrep|Semgrep]]'s July 2026 survey of open-source AI code-security harnesses.

## Relevance to This Wiki

[[ai-deep-sast|ai-deep-sast]] holds about 50 GitHub stars, the fewest of the nine projects in Semgrep's comparison against a range that reaches 6K.[^stars] Semgrep also names it as the only project able to run fully local and offline: an optional mode runs a security-tuned 8B-parameter model, Foundation-Sec-8B, entirely on-device, so no code leaves the machine. Semgrep frames that choice as depth and proof exchanged for breadth and speed, and recommends the tool to teams that need an offline or air-gapped scan over the broadest possible coverage.

Cisco also works with the cyber programmes of both Anthropic and OpenAI. It is one of the twelve named launch partners of [[glasswing|Project Glasswing]],[^glasswing] and one of the twenty partners on the [[openai-daybreak|OpenAI Daybreak]] Defense Network page.[^daybreak-network] OpenAI's cybersecurity solutions page also lists it under "Trusted by leading security organizations".[^daybreak-solutions]

## Outputs / Products

- **ai-deep-sast** — fast SAST scanner combining Semgrep's rule-based detection with LLM triage behind an evidence gate; a fully local fast-scan mode and a frontier-model deep-scan mode; Markdown, JSON, and JUnit output with CI severity gates.[^llm]

## Positioning in the comparison

Semgrep's capability matrix scopes ai-deep-sast to the OWASP/CWE Top 25, secrets, and AI/ML-specific findings across 30 or more languages, with an isolation posture of "n/a (static)" since the tool performs no dynamic execution.[^llm] In the survey's "finding" and execution tables, an ai-deep-sast finding is a triaged static match, and the tool produces no proof of concept or patch, offering advice only.

## See Also

- [[ai-deep-sast|ai-deep-sast]] — the product covered here.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
[^glasswing]: [Anthropic — Project Glasswing](https://www.anthropic.com/glasswing), 2026-05-12: the twelve organizations the initiative brings together, Cisco among them, and the quote from Cisco's SVP and Chief Security & Trust Officer, Anthony Grieco. Local copy: `.raw/articles/anthropic-glasswing-2026-05-13.md`. Summarized at [[anthropic-glasswing-announcement|Project Glasswing: Securing Critical Software]].
[^daybreak-network]: [OpenAI — Daybreak Defense Network](https://openai.com/daybreak/partners-new/), undated, fetched 2026-09-28: the twenty listed partners. Local copy: `.raw/articles/openai-daybreak-defense-network-2026-09-28.md`.
[^daybreak-solutions]: [OpenAI — AI for Cybersecurity Teams](https://openai.com/business/solutions/cybersecurity/), undated, fetched 2026-09-28: the "trusted by" strip. Local copy: `.raw/articles/cybersecurity-2026-09-28.md`. Summarized at [[openai-daybreak|OpenAI Daybreak]].
