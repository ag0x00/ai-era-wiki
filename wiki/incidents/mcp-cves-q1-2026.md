---
type: incident
title: "MCP CVEs Q1 2026"
created: 2026-04-30
updated: 2026-04-30
tags:
  - incidents
  - mcp
  - cves
  - rolling
status: developing
scope_axis:
  - sec-of-ai
incident_class: mcp-vuln
attack_with_or_on_ai: "on AI"
date_observed: 2026-01-01
date_disclosed: 2026-02-29
target: "Model Context Protocol implementations across the ecosystem"
threat_actor: "n/a (vulnerability class, not single actor)"
impact: "30+ MCP-related CVEs filed January–February 2026; 82% of 2,614 surveyed MCP implementations vulnerable to path traversal."
related:
  - "[[mcp-security]]"
  - "[[sandworm-mode-npm-worm]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[cosai]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# MCP CVEs Q1 2026

## Summary

January and February 2026 saw a wave of **30+ MCP-related CVEs** disclosed across the ecosystem. A surveying-style analysis of 2,614 MCP implementations found that **82% were vulnerable to path traversal**. This is a "vulnerability-class" incident — a flood of disclosures rather than a single named campaign — and is best tracked as a rolling page that gets new entries as more CVEs land.

## Causes of endemic path traversal

MCP servers commonly expose filesystem-rooted operations as tools. Many implementations did not properly sanitize paths supplied by the AI agent, allowing the AI agent (or whatever was driving its prompts) to escape intended directories. This is the most basic of input-validation failures, but it's representative: MCP implementations were rushed and lack the hardening that mature web servers and registries built up over decades.

## Defensive Lessons

- **MCP has a security taxonomy now**: see CoSAI's January 27, 2026 [[cosai|MCP Security White Paper]] — nearly 40 threats across 12 categories (identity/access, input validation, data protection, supply chain, guardrails, systems security enforcement).
- **Treat every MCP server as untrusted** by default. A signed-server provenance system + capability scoping + rooted filesystem primitives ("rooted file system access that is impossible to misuse") addresses most of the observed CVEs.
- This page is **rolling** — append new MCP CVE entries as they land. Consider creating per-CVE pages once anything material crosses ~5 CVEs in a quarter.

## Sources
- See frontmatter.
