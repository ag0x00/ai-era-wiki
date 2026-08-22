---
type: entity
entity_type: organization
title: "Novee Security"
address: c-000292
created: 2026-08-16
updated: 2026-08-16
tags:
  - entities
  - organizations
  - vulnerability-research
  - ci-cd
status: seed
scope_axis:
  - sec-of-ai
origin: aggregated
org_type: vendor
role: "Security research firm; credited discoverer of the Gemini CLI headless workspace-trust flaw in GHSA-wpqr-6v78-jr5g"
homepage: "https://novee.security"
first_mentioned: "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
related:
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[pillar-security|Pillar Security]]"
sources:
  - "https://novee.security"
  - "https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/"
  - "https://github.com/advisories/GHSA-wpqr-6v78-jr5g"
  - "https://www.theregister.com/2026/04/30/googles_fix_for_critical_gemini/"
---

# Novee Security

**Sources:** [Novee Security](https://novee.security) · [Gemini CLI advisory write-up](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/)

Security research firm. **Elad Meged**, described on the firm's write-up as founding engineer and security researcher, is one of two credited discoverers in [GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g), the CVSS 10.0 [[gemini-cli|Gemini CLI]] advisory. Meged identified it while studying CI/CD supply-chain risk ([The Register, 2026-04-30](https://www.theregister.com/2026/04/30/googles_fix_for_critical_gemini/)).

Novee's contribution is the **folder-trust half** of that advisory: headless Gemini CLI trusting the workspace folder for configuration and environment loading, which the firm characterizes as infrastructure-level execution reached before the agent reasons and before its sandbox initializes. That sequencing claim bears on the [[agentic-ai-security-reference-architecture|AAI-S RA]]'s Runtime plane, which specifies what each control enforces and not when enforcement begins. Mechanisms and timeline: [[gemini-cli-workspace-trust-rce|the incident record]].

Only this one finding is catalogued here. The firm's wider body of work is not assessed.
