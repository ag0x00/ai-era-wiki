---
type: entity
title: "Yan Shoshitaishvili"
address: c-000274
created: 2026-08-16
updated: 2026-08-16
tags:
  - entities
  - people
  - vuln-discovery
  - academic
status: seed
entity_type: person
role: "Associate Professor, vulnerability analysis and exploitation"
affiliation: "[[arizona-state-university|Arizona State University]]"
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
origin: aggregated
related:
  - "[[arizona-state-university|Arizona State University]]"
  - "[[angr|angr]]"
  - "[[vulnerability-research-agentic-age-keynote|Vulnerability Research in the Agentic Age]]"
  - "[[vulnerability-properties|Vulnerability Properties]]"
  - "[[analyzer-ordering-confound|Analyzer Ordering Confound]]"
sources:
  - "https://www.youtube.com/watch?v=VNYe3Cnk5Pw"
  - "https://blackhat.com/us-26/features/schedule/index.html#keynote-vulnerability-research-in-the-agentic-age-55627"
---

# Yan Shoshitaishvili

Associate professor at [[arizona-state-university|Arizona State University]], where his research covers vulnerability analysis, exploitation, and automated repair. Author of the [[angr|angr]] binary-analysis framework, built during his own graduate work and now the substrate for a long line of research prototypes from the same lab.

Delivered the closing keynote of Black Hat USA 2026, [[vulnerability-research-agentic-age-keynote|Vulnerability Research in the Agentic Age]], introduced by Jeff Moss.[^keynote] The keynote is the wiki's source for two concepts: the [[analyzer-ordering-confound|analyzer ordering confound]], which holds that cross-tool finding-count comparisons measure which tool ran first, and [[vulnerability-properties|vulnerability properties]] as an explicit input to agentic discovery pipelines.

His career runs through competitive CTF — team Shellphish, and Order of the Overflow, which organized the DEF CON CTF — into academic automation of the same work.

**His stated position on capability restriction is that it is misguided.** Fifteen years of offensive research conducted in the open, including open-sourcing angr and the team's cyber reasoning system from the Cyber Grand Challenge and the AI Cyber Challenge, is the basis: the defending population is the larger one, and a capability gate costs it more than it costs an adversary. He extends this to proposals for restricting capable open-weight and next-generation models.

He also runs pwn.college, [[arizona-state-university|Arizona State University]]'s open security-education platform, and argues that agentic tooling raises the return on human security expertise rather than retiring it.

[^keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
