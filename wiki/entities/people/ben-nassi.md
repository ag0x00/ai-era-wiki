---
type: entity
entity_type: person
title: "Ben Nassi"
created: 2026-05-03
updated: 2026-05-03
tags:
  - entities
  - people
  - researcher
  - prompt-injection
status: stub
role: "Security researcher; lead author on 'Invitation Is All You Need' (BlackHat) and 'Promptware Kill Chain' (2026)"
related:
  - "[[indirect-prompt-injection]]"
  - "[[lethal-trifecta]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[promptware]]"
  - "[[your-agent-works-for-me-now-talk]]"
sources: []
---

# Ben Nassi

Security researcher. Author of two papers with direct relevance to this wiki:

**"Invitation Is All You Need"** (BlackHat, prior year) — demonstrated indirect prompt injection through Google Calendar invites against Gemini. The attack: an attacker plants a hidden payload in a Google Calendar invite; nothing happens until the user asks the agent something benign like *"what's on my schedule today?"*; the agent ingests the invite, follows the hidden instructions, and (in Lidzborski's deployment context) extends the impact to smart-home control. Cited extensively in [[securing-workspace-genai-at-google-talk|Lidzborski's Workspace talk]].

**"Promptware Kill Chain"** (March 2026, "released very recently" at the time of Unprompted) — the academic framing for the [[promptware|Promptware]] concept: analyzing prompt injection not as an atomic event but as the initial phase of a multi-stage attack chain with its own persistence and exfiltration phases. Johann Rehberger credits this paper in his Unprompted 2026 talk ([[your-agent-works-for-me-now-talk|"Your Agent Works for Me Now"]]) as the academic parallel to his practitioner Promptware framing.

> [!gap] Stub
> This page is a stub. Background, affiliation, and the full citation for the Promptware Kill Chain paper need to be researched. The "Invitation Is All You Need" research warrants its own paper page once primary sources are available. Rehberger also cites "Steph Cohen and a few others" as co-authors on that paper — full author list needs verification.
