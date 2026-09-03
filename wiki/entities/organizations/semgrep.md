---
type: entity
entity_type: organization
org_type: vendor
title: "Semgrep"
address: c-000270
created: 2026-08-15
updated: 2026-09-01
tags:
  - entities
  - organization
  - vendor
  - code-security
status: developing
scope_axis:
  - ai-in-sec-offense
  - ai-in-sec-defense
  - sec-against-ai
origin: aggregated
homepage: "https://semgrep.dev"
role: "Code security company; author of the first comparative survey of open-source AI code-security harnesses (July 2026), maintainer of the fork of Anthropic's unmaintained reference harness, and supplier of the static-analysis engine several surveyed harnesses run as input"
related:
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
  - "[[reuters-taiwan-ai-hacking-campaign|Taiwan Confirms AI-Agent Hacking Campaign (Reuters)]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
  - "[[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]"
  - "[[defending-code-harness|defending-code-harness]]"
  - "[[anthropic|Anthropic]]"
sources:
  - "https://www.reuters.com/world/china/taiwan-says-it-was-targeted-last-month-ai-driven-hacking-campaign-2026-08-13/"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# Semgrep

Code security company ([semgrep.dev](https://semgrep.dev)). Semgrep researcher Cris Thomas, quoted in Reuters' coverage of the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]], supplied the wiki's operative caution against reading a "near-autonomous" attack framework as fully autonomous: *"There's still a human in there somewhere. Somebody had to choose who to attack, had to establish an objective and give it a directive. It's not totally 100% autonomous."*[^reuters]

## Comparative Harness Survey (July 2026)

In July 2026 Semgrep published a comparison of nine open-source harnesses that point a language model at a codebase, sorting them into LLM-led exploit generation, LLM-skill-boosting vulnerability research, and SAST-plus-LLM hybrids, and setting out six cross-cutting findings on discovery-validation separation, the blurring static/dynamic line, adversarial validation, language coverage, what each harness counts as a finding, and how uncommon patch generation remains. Semgrep answers the market-consolidation question directly — a reference open-source harness will not emerge today, and many organizations will build their own "shop jigs" for vulnerability finding — and names that pace as the reason harnesses ship marked unmaintained or closed to external contributions. Semgrep's own engine appears inside the field it surveys: `ai-deep-sast` triages Semgrep findings with a language model, RAPTOR runs Semgrep alongside CodeQL, and Trail of Bits' `static-analysis` skill wraps both.[^semgrep] Semgrep also maintains `semgrep/defending-code-harness`, a fork of [[anthropic|Anthropic]]'s `anthropics/defending-code-reference-harness`, which the survey reports as unmaintained. See [[semgrep-oss-ai-security-harness-comparison|the paper page]] for the full nine-project comparison and [[oss-ai-vuln-discovery-harness-landscape|the open-source harness landscape]] for the per-project detail.

## Notes

[^reuters]: [Taiwan says it was targeted last month in AI-driven hacking campaign](https://www.reuters.com/world/china/taiwan-says-it-was-targeted-last-month-ai-driven-hacking-campaign-2026-08-13/), Reuters, 2026-08-13.
[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The taxonomy, the six findings and the market-consolidation argument are human-written; the `static-analysis` skill's description is from Semgrep's LLM-generated repository summary. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
