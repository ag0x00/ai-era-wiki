---
type: maturity-model
title: "Agentic SOC CMM D2 Threat Intelligence and Knowledge"
address: c-000202
created: 2026-06-03
updated: 2026-06-03
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - threat-intelligence
  - knowledge-graph
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[osint-to-knowledge-graph-talk]]"
  - "[[mate-cd-cr-continuous-detection-response]]"
  - "[[zero-day-clock]]"
  - "[[nist-ir-8596-cyber-ai-profile]]"
  - "[[mitre-atlas]]"
  - "[[cyber-poverty-line]]"
  - "[[cti-realm]]"
  - "[[vulnops]]"
sources:
  - "[[osint-to-knowledge-graph-talk]]"
  - "[[mate-cd-cr-continuous-detection-response]]"
  - "[[zero-day-clock]]"
  - "[[nist-ir-8596-cyber-ai-profile]]"
---

# Agentic SOC CMM D2 Threat Intelligence and Knowledge

Companion deep-dive to [[agentic-soc-cmm|the Agentic SOC CMM]]'s D2 domain. D2 measures the grounding and knowledge layer the analysis functions draw on: threat-intelligence ingestion and curation (OSINT into a queryable knowledge graph), the currency of that knowledge, and coverage of novel threats that lagging KEV and CVE feeds do not yet carry. Its *ingestion and curation* live on the [[agentic-soc-reference-architecture|reference architecture]]'s Data & Knowledge plane; its *application* runs through detection engineering, triage, hunting, and exposure/VulnOps.

D2 is an **efficacy gate, not an autonomy gate.** This is the distinction that governs how it scores. The autonomy gates (D1, D3, D4, D5, D7, D8) determine whether an agent can be trusted to act with less supervision; a function's earned autonomy ceiling is set by its least-mature *autonomy* gate. D2 does not cap that ceiling. A SOC can run triage or hunting at L3 with every autonomy gate satisfied and still surface nothing of value if its threat-intel grounding is stale or blind to the threat in front of it. D2 therefore caps the *value* of whatever autonomy is exercised, not the autonomy itself. A function can be both fully delegated and useless at once: D2 is the domain that measures the second half of that sentence. It is scored like every other domain, but a low score is read as degraded output quality, never as a reason to throttle delegation.

> [!gap] Wiki-internal calibration
> The level criteria, cost model, and right-sizing below synthesize the wiki's own design spec against four grounding sources: a production OSINT-to-graph pipeline ([[osint-to-knowledge-graph-talk|Palo Alto Networks]]), the investigation-compresses-to-detection framing ([[mate-cd-cr-continuous-detection-response|Mate CD/CR]]), the [[zero-day-clock|Zero Day Clock]] on the structural lag of KEV/CVE feeds, and the [[nist-ir-8596-cyber-ai-profile|NIST IR 8596]] "Defend" focus area. They are wiki-internal calibration, not an externally ratified standard, and will firm up as the sibling domains and crosswalks are tested.

## Control landscape (dated)

Threat-intelligence tooling has a mature interchange and storage layer; the new layer is AI-built knowledge graphs and AI-assisted intel triage, which sit on top of it. The AI-specific particulars are dated and swappable, and are kept here rather than in the level definitions.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Knowledge spine | MITRE ATT&CK as the shared technique vocabulary; CVE/CWE for vulnerabilities; [[mitre-atlas\|MITRE ATLAS]] for the in-house AI-application surface | GA and stable; the de facto common language detections and intel map against |
| Interchange and storage | STIX 2.1 / TAXII 2.1 for structured intel exchange; MISP and OpenCTI as open intel platforms; commercial TIPs | GA; STIX is machine-readable but verbose and a poor fit for direct LLM consumption (see [[osint-to-knowledge-graph-talk\|the OSINT-to-graph talk]]) |
| Feeds and sources | CISA KEV and VulnCheck KEV; commercial and OSINT indicator feeds; ISAC / sector intel-sharing groups | GA; KEV/CVE are structurally lagging signals — they capture *observed* exploitation, not novel discovery (see [[zero-day-clock\|Zero Day Clock]]) |
| AI ingestion and curation | LLM extraction of entities and relationships from unstructured OSINT into a semi-structured knowledge graph, with per-statement source citation and an evaluation harness gating extraction quality | Production at scale at a large vendor ([[osint-to-knowledge-graph-talk\|Palo Alto Networks]], ~10,000 reports/week into a queryable graph); a leading-edge pattern, not yet a packaged product for most SOCs |
| Knowledge graph in the loop | Investigation-side context graph where closed investigations compress into context-specific detections ([[mate-cd-cr-continuous-detection-response\|Mate Security Context Graph]]); queryable CTI graph as the substrate for tool-use over intel ([[cti-realm\|CTI-REALM]]) | Vendor-coined and directional; no independent benchmark yet |

