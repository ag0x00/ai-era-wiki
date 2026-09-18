---
type: gap
title: "D3FEND AI-Defense Technique Gap"
address: c-000186
created: 2026-06-03
updated: 2026-09-18
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
  - "https://d3fend.mitre.org/about/"
  - "https://d3fend.mitre.org/dao/"
  - "https://d3fend.mitre.org/tactic/d3f:Deceive/"
verified: 2026-09-18
verified_against: []
verified_findings: 0
verified_note: "Read against d3fend.mitre.org — the About page, the digital artifact ontology and the Deceive group — and against the Agentic SOC CMM D6 text; no archived document opened. Six previously unsourced claims now carry a D3FEND deep link or the CMM page; the tactic list corrected to seven and the unbounded 'not catalogued anywhere' claim scoped to D3FEND"
---

# D3FEND AI-Defense Technique Gap

MITRE **D3FEND** encodes a countermeasure knowledge base as a knowledge graph whose types and relations define the cybersecurity countermeasure domain and whose queries map countermeasures onto offensive tactics and techniques.[^d3fend] Where **ATT&CK** catalogues what attackers do, D3FEND catalogues the countermeasures defenders run against it, organized as defensive tactics over a digital-artifact ontology. The [[agentic-soc-cmm|Agentic SOC CMM]]'s **D6 (Detection & Response Tradecraft)** scores a SOC's coverage against three catalogues: ATT&CK for offence, D3FEND for defence, and [[mitre-atlas|MITRE ATLAS]] for threats to AI systems. The D3FEND axis is the one that under-reports, because the AI-era defensive techniques that distinguish an agentic SOC have no entry in it.

## The gap

D3FEND's artifact ontology is built around conventional digital artifacts: its top-level classes are software, digital information, network traffic, process, file, credential, sensor, system and physical artifact, and it defines no class for a prompt, a model, model weights, an agent, an agent plan, a tool call or retrieved context.[^d3fend-dao] Its techniques map countermeasures onto those artifacts, so two AI-era surfaces fall outside the frame:

- **The defender's own agents are not modelled as defensive instruments.** D3FEND has no techniques for supervising an autonomous agent, gating its authority by earned evaluation, attributing an action to a human or an agent, or bounding its blast radius. These are the controls the agentic SOC runs as its core discipline, and they have no D3FEND counterpart.
- **The AI-powered attacker is not modelled as a distinct adversary.** Every one of the eleven techniques under D3FEND's deceive tactic is a decoy environment, honeynet, object, file, session token, persona, credential or network resource — bait keyed to a conventional artifact.[^d3fend-deceive] An attacker operating at machine speed through an agentic pipeline (the [[zero-day-clock|time-to-exploit collapse]]) presents behaviour none of those techniques describes.

[[mitre-atlas|MITRE ATLAS]] reaches the first surface from the threat side: it enumerates attacks on AI systems and attaches mitigations to its techniques, so its defensive content is a mitigation note per offensive technique. The operational technique taxonomy D3FEND supplies for conventional defence has no counterpart there. Across the three catalogues D6 scores against, the AI defender's own agents appear as neither an artifact nor a countermeasure. That is the open space.

## A candidate agentic-SOC defensive-technique layer

A D3FEND-shaped layer for the agentic SOC would extend the artifact ontology with AI-system artifacts — prompt, model, agent plan, tool-call trace, retrieved context, agent identity — and add defensive techniques in five clusters, each already realized as a plane or domain in the [[agentic-soc-reference-architecture|Agentic SOC RA]] and scored by the CMM:

| Technique cluster | What it covers | CMM domain |
|---|---|---|
| Agent supervision | Human-on-the-loop oversight, per-action authority tiers, blast-radius limits, override and revocation paths | D4, D5 |
| Evaluation-gating | Scoring agent decisions against ground truth before autonomy is raised; the gating rule itself as a defensive control | D3 |
| Intent attribution | Distinguishing human from agent activity in telemetry — the [[genai-endpoint-observability-talk\|broken-intent-attribution problem]] — and carrying AI-application telemetry | D1, D5, D6 |
| Deception against AI attackers | Canaries, honeytokens, and behavioural monitoring keyed to agentic-attacker TTPs rather than tool or vulnerability signatures (the [[detection-deception-engineering-orbie-talk\|deception-detection]] direction) | D6 |
| Machine-speed response | Pre-authorized containment that executes at machine speed under deterministic policy gates | D4, D6 |

