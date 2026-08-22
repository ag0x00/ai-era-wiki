---
type: concept
title: "Security Data Pipeline Architecture"
created: 2026-06-03
updated: 2026-06-23
tags:
  - concepts
  - agentic-soc
  - security-data-pipeline
  - siem-less
  - detection-engineering
status: developing
origin: aggregated
address: c-000183
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-state-of-the-field]]"
  - "[[agentic-soc-autonomy-ladders]]"
sources:
  - "[[soc-prime-detectflow-datasheet-2026.pdf]]"
  - "https://cribl.io/resources/ds/cribl-search/"
  - "https://socprime.com/blog/what-is-a-security-data-pipeline-platform/"
  - "https://www.anvilogic.com/learn/bg-data-lake"
  - "https://softwareanalyst.substack.com/p/the-rise-of-security-data-pipeline"
---

# Security Data Pipeline Architecture

A shift in security-operations architecture: the security data layer is decoupled from the SIEM, so storage, detection, and search no longer live in one centralized, ingest-everything system. The pattern is called a security data pipeline platform (SDPP), SIEM-less or decoupled SIEM, and data-pipeline-native detection. It recasts the SIEM from a data warehouse into a query engine that reasons over decentralized data instead of owning all of it. For the agentic SOC it is the concrete form of the data plane, and it removes the cost, retention, and parsing barriers that gated traditional log management.

## The decoupled stack

Three capabilities, previously fused inside the SIEM, are now separable layers:

- **Pipeline.** Collect, reduce, enrich, normalize, and route telemetry from any source to any destination, vendor-agnostically, before it lands anywhere. Cribl Stream and the open-core Tenzir are examples; the category also includes Abstract Security, DataBahn, and hyperscaler entrants such as CrowdStrike Falcon Onum and SentinelOne data pipelines.
- **Detection-in-pipeline.** Run detections on the live stream before data reaches the SIEM. SOC Prime DetectFlow applies Sigma rules to Apache Kafka topics through a five-stage Apache Flink pipeline (Parse, Map, Filter, Match, Tag). It tags matched events with detection metadata (rule ID, severity, MITRE ATT&CK technique IDs) and routes them to a SIEM, SOAR, or data lake. Its datasheet ([socprime.com](https://socprime.com/)) reports sub-second detection latency (0.005 s), over 12,000 rules per pipeline, and a 600,000-rule library. It targets two failure modes: SIEM detection degrades past roughly 500 custom rules, and "store-everything" log growth becomes unaffordable. The figures are vendor-reported.
- **Search-in-place / federated search.** Query data where it already lives — object storage such as Amazon S3 or Azure Blob, APIs, data lakes — without indexing or ingesting it first. Cribl Search and Query.ai's security data mesh decouple compute from storage; Anvilogic's SIEM-less model and composable platforms such as Panther place a queryable detection layer on top of a customer-owned data lake.

## The barrier it removes

> Centralized storage, ETL normalization, and SIEM rule-scaling limits were the cost-and-complexity barriers that kept mature detection out of reach for small teams. Moving detection into the pipeline and search to where the data lives turns those fixed barriers into pay-for-what-you-query economics. That is why this architecture matters for the org-profile floor, not only for the enterprise.

Traditional SIEM economics scale with ingest volume: every log stored and indexed costs money whether or not it is ever queried, and rule engines degrade as custom content grows. The decoupled model breaks that coupling. Low-value events are filtered or routed before ingestion, retention moves to cheap object storage, and detection runs where it is cheapest, in-stream or in-place.

## The agentic angle

The architecture is increasingly AI-assisted in two ways. First, agentic data understanding lowers the ETL barrier: AI-assisted field mapping (SOC Prime's Uncoder AI) and on-demand schema interpretation reduce the need for hand-written parsers and static normalization. Second, AI is becoming the forcing function for a federated layer that correlates heterogeneous data across distributed stores, writes cross-platform detection queries, and reconciles schema drift faster than a human can. This is the practical path to the no-static-parsers end state: the agent reasons over heterogeneous data on demand instead of requiring it pre-parsed into a fixed schema.

## Relevance to the agentic SOC

This architecture is the substrate for the [[agentic-soc-state-of-the-field|agentic SOC]]'s data operations:
- It realizes the **data-management** function in its data-pipeline-native form — ingest, route, access, and retain.
- It is the concrete form of the reference architecture's **data and knowledge plane**: detection-in-pipeline, search-in-place, and on-demand correlation rather than a centralized store.
- It separates two questions the maturity model keeps distinct: *whether the data is usable* (the telemetry-and-data-readiness gate, now reachable by on-demand access as well as classic ETL) from *the capability to operate without static parsers* (an autonomy property — the agent understands data on demand).

## Convergence

The pipeline, lakehouse, and SIEM layers are converging toward an integrated platform that ingests, stores, and detects on a pipeline foundation: a next-generation SIEM built bottom-up from the data pipeline rather than the query console. Whether that reconverges into a single product or remains a composable stack is unsettled.

> [!gap] Emerging category, vendor-reported claims
> Security data pipeline platforms are an emerging category (market guides date to 2025–2026), and the performance and scale figures here are vendor-reported, not independently benchmarked. A deeper treatment — independent cost/coverage comparison, the OCSF normalization angle, and the security-data-lake storage tier (Cribl Lake, Amazon Security Lake) — is warranted before the reference architecture's data plane is finalized.

## Relations

- Grounds the data plane of the planned [[agentic-soc-reference-architecture|agentic SOC reference architecture]] and the data-management function described in [[agentic-soc-state-of-the-field|Agentic SOC State of the Field]].
- Detection-in-pipeline tagging with ATT&CK metadata connects to the detection-engineering tradecraft discussed in [[agentic-soc-autonomy-ladders|Agentic SOC Autonomy Ladders]].

<!-- sources:auto -->
## Sources

- [cribl.io](https://cribl.io/resources/ds/cribl-search/)
- [socprime.com](https://socprime.com/blog/what-is-a-security-data-pipeline-platform/)
- [anvilogic.com](https://www.anvilogic.com/learn/bg-data-lake)
- [softwareanalyst.substack.com](https://softwareanalyst.substack.com/p/the-rise-of-security-data-pipeline)
<!-- /sources -->
