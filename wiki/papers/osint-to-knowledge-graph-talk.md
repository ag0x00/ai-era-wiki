---
type: talk
title: "OSINT to Knowledge Graph for Threat Intel"
address: c-000172
created: 2026-06-02
updated: 2026-07-30
tags:
  - papers
  - talks
  - threat-intelligence
  - knowledge-graph
  - osint
  - palo-alto-networks
status: summarized
scope_axis:
  - ai-in-sec-defense
origin: aggregated
year: 2026
authors: ["Dongdong Sun"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 4, 2026 (Day 2 / Stage 2)"
key_claim: "A production AI pipeline at Palo Alto Networks converts ~10,000 unstructured OSINT reports per week into a semi-structured knowledge graph that downstream agents query with per-statement source citations, grounding answers in curated graph data rather than model knowledge."
methodology: "Practitioner talk with live query demo. Transcript and slide deck captured to .raw/talks/."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[automated-prompt-optimization|Automated Prompt Optimization]]"
  - "[[syara-semantic-detection-talk|SYARA Semantic Detection]]"
  - "[[detection-deception-engineering-orbie-talk|Orbie / GreyNoise]]"
  - "[[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[palo-alto-networks|Palo Alto Networks]]"
  - "[[dongdong-sun|Dongdong Sun]]"
  - "[[cti-realm|CTI Realm]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
  - .raw/talks/2026-03-04_Dongdong-Sun_From-OSINT-Chaos-to-Knowledge-Graph_transcript.md
  - .raw/talks/2026-03-04_Dongdong-Sun_From-OSINT-Chaos-to-Knowledge-Graph_slides.pdf
---

# OSINT to Knowledge Graph for Threat Intel

A practitioner talk by [[dongdong-sun|Dongdong Sun]], a machine-learning engineer on threat intelligence at [[palo-alto-networks|Palo Alto Networks]], at the March 2026 [[unprompted-conference-march-2026|Unprompted Conference]].

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#)

## The Problem

Open-source threat intelligence is free to obtain but expensive to use. Most of its value sits in unstructured prose — vendor blog posts, incident write-ups, malware analyses, advisories — not in the indicator feeds that an EDR can plug in and check directly. Indicator feeds carry the atomic facts but drop the contextual clue: what an indicator means, which actor and campaign it belongs to, how it was used.

Palo Alto Networks tracks over 200 sources that together publish roughly 10,000 reports per week. No analyst team can read that volume at the speed intel stays current. The data also fragments: different vendors describe the same event with partial, overlapping coverage, so each report alone gives only a slice. The corpus must be read holistically to recover the full picture, and that is precisely what manual reading cannot scale to.

## Reframe to Question-Answering

Rather than ask analysts to read everything, the pipeline inverts the problem: let users ask what they actually need. Representative questions are "what malware does APT29 use?" and "I saw these indicators in my logs — what does the open-source community already know about them?"

A language model cannot answer these by ingesting the raw corpus. Context windows do not hold it, and a model's parametric knowledge goes stale. The reports need a structure that both holds the facts and supports retrieval. Cybersecurity data is naturally interconnected — actors use malware, malware leaves indicators, campaigns hit victims — so a knowledge graph is the structure that fits.

## Basis for a semi-structured graph

STIX is machine-readable but verbose, hard for humans to read, and grows long quickly. It is also a poor fit for language models, which prefer unstructured text yet cannot absorb an unbounded amount of it. The design choice is a **semi-structured** format: constrain the entity types and the allowed relationships, but retain unstructured descriptions that explain each edge and supply the context a bare triplet would lose.

A worked example anchors the format. A BeyondTrust critical-vulnerability report — about 11 minutes to read, covering the malware vShell and SparkRAT, the attack scope, live shell activity, prior BeyondTrust vulnerabilities, and a closing indicator list — yields over 100 entities and over 100 relationships once extracted. Zooming out, graph expansion links the report's nodes to a prior 2024 BeyondTrust exploitation and to cross-campaign attack patterns such as ingress tool transfer, which recurs across many actors and campaigns.

## Extraction Pipeline

A language model cannot reliably one-shot ~100 entities from a dense report, so extraction is split into stages.

- **Skeleton extractor as router.** A first pass scans the report for anything worth extracting and routes each finding to a per-entity-type extractor. Each type-specific extractor pulls detailed fields, grounded to the report's own phrasing wherever possible.
- **Attack-pattern induction with self-correction.** Reports rarely carry a MITRE ATT&CK ID. Asking the model directly for the technique produces fuzzy recall — many similar techniques, much noise. Instead the model reads the described behavior, induces a few candidate techniques, then reads the related techniques from a knowledge base and self-corrects to a final technique. (The closest existing MITRE page on the wiki is [[mitre-atlas|MITRE ATLAS]]; the talk grounds against ATT&CK.)
- **Constrained relationship extraction.** With 100+ entities the possible relationships explode, so extraction is limited to an allowed set of entity-relationship triplets. Each edge carries a natural-language description rather than a bare verb, preserving the contextual clue of how the relationship actually held.

