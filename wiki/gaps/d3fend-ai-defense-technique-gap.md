---
type: gap
title: "D3FEND AI-Defense Technique Gap"
address: c-000186
created: 2026-06-03
updated: 2026-06-03
tags:
  - gaps
  - agentic-soc
  - d3fend
  - mitre
  - detection-engineering
status: open
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
  - sec-of-ai
question: "MITRE D3FEND catalogues defensive techniques against conventional attacker behaviour but is thin on AI-era defence — agent supervision, evaluation-gating, intent attribution, and deception against AI-powered attackers. What would a D3FEND-shaped defensive-technique layer for the agentic SOC cover, and who would maintain it?"
why_it_matters: "The Agentic SOC CMM's D6 (Detection & Response Tradecraft) scores coverage against ATT&CK (offence), D3FEND (defence), and ATLAS (AI-system threats). D3FEND's defensive axis under-counts AI-era defences because the techniques are not yet catalogued, so a SOC can be mature on the named techniques and still have no standard reference for the controls that distinguish an agentic SOC."
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-autonomy-ladders]]"
  - "[[mitre-atlas]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[detection-deception-engineering-orbie-talk]]"
  - "[[mythos-ready-security-program]]"
sources:
  - "[[agentic-soc-cmm]]"
---

# D3FEND AI-Defense Technique Gap

MITRE **D3FEND** is the defensive-technique knowledge graph that complements **ATT&CK**: where ATT&CK catalogues what attackers do, D3FEND catalogues the countermeasures defenders run against it, organized as defensive tactics (harden, detect, isolate, deceive, evict, restore) over a digital-artifact ontology. The [[agentic-soc-cmm|Agentic SOC CMM]]'s **D6 (Detection & Response Tradecraft)** scores a SOC's coverage against three catalogues — ATT&CK for offence, D3FEND for defence, and [[mitre-atlas|MITRE ATLAS]] for threats to AI systems. The D3FEND axis is the one that under-reports, because the AI-era defensive techniques that distinguish an agentic SOC are not yet catalogued anywhere with D3FEND's rigour. This page names that gap and sketches the layer that would close it.

## The gap

D3FEND's artifact ontology is built around conventional digital artifacts — processes, files, network traffic, accounts, certificates. Its techniques map countermeasures onto those artifacts. Two AI-era surfaces fall outside that frame:

- **The defender's own agents are not modelled as defensive instruments.** D3FEND has no techniques for supervising an autonomous agent, gating its authority by earned evaluation, attributing an action to a human or an agent, or bounding its blast radius. These are the controls the agentic SOC runs as its core discipline, and they have no D3FEND counterpart.
- **The AI-powered attacker is not modelled as a distinct adversary.** D3FEND's deception and detection tactics assume tool- and vulnerability-signature behaviour. An attacker operating at machine speed through an agentic pipeline (the [[zero-day-clock|time-to-exploit collapse]]) presents behaviour D3FEND's catalogue does not describe, so its deceive and detect tactics do not yet name the techniques that catch it.

ATLAS partly covers the first surface from the threat side — it enumerates attacks *on* AI systems and lists mitigations — but ATLAS is an offence catalogue with mitigation notes, not a defensive-technique taxonomy of the operational kind D3FEND provides. Neither ATT&CK, D3FEND, nor ATLAS treats the AI defender's own agents as defensive instruments with their own countermeasure catalogue. That is the open space.

## A candidate agentic-SOC defensive-technique layer

A D3FEND-shaped layer for the agentic SOC would extend the artifact ontology with AI-system artifacts — prompt, model, agent plan, tool-call trace, retrieved context, agent identity — and add defensive techniques in five clusters, each already realized as a plane or domain in the [[agentic-soc-reference-architecture|Agentic SOC RA]] and scored by the CMM:

| Technique cluster | What it covers | CMM domain |
|---|---|---|
| Agent supervision | Human-on-the-loop oversight, per-action authority tiers, blast-radius limits, override and revocation paths | D4, D5 |
| Evaluation-gating | Scoring agent decisions against ground truth before autonomy is raised; the gating rule itself as a defensive control | D3 |
| Intent attribution | Distinguishing human from agent activity in telemetry — the [[genai-endpoint-observability-talk\|broken-intent-attribution problem]] — and carrying AI-application telemetry | D1, D5, D6 |
| Deception against AI attackers | Canaries, honeytokens, and behavioural monitoring keyed to agentic-attacker TTPs rather than tool or vulnerability signatures (the [[detection-deception-engineering-orbie-talk\|deception-detection]] direction) | D6 |
| Machine-speed response | Pre-authorized containment that executes at machine speed under deterministic policy gates | D4, D6 |

The last two clusters answer the AI-powered attacker; the first three are the techniques for operating AI defenders accountably, which is also the [[agentic-soc-cmm|CMM]]'s shared securing-the-agents layer. The deception and machine-speed-response clusters correspond directly to the [[mythos-ready-security-program|Mythos-ready]] Priority Actions for a deception capability and an automated response capability.

## Relationship to D3FEND, ATLAS, and CMM D6

The proposed layer would stand to ATLAS **as D3FEND stands to ATT&CK: the defensive counterpart to an offence catalogue**. ATLAS extended ATT&CK's offence model to AI-system threats; no equivalent extension yet covers the defence half. The layer would sit alongside D3FEND — extending its ontology and tactics rather than replacing it — and would be the catalogue CMM D6 scores its AI-era coverage against, in place of the under-counting D3FEND axis it uses today. Until the layer exists, D6's D3FEND score should be read as a coverage floor for AI-era defences, not a measure of them.

## Closure conditions

- **MITRE extends D3FEND.** The ontology gains AI-system artifacts and the tactic set gains agent-supervision, evaluation-gating, and intent-attribution techniques. This is the highest-authority path and the slowest.
- **A community or academic taxonomy emerges** — a "D3FEND-for-AI-defence" counterpart to how ATLAS emerged for AI threats, maintained outside MITRE if MITRE does not move.
- **The wiki authors a candidate layer.** The Agentic SOC RA planes and CMM domains already enumerate these controls; mapping them into D3FEND-style technique entries (artifact, tactic, references) would be a concrete contribution and a near-term option, reviewable against D3FEND's published methodology.

## Edges touched

- [[agentic-soc-cmm|Agentic SOC CMM]] — D6 scores coverage against ATT&CK + D3FEND + ATLAS; this gap is why the D3FEND axis under-counts AI-era defences.
- [[agentic-soc-reference-architecture|Agentic SOC RA]] — its planes already realize the candidate techniques; the layer would catalogue them.
- [[mitre-atlas|MITRE ATLAS]] — the offence-side precedent for extending a MITRE catalogue to the AI era.
- [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]] — the autonomy ladder the supervision and evaluation-gating techniques bound.

## Status notes

Opened 2026-06-03 from the Agentic SOC RA+CMM build. Referenced by the `[!gap]` callouts in the CMM (Open questions) and RA (Gaps in the architecture). No catalogue work has started; the candidate-layer path above is the recommended first step if the gap is taken up.
