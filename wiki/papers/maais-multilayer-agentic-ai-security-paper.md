---
type: paper
title: "Securing Agentic AI Systems: A Multilayer Framework"
address: c-000005
created: 2026-05-07
updated: 2026-05-07
tags:
  - papers
  - agentic-ai
  - multi-layer
  - mitre-atlas
  - academic
  - arxiv
status: summarized
publication_date: 2025-12-19
authors:
  - "[[sunil-arora|Sunil Arora]]"
  - "[[john-hastings|John Hastings]]"
arxiv_id: 2512.18043v1
doi: "10.48550/arXiv.2512.18043"
source_url: https://arxiv.org/abs/2512.18043
related:
  - "[[maais-multilayer-agentic-ai-security|MAAIS Framework (framework page)]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
sources:
  - "[[.raw/papers/maais-arora-hastings-2025-12-19.md]]"
---

# Securing Agentic AI Systems — A Multilayer Security Framework — Source Summary

The arXiv preprint that introduced the [[maais-multilayer-agentic-ai-security|MAAIS framework]] (2025-12-19). Authored by [[sunil-arora|Sunil Arora]] and [[john-hastings|John Hastings]]; subject categories cs.CR / cs.AI / cs.CY; preprint, no journal reference.

## Description

A seven-layer defense-in-depth security framework for agentic AI systems, paired with an explicit augmentation of the classical CIA security triad to **CIAA** (adding Accountability). The seven layers are: Infrastructure, Data, Model, Agent Execution & Control, Accountability & Trustworthiness, User & Access Management, Monitoring & Audit. Validation is via mapping each of MITRE ATLAS's twelve adversarial tactics to the responsible layer(s).

## Methodology

Design Science Research (DSR) framing: problem identification → objective definition → artifact design → evaluation. Primary qualitative method is systematic literature review (SLR) of work published from 2022 onward on AI security, agentic AI, AI agents, and agentic AI risks. Validation is the MITRE ATLAS tactic-level coverage exercise.

## Relevance to this corpus

- **CIAA framing** is the cleanest single contribution and is referenced inline on [[agentic-ai-security-cmm-2026|the CMM's D1 (Governance & Accountability)]] as the foundation principle. CIA + Accountability gives the wiki a memorable shorthand for the agentic-AI extension of the classical security triad.
- **Comparative framework** — MAAIS is now the *fourth* multi-layer framework documented in the wiki (alongside [[csa-maestro|CSA MAESTRO]], the wiki's [[agentic-ai-security-cmm-2026|CMM]], and the [[aws-agentic-ai-security-scoping-matrix|AWS Scoping Matrix]]). The framework page contains a side-by-side comparison.
- **Stickiness assessment** at 5 months out: CIAA framing is moderately sticky; MAAIS as a name is unlikely to propagate; the seven-layer structure is convergent in shape but distinct in content cuts from MAESTRO and the CMM.

## Key terms (from the source)

- **MAAIS** — Multilayer Agentic AI Security framework. The paper's named contribution.
- **CIAA** — Confidentiality, Integrity, Availability, Accountability. Augmentation of CIA for agentic systems.
- **DSR** — Design Science Research methodology. Standard IS research framing the paper uses.

## Limitations (per the framework page)

Tactic-level (not technique-level) ATLAS validation; no threat enumeration per layer; no treatment of [[lethal-trifecta|Lethal Trifecta]], [[indirect-prompt-injection|indirect prompt injection]], MCP / A2A risks, or [[promptware|promptware]]; no agency-vs-autonomy distinction; light identity / NHI treatment.

## See also

The full structural analysis — seven layers, ATLAS mapping table, cross-walk to wiki ladders, comparison with MAESTRO / CMM / AWS — lives at [[maais-multilayer-agentic-ai-security|the framework page]]. This source summary is provenance; cite the framework page for substantive claims.
