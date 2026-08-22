---
type: entity
entity_type: organization
org_type: project
title: "AgentCordon (project)"
created: 2026-05-04
updated: 2026-05-23
tags:
  - entities
  - organizations
  - oss
  - agentic-idp
  - credential-management
status: seed
homepage: https://agentcordon.dev
related:
  - "[[agentcordon|AgentCordon (product)]]"
  - "[[credential-proxy-pattern]]"
  - "[[cedar]]"
sources:
  - "https://agentcordon.dev"
  - "https://github.com/agentcordon/agentcordon"
  - "[[.raw/articles/agentcordon-readme-2026-05-04.md]]"
---

# AgentCordon (project)

**Sources:** [Homepage](https://agentcordon.dev) · [GitHub org](https://github.com/agentcordon) · [Repo](https://github.com/agentcordon/agentcordon) · [Discord](https://discord.gg/agentcordon)

## Description

The AgentCordon project ships [[agentcordon|AgentCordon]], a self-hostable open-source Agentic IDP and credential broker for AI agents. License: GPL-3.0. Implementation: Rust. Container distribution: `ghcr.io/agentcordon/agentcordon`. Public domains: `agentcordon.dev` and `getcordoned.sh`.

## Project state (2026-05-04)

- GitHub repo: 4 stars, 0 forks, 7 releases
- License: GPL-3.0
- Funding / commercial entity: not publicly disclosed as of fetch date
- Affiliation: not publicly disclosed; treated here as an independent open-source project

## Rationale for inclusion

AgentCordon is one of the few self-hostable OSS implementations of the [[credential-proxy-pattern|credential proxy pattern]] specifically positioned for AI agents (rather than retrofitted from a generic secrets-management tool). It is also one of the only OSS projects in the 2025–2026 cohort that applies [[cedar|Cedar]] to credential and MCP-tool-call authorization. The project belongs alongside the [[comprehensive-agentic-ai-security-landscape-2026|2025–2026 funding cohort]] as a *non-funded* reference, useful for buy-vs-build comparisons against [[cyberark-conjur|Conjur]], [[okta-for-ai-agents|Okta]], [[runlayer|Runlayer]], and [[helmet-security|Helmet]].

## Outputs

- [[agentcordon|AgentCordon]] — the product (Rust binaries: `agent-cordon-server`, `agentcordon-broker`, `agentcordon` CLI)
- Documentation site at `agentcordon.dev/docs`

## See also

- [[agentcordon|AgentCordon (product page)]]
- [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]]
