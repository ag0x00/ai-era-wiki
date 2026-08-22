---
type: incident
title: "SANDWORM_MODE npm Worm: AI Toolchain Poisoning"
created: 2026-04-30
updated: 2026-04-30
tags:
  - incidents
  - toolchain-poisoning
  - mcp
  - supply-chain
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
incident_class: toolchain-poisoning
attack_with_or_on_ai: both
date_observed: 2026-02-20
date_disclosed: 2026-02-20
target: "Claude Code, Claude Desktop, Cursor, VS Code, Windsurf — via injected MCP servers"
threat_actor: "unknown"
impact: "19 malicious npm packages (typosquats); MCP-server injection enabled credential exfiltration via the AI assistant itself; coordinated takedown required Cloudflare, npm, and GitHub action."
related:
  - "[[mcp-security]]"
  - "[[clawhavoc]]"
  - "[[litellm-supply-chain-compromise]]"
  - "[[ai-security-standards-in-q1-2026]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# SANDWORM_MODE npm worm — AI Toolchain Poisoning

## Summary

A novel attack class observed February 20, 2026: **AI toolchain poisoning**. Nineteen malicious npm packages — typosquats of `claude-code`, `supports-color`, and similar — included a self-propagating worm with multi-stage AES-256-GCM-encrypted payloads, 48–96 hour delayed execution on developer machines, and — critically — **MCP server injection** into Claude Code, Claude Desktop, Cursor, VS Code, and Windsurf. The injected MCP servers used hidden prompt injection to read SSH keys, AWS credentials, and `.env` files **through the AI assistant itself**.

## Attack Vector

1. Developer installs a typosquatted package (resembles a popular dependency).
2. Worm encrypts and delays execution to evade fast-path detection.
3. After 48–96 hours, payload deploys an MCP server config and registers it with the developer's installed AI tools.
4. The MCP server includes hidden prompt-injection text. When the developer next interacts with the AI assistant, the assistant reads the injected prompt as if it came from the user — and follows instructions to surface credentials.
5. Credentials are exfiltrated through the AI assistant's normal output channel.

The novelty: the attack uses **the AI assistant as the credential-exfiltration tool**. Standard endpoint controls don't recognize this as exfiltration because it routes through legitimate AI tooling.

## Timeline

- **2026-02-20** — packages disclosed and named SANDWORM_MODE
- Coordinated takedown by Cloudflare, npm, and GitHub
- (Mitigation timeline details: see source)

## Defensive Lessons

- **MCP-config provenance is now a security control surface.** Trust paths for installed MCP servers need to be enforced upstream of the AI assistant, not by the assistant itself.
- **Context-aware trimming and pinned-tag observability** ([[agent-observability|Agent Observability]] §5) would help detect injected MCP-server interactions if labelled, but only if logs survive context-window pressure.
- **Hidden prompt injection in tool/server descriptions** is a real operational pattern, not just a paper risk. The AI agent treats MCP server text as instructions.
- This incident plus [[clawhavoc|ClawHavoc]] together establish that **the agentic-AI distribution layer (skills, MCP servers, plugins) is now an active supply-chain attack surface** — at parity with traditional package registries but without their accumulated controls.

## Sources
- See frontmatter.
