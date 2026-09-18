---
type: entity
entity_type: organization
org_type: vendor
title: "Manifold Security"
created: 2026-09-17
updated: 2026-09-17
tags:
  - entities
  - organization
  - vendor
  - runtime-protection
  - coding-agent
  - vulnerability-research
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
role: "Agent runtime-protection vendor and publisher of the GitSpawn coding-agent disclosures"
homepage: "https://www.manifold.security"
first_mentioned: "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
related:
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
  - "[[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]"
  - "[[agent-runtime-protection-canvass-2026-09|Agent Runtime Protection Market Canvass]]"
  - "[[vulncheck|VulnCheck]]"
  - "[[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]]"
  - "[[claude-code|Claude Code]]"
  - "[[hermes-agent|Hermes]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]"
sources:
  - "https://www.manifold.security"
  - "https://www.manifold.security/blog/ai-coding-agents-git-hijack"
  - "https://www.cve.org/CVERecord?id=CVE-2026-71963"
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified: 2026-09-17
verified_against:
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified_findings: 0
verified_note: "Vendor claims re-attributed to Manifold; the superlative about wiki-wide breadth was removed. Product capability remains ungraded."
---

# Manifold Security

**Sources:** [Manifold Security (homepage)](https://www.manifold.security) · [GitSpawn disclosure, 2026-09-01](https://www.manifold.security/blog/ai-coding-agents-git-hijack) · [CVE-2026-71963](https://www.cve.org/CVERecord?id=CVE-2026-71963)

## Identity and role

Security vendor selling runtime observation of AI agent behaviour. It has also published original vulnerability research against coding agents, credited on the [[gitspawn-coding-agent-git-config-rce|GitSpawn]] disclosure to Francisco Rosales, an offensive security engineer at the company.

## Relevance to this wiki

GitSpawn is eight findings across seven CLI coding agents, published on 2026-09-01 under one mechanism: a context-gathering `git` subprocess that executes a program the repository's own configuration names. It covers [[claude-code|Claude Code]], Goose, Qwen Code, Grok Build, [[hermes-agent|Hermes]], OpenAI Codex and [[cursor-ide|Cursor]], and it produced two CVE identifiers, one of them assigned by [[vulncheck|VulnCheck]] after the vendor left the report untriaged. For a wider agent population under a different flaw class, see the [[guardfall-shell-injection-audit|GuardFall audit]], which surveyed eleven open-source coding agents in June 2026.

The company reports that it withheld the configuration key behind one unpatched finding and published no proof-of-concept repository, giving a sink, a trigger and a screen recording per finding instead.

## Positions

Manifold's stated product thesis is that endpoint detection sees familiar developer tooling behaving familiarly and a gateway sees authenticated traffic it already permits, so neither instrument carries a view of what an agent decided to do. That argument is a vendor pitch as well as a research finding, and it belongs to the instrumentation camp catalogued at [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]. The GitSpawn class is the sharper form of the same claim, because the executing subprocess is the agent's own rather than one the model requested.

The company also generalizes the finding past git: skills, MCP servers and plugins arrive as files, carry their own configuration and are trusted on arrival for the reason a copied repository is.

> [!gap] No independent assessment
> Nothing here describes Manifold's product beyond its own marketing copy. The company is absent from [[agent-runtime-protection-canvass-2026-09|the September 2026 runtime-protection canvass]], which graded twenty-one products against the [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] L4 capability set, so its coverage of those capabilities is ungraded. Funding, headcount and customer base are unrecorded.

## Outputs

| Date | Output | Form |
|---|---|---|
| 2026-09-01 | [[gitspawn-coding-agent-git-config-rce\|GitSpawn]] | Eight coding-agent code-execution findings; two CVEs |