The defining shift in this landscape is the move from *feeds an analyst reads* to *a graph an agent queries*. Indicator feeds carry atomic facts but drop the context (which actor, which campaign, how an indicator was used), and that context is what grounds a downstream agent's answer. The OSINT-to-graph pattern preserves it by attaching a natural-language description to each edge and citing the source node per statement, which forces the agent to answer from curated graph evidence rather than model knowledge.

## Capability levels

Stated as capabilities specific to threat-intel grounding; cumulative, so Level N assumes every Level N−1 criterion. The level text is mechanism-agnostic and would survive AI normalizing into ordinary tooling. Because D2 is an efficacy gate, the levels describe rising output quality and grounding discipline, not a rising autonomy ceiling.

- **L1 — Initial.** Threat intelligence is ad hoc. Indicator feeds and CVE/KEV lists are consumed manually or not at all; analysts hold the actor and campaign context in their heads. There is no shared, queryable knowledge store, and grounding for any automated function is whatever the model already knows.
- **L2 — Developing.** A standing threat-intel function exists. Feeds are normalized to a common schema (STIX/TAXII or a TIP such as MISP / OpenCTI), tied to MITRE ATT&CK, and stored where the analysis functions can reach them. Intel is curated, not merely collected. At this level a function's automated output can be grounded in *current* intel rather than the model's parametric knowledge, the minimum for the value of any delegated function to track reality.
- **L3 — Defined.** Intel is structured as a queryable knowledge graph (actors, malware, campaigns, indicators, techniques, with relationships), and the analysis agents ground answers in it with traceable source attribution per claim. Ingestion is continuous and curation has an evaluation step that gates extraction quality, on the principle that a wrong graph poisons every downstream answer. Co-occurrence and graph-walk retrieval surface relationships the ontology does not encode. At L3 the grounding is good enough that a delegated triage or hunting agent's conclusions are auditable back to cited evidence.
- **L4 — Managed.** The knowledge graph's *currency* and *coverage* are measured and managed, not assumed. Ingestion latency from source publication to graph availability is tracked; gaps against the org's threat model are identified and closed; annotator and source conflicts are surfaced and routed to resolution. Closed investigations feed back into the knowledge layer — the [[mate-cd-cr-continuous-detection-response\|investigation-as-ground-truth]] loop — so coverage compounds as a byproduct of operations rather than depending on vendor rule libraries. Grounding is now reliable enough that the most consequential delegated functions inherit measured, not hoped-for, context quality.
- **L5 — Optimizing.** The SOC's grounding is not bounded by lagging public feeds. Novel-threat coverage is addressed directly: behavior-induced technique attribution (inferring a technique from described behavior rather than waiting for an assigned ATT&CK ID), enrichment from internal telemetry and first-party detections, and intel-sharing that closes the gap KEV/CVE structurally cannot — because a vulnerability discovered by an AI system has no KEV listing by definition (see [[zero-day-clock\|Zero Day Clock]]). Knowledge currency, extraction accuracy, and coverage-against-threat-model are continuously optimized against measured drift.
- **L5+ — Leading Edge.** All of L5, plus a named contribution to the shared knowledge spine: published extraction benchmarks for long threat reports, ATT&CK or ATLAS technique submissions, or open intel-sharing infrastructure that other defenders consume.

Because D2 is an efficacy gate, none of these levels appears in the gating table that caps a function's autonomy. A SOC reads its D2 level alongside a function's autonomy: a delegated function (L4 autonomy) governed by weak D2 acts fast on weak grounding, and the prescriptive output is "improve grounding," not "reduce delegation."

## Right-sizing by org profile

The realistic D2 target is scored against the organization's scale and the intel it can source or borrow. A small team grounding its agents on a borrowed, curated feed is right-sized, not immature.

| Band | Realistic D2 target | Why |
|---|---|---|
| Solo / small | L2 → L3 | Near or below the [[cyber-poverty-line\|cyber poverty line]], a small team cannot build or curate a knowledge graph from raw OSINT. The path is borrowing capability: ISAC / sector-group intel and MDR/MSSP-provided grounding give curated, current context the team consumes rather than produces. ISACs socialize sector-specific intel a small SOC could never gather alone. |
| Mid | L3 → L4 | An in-house intel function can stand up a queryable graph on open platforms (MISP / OpenCTI) and ground its higher-volume delegated functions in it, with a curation evaluation step. Measuring currency and closing coverage gaps is the stretch goal. |
| Enterprise | L4 → selective L5 | A dedicated intel team and the source volume to justify an AI ingestion pipeline. L5 novel-threat coverage earns its cost where the org is a likely target of AI-driven discovery and the lag in public feeds is an operational exposure. |

