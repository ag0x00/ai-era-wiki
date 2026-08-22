---
type: entity
title: "Affaan M"
address: c-000056
created: 2026-05-15
updated: 2026-05-15
tags:
  - entities
  - people
status: seed
scope_axis: [sec-of-ai]
entity_type: person
role: "Independent developer; author of AgentShield and maintainer of the Everything Claude Code ecosystem (42K+ stars)."
github: https://github.com/affaan-m
x_handle: affaanmustafa
homepage: https://github.com/affaan-m
first_mentioned: "[[agentshield-announcement|AgentShield README]]"
related:
  - "[[agentshield|AgentShield]]"
  - "[[agentshield-announcement|AgentShield paper page]]"
  - "[[anthropic|Anthropic]]"
sources:
  - https://github.com/affaan-m
  - https://github.com/affaan-m/agentshield
  - https://github.com/affaan-m/everything-claude-code
  - https://x.com/affaanmustafa
  - "[[.raw/articles/agentshield-2026-05-15.md]]"
---

# Affaan M

**Sources:** [GitHub profile](https://github.com/affaan-m) · [AgentShield repository](https://github.com/affaan-m/agentshield) · [Everything Claude Code repository](https://github.com/affaan-m/everything-claude-code) · [@affaanmustafa on X](https://x.com/affaanmustafa)

## Identity

Independent developer working on tooling for the Claude Code ecosystem. Built [[agentshield|AgentShield]] — the agent-configuration security scanner — at the Cerebral Valley × Anthropic Claude Code Hackathon in February 2026. Maintainer of *Everything Claude Code* (ECC), an ecosystem repository the AgentShield README cites at *42K+ stars* at the time of fetch, which provides the distribution channel for the ECC plugin form of AgentShield and for the *ECC Tools* GitHub App and *ECC Tools Pro* commercial tier.

## Relevance to This Wiki

Author of the first open-source scanner the wiki has cataloged whose unit of analysis is the *Claude Code config tree itself*. The design choices encoded in AgentShield — provenance-aware `runtimeConfidence` labeling, cross-file hook-manifest awareness, corpus-gate with prioritized improvement plan, time-bound policy exceptions, stable-fingerprint remediation plans — are documentary precedents for what agent-config audit tooling should look like.

## Outputs

- [[agentshield|AgentShield]] — 102-rule scanner for `.claude/` configurations; CLI / GitHub Action / ECC plugin / GitHub App.
- *Everything Claude Code* (ECC) — distribution ecosystem for Claude Code plugins, including the ECC plugin form of AgentShield and the *ECC Tools* GitHub App.
- *ECC Tools Pro* — paid tier (\$19 / seat / month, Stripe-billed) for org-wide automated repo analysis built on top of AgentShield.

## Notable Statements / Positions

> *"The AI agent ecosystem is growing faster than its security tooling … Developers install community skills, connect MCP servers, and configure hooks without any automated way to audit the security of their setup."*
> — [[agentshield-announcement|AgentShield README]], framing for why agent-config auditing should be a first-class CI step.
