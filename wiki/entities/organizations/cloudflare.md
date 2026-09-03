---
type: entity
entity_type: organization
org_type: vendor
title: "Cloudflare"
address: c-000090
created: 2026-05-22
updated: 2026-09-01
tags:
  - entities
  - organizations
  - glasswing
  - ai-vuln-discovery
  - ai-in-sec-defense
  - critical-infrastructure
status: developing
scope_axis:
  - ai-in-sec-defense
website: "https://www.cloudflare.com/"
homepage: "https://www.cloudflare.com"
role: "Project Glasswing partner — applied Claude Mythos Preview to its critical-path systems and reported 2,000 bugs found at a false-positive rate better than human testers — and publisher of security-audit-skill, an open-source six-phase adversarially-validated audit methodology"
related:
  - "[[glasswing]]"
  - "[[mythos]]"
  - "[[anthropic-glasswing-initial-update]]"
  - "[[security-audit-skill|security-audit-skill]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
sources:
  - "[[anthropic-glasswing-initial-update]]"
  - "https://blog.cloudflare.com/cyber-frontier-models/"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# Cloudflare

**Sources:** [Cloudflare (homepage)](https://www.cloudflare.com) · [Cyber Frontier Models blog post](https://blog.cloudflare.com/cyber-frontier-models/)

Cloudflare is an internet-infrastructure and security company and a partner in [[glasswing|Project Glasswing]]. Its inclusion among the ~50 Glasswing partners places it in the coalition's extended membership beyond the [[anthropic-glasswing-announcement|twelve named launch partners]].

## Glasswing Result (One Month In)

Per [[anthropic-glasswing-initial-update|Anthropic's one-month update]] and Cloudflare's own [Cyber Frontier Models post](https://blog.cloudflare.com/cyber-frontier-models/), Cloudflare used [[mythos|Claude Mythos Preview]] to find **2,000 bugs** across its critical-path systems, of which **400 were high- or critical-severity**, at a **false-positive rate the Cloudflare team considers better than human testers**.

**FP rate "better than human testers".** Cloudflare's false-positive-rate claim is one of the clearest first-party defender statements that frontier-AI vulnerability discovery has crossed the precision threshold where it competes with skilled human review on both recall and precision. It corroborates the [[frontier-ai-for-vuln-discovery|harness-over-model precision argument]] from a deploying enterprise rather than a model vendor.

## Open-Source Security Methodology

Cloudflare also publishes a security-audit methodology as open source. [[security-audit-skill|`security-audit-skill`]] is an MIT-licensed six-phase multi-agent audit with adversarial validation, distributed as a skill a coding agent installs, and Semgrep's July 2026 survey reports it at ~2K stars.[^semgrep] The phases run reconnaissance, a parallel hunt across attack classes, adversarial validation in which separate agents attempt to disprove each finding, a report, a schema-validated findings file, and an independent verification pass against source; the skill carries the methodology and the host agent supplies the model. Semgrep recommends it to a team already working inside a coding agent that wants a rigorous audit method with no new infrastructure.

## See Also

- [[glasswing|Project Glasswing]] — the coalition.
- [[anthropic-glasswing-initial-update|Glasswing initial update]] — source for the result above.
- [[mythos|Claude Mythos Preview]] — the model deployed.
- [[security-audit-skill|security-audit-skill]] — Cloudflare's open-source audit methodology.

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The licence, star count and six-phase framing are human-written; the per-phase detail is from Semgrep's LLM-generated repository summary. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
