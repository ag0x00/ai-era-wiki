---
type: incident
title: "Clinejection: AI Attacks AI via GitHub Issue Title"
created: 2026-04-30
updated: 2026-04-30
tags:
  - incidents
  - prompt-injection
  - ai-vs-ai
  - supply-chain
status: developing
incident_class: prompt-injection
attack_with_or_on_ai: both
date_observed: 2026-02-17
date_disclosed: 2026-02-17
target: "Cline (AI coding tool); the malicious cline@2.3.0 npm package was published for ~8 hours"
threat_actor: "unknown"
impact: "Prompt injection in a GitHub issue title tricked Claude (used by Cline for triage) into running npm install from an attacker-controlled commit, exfiltrating NPM_RELEASE_TOKEN and VSCE_PAT credentials. Malicious cline@2.3.0 published for 8 hours."
related:
  - "[[mcp-security]]"
  - "[[agent-observability]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[sandworm-mode-npm-worm]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# Clinejection — AI Attacks AI via GitHub Issue Title

## Summary

On February 17, 2026, an attacker placed prompt-injection text in a **GitHub issue title**. Cline (an AI coding agent) used Claude to triage incoming issues. When Claude ingested the issue title as part of normal triage, it followed the injected instructions: ran `npm install` from an attacker-controlled commit, which exfiltrated `NPM_RELEASE_TOKEN` and `VSCE_PAT` credentials. The attacker used the stolen tokens to publish a malicious `cline@2.3.0` package, which remained available for approximately **8 hours**.

## Attack Vector

The incident chains three classic patterns into a new shape:

1. **Indirect prompt injection** through untrusted text (issue title) that the AI agent reads in normal operation.
2. **AI-as-execution-tool** — the AI agent had `npm install` capability without strong human-in-the-loop confirmation.
3. **Token-based supply-chain takeover** — once the agent's credentials were exfiltrated, the attacker's leverage extended to the package registry directly.

## Timeline

- **2026-02-17** — attack executed and detected
- ~8 hours later — malicious `cline@2.3.0` taken down
- (Forensics and post-mortem timing: see source)

## Defensive Lessons

- **Tool annotation + human-in-the-loop gates** on credential-touching actions would have severed the chain. The Human-in-the-Loop primitive applied to the AI coding-agent class would have caught this before exfiltration.
- **The issue title was untrusted external input.** Treating ALL externally-sourced text as untrusted (per the lethal-trifecta containment pattern) is the architectural answer.
- **Token scoping.** Wide-scope tokens (`NPM_RELEASE_TOKEN`, `VSCE_PAT`) made the exfiltration high-impact. Capability-scoped credentials (task-scoped, signed, ephemeral Warrants) would have constrained blast radius.
- This incident is the first widely-discussed "**AI attacks AI**" pattern: an attacker leveraged one AI agent (used for triage) to attack another AI agent (used for development).

## Sources
- See frontmatter.