A small SOC at L2 grounded by a sector ISAC has right-sized D2: the value of its delegated functions tracks real, current intel without the team owning the pipeline. The model records that as an intentional borrow, not a deficiency.

## Cost model

The dominant cost in D2 is curation labor and evaluation, not licensing. Interchange formats and open platforms are free; the spend is the human and compute effort to keep the knowledge correct and current.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 on open platforms (MISP / OpenCTI); commercial TIP/feed subscriptions optional | ~0.25–0.5 FTE to normalize feeds, map to ATT&CK, and curate | Borrowable via ISAC / MDR at near-zero tooling cost for a small team |
| L3 | Graph store and an LLM extraction budget; commercial graph CTI where bought | ~0.5–1 FTE recurring: graph curation plus the evaluation loop that gates extraction quality | Evaluation is the load-bearing labor — a wrong graph poisons every downstream answer, so it is not optional |
| L4 | As L3, plus currency/coverage instrumentation | Recurring curation, conflict resolution between sources/annotators, and the investigation-to-knowledge feedback loop | The investigation-side loop offsets some cost — coverage grows from work already done |
| L5 | As L4, plus internal-telemetry enrichment and intel-sharing infrastructure | Heaviest: behavior-induced attribution, novel-threat coverage, continuous drift measurement | The spend buys independence from lagging public feeds, justified by target profile |

D2 is a curation-and-evaluation cost, not a feed-subscription cost. In the production OSINT-to-graph case, more than half the project's effort went to evaluation, because for threat intelligence the quality of the extracted graph is the only reliable indicator of whether downstream grounding can be trusted. Price the curation rhythm, not the first feed.

## Open questions

- The gating model treats D2 as an efficacy gate that does not cap autonomy. Whether some minimum D2 floor should be a *precondition* for delegating an intel-dependent function — rather than only a quality signal — is a calibration question the sibling autonomy gates do not settle.
- The OSINT-to-graph extraction-accuracy figure reported by [[osint-to-knowledge-graph-talk|Sun's OSINT-to-knowledge-graph talk]] (roughly 90% after a few curation iterations in one production pipeline)[^sun-osint] is a single-vendor data point, not an industry benchmark. No standard benchmark exists for extracting entities and relationships from long threat reports.
- The investigation-compresses-to-detection loop ([[mate-cd-cr-continuous-detection-response\|CD/CR]]) is vendor-coined and directional; its coverage-compounding claim is qualitative and not independently evaluated.
- Novel-threat coverage beyond KEV/CVE is structurally hard to *measure*: a SOC cannot easily score coverage of threats that have no public catalogue entry. The L5 criterion names the capability; a rigorous metric for it is an open gap.

## Relations

- Companion deep-dive to [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|the Agentic SOC CMM]]'s D2 domain, which classifies D2 as an efficacy gate alongside D6 Tradecraft.
- Scores the threat-intel knowledge graph on the [[agentic-soc-reference-architecture|reference architecture]]'s Data & Knowledge plane; its application runs through the detection, triage, hunting, and VulnOps agent surfaces.
- Sibling autonomy gates that *do* cap a function's autonomy: D1 Telemetry & Data Readiness, D3 Evaluation & Ground-Truth, D4 Agent Identity & Action-Authority, D5 Observability & Oversight, D7 Resilience & Agent Supply Chain, D8 People & Governance. The contrasting efficacy gate is D6 Detection & Response Tradecraft.
- Grounded by [[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph for Threat Intel]] (ingestion and curation), [[mate-cd-cr-continuous-detection-response|Mate CD/CR]] (investigation-to-detection compression), [[zero-day-clock|Zero Day Clock]] (the structural lag of KEV/CVE), and [[nist-ir-8596-cyber-ai-profile|NIST IR 8596]]'s "Defend" focus area.
- The novel-threat-coverage argument connects to [[vulnops|VulnOps]] and [[cti-realm|CTI-REALM]]: continuous discovery and tool-use over a CTI graph are the operational responses to feeds that arrive after exploitation.

## Notes

[^sun-osint]: Dongdong Sun (Palo Alto Networks), *From OSINT Chaos to Knowledge Graph: Production-Scale AI-Powered Threat Intel*, Unprompted Conference I, March 2026. Abstract: [unpromptedcon.org/abstract-march2026](https://unpromptedcon.org/abstract-march2026/). Wiki summary: [[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph]]. Practitioner figure, not an industry benchmark.
