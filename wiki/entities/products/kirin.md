---
type: entity
entity_type: product
title: "Kirin (Knostic)"
created: 2026-04-30
updated: 2026-06-21
tags: [entities, product, ai-security, coding-agents, ide-security]
status: stub
homepage: "https://www.knostic.ai/ai-coding-security-solution-kirin"
producer: "[[knostic]]"
category: "Coding-agent runtime security / governance enforcement"
related:
  - "[[knostic]]"
  - "[[ai-coding-agent-governance]]"
  - "[[mcp-security]]"
  - "[[supply-chain-security-for-agents]]"
  - "[[indirect-prompt-injection]]"
sources:
  - "https://www.knostic.ai/ai-coding-security-solution-kirin"
  - "https://www.knostic.ai/blog/ai-coding-agent-governance"
---

# Kirin (Knostic)

**Sources:** [Kirin (Knostic)](https://www.knostic.ai/ai-coding-security-solution-kirin) · [AI Coding Agent Governance (Knostic)](https://www.knostic.ai/blog/ai-coding-agent-governance)

Coding-agent runtime security and governance enforcement product. Targets Cursor, GitHub Copilot, and other AI-coding IDEs. Per the [[ai-coding-agent-governance|Knostic blog post]], Kirin's stated capabilities:

| Capability | Maps to [[agentic-ai-security-cmm-2026\|Agentic AI Security Capability Maturity Model]] |
|---|---|
| Hidden prompt-injection detection in code/context | **D4 Runtime & Guardrails** |
| Malicious agent rules-file analysis (Cursor / Copilot rules) | **D6 Data, Memory & RAG** (Cognitive File Integrity extension to rules files) |
| Rogue IDE extension detection | **D8 Supply Chain & AI-BOM** |
| Typosquatted package blocking | **D8 Supply Chain & AI-BOM** |
| MCP server validation + CVE checks | **D5 Egress & Network**, **D8 Supply Chain** |
| Destructive agent action blocking | **D3 Control & Least-Agency** |
| Continuous policy enforcement at runtime | **D4 Runtime**, **D7 Observability** |
| Single dashboard tracking MCP usage, rule changes, policy violations | **D7 Observability & Detection** |

## Placement in the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]

A coding-agent-specific overlay that touches the **Runtime**, **Egress**, **Data** (rules files), and **Observability** planes. Conceptually similar to a coding-agent EDR. Closest OSS analogue is [[llamafirewall|LlamaFirewall]] (PromptGuard 2 + AlignmentCheck + CodeShield), which Kirin is not directly comparable to but plays an adjacent role.

## Strengths / Weaknesses

**Strengths**: targets coding-agent-specific threats (rules-file poisoning, IDE extension marketplace, typosquats) that broader AI security platforms don't sharpen on. Single dashboard for MCP/rule/policy view.

**Weaknesses (vendor-marketing-stated, uncorroborated)**: not benchmark-published; no AgentDojo-class evaluation cited; no head-to-head with LlamaFirewall, AgentGateway, or Cursor's own Rulesets enforcement.

## Relations

- Producer: [[knostic|Knostic]]
- Documented in: [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]]
