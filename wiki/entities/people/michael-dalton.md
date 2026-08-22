---
type: entity
title: "Michael Dalton"
address: c-000257
created: 2026-08-14
updated: 2026-08-14
tags:
  - entities
  - people
status: seed
entity_type: person
role: "Security and infrastructure"
affiliation: "[[openai|OpenAI]]"
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: aggregated
related:
  - "[[openai|OpenAI]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[eric-wallace|Eric Wallace]]"
sources:
  - "https://www.youtube.com/watch?v=87DyyMV0kCY"
---

# Michael Dalton

Security and infrastructure at [[openai|OpenAI]].

Co-presented the Black Hat USA 2026 reconstruction of the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] with [[eric-wallace|Eric Wallace]], covering the exploitation and response half: the two Artifactory zero-day chains, the privilege escalation and lateral movement across OpenAI's container-as-a-service estate, the Hugging Face intrusion, and the detection and attribution sequence.[^bh]

Dalton states the talk's defender position: fully automated offense now has an existence proof and fully automated defense does not, so partial automation of the defensive loop relocates the bottleneck rather than closing the gap. See [[openai-hugging-face-incident-blackhat-2026|the reconstruction summary]].

[^bh]: Michael Dalton and Eric Wallace, [*The 'Breaking' News: The OpenAI–Hugging Face Incident*](https://www.youtube.com/watch?v=87DyyMV0kCY), Black Hat USA 2026, 2026-08-06. Exploitation and response at 13:22–29:37; defender position at 30:26–37:12.
