---
type: entity
entity_type: product
title: "OpenClaw"
address: c-000268
created: 2026-08-15
updated: 2026-08-15
tags:
  - entities
  - products
  - agentic-framework
  - open-source
  - dual-use
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-offense
origin: aggregated
related:
  - "[[hermes-agent|Hermes]]"
  - "[[clawhavoc|ClawHavoc]]"
  - "[[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack|Taiwan Multi-Agent Attack Reconstruction]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[offensive-agent-collective|Offensive Agent Collective]]"
sources:
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
  - "https://www.reuters.com/world/china/taiwan-says-it-was-targeted-last-month-ai-driven-hacking-campaign-2026-08-13/"
---

# OpenClaw

Open-source, self-hostable agentic coding and task-execution platform with its own skill marketplace, ClawHub. The wiki's most frequently cited open-source agent ecosystem — used as a kill-chain comparator alongside Claude Code, Gemini Workspace, and Microsoft Enterprise Copilot in [[your-agent-works-for-me-now-talk|Rehberger's Unprompted 2026 demonstration]], and the origin of an entire third-party security-tooling ecosystem (RAGShield, TrustRAG, SecureClaw, Brain Git, Aguara Watch) classified `Exploratory` in the [[agentic-ai-security-reference-architecture|reference architecture]]. Its marketplace was the target of [[clawhavoc|ClawHavoc]], a January–February 2026 supply-chain campaign publishing malicious skills at scale (figures on the incident page).

## Relevance to This Wiki

Prior wiki coverage treated OpenClaw as production developer-agent infrastructure and, via ClawHavoc, as a supply-chain attack *surface*. The [[dream-taiwan-multi-agent-ai-attack|Dream Security reconstruction]] adds a distinct role: OpenClaw as attack *tooling*. Dream found it deployed as one of two orchestration platforms — alongside [[hermes-agent|Hermes]] — underlying the [[taiwan-ai-agent-government-intrusion|Taiwan multi-agent AI intrusion]], operating under the workspace identifier `.openclaw`. Taiwan's Ministry of Digital Affairs independently named "Open Claw" in its own public statement on the same incident, corroborating Dream's account from the victim side.[^reuters]

This is the first sourced instance of OpenClaw itself, rather than a marketplace skill or a downstream tool built on it, functioning as offensive orchestration infrastructure — the same dual-use property that makes it valuable as production developer tooling (autonomous multistep task execution, tool-calling, persistent workspaces) is what an operator repurposed here. No page in this wiki previously carried an OpenClaw entity page despite the ecosystem's frequency of mention; this page is created now to anchor that dual role.

## Notes

[^reuters]: [Taiwan says it was targeted last month in AI-driven hacking campaign](https://www.reuters.com/world/china/taiwan-says-it-was-targeted-last-month-ai-driven-hacking-campaign-2026-08-13/), Reuters, 2026-08-13.
