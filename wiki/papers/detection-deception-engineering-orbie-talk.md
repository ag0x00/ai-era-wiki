---
type: talk
title: "Detection & Deception Engineering in the Matrix (Orbie)"
address: c-000147
created: 2026-05-25
updated: 2026-05-25
tags:
  - papers
  - talks
  - agentic-soc
  - detection-engineering
  - deception
  - ai-in-sec-defense
status: stub-summary
scope_axis: [ai-in-sec-defense]
year: 2026
authors: ["Bob Rudis", "Glenn Thorpe"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 4, 2026 (Day 2 / Stage 1 / 15:30)"
key_claim: "An AI agent (Orbie) operating on internet-scale honeypot telemetry can surface emergent threats, identify campaigns, and author detection rules — and the domain-expert knowledge embedded in the tooling matters more than the underlying model."
methodology: "Practitioner talk on GreyNoise Labs' Orbie agent. The published abstract is the only primary source captured here; slides and transcript are not yet ingested."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[greynoise|GreyNoise]]"
  - "[[bob-rudis|Bob Rudis]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
---

# Detection & Deception Engineering in the Matrix (Orbie)

A practitioner talk by [[bob-rudis|Bob Rudis]] and Glenn Thorpe ([[greynoise|GreyNoise]]) at Unprompted (March 2026) on Orbie, an AI agent that operates over internet-scale honeypot data. Abstract-only; slides and video are not yet captured.

## The Argument

Orbie surfaces emergent threats, identifies campaigns, and writes detection rules from GreyNoise's internet-scale honeypot telemetry. The talk reports what works and what does not, and points to specific campaigns the agent caught that traditional methods missed. Its central claim is that **domain-expert knowledge embedded in the tooling**, not the choice of model, is what lets an LLM operate usefully over billions of network sessions.

## Placement

Direct evidence for the **detection-engineering** capability in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis: an agent that authors detection content from live telemetry, rather than a human writing rules against vendor libraries. It pairs with the Palo Alto SYARA semantic-detection talk and the Microsoft BinaryShield threat-intel-sharing talk as the detection cluster of the Unprompted agenda. The "domain knowledge in tooling beats model choice" claim is a useful counterweight to model-centric framings of agentic detection.