The last two clusters answer the AI-powered attacker. The first three are the techniques for operating AI defenders accountably, which the [[agentic-soc-cmm|Agentic SOC CMM]] carries as its shared securing-the-agents layer. The deception and machine-speed-response clusters correspond directly to the [[mythos-ready-security-program|Mythos-ready]] Priority Actions for a deception capability and an automated response capability.

## Relationship to D3FEND, ATLAS, and CMM D6

The proposed layer would stand to ATLAS **as D3FEND stands to ATT&CK: the defensive counterpart to an offence catalogue**. ATLAS extended ATT&CK's offence model to AI-system threats; no equivalent extension yet covers the defence half. The layer would sit alongside D3FEND, extending its ontology and tactics, and would be the catalogue [[agentic-soc-cmm|CMM]] D6 scores its AI-era coverage against in place of the under-counting D3FEND axis it uses today. Until the layer exists, D6's D3FEND score reads as a coverage floor for AI-era defences and measures none of them.

## Closure conditions

- **MITRE extends D3FEND.** The ontology gains AI-system artifacts and the tactic set gains agent-supervision, evaluation-gating, and intent-attribution techniques. This is the highest-authority path and the slowest.
- **A community or academic taxonomy emerges** — a "D3FEND-for-AI-defence" counterpart to how ATLAS emerged for AI threats, maintained outside MITRE if MITRE does not move.
- **The wiki authors a candidate layer.** The Agentic SOC RA planes and CMM domains already enumerate these controls; mapping them into D3FEND-style technique entries (artifact, tactic, references) would be a concrete contribution and a near-term option, reviewable against D3FEND's published methodology.

## Edges touched

- [[agentic-soc-cmm|Agentic SOC CMM]] — D6 scores coverage against ATT&CK + D3FEND + ATLAS; this gap is why the D3FEND axis under-counts AI-era defences.
- [[agentic-soc-reference-architecture|Agentic SOC RA]] — its planes already realize the candidate techniques; the layer would catalogue them.
- [[mitre-atlas|MITRE ATLAS]] — the offence-side precedent for extending a MITRE catalogue to the AI era.
- [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]] — the autonomy ladder the supervision and evaluation-gating techniques bound.

## Notes

[^d3fend]: [MITRE — D3FEND: About](https://d3fend.mitre.org/about/), retrieved 2026-09-18. D3FEND is described there as a framework encoding a countermeasure knowledge base, and more specifically a knowledge graph carrying semantically rigorous types and relations that define the key concepts of the cybersecurity countermeasure domain, whose queries inferentially map countermeasures to offensive tactics, techniques and procedures.
[^d3fend-dao]: [MITRE — D3FEND Digital Artifact Ontology](https://d3fend.mitre.org/dao/), retrieved 2026-09-18. The ontology's top-level classes are digital artifact, physical artifact, software, digital information, network traffic, process, file, credential, sensor and system; a read of the class list for a prompt, a model, model weights, an agent, an agent plan, a tool call or retrieved context returned none.
[^d3fend-deceive]: [MITRE — D3FEND Deceive group details](https://d3fend.mitre.org/tactic/d3f:Deceive/), retrieved 2026-09-18. The tactic carries eleven techniques: decoy environment, integrated honeynet, standalone honeynet, connected honeynet, decoy object, decoy public release, decoy file, decoy session token, decoy persona, decoy user credential and decoy network resource.

## Status notes

Opened 2026-06-03 from the Agentic SOC RA+CMM build. Two pages reference it from a `[!gap]` callout: [[agentic-soc-cmm|Agentic SOC CMM]] §Open questions and gaps, and [[agentic-soc-reference-architecture|Agentic SOC RA]] §Gaps in the architecture. The AAI-S [[agentic-ai-security-reference-architecture|reference architecture]] carries a section under the same name and does not reach D3FEND. No catalogue work has started; the candidate-layer path above is the recommended first step if the gap is taken up.
