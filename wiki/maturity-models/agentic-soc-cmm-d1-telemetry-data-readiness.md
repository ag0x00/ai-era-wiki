---
type: maturity-model
title: "Agentic SOC CMM D1 Telemetry and Data Readiness"
address: c-000201
created: 2026-06-03
updated: 2026-06-23
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - telemetry
  - data-readiness
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[security-data-pipeline-architecture]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[mitre-atlas]]"
  - "[[opentelemetry-gen-ai]]"
  - "[[cyber-poverty-line]]"
  - "[[nist-ai-800-4]]"
sources:
  - "[[security-data-pipeline-architecture]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[mitre-atlas]]"
---

# Agentic SOC CMM D1 Telemetry and Data Readiness

Companion deep-dive to [[agentic-soc-cmm|the Agentic SOC CMM]]'s D1 domain. D1 measures whether the data an agent must act on is actually usable by it: telemetry coverage and quality across the monitored estate, the availability of ground truth, and the readiness of cloud-native and internally built GenAI-application telemetry. It scores the [[agentic-soc-reference-architecture|reference architecture]]'s Data & Knowledge plane on the readiness question — whether an agent can be fed real, sufficient data — separately from how the data is moved or stored.

D1 is an **autonomy gate**, and one of the two that govern the first delegation step. With [[agentic-soc-cmm#the-gating-rule|D4 Agent Identity & Action-Authority]], it caps whether a function can reach **L2 — act with approval**: an agent cannot be trusted to propose a consequential action if the data behind the proposal is partial, stale, or absent. Most other domains assume D1 is already in place, because evaluation, observability, and tradecraft all read the same data. It is the foundation the rest of the model rests on.

> [!gap] Wiki-internal calibration
> The level criteria, cost model, and right-sizing below synthesize the wiki's own design spec against three grounding sources: the [[security-data-pipeline-architecture|security data pipeline architecture]] (the load-bearing source on coverage, parsing, and storage economics), the [[genai-endpoint-observability-talk|GenAI endpoint observability talk]] (AI-application and agent telemetry, broken intent attribution), and [[mitre-atlas|MITRE ATLAS]] (the in-house AI-application surface). They are wiki-internal calibration, not an externally ratified standard, and will firm up as the sibling domains and crosswalks are tested.

A single distinction shapes the whole domain.

**D1 measures whether the data is usable by an agent, not how it gets there.** Readiness can be reached two ways: classic ETL with static parsers and centralized storage, or agentic on-demand access with schema-on-read and detection-in-pipeline. Either path satisfies D1. The *capability* to operate without static parsers is an autonomy property of the Data Management function, scored on the autonomy ladder; *whether the data is usable* by either means is this domain. A SOC that pre-normalizes everything into a fixed schema and a SOC that reasons over heterogeneous data on demand can both be D1-mature. Decoupling readiness from centralization lets the model score a SIEM-less stack and a classic SIEM-centric stack on the same scale.

## Control landscape (dated)

The data layer has a mature normalization-and-storage core; the new layers are the decoupled pipeline and the AI-application telemetry that traditional sensors do not emit. The AI-specific particulars are dated and swappable, and are kept here rather than in the level definitions.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Normalization schema | [OCSF](https://ocsf.io/) (Open Cybersecurity Schema Framework) as the vendor-neutral event schema; ASIM and ECS as platform-specific equivalents | GA; OCSF v1.8 shipped March 2026 with AI-observability fields, and ITU ratification as an international standard is expected by mid-2026 |
| Detection content portability | [Sigma](https://sigmahq.io/) as the vendor-agnostic detection-rule format, compiled to each backend's query language | GA and widely supported; the de facto open detection-content interchange |
| Data pipeline (decoupled) | Security data pipeline platforms — Cribl Stream / Search, Tenzir, Anvilogic, Panther, Query.ai; SOC Prime DetectFlow for detection-in-pipeline; search-in-place over object storage and security data lakes (see [[security-data-pipeline-architecture\|the pipeline architecture]]) | Emerging category (market guides date to 2025–2026); detection-in-pipeline and search-in-place are GA in the named products, but performance/scale figures are vendor-reported |
| AI-application telemetry | Agent hooks (pre-tool-call, session-start, prompt-submit) emitting tool-call context over [[opentelemetry-gen-ai\|OpenTelemetry `gen_ai`]] conventions; prompt/agent traces, model-refusal and guardrail events (see [[genai-endpoint-observability-talk\|the endpoint-observability talk]]) | OTel `gen_ai` client spans stabilized in early 2026; agent/framework spans remain experimental; native emission is the exception, not the norm, across AI tools |
| AI-application threat surface | [[mitre-atlas\|MITRE ATLAS]] as the threat model for the in-house AI-application monitored surface — the readiness target that prompt-injection, jailbreak, and agent-hijack telemetry must cover | GA reference; the in-house AI app is a monitored asset class because its attacks rarely leave traditional file/network signatures |
| Small-team open stacks | Wazuh, Security Onion, and the Elastic stack as open ingest-plus-detection foundations; SigmaHQ content layered on top | GA and free at the licensing line; the labor to operate them is the real cost (see [[cyber-poverty-line\|cyber poverty line]]) |

The decisive row in this landscape is **AI-application telemetry**. Endpoint, cloud, identity, and network telemetry are well understood; what most SOCs lack is the signal to attribute an action to an agent rather than the human operating it. [[nist-ai-800-4|NIST AI 800-4]] names the same problems at the field level — fragmented logging across distributed infrastructure and the absence of common terminology and standardized agent identifiers — and the readiness work in this domain (a common schema such as OCSF, the `gen_ai` conventions, and per-agent attribution) is the response to that fragmentation. On the endpoint, a developer and an AI agent running the same shell command produce nearly identical telemetry (same process, same user, same command line), so intent attribution breaks. The durable fix is native tool-call context emitted through a shared schema rather than inference from a process tree. AI attacks against an in-house app such as [[indirect-prompt-injection|indirect prompt injection]], RAG exfiltration, and agent hijack rarely surface as a malicious file or a network anomaly, so the readiness requirement is to capture prompt and agent traces alongside host and packet data.

## Capability levels

Stated as capabilities specific to telemetry and data readiness; cumulative, so Level N assumes every Level N−1 criterion. The level text is mechanism-agnostic, would survive AI normalizing into ordinary tooling, and does not prescribe centralized storage. AI-specific readiness particulars (AI-application telemetry, ATLAS coverage) sit in the control landscape above; the levels describe the rising data quality the gating rule reads.

- **L1 — Initial.** Telemetry collection is ad hoc and partial. Some sources are onboarded, others are not; coverage gaps are unknown rather than measured; there is no consistent schema and no ground-truth store. Any automated function runs on whatever data happens to be present, and a query may silently return an incomplete answer.
- **L2 — Developing.** Coverage of the core monitored estate (endpoint, cloud control plane, identity, network, SaaS) is intentional and inventoried. Telemetry is made usable — either normalized to a common schema such as OCSF on ingest, or accessible on demand with schema applied at read time — and reaches the analysis functions in a consistent shape. This is the readiness floor for **L2 function autonomy (act with approval)**: an agent can propose an action because the data behind the proposal is real and reasonably complete. With D4 satisfied, D1 at this level unblocks the first delegation step.
- **L3 — Defined.** Coverage is measured against the threat model, and gaps are tracked as a managed list rather than discovered during an incident. A ground-truth store exists — labelled true/false positives and confirmed-incident data the evaluation domain reads. The in-house AI-application surface is onboarded as a monitored asset class, with prompt and agent telemetry captured, not only host effects. Data quality is sufficient that a function delegated within bounds (the readiness contribution to **L3**, alongside D3 and D5) is acting on a defensible picture rather than a partial one.
- **L4 — Managed.** Telemetry health is instrumented and governed: source-silence and pipeline-drop detection, freshness and completeness metrics, and schema-drift handling are operational, so a blind spot is alerted rather than inferred after a miss. Ground-truth currency is maintained, not assumed. Whether the stack centralizes or queries in place, the data feeding the most consequential delegated functions carries measured, not hoped-for, coverage and quality.
- **L5 — Optimizing.** Coverage, freshness, and ground-truth quality are continuously optimized against measured drift, and the readiness layer adapts to new sources and new AI-application surfaces without a re-platforming project. Detection runs where it is cheapest (in-stream, in-place, or centralized) as a tuned cost/coverage trade-off rather than a fixed architecture, and AI-application telemetry coverage keeps pace with the org's own GenAI build-out.
- **L5+ — Leading Edge.** All of L5, plus a named contribution to the shared data spine: OCSF schema extensions, OpenTelemetry `gen_ai` field submissions, or published telemetry-coverage or intent-attribution methods that other defenders adopt.

Because D1 is an autonomy gate, its level appears directly in the [[agentic-soc-cmm#the-gating-rule|gating table]]: a function's earned autonomy is capped at the level its weakest governing domain supports, and for the first delegation step D1 is one of the two governing domains. A SOC cannot legitimately delegate triage if the triage agent is fed partial telemetry, however strong its model or its identity controls.

## Right-sizing by org profile

The realistic D1 target is scored against the organization's estate and the data it can source or borrow. A small team consuming a managed provider's telemetry rather than running its own pipeline is right-sized, not immature.

| Band | Realistic D1 target | Why |
|---|---|---|
| Solo / small | L2 → L3 | Near or below the [[cyber-poverty-line\|cyber poverty line]], a small team cannot operate broad collection or a managed pipeline alone. The path is borrowing capability — an MDR/MSSP supplies coverage, normalization, and retention, or an open stack (Wazuh, Security Onion, Elastic) is run lean. AI lowers the barrier here too: schema-on-read and on-demand access cut the ETL/parser engineering that historically gated mature collection. |
| Mid | L3 → L4 | An in-house SOC can run a commercial SIEM/XDR plus a data-pipeline platform, inventory coverage against its threat model, and stand up a ground-truth store. Instrumenting telemetry health and onboarding the AI-application surface is the stretch goal. |
| Enterprise | L4 → selective L5 | A data-pipeline-native, multi-source estate with a security data lake and the scale to justify continuous coverage and freshness instrumentation. L5 earns its cost where the estate is large, heterogeneous, and itself running a growing fleet of in-house AI applications. |

A small SOC at L2, its coverage and normalization supplied by an MDR, has right-sized D1: its agents act on real, sufficient data without the team owning the pipeline. The model records that as an intentional borrow, not a deficiency. Decoupling readiness from centralization makes the lower bands reachable, because schema-on-read and search-in-place turn the old fixed ETL-and-storage barrier into pay-for-what-you-query economics.

## Cost model

The dominant cost in D1 is collection-and-coverage labor plus data volume, not detection licensing. The schema and the open stacks are free at the licensing line; the spend is the engineering to onboard sources and keep coverage honest, and the data-movement and storage bill.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 on an open stack (Wazuh / Security Onion / Elastic) and OCSF; commercial SIEM/XDR where bought | ~0.25–0.5 FTE to onboard core sources, apply a consistent schema, and inventory coverage | Borrowable via MDR/MSSP at near-zero in-house tooling cost for a small team |
| L3 | Add a data-pipeline platform where used; ground-truth labelling tooling | ~0.5–1 FTE recurring: gap tracking against the threat model, ground-truth curation, AI-application onboarding | Ground-truth labelling is load-bearing labor — the evaluation domain cannot score without it |
| L4 | As L3, plus telemetry-health instrumentation; storage/egress for retained volume | Recurring coverage governance, freshness/silence monitoring, schema-drift handling | Detection-in-pipeline and search-in-place trade ingest cost for query cost — price the architecture against the query pattern, not the licence |
| L5 | As L4, tuned across in-stream / in-place / centralized detection | Heaviest: continuous coverage and freshness optimization, keeping pace with the org's AI-application build-out | The spend buys adaptivity — new sources and AI surfaces onboarded without a re-platforming project |

D1 is a coverage-and-volume cost, not a feed-licence cost. The expensive failure is the unknown blind spot: a source that went silent or a surface never onboarded. Telemetry-health instrumentation at L4 is the control that addresses it. Price the coverage rhythm and the data volume, not the schema.

## Open questions

- The gating model places D1 at the L2 step alongside D4. Whether a minimum D1 coverage floor should also be a precondition for higher autonomy — independent of the evaluation and observability gates that govern L3 — is a calibration question the sibling gates do not fully settle.
- Security data pipeline platforms are an emerging category, and the coverage, latency, and scale figures for detection-in-pipeline and search-in-place are vendor-reported, not independently benchmarked (see the [[security-data-pipeline-architecture|pipeline architecture]] gap callout). The cost/coverage trade-off between in-stream, in-place, and centralized detection is not yet independently characterized.
- AI-application telemetry depends on native tool-call emission that most AI tools do not yet provide; coverage of the in-house AI surface is therefore bounded by upstream adoption of the OpenTelemetry `gen_ai` conventions, which the SOC does not control.
- There is no standard metric for telemetry coverage completeness. A SOC can inventory the sources it onboarded, but scoring coverage against the threat model — including AI-application attacks that leave no traditional signature — relies on judgment, not a ratified benchmark.

## Relations

- Companion deep-dive to [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|the Agentic SOC CMM]]'s D1 domain, which classifies D1 as an autonomy gate and one of the two that govern the L2 delegation step.
- Scores the [[agentic-soc-reference-architecture|reference architecture]]'s Data & Knowledge plane on the readiness question, separately from how the data is moved or stored.
- The other L2 gate is D4 Agent Identity & Action-Authority; the higher autonomy steps add D3 Evaluation & Ground-Truth and D5 Observability & Oversight (L3), then D7 Resilience & Agent Supply Chain and D8 People & Governance (L4). The efficacy-gate sibling that reads the same data is [[agentic-soc-cmm-d2-threat-intel-knowledge|D2 Threat Intelligence & Knowledge]].
- The data-readiness paths — classic ETL versus agentic on-demand access — are detailed in [[security-data-pipeline-architecture|Security Data Pipeline Architecture]], which separates *whether the data is usable* (this domain) from *the capability to operate without static parsers* (a Data Management autonomy property).
- AI-application telemetry and the broken-intent-attribution problem are grounded in [[genai-endpoint-observability-talk|GenAI Endpoint Observability for Detection Engineers]]; the in-house AI-application threat surface is modelled by [[mitre-atlas|MITRE ATLAS]] in D6 Tradecraft.
