---
type: entity
title: "Security Guidance Plugin"
address: c-000246
created: 2026-07-30
updated: 2026-08-22
tags:
  - entities
  - product
  - tool
  - sast
  - claude-code
  - foss
status: seed
scope_axis:
  - sec-of-ai
entity_type: product
role: "Free first-party Claude Code plugin that runs as a pre-tool hook on Write, Edit, and MultiEdit, pattern-matching the code the agent is about to write against a catalog of dangerous constructs and returning a warning with remediation advice before the edit proceeds. Warns; does not block. Published on Anthropic's plugin marketplace 2026-05-26."
homepage: https://claude.com/plugins/security-guidance
maintainer: "[[anthropic|Anthropic]]"
related:
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[anthropic|Anthropic]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]"
  - "[[injecting-security-context-vibe-coding-talk|Injecting Security Context During Vibe Coding]]"
  - "[[agentshield|AgentShield]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|D4 Runtime & Guardrails]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
sources:
  - https://claude.com/plugins/security-guidance
  - https://code.claude.com/docs/en/security
  - https://github.com/anthropics/claude-code/tree/main/plugins/security-guidance
---

# Security Guidance Plugin

A first-party [[claude-code-security|Claude Code]] plugin that intercepts the agent's own file writes. It registers a pre-tool hook on Write, Edit, and MultiEdit, matches the pending content against a catalog of dangerous constructs, and surfaces a warning with remediation guidance before the edit lands. Published on Anthropic's plugin marketplace on 2026-05-26 and free.

## Detection scope

Reported categories include command injection in GitHub Actions workflow files, `child_process.exec()` on interpolated input, `eval()` and `new Function()`, DOM injection through `innerHTML` and `dangerouslySetInnerHTML`, Python `pickle` deserialization, and `os.system()`. Trade coverage of the launch cites roughly 25 patterns.

## The Design Choice That Defines It

The plugin **warns and lets the edit proceed**. Warnings are session-scoped, so each fires once. This is deliberate and it sets the control's ceiling: it is a defect-reduction instrument operating at authoring time, not an enforcement point. An agent under [[indirect-prompt-injection|indirect prompt injection]] is not deterred by a warning it authored past, and a developer running unattended never reads one.

Anthropic reports internal testing showing a **30–40% reduction in security-related pull-request comments**, and the marketplace counter passed **157,000 installs in the first 24 hours** per trade coverage ([plugin listing](https://claude.com/plugins/security-guidance)). Both figures measure adoption and reviewer burden, not vulnerability escape rate.

> [!gap] No published efficacy data
> Neither the false-negative rate nor the residual-defect rate is published. A 30–40% drop in reviewer comments is consistent with fewer defects reaching review and equally consistent with reviewers commenting less because a tool already spoke. Treat as a productivity result until an escape-rate measurement exists.

## Companion Instruments

Anthropic ships several adjacent capabilities that act at different points in the same lifecycle, and they are easily confused:

| Instrument | Point in the lifecycle | Enforcement |
| --- | --- | --- |
| Security Guidance plugin | Pre-write, in-session | Warning only |
| `/security-review` slash command | On demand, over branch changes | Report |
| [`claude-code-security-review`](https://github.com/anthropics/claude-code-security-review) GitHub Action | Pull request, in CI | Gateable |
| [[claude-code-security\|Claude Code Security]] | Whole-codebase discovery, research preview | Dashboard with human approval |

Only the GitHub Action can hold a merge. An organization treating the plugin as its AI-code security control has placed the only non-gating instrument of the four at the gate.

## Placement

Runtime plane of the [[agentic-ai-security-reference-architecture|AAI-S RA]], and the cheapest available [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] artifact for the generative-coding shape — zero cost, first-party, hook-based. It does not on its own carry a D4 level, because the level criteria ask for controls that stop an action, and this one does not.

[[injecting-security-context-vibe-coding-talk|Gupta's security-context MCP server]] is the closest non-Anthropic instrument and inverts this one on both axes. The plugin fires deterministically, because it is a hook, and carries fixed content — roughly 25 general dangerous-construct patterns that are the same in every organization. Gupta's server fires only when the model chooses to call it, and carries the content the plugin cannot hold: the organization's own data-classification rules, the ticket, and the trust boundaries in the design document. Neither combination is the one an enforcement point needs, which is deterministic firing over organization-specific content, and each product's weakness is the other's strength. The two share the deficiency recorded in the gap callout above — both report benefit and neither publishes a residual-defect rate.
