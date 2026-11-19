---
type: entity
entity_type: product
title: "Cursor"
created: 2026-05-07
updated: 2026-10-21
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
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
  - "[[manifold-security|Manifold Security]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[gartner-mq-enterprise-ai-coding-agents-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[endor-labs-ai-code-governance]]"
  - "[[following-the-ai-audit-trail-talk]]"
sources:
  - "https://www.cursor.com"
  - "https://socket.dev/blog/malicious-npm-packages-hijack-cursor-editor-on-macos"
  - "https://www.manifold.security/blog/ai-coding-agents-git-hijack"
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified: 2026-10-21
verified_against:
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf"
verified_findings: 0
verified_note: "Evidence-source bullet verified against deck slides 6, A1 (2/2), A7, A9; Cloud Agents and Grok Bot scope restored, cross-checked with live Cursor docs; older sources not reopened."
---

# Cursor

**Sources:** [Cursor (cursor.com)](https://www.cursor.com)

Cursor is an AI-native code editor produced by Anysphere and one of the dominant coding-agent IDE surfaces in 2025–2026, alongside Claude Code, Gemini CLI, OpenAI Codex, Amazon Kiro, and Google Antigravity. It is built on a VS Code fork and surfaces inline AI completions, agent-mode multi-step edits, and external-tool MCP integrations.

In the context of this wiki, Cursor appears across four distinct surfaces:

- **As a target**. In the [[cursor-npm-credential-stealer|May 2025 npm supply-chain attack]], three malicious npm packages totalling over 3,200 downloads ([Socket Threat Research, 2025-05-07](https://socket.dev/blog/malicious-npm-packages-hijack-cursor-editor-on-macos)) harvested credentials from macOS Cursor users, overwrote `main.js` and disabled auto-update for persistence — one of four primary-source incidents cited in [[breaking-the-lethal-trifecta-talk|Andrew Bullen's "Breaking the Lethal Trifecta"]] talk at Unprompted March 2026. Cursor is also one of the seven agents in the [[gitspawn-coding-agent-git-config-rce|GitSpawn]] set: [[manifold-security|Manifold Security]] reported a context-gathering git-configuration execution against it on 2026-07-08, the report was closed as a duplicate of an earlier one, and the finding was patched before the 2026-09-01 publication. Manifold published no case study for it and names no sink.
- **As a defended runtime**. [[sondera|Sondera]]'s [[hooking-coding-agents-with-cedar-talk|Cedar-policy harness]] explicitly enumerates Cursor as one of the three coding-agent surfaces it intercepts (alongside Claude Code and Gemini CLI), via the per-agent local adapter pattern.
- **As an exploitation case study**. Mindgard's "Vibe Check: Security Failures in AI-Assisted IDEs" talk at Unprompted March 2026 (Piotr Ryciak) catalogues exploitation patterns across **Codex, Kiro, Antigravity, and Cursor**.
- **As an evidence source**. Cursor's audit logs cover the control plane and are read by API poll or vendor stream, its OpenTelemetry export is sent from Cursor's side, and both require the Enterprise plan. Conversation content is a separate opt-in that covers only Cloud Agents and Grok Bot, and the team-command audit event is not shell-command telemetry ([[following-the-ai-audit-trail-talk|Following the AI Audit Trail]]). That event records a custom command created, updated or deleted ([Cursor compliance and monitoring](https://cursor.com/docs/enterprise/compliance-and-monitoring)), and IDE, CLI and desktop conversations are outside the content export ([Cursor OpenTelemetry export](https://cursor.com/docs/enterprise/opentelemetry-export)).

## See also

- [[cursor-npm-credential-stealer|Cursor npm Credential Stealer (May 2025)]] — supply-chain incident
- [[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]] — git-config execution class covering Cursor, patched
- [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]] — runtime policy enforcement covering Cursor
- [[unprompted-conference-march-2026|Unprompted March 2026]] — multiple talks reference Cursor
