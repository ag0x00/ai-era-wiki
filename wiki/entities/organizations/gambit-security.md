---
type: entity
title: "Gambit Security"
address: c-000078
created: 2026-05-15
updated: 2026-05-23
tags:
  - entities
  - organizations
  - threat-intel
  - ai-assisted-attacks
status: seed
scope_axis: [sec-against-ai, ai-in-sec-offense, ai-in-sec-defense]
entity_type: organization
org_type: vendor
role: "Cybersecurity company that published the canonical technical report on the Mexican government multi-agency AI-assisted breach (Feb 2026). Operates the *Balens* platform / blog. Recovered attacker materials enabling reconstruction of the dual-AI-platform operation (Claude Code + GPT-4.1)."
homepage: "https://gambit.security"
first_mentioned: "[[mythos-ready-briefing|Mythos-ready paper]]"
related:
  - "[[mexican-government-ai-breach|Mexican Government Multi-Agency AI-Assisted Breach]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[gtg-1002-ai-orchestrated-espionage|GTG-1002 — paired canonical incident]]"
  - "[[dragos|Dragos]]"
sources:
  - https://gambit.security
  - https://gambit.security/blog-post/a-single-operator-two-ai-platforms-nine-government-agencies-the-full-technical-report
---

# Gambit Security

**Sources:** [gambit.security](https://gambit.security) · [Mexican-gov breach technical report (full)](https://gambit.security/blog-post/a-single-operator-two-ai-platforms-nine-government-agencies-the-full-technical-report).

## Identity and role

Cybersecurity company (founders, headcount, prior history not yet captured) that surfaced on the wiki for its **February 2026 technical report on a multi-agency AI-assisted breach of nine Mexican government organizations**. The investigative work is published via the company's *Balens* blog. Dragos assisted with the OT-adjacent water-utility subplot.

## Relevance to This Wiki

- **Disclosed [[mexican-government-ai-breach|the Mexican Government AI-Assisted Breach]]** — the wiki's second canonical AI-assisted state-scale operation alongside [[gtg-1002-ai-orchestrated-espionage|GTG-1002]]. The Gambit report is the **load-bearing source** for the dual-platform (Claude Code + GPT-4.1) operational template, the 5,317-AI-executed-commands / 1,088-prompts / 34-sessions quantitative footprint, the OT-adjacency observation (Claude independently identifying OT crown-jewel relevance), and the central methodological argument that *AI compresses the cost of turning ordinary weaknesses into multi-agency compromise*.
- **Cited inline by the [[mythos-ready-briefing|Mythos-ready strategic briefing]]** as one of the early-2026 escalation timeline anchors (alongside Anthropic FRT, AISLE, Sysdig).

## Adjacent / Open

- **Company history and team**: not captured in current ingest. Worth a follow-up.
- **Other Gambit research output** beyond the Mexican-gov breach report is not yet on the wiki — relationship between *Balens* platform and Gambit Security as the legal entity TBD.
- **Attribution**: Gambit does not publicly attribute the Mexican-gov-breach operator; if subsequent attribution lands (Mexican federal authorities, intelligence services, Anthropic/OpenAI threat-intel), it would be added to the [[mexican-government-ai-breach|incident page]] rather than this org page.
