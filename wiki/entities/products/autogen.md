---
type: entity
entity_type: product
title: "AutoGen"
created: 2026-05-04
updated: 2026-06-21
tags:
  - products
  - agent-framework
  - oss
  - multi-agent
  - python
  - microsoft
status: seed
license: MIT
vendor: "Microsoft"
homepage: "https://github.com/microsoft/autogen"
related:
  - "[[ai-agents-are-here-so-are-the-threats-unit42]]"
  - "[[crewai]]"
  - "[[microsoft]]"
  - "[[mcp-security]]"
sources:
  - "https://github.com/microsoft/autogen"
  - "https://microsoft.github.io/autogen"
  - "[[.raw/articles/agentic-ai-threats-unit42-2025-05-01.md]]"
---

# AutoGen

**Sources:** [AutoGen (repo)](https://github.com/microsoft/autogen) · [AutoGen docs](https://microsoft.github.io/autogen) · [[ai-agents-are-here-so-are-the-threats-unit42|Unit 42 — AI Agents Are Here. So Are the Threats.]]

> [!gap] Stub
> Open-source Python multi-agent framework from [[microsoft|Microsoft]] Research. Provides primitives for conversational, autonomous, and human-in-the-loop multi-agent applications; integrates with Azure OpenAI Service, OpenAI, and other model providers. Documents a [Swarm](https://microsoft.github.io/autogen/dev//user-guide/agentchat-user-guide/swarm.html) pattern where agents `transfer_to_*` each other for task handoff.
>
> **Wiki relevance:** One of the two frameworks Unit 42 used to demonstrate framework-agnostic agentic-AI vulnerabilities in [[ai-agents-are-here-so-are-the-threats-unit42|"AI Agents Are Here. So Are the Threats."]] (May 2025). All nine attack scenarios in that study work identically against AutoGen and [[crewai|CrewAI]] — the article explicitly notes neither framework is inherently vulnerable; the vulnerabilities arise from insecure design and tool integration, not the framework itself.
>
> **Transfer-tool naming convention**: AutoGen agents in the Swarm pattern expose handoff tools prefixed `transfer_to_*` — a naming convention the Unit 42 attack scenario #1 (identifying participant agents) directly exploits to enumerate the multi-agent topology. The same delegation channel is what allows attackers to reach internal agents through the user-facing orchestrator (mapping to OWASP ASI07/08/10).
>
> Pending content: full architecture overview, AgentChat / Magentic / Swarm pattern comparison, Azure-specific deployment guidance, security configuration guide, MCP integration status, comparison with CrewAI and other frameworks, production deployment evidence.
