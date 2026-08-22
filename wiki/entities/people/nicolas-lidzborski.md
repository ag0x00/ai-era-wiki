---
type: entity
entity_type: person
title: "Nicolas Lidzborski"
created: 2026-05-03
updated: 2026-05-03
tags:
  - entities
  - people
  - google
  - workspace-security
status: developing
role: "Principal Software Engineer at Google; Workspace security; 25 years in security; 3 years on GenAI security"
related:
  - "[[google]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[unprompted-conference-march-2026]]"
sources:
  - ".raw/talks/2026-03-04_Nicolas-Lidzborski_Securing-Workspace-GenAI-at-Google_transcript.md"
---

# Nicolas Lidzborski

Principal Software Engineer at [[google|Google]] working on Google Workspace security. 25 years in security overall, ~3 years focused on Generative AI security. Goes by "Nico" in person.

## Talks ingested in this wiki

- [[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]] — Unprompted Conference, March 4, 2026 (Stage 1, Lecture 07). Three-year retrospective on GenAI security lessons; introduces the *prompt-as-code* framing, the four-layer "Architecting the Fortress" structural blueprint, and the Plan-Validate-Execute pattern for high-stakes actions.

## Distinctive contributions to this wiki's vocabulary

- **[[prompt-as-code|"Prompt as code"]]** — structural framing of why GenAI security cannot rely on syntactic filtering: every input token is a potential instruction, and the natural-language grammar is fuzzy
- **[[agency-gap|Agency gap]]** — the non-deterministic disconnect between user intent and autonomous AI execution; named contribution
- **[[orchestration-hijacking|Orchestration hijacking]]** — compromised orchestration layer where the LLM-as-planner is manipulated; named contribution
- **[[plan-validate-execute|Plan-Validate-Execute]]** — Google's structural pattern for high-stakes irreversible actions

## Position in the field

Lidzborski's perspective is the **Google Workspace** counterpart to [[andrew-bullen|Andrew Bullen]]'s **Stripe** containment work: both are practitioner deep-dives into platform-layer containment from a major production deployment. Bullen emphasizes the egress + tool-policy enforcement surface; Lidzborski emphasizes the input + orchestration + output surface. The complementarity reflects each org's deployment shape (Stripe: programmatic agents + payment infrastructure; Google: knowledge-worker productivity at scale).

> [!gap]
> Background details (academic affiliation, prior roles, public writing outside the Unprompted talk) not in the source material. To be filled when additional sources surface.