## Querying Agent

An agent interacts with the graph the way an analyst would. It pivots from a node (search), walks the graph to assemble a subgraph, takes multiple hops, runs semantic search, and uses **co-occurrence** — entities mentioned in the same report — to surface relationships that the ontology does not encode but the source text implies.

Every answer is grounded with per-statement **source citations**: each statement carries a bracketed pointer to the node or report that supplied it. This forces reliance on curated graph data over model knowledge. The agent also returns an **investigation graph** showing the steps and entities it traversed to reach the answer.

Demonstrated queries:

| Query | What the agent does |
|---|---|
| Most recent BeyondTrust vulnerability and its exploitation chain | Searches for BeyondTrust nodes, finds the latest CVE, pulls a subgraph (related actor, related campaigns), returns an attack chain |
| What malware does APT28 use? | Resolves multiple APT28 aliases to one actor, multi-hops and semantic-searches to recent malware and tooling |
| What vulnerabilities does APT28 exploit, and which other actors exploit them? | Pivots actor → vulnerability, then hops back to other actors on the same exploit relationship |
| Any relationship between APT28 and APT29? | Finds a connecting path, uses co-occurrence, reports common tooling and infrastructure, shared strategic targeting, and attribution to Russian GRU versus SVR |

## Evaluation

Sun spends more than half the project's time on evaluation, framing it as the only real indicator of data quality for threat intelligence: if the extracted graph is wrong, every downstream agent answer inherits the error.

No benchmark existed for extracting entities and relationships from long threat reports, so the team built one. Fact-checking against external sources only covers what is already known publicly; behavior-induced attack patterns and facts missing from a report cannot be validated that way, so human curation remains necessary. To keep researcher effort low, the loop starts with a small researcher-curated dataset, has the model memorize it, runs inference on a fresh batch, and after two or three iterations reaches roughly 90% correct — leaving only the minority of cases for a researcher to check.

At scale the team uses **automatic prompt optimization**, but standard APO loops fail on this task: the model rewrites the prompt to fix one observed error and breaks another, producing an inescapable loop. The fix is to split the prompt into an **LLM-editable section and a human-editable section**, which stops the model from undoing prior corrections.

The reflection-and-aggregation step produced an unplanned finding: it surfaced **inconsistencies between human annotators** — one labels something malware, another a tool; one extracts a campaign, another does not. The system lists these conflicts and routes them back to threat researchers to resolve. (Annotator disagreement and the ">50% of time on evaluation" pattern also appear in [[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]].)

[[llm-as-a-judge|LLM-as-a-Judge]] runs in production but is secondary to the curated evaluation dataset. Cross-checking also runs against Palo Alto's internal threat-intel sources, not OSINT alone.

## Notable Findings

A newer model does not necessarily score better on this benchmark. More striking, a "thinking" model did worse than a non-thinking model: the thinking model over-relied on its internal knowledge, while the task demands grounding strictly in the source and reducing model bias as far as possible.

A Q&A exchange reinforced the anti-hallucination design. Asked whether the system might conclude that APT28 and APT29 are related simply because both carry "bear" aliases — the kind of inference an ungrounded model would make — Sun pointed to the grounding mechanism: the agent answers only from graph evidence and cites the source node per statement, so alias coincidence cannot drive an attribution claim.

## Placement

This talk is the *upstream* counterpart to two Palo Alto-adjacent talks the wiki tracks. [[syara-semantic-detection-talk|SYARA]] (Mohamed Nabeel, Palo Alto Networks) addresses detection over telemetry with cost-ordered semantic layers. [[detection-deception-engineering-orbie-talk|Orbie]] (GreyNoise Labs) addresses an agent over internet-scale honeypot data. Sun's work is the threat-intel ingestion layer — distinct from telemetry, but the substrate the others lean on when an alert needs context about an actor or campaign.

Within the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis, the queryable graph is the structural form that lets downstream agents reach for context without re-parsing the corpus on each call — the precondition for tool-use over CTI at scale (see [[cti-realm|CTI-REALM]]). The grounding discipline — per-statement citation, an investigation graph, evaluation as the gate on quality — is the same trust-engineering posture seen across the conference's defensive-agent talks.

This talk is the sourced basis for the constraint recorded on [[automated-prompt-optimization|Automated Prompt Optimization]]: a naive optimization loop over threat-report extraction never converges, because each fixed false positive introduces another. The fix reported here — splitting the objective rather than optimizing one blended score — is what that page cites as the practical bound on the technique.

## Status

`summarized` — based on the transcript and slide deck captured to `.raw/talks/`.
