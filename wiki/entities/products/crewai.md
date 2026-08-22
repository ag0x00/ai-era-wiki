---
type: entity
entity_type: product
title: "CrewAI"
created: 2026-05-04
updated: 2026-05-04
tags:
  - products
  - agent-framework
  - oss
  - multi-agent
  - python
status: seed
license: MIT
vendor: "CrewAI Inc."
homepage: "https://github.com/crewAIInc/crewAI"
related:
  - "[[ai-agents-are-here-so-are-the-threats-unit42]]"
  - "[[autogen]]"
  - "[[mcp-security]]"
sources:
  - "https://github.com/crewAIInc/crewAI"
  - "https://docs.crewai.com"
  - "[[.raw/articles/agentic-ai-threats-unit42-2025-05-01.md]]"
---

# CrewAI

> [!gap] Stub
> Open-source Python multi-agent framework. Models agents as "crew members" with roles, goals, and tools; orchestration via "crew" hierarchies that delegate tasks between agents. Tool ecosystem includes [SerperDevTool](https://github.com/crewAIInc/crewAI-tools/tree/main/crewai_tools/tools/serper_dev_tool) (Google search), [ScrapeWebsiteTool](https://github.com/crewAIInc/crewAI-tools/tree/main/crewai_tools/tools/scrape_website_tool) (web fetcher), and many others.
>
> **Wiki relevance:** One of the two frameworks Unit 42 used to demonstrate framework-agnostic agentic-AI vulnerabilities in [[ai-agents-are-here-so-are-the-threats-unit42|"AI Agents Are Here. So Are the Threats."]] (May 2025). All nine attack scenarios in that study work identically against CrewAI and [[autogen|AutoGen]] — the article explicitly notes neither framework is inherently vulnerable; the vulnerabilities arise from insecure design and tool integration, not the framework itself.
>
> **Hierarchical delegation pattern**: CrewAI uses a documented [hierarchical process](https://docs.crewai.com/how-to/hierarchical-process) where an orchestrator agent `delegates` tasks to coworker agents. This delegation channel is the load-bearing mechanism the Unit 42 attacks exploit to reach internal agents through the user-facing orchestrator (mapping to OWASP ASI07/08/10 multi-agent threats).
>
> Pending content: full architecture overview, security configuration guide, comparison with AutoGen and other frameworks, MCP integration status, version history, production deployment evidence.
