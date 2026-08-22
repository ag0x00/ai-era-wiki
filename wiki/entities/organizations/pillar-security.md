---
type: entity
entity_type: organization
title: "Pillar Security"
address: c-000293
created: 2026-08-16
updated: 2026-08-16
tags:
  - entities
  - organizations
  - vulnerability-research
  - agentic-coding
  - prompt-injection
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
org_type: vendor
role: "AI security vendor with a research team; credited discoverer of the Gemini CLI --yolo allowlist bypass in GHSA-wpqr-6v78-jr5g"
homepage: "https://www.pillar.security"
first_mentioned: "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
related:
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[novee-security|Novee Security]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
sources:
  - "https://www.pillar.security"
  - "https://www.pillar.security/blog/my-agentic-trust-issues-from-prompt-injection-to-supply-chain-compromise-on-gemini-cli"
  - "https://github.com/advisories/GHSA-wpqr-6v78-jr5g"
---

# Pillar Security

**Sources:** [Pillar Security](https://www.pillar.security) · ["My Agentic Trust Issues"](https://www.pillar.security/blog/my-agentic-trust-issues-from-prompt-injection-to-supply-chain-compromise-on-gemini-cli)

AI security vendor operating a named research team. **Dan Lisichkin** is one of two credited discoverers in [GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g), the CVSS 10.0 [[gemini-cli|Gemini CLI]] advisory.

Pillar's contribution is the **`--yolo` half**: the autonomy flag suppressed the fine-grained tool allowlist entirely rather than merely skipping confirmation prompts. The published chain runs from a public GitHub issue on a live Google repository through credential extraction at `/proc/$PPID/environ` and `.git/config` to repository write, obtained by dispatching a second workflow with the triage token's `actions:write` permission. It is the most completely documented public exploit chain against a CI-runner coding agent catalogued here, and the only published source for the advisory's disclosure dates. Mechanisms and timeline: [[gemini-cli-workspace-trust-rce|the incident record]].

Only this one finding is catalogued here. The firm's product line and wider research are not assessed.
