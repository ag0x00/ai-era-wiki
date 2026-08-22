---
type: entity
entity_type: organization
org_type: vendor
title: "Palo Alto Networks"
created: 2026-04-30
updated: 2026-05-26
tags:
  - entities
  - organizations
  - network-security
  - cnapp
  - sase
  - ai-security
  - glasswing
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - sec-against-ai
role: "Cybersecurity platform vendor (NGFW, SASE, CNAPP, AI security); acquirer of Protect AI (2025), Dig Security (2023), and CyberArk (2026); Project Glasswing launch partner (May 2026)"
related:
  - "[[palo-alto-prisma-airs]]"
  - "[[cyberark]]"
  - "[[dongdong-sun]]"
  - "[[mohamed-nabeel]]"
  - "[[syara-semantic-detection-talk]]"
  - "[[osint-to-knowledge-graph-talk]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mythos]]"
  - "[[claude-partners-opus-cybersecurity]]"
sources:
  - "https://www.paloaltonetworks.com/"
  - "https://www.anthropic.com/glasswing"
  - "[[.raw/articles/claude-partners-opus-cybersecurity-2026-05-23.md]]"
---

# Palo Alto Networks

Palo Alto Networks (NASDAQ: PANW) is one of the largest publicly-traded cybersecurity vendors. Its portfolio is anchored on next-generation firewalls (PAN-OS) and has expanded through acquisition into SASE (Prisma Access), CNAPP (Prisma Cloud, originally RedLock + Dig Security), endpoint and XDR (Cortex), and, beginning 2025, dedicated AI security under [[palo-alto-prisma-airs|Prisma AIRS]].

## Core platforms

| Family | Role |
|---|---|
| **PAN-OS / Strata** | Next-generation firewall and network security |
| **Prisma SASE** | Secure Access Service Edge: Prisma Access + Prisma SD-WAN |
| **Prisma Cloud** | CNAPP: incorporates AI-SPM via the Dig Security acquisition |
| **[[palo-alto-prisma-airs\|Prisma AIRS]]** | Dedicated AI security pillar: runtime, posture, model security, red teaming |
| **Cortex** | XDR + XSIAM (security analytics + autonomous SOC) |

## Acquisitions relevant to AI security

| Acquisition | Year | Brought into Palo Alto |
|---|---|---|
| **Dig Security** | 2023 | Cloud DSPM → became Prisma Cloud AI-SPM module |
| **Protect AI** | 2025 | Model scanning (Guardian, ModelScan) → integrated into Prisma AIRS 2.0 (Oct 2025) |
| **[[cyberark\|CyberArk]]** | 2026 | Identity Security Platform (PAM, [[cyberark-conjur\|Conjur]] secrets management, NHI governance) |

The CyberArk acquisition is the most strategically significant for the agent security space. It pairs Palo Alto's runtime/network/posture stack with CyberArk's identity/secrets stack, creating a single-vendor portfolio covering identity → policy → runtime → network → posture → red-team for agentic AI.

## Project Glasswing partnership (May 2026)

Palo Alto Networks is a named launch partner in [[glasswing|Project Glasswing]] (Anthropic coalition initiative). **Lee Klarich** (Chief Product & Technology Officer) is the quoted executive, with the canonical wiki citation on the AI-attacker threat reframing: *"There will be more attacks, faster attacks, and more sophisticated attacks. Now is the time to modernize cybersecurity stacks everywhere."* The quote directly supports the [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] thesis.

## Notable 2025–2026 events

- **April 28, 2025**: Prisma AIRS launched
- **August 2025**: Portkey integration with Prisma AIRS
- **October 29, 2025**: Prisma AIRS 2.0 GA, integrating Protect AI; agent-lifecycle protection expanded
- **2026**: CyberArk acquisition announced (~\$25B)

## Unit 42 — threat research arm

[Unit 42](https://unit42.paloaltonetworks.com/) is Palo Alto Networks' threat-intelligence and incident-response group. Two Unit 42 publications anchor the agentic-AI security material in this wiki:

| Publication | Date | Type | Wiki page |
|---|---|---|---|
| [AI Agents Are Here. So Are the Threats.](https://unit42.paloaltonetworks.com/agentic-ai-threats/) (Jay Chen, Royce Lu) | 2025-05-01 | Lab study: 9 framework-agnostic attack scenarios on CrewAI + AutoGen with open-source reference impl | [[ai-agents-are-here-so-are-the-threats-unit42\|paper page]] |
| In-the-wild prompt injection observations (22 distinct techniques) | 2026-03-03 | Production telemetry from PAN customer base | [[unit-42-prompt-injection-observations\|incident page]] |

Together: lab evidence (May 2025) + production-telemetry confirmation (March 2026) for the same indirect-prompt-injection attack class. Unit 42 also operates the [AI Security Assessment](https://www.paloaltonetworks.com/unit42/assess/ai-security-assessment) consulting offering and the Unit 42 Incident Response team.

**[Unit 42 Frontier AI Defense](https://www.paloaltonetworks.com/unit42/ai-advantage)** (2026) is an expert-led service built on [[mythos|Claude Opus]]: it finds hidden vulnerabilities, maps how they chain into critical attack paths, and builds a hardening roadmap against AI-enabled attacks, paired with a benchmarked machine-speed-defense blueprint and hands-on transformation work. In internal testing it compressed **a year's worth of penetration-testing effort into under three weeks** (see [[claude-partners-opus-cybersecurity|the Opus partner ecosystem]]). SVP Sam Rubin: *"As attackers weaponize frontier models to automate cyberattacks, the defense must move faster."*

## Wiki references

- [[palo-alto-prisma-airs|Palo Alto Prisma AIRS]]: primary AI security product page
- [[cyberark|CyberArk]]: acquired identity-security vendor
- [[dongdong-sun|Dongdong Sun]], [[mohamed-nabeel|Mohamed Nabeel]]: Palo Alto researchers (Senior Staff ML Engineer, Sr Principal Researcher)
- [[syara-semantic-detection-talk|Detecting GenAI Threats at Scale with YARA-Like Semantic Rules (SYARA)]]: Nabeel's Unprompted talk on cost-ordered semantic detection
- [[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph]]: Sun's Unprompted talk on a production-scale threat-intelligence knowledge graph
- [[ai-agents-are-here-so-are-the-threats-unit42|Unit 42 — AI Agents Are Here. So Are the Threats.]]: lab study
- [[unit-42-prompt-injection-observations|Unit 42 — In-the-Wild Prompt Injection Observations]]: production telemetry
- [[agentic-ai-security-reference-architecture|RA]]: Prisma AIRS appears in Runtime, Egress, Data, and Observability planes
