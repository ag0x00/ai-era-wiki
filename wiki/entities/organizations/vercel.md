---
type: entity
title: "Vercel"
address: c-000338
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
homepage: "https://vercel.com"
role: "Frontend cloud and hosting platform company; deepsec is published under its Vercel Labs GitHub organization."
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[deepsec]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# Vercel

**Sources:** [Vercel (homepage)](https://vercel.com) · [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].

## Identity and role

Vercel is a frontend cloud and hosting platform company. [[semgrep|Semgrep]]'s July 2026 survey of open-source AI code-security harnesses attributes deepsec to "Vercel Labs," published under the `vercel-labs` GitHub organization.

## Relevance to This Wiki

deepsec is a SAST+LLM hybrid vulnerability scanner with strong web and application-framework coverage, Next.js and Django named explicitly, plus infrastructure-as-code support, matching Vercel's own hosting focus.[^llm] Semgrep records the project at about 5K GitHub stars, the second-highest count among the nine projects compared.[^stars] The source's own two accounts of the project's licence disagree: the category list states Apache 2.0, while the capability matrix instead reads "see repo," with a footnote asking readers to confirm the licence directly. See [[deepsec|deepsec]] for the disagreement.

Semgrep recommends deepsec to a Next.js or Vercel shop specifically: cheap regex-first triage, an AI second look, and a `--diff` mode for pull-request review, at the cost of remaining static-only.

## Outputs / Products

- **deepsec** — regex prefilter, then AI investigation, then revalidation; multi-backend (default Codex GPT-5.5, or Claude Opus 4.8, or Pi); sandboxed locally with bubblewrap or Seatbelt, or via Vercel microVMs for distributed runs.[^llm]

## See Also

- [[deepsec|deepsec]] — the product covered here.
- [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]] — source.

[^stars]: Star count as reported by Semgrep in its July 2026 survey; a point-in-time figure.
[^llm]: From Semgrep's LLM-generated summary of the repository, not the survey's human-written body.
