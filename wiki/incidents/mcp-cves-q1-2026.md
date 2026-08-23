---
type: incident
title: "MCP CVEs Q1 2026"
created: 2026-04-30
updated: 2026-08-22
tags:
  - incidents
  - mcp
  - cves
  - snapshot
status: superseded
scope_axis:
  - sec-of-ai
incident_class: mcp-vuln
attack_with_or_on_ai: "on AI"
date_observed: 2026-01-01
date_disclosed: 2026-02-28
target: "Model Context Protocol implementations across the ecosystem"
threat_actor: "n/a (vulnerability class, not single actor)"
impact: "30+ MCP-related CVEs filed January–February 2026. Contemporaneous static analysis of 2,614 MCP implementations found 82% calling file-system APIs prone to path traversal — an API-usage rate, not a confirmed-defect rate."
related:
  - "[[mcp-exposure-measurements]]"
  - "[[mcp-security]]"
  - "[[sandworm-mode-npm-worm]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[cosai]]"
  - "[[tool-poisoning]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/articles/endor-labs-mcp-classic-vulnerabilities-2026-01-23.md]]"
---

# MCP CVEs Q1 2026

## Scope

This page is a closed snapshot of the January–February 2026 MCP disclosure wave. It is not maintained as a running tally. Current ecosystem figures — reachable unauthenticated servers, API-usage rates, and confirmed defect rates, each with its own method — live on [[mcp-exposure-measurements|MCP Exposure Measurements]].

## Summary

January and February 2026 saw a wave of **30+ MCP-related CVEs** disclosed across the ecosystem. This was a vulnerability-class event — a flood of disclosures rather than a single named campaign.

Static analysis published in the same window measured how much of the ecosystem's code sat on a risky primitive. Endor Labs analyzed 2,614 MCP implementations and found 82% using file-system operations prone to path traversal (CWE-22), 67% using APIs related to code injection (CWE-94), and 34% using APIs related to command injection (CWE-78).[^endor]

That figure counts sensitive API calls. A file-system call becomes path traversal only where the path escapes canonicalization against a configured boundary, and most are bounded correctly. Trend Micro's later repository sweep put the confirmed-exploitable rate near 4% of open-source MCP repositories, roughly twenty times lower.[^tm-sweep] Both numbers are accurate about different things; [[mcp-exposure-measurements|MCP Exposure Measurements]] sets out which supports which decision.

## Causes of endemic path traversal

MCP servers commonly expose filesystem-rooted operations as tools. Many implementations did not sanitize paths supplied by the AI agent, allowing the agent — or whatever was driving its prompts — to escape intended directories. This is the most basic of input-validation failures, and it is representative: MCP implementations were rushed and lack the hardening that mature web servers and registries built up over decades.

The three CVEs filed against Anthropic's own `mcp-server-git` on 2026-01-20 show the pattern reaching reference code. CVE-2025-68143 and CVE-2025-68145 let the `git_init` tool create repositories at arbitrary paths because the configured `--repository` boundary was never validated; CVE-2025-68144 passed user-controlled arguments to the Git CLI unsanitized.[^endor] Developers were meant to copy that implementation.

## Defensive lessons

- **MCP has a security taxonomy now**: see CoSAI's January 27, 2026 [[cosai|MCP Security White Paper]] — nearly 40 threats across 12 categories (identity/access, input validation, data protection, supply chain, guardrails, systems security enforcement).
- **Treat every MCP server as untrusted** by default. A signed-server provenance system, capability scoping, and rooted filesystem primitives address most of the observed CVEs.
- **The disclosure count undercounts the problem.** An advisory exists only where someone looked and filed. Deployment posture is measured separately, and by April 2026 it was worse than the CVE record implied.

## Position history

- **2026-04-30** — filed as a rolling page, to be appended as new CVEs landed. No entries were appended.
- **2026-08-22** — closed as a dated snapshot. The rolling cadence carried no review interval, so the page served a four-month-old figure with the authority of a current one. Two corrections landed with the closure: the 82% statistic was restated as an API-usage rate, and it was attributed to Endor Labs, which the page had not named. Anyone quoting the earlier wording should re-check it. Current figures moved to [[mcp-exposure-measurements|MCP Exposure Measurements]].

## Notes

[^endor]: Peyton Kennedy, [*Classic Vulnerabilities Meet AI Infrastructure: Why MCP Needs AppSec*](https://www.endorlabs.com/learn/classic-vulnerabilities-meet-ai-infrastructure-why-mcp-needs-appsec), Endor Labs, 2026-01-23, restating figures from the Endor Labs [2025 Dependency Management Report](https://www.endorlabs.com/lp/state-of-dependency-management-2025). Local copy: `.raw/articles/endor-labs-mcp-classic-vulnerabilities-2026-01-23.md`.

[^tm-sweep]: Alfredo Oliveira and David Fiser, [*Hunt Them All: An AI-Powered Vulnerability Sweep of 19,000 MCP Servers*](https://www.trendmicro.com/vinfo/us/security/news/vulnerabilities-and-exploits/hunt-them-all-an-ai-powered-vulnerability-sweep-of-19-000-mcp-servers), Trend Micro, 2026-05-27. Local copy: `.raw/articles/trend-micro-mcp-vulnerability-sweep-2026-05-27.md`.
