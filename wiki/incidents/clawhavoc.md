---
type: incident
title: "ClawHavoc: Agentic Skill Marketplace Supply Chain Attack"
created: 2026-04-30
updated: 2026-08-21
tags:
  - incidents
  - supply-chain
  - agentic-ai
  - infostealer
status: developing
scope_axis:
  - sec-of-ai
incident_class: supply-chain
attack_with_or_on_ai: "on AI"
date_observed: 2026-01-27
date_disclosed: 2026-02-01
target: "OpenClaw skill marketplace; macOS users running Claude with installed skills"
threat_actor: "hightower6eu (primary), multiple coordinated uploaders"
impact: "1,184+ malicious skills published; Atomic Stealer (AMOS) deployed harvesting browser credentials, keychains, crypto wallets, SSH keys, Telegram data; 36% of all ClawHub skills found to contain security flaws (Snyk)."
related:
  - "[[mitre-atlas]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[ai-security-standards-in-q1-2026]]"
  - "[[openclaw]]"
  - "[[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
---

# ClawHavoc — Agentic Skill Marketplace Supply Chain Attack

## Summary

Between January 27 and February 16, 2026, attackers uploaded **1,184+ malicious skills** to the [[openclaw|OpenClaw]] marketplace. The campaign represents the first large-scale supply chain attack targeting an agentic AI ecosystem. The primary actor "hightower6eu" uploaded 354 skills in a single burst on January 31. Payloads deployed the **Atomic macOS Stealer (AMOS)**, harvesting browser credentials, keychains, crypto wallets, SSH keys, and Telegram data.

This targets OpenClaw's marketplace as distribution *surface*. A separate later finding targets the platform itself as offensive *tooling*: see [[taiwan-ai-agent-government-intrusion|the Taiwan AI-agent government intrusion]], where OpenClaw was one of two orchestration platforms underlying a deliberately constructed attack framework. Attribution in that incident rests on linguistic evidence alone; no threat actor or state sponsor is named.

## Attack Vector

The marketplace had no pre-publish verification, no code signing, and no behavioural analysis — gaps that a January 2026 research note had identified theoretically but that the campaign confirmed at scale. Attackers exploited the open submission flow to publish skills bundling AMOS payloads. End users installed the skills through normal flows, triggering credential exfiltration on first invocation.

## Timeline

- **2026-01-27** — first malicious uploads detected (in retrospect)
- **2026-01-31** — "hightower6eu" uploads 354 skills in a single burst
- **2026-02-01** — Koi Security names the campaign "ClawHavoc"
- **2026-02-07** — OpenClaw partners with VirusTotal for marketplace scanning
- **2026-02-12** — OpenClaw releases 40+ vulnerability patch
- **2026-02-16** — campaign considered contained; long-tail review continues
- Later: Snyk's analysis finds **36% of all ClawHub skills contain security flaws** (broader-than-campaign baseline issue)

## Defensive Lessons

- **Marketplace controls are upstream of agent controls.** The campaign succeeded not because of agent-level vulnerabilities but because the distribution layer had none of the controls software registries (e.g., npm, PyPI) have built up over 15 years.
- **Skill / tool annotation matters.** Frameworks like [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] flag tool-level risk; this campaign is concrete evidence.
- **Mapping to MITRE ATLAS:** the new "Publish Poisoned AI Agent Tool" technique added to ATLAS in Q1 2026 corresponds to this attack class.
- **Pre-publish verification + code signing + behavioural analysis** are the three controls that, in combination, would have substantially reduced the blast radius.

## Standing in the CMM dependency rules

ClawHavoc is the single catalogued incident behind candidate rule DR-C001 in the [[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]], which proposes that a weak supply-chain domain (D8) caps the achievable data-integrity score (D6): a swapped skill poisons a downstream RAG corpus. Promotion of that rule requires a second documented cross-domain incident, so the rule stays a candidate on this incident alone.

## Sources
- See frontmatter `sources:`. Specific quote location: opening of the Q1 2026 threat-landscape section.
