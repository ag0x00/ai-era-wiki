---
type: entity
entity_type: product
title: "Cursor"
created: 2026-05-07
updated: 2026-07-30
tags:
  - products
  - cursor
  - ide
  - coding-agent
  - editor
status: seed
homepage: "https://www.cursor.com"
related:
  - "[[cursor-npm-credential-stealer]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[gartner-mq-enterprise-ai-coding-agents-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[endor-labs-ai-code-governance]]"
sources:
  - "https://www.cursor.com"
---

# Cursor

**Sources:** [Cursor (cursor.com)](https://www.cursor.com)

AI-native code editor produced by Anysphere; one of the dominant coding-agent IDE surfaces in 2025–2026 alongside Claude Code, Gemini CLI, OpenAI Codex, Amazon Kiro, and Google Antigravity. Built on a VS Code fork; surfaces inline AI completions, agent-mode multi-step edits, and external-tool MCP integrations.

In the context of this wiki, Cursor appears across three distinct surfaces:

- **As a target**. The [[cursor-npm-credential-stealer|May 2025 npm supply-chain attack]] trojanized 3,200+ macOS Cursor installs via three malicious npm packages that overwrote `main.js` and disabled auto-update for persistence — one of four primary-source incidents cited in [[breaking-the-lethal-trifecta-talk|Andrew Bullen's "Breaking the Lethal Trifecta"]] talk at Unprompted March 2026.
- **As a defended runtime**. [[sondera|Sondera]]'s [[hooking-coding-agents-with-cedar-talk|Cedar-policy harness]] explicitly enumerates Cursor as one of the three coding-agent surfaces it intercepts (alongside Claude Code and Gemini CLI), via the per-agent local adapter pattern.
- **As an exploitation case study**. Mindgard's "Vibe Check: Security Failures in AI-Assisted IDEs" talk at Unprompted March 2026 (Piotr Ryciak) catalogues exploitation patterns across **Codex, Kiro, Antigravity, and Cursor**.

## See also

- [[cursor-npm-credential-stealer|Cursor npm Credential Stealer (May 2025)]] — supply-chain incident
- [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]] — runtime policy enforcement covering Cursor
- [[unprompted-conference-march-2026|Unprompted March 2026]] — multiple talks reference Cursor
