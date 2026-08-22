---
type: entity
title: "Endor Labs AI Code Governance"
address: c-000247
created: 2026-07-30
updated: 2026-07-30
tags:
  - entities
  - product
  - tool
  - governance
  - agentic-coding
  - cots
status: seed
scope_axis:
  - sec-of-ai
entity_type: product
role: "Commercial control plane for AI coding agents: inventories which agentic IDEs, versions, MCP servers, skills, and hooks are in use across an organization; traces MCP calls, prompts, and skill executions back to the agent and the human behind them; and enforces real-time allow/block policy across shell commands, file access, MCP tool calls, prompts, and skills. Named support for Claude Code and Cursor."
homepage: https://www.endorlabs.com/ai-coding-agent-governance
vendor: "[[endor-labs|Endor Labs]]"
related:
  - "[[agentic-ai-security-cmm-d2-identity|D2 Identity & Authorization]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[ai-spm|AI Security Posture Management]]"
  - "[[ai-bom|AI-BOM]]"
  - "[[shadow-automation|Shadow Automation]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[agentshield|AgentShield]]"
  - "[[mcp-security|MCP Security]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|D8 Supply Chain & AI-BOM]]"
  - "[[ai-coding-agent-governance|AI Coding Agent Governance]]"
sources:
  - https://www.endorlabs.com/ai-coding-agent-governance
  - https://www.endorlabs.com/use-cases/ai-code-governance
---

# Endor Labs AI Code Governance

**Product page:** [AI Coding Agent Governance](https://www.endorlabs.com/ai-coding-agent-governance)

A commercial governance layer for organizations that have deployed agentic coding harnesses and cannot answer which ones, where, connected to what. It is an early instance of a COTS category that did not exist a year ago: the agent-side control plane for the generative-coding deployment shape.

## Control Surface

**Inventory.** Which agentic IDEs are in use — [[cursor-ide|Cursor]] and [[claude-code-security|Claude Code]] are named — with version and session counts; connected MCP servers with usage; skills and hooks with risk scores.

**Attribution.** MCP calls, prompts, and skill executions traced back to the agent and the human operator behind each action. This is the control that closes the attribution gap: an organization that cannot distinguish agent-authored from human-authored change cannot scope a scanning policy, trace a vulnerability to its origin, or measure which tool introduced what.

**Enforcement.** Real-time block or alert across five surfaces — shell commands (destructive operations, reverse shells), file access (`.env`, `.pem` reads), MCP tool calls (`DROP`, `DELETE`), prompts (injection patterns, API-key leakage), and agent skills. Policies are built-in or operator-defined by regular expression.

**Provenance.** The adjacent AI Code Governance capability signs artifacts cryptographically for admission control and traceability, and reviews every pull request.

Marketed as available today; no dated GA announcement is published.

## Assessment

The inventory and attribution halves are the durable contribution. They address the gap that [[shadow-automation|shadow automation]] names from the [[ai-coding-agent-governance|governance side]] and [[harness-config-as-supply-chain-artifact|harness config as supply-chain artifact]] names from the audit side, and no first-party harness feature covers it across vendors. Anthropic ships two analytics products rather than one — the Enterprise Analytics API for claude.ai organizations, which is not available on the Teams plan, and the Claude Code Analytics API for Console organizations — and neither sees sessions routed through Bedrock, Vertex, or Foundry.

The enforcement half has a stated design that limits it. Regular-expression policy over shell commands is precisely the construction the [[guard-canonicalization-gap|guard canonicalization gap]] describes: the pattern inspects a string the shell will rewrite. The [[guardfall-shell-injection-audit|GuardFall audit]] found ten of eleven such implementations bypassable. This does not make the product's enforcement worthless — a blocked `rm -rf` still blocks the unsophisticated case, and the alerting is genuine detective value — but it means the enforcement should be scored as detective and advisory rather than preventive, and layered above an OS boundary rather than instead of one.

> [!gap] Vendor-claimed, not independently evaluated
> Every capability above comes from vendor marketing material. The wiki has no independent evaluation, no efficacy measurement, and no peer product assessed on the same criteria. A second sourced instrument in this category is the precondition for treating "agent control plane" as a settled control rather than a vendor claim.

## Placement

Spans the control, observability, and supply-chain concerns of the [[agentic-ai-security-reference-architecture|AAI-S RA]]. Its inventory output is the direct evidence artifact for [[agentic-ai-security-cmm-d8-supply-chain|D8]] extended to harness configuration, and its attribution output feeds [[agentic-ai-security-cmm-d7-observability|D7]] for the coding-agent shape.

[[agentic-ai-security-cmm-d2-identity|D2]] states the assessable form of the gap this product targets: whether a given commit, tool call, or MCP invocation is traceable to both an agent identity *and* the human accountable for it. Per-agent identity is necessary and not sufficient for that test, which is the specific claim the attribution half of this product is sold against.
