---
type: maturity-model
title: "Agentic SOC CMM D5 Observability and Oversight"
address: c-000205
created: 2026-06-03
updated: 2026-06-23
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - observability
  - oversight
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[opentelemetry-gen-ai]]"
  - "[[agent-observability]]"
  - "[[behavioral-anomaly-detection-for-agents]]"
  - "[[llm-as-a-judge]]"
  - "[[oversight-layer]]"
  - "[[non-human-identity]]"
  - "[[nist-ai-800-4]]"
sources:
  - "[[agentic-soc-cmm]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[agent-observability]]"
  - "[[opentelemetry-gen-ai]]"
---

# Agentic SOC CMM D5 Observability and Oversight

Companion deep-dive to the [[agentic-soc-cmm|Agentic SOC CMM]]'s D5 domain. D5 measures whether the SOC can *see and override* its own agents: whether agent activity is instrumented as telemetry, whether that telemetry distinguishes a human's action from an agent's, whether a human-on-the-loop oversight surface lets an operator monitor and intervene in flight, and whether every agent action lands in a tamper-evident audit record. It is one of the two autonomy gates for **L3** function autonomy (autonomous in-bounds): an agent may not run unsupervised within bounds until the SOC can watch what it is doing and stop it. The domain scores the **Observability & Evaluation plane** of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] and the human-authority boundary where oversight is exercised.

D5 and D3 (Evaluation & Ground-Truth) read some of the same telemetry, and both gate L3, but they are different disciplines. D3 *scores* an agent's decisions against ground truth to decide whether its output can be trusted. D5 is about *visibility and control*: seeing the agent act in real time and being able to override it, independent of whether any retrospective score has been computed. An eval harness consumes D5's telemetry; it does not replace D5's oversight surface.

D5 is the SOC's instance of the **securing-the-agents shared layer**, overlapping the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]'s D7 (Observability & Detection). This page does not restate that material; it cross-references it. The SOC-specific twist is a duality the application-security pair does not carry: the SOC both *consumes* agent telemetry as a defender (it monitors AI activity across the estate, including in-house AI applications) and *emits* it as a defended surface (its own response, triage, and hunting agents are an observed asset class). The same `gen_ai` conventions that let the SOC detect a rogue third-party agent let it detect a misbehaving agent of its own.

> [!gap] Wiki-internal calibration
> The level criteria and the L3 gating threshold are wiki-internal calibration synthesized from the CMM design spec, the reference architecture, and the grounding sources below, not an externally ratified standard. The control landscape names real, dated tools and standards; the maturity ladder and the gate are the model's own and will firm up as the SOC pair is stress-tested against practitioner deployments.

The monitoring and oversight challenges this domain measures are catalogued at the field level by [[nist-ai-800-4|NIST AI 800-4]], which documents the gaps and barriers in monitoring deployed AI — including the lack of direct visibility into model properties and the difficulty of detecting deceptive or monitor-evading behavior — and treats agentic systems as a recurring lens. The load-bearing problem this domain has to solve is **broken intent attribution**, named by [[genai-endpoint-observability-talk|Mika Ayenson (Elastic)]]: a developer and an AI agent running the same shell command produce near-identical endpoint telemetry — same PID, same user, same command line — so EDR records what ran but not what drove it. Until that gap closes, a SOC cannot reliably tell its own agents' actions apart from its analysts', and "override the agent" has no clean target. The durable fix routes agent-hook signals (pre-tool-call, session-start, prompt-submit) into [[opentelemetry-gen-ai|OpenTelemetry `gen_ai` semantic conventions]] that any SIEM or EDR can ingest, so the action carries its own attribution rather than leaving the SOC to infer it from a process tree.

## Control landscape (dated)

The controls are telemetry conventions, the agent-hook instrumentation that emits them, identity-multiplexing of logs, the oversight surfaces that let a human watch and intervene, and the audit pipeline that retains the record. Most are general-purpose observability technology applied to the SOC's own agents; the oversight and override surfaces are usually carried by the orchestration or response platform.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| GenAI telemetry conventions | [[opentelemetry-gen-ai\|OpenTelemetry `gen_ai.*` semantic conventions]] — standardized span and attribute names for model calls, tool calls, retrieval, and agent steps, vendor-neutral over OTLP | **Experimental** — the SemConv is not yet stable; pin the convention version so a bump does not silently break detection or oversight wiring |
| Agent-hook instrumentation | pre-tool-call / session-start / prompt-submit hooks emitting tool name, approval state, provider, prompt, and model reasoning into OTel ([[genai-endpoint-observability-talk\|the hooks-to-EDR pattern]]); coding-tool hooks GA | Hooks GA; a few tools (for example Claude Code) emit OTel natively; broad native emission across tools is still catching up |
| Intent attribution | identity-multiplexing of logs — inject `agent_id` / `session_id` / `trace_id` so every action traces to the agent and the invoking human ([[agent-observability\|Agent Observability §3]]); native tool-call context over OTel | Pattern-level; GA where per-agent identity exists (the D4 dependency). Native attribution beats the brittle parent-process-name heuristic |
| Behavioral / anomaly monitoring | [[behavioral-anomaly-detection-for-agents\|per-agent behavioral baselines]] over agent / user / org axes; [[llm-as-a-judge\|LLM-as-a-judge]] checks on agent reasoning for anomaly detection; COTS drift detectors | Production at hyperscaler scale; COTS GA. Some platform-native detectors are preview-only |
| Human-on-the-loop oversight | agent-activity dashboards; approval / override / kill-switch surfaces at the human-authority boundary; review queues for in-bounds actions | GA in mainstream orchestration and response platforms; the per-agent oversight *surface* is configuration over them, not a single shipped product |
| Action audit pipeline | tamper-evident audit-log pipelines retaining every tool call with a rollback reference; context-pinning so security events survive context-window trimming | GA; audit retention is standard SIEM/data-lake capability, applied to agent action logs |

For a single-stack buyer the telemetry primitives often land within existing entitlements — an OTel-instrumented estate already carries the collection layer, and a SOAR or orchestration platform already carries approval and override workflows — so the marginal licensing cost of D5 is frequently near zero and the spend is instrumentation and ingestion labor. The experimental status of the `gen_ai` conventions is the standing caveat: it is the one piece of this stack that can shift under the upper levels before it stabilizes.

## Capability levels

Levels are cumulative: Level N assumes every Level N−1 criterion. Each is stated as a capability specific to the SOC's own agents.

- **L1 — Initial.** Agent activity is visible only through the vendor console, if at all. There is no agent-specific telemetry and no reliable way to tell an agent's action apart from a human analyst's on the host. No oversight surface exists beyond reading after-the-fact logs; an operator cannot watch an agent act or stop it mid-action.

- **L2 — Developing.** A tool-call audit log records every agent action with user attribution — what the agent did and on whose behalf — retained in a queryable store. Intent attribution exists in at least coarse form: agent actions are tagged as agent-originated rather than buried in undifferentiated EDR telemetry. A human can review the agent's action history after the fact, though not yet intervene in flight.

- **L3 — Defined.** Agents emit [[opentelemetry-gen-ai|OpenTelemetry `gen_ai`]] spans (model / request / tool / retrieval / agent-step) into an in-path trace backend, and logs carry **per-agent identity multiplexing** so every action traces to an agent identity and the invoking human — the working fix for [[genai-endpoint-observability-talk|broken intent attribution]]. A **human-on-the-loop oversight surface** is in production: an operator monitors agent activity on a live dashboard and can intervene — pause, override, or kill — a running agent before a consequential action completes. Every tool call meets a minimum action-log schema with a recoverable rollback reference. **This is the rung that unlocks function-autonomy L3 (autonomous in-bounds), as the observability half of the L3 gate.** Per the gating rule, a function may run autonomously within bounds only when the SOC can both see the agent act (telemetry plus attribution) and override it (the oversight surface); D3 Evaluation must also hold for the L3 gate to be satisfied. Maturity note: the `gen_ai` SemConv is experimental, so pin the convention version to keep a bump from silently breaking the oversight and detection wiring.

- **L4 — Managed.** Oversight is measured and governed quantitatively. Per-agent behavioral baselines and drift detection run in production and wire into the alerting pipeline; override rates, intervention latency, and the [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]] are reported as metrics. Anomaly detection runs over agent reasoning as well as outputs — for example an [[llm-as-a-judge|LLM-as-a-judge]] check that flags an agent whose reasoning trajectory departs from its baseline. The audit record is tamper-evident and complete enough to reconstruct any agent action for forensics. At this level D5 contributes to the L4 (delegated) posture by supplying the continuous-oversight evidence the delegated tier depends on, alongside D7 and D8.

- **L5 — Optimizing.** Oversight adapts under governance: baselines, anomaly thresholds, and intervention triggers are tuned from operational evidence (override outcomes, false-anomaly rates) rather than set once. The oversight surface unifies the agent fleet rather than being configured per tool, and it spans the defender-and-defended duality in one pane — the same console that monitors AI activity across the estate also monitors the SOC's own agents. Intent attribution is native end-to-end (tool-call context emitted at the source, not inferred), and the audit pipeline supports replay of an agent's full reasoning-and-action trajectory.

- **L5+ — Leading Edge.** All of L5, plus a named contribution to the agent-observability standards layer — for example merged PRs into the OpenTelemetry `gen_ai` semantic conventions, an agent-hook telemetry schema published for others to adopt, or a reference pattern for end-to-end intent attribution across chained tool calls. (Elastic's work to establish the `gen_ai` conventions in the OTel community is the model for this rung.)

## Right-sizing by org profile

The realistic target rises with the size of the agent fleet, the blast radius of what those agents can do, and whether the SOC also monitors in-house AI applications. A small team with few agents and a narrow oversight need is right-sized at a lower level for its scale, not immature.

| Band | Realistic D5 target | Why |
|---|---|---|
| Solo / small | L2, often via the provider | Near or below the [[cyber-poverty-line\|cyber-poverty line]]. A team borrowing capability through an MSSP/MDR inherits the provider's telemetry and oversight surface; for its few in-house agents, an attributed tool-call audit log and after-the-fact review are achievable and sufficient. A live oversight console is rarely warranted when the agent footprint is small |
| Mid | L3 | An in-house SOC running selective delegation on high-volume functions needs `gen_ai` spans, per-agent identity multiplexing, and a working human-on-the-loop surface before it can let a triage or response agent act in-bounds. This is the band where the L3 gate becomes load-bearing |
| Enterprise | L4, selective L5 | A full agent fleet with broad blast radius and an in-house AI-application surface needs measured oversight, per-agent behavioral baselines, reasoning-level anomaly detection, and forensic-grade audit. L5 where the fleet is largest and the defender-and-defended duality is most acute |

A small team at L2 is right-sized: with a handful of well-gated agents, the marginal value of a live oversight console is low, and an attributed audit log plus deterministic [[agentic-soc-cmm#the-gating-rule|D4 authority limits]] already bound the blast radius the L3 oversight surface would otherwise watch.

## Cost model

The cost driver is ingestion run-rate and instrumentation labor, not licensing. Agent logs run roughly 10–20× human log volume into the SIEM, so cost scales with agent count rather than with control coverage. The telemetry primitives are largely incumbent entitlements; the spend is wiring them up and storing the output.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 (audit log into an existing store) | ~0.25 FTE to turn on tool-call audit, tag agent-originated actions, and stand up review | Low; modest log volume at small agent counts |
| L3 | ~0 to low (OTel SDK is OSS; trace backend may be entitled; oversight surface is configuration over the orchestration/response platform) | ~0.5 FTE recurring: span instrumentation, identity-multiplex wiring, schema conformance, and standing up the human-on-the-loop console | The inflection — `gen_ai` trace volume hits the SIEM at roughly 10–20× human log volume; tier high-volume low-fidelity spans to a cheaper data-lake tier |
| L4 | behavioral-baseline / drift tooling where not already present | baseline tuning, anomaly false-positive triage, override-metric reporting, forensic-audit upkeep | Behavioral-analysis compute plus full agent-log retention, scaling with fleet size |
| L5 | mostly labor; standards-contribution tooling is OSS | continuous tuning of baselines and intervention triggers, fleet-wide oversight maintenance, kept under audit | Peak: every-surface agent telemetry into the SIEM across the full fleet |

D5 spend is ingestion run-rate and instrumentation labor, not a licensing line. The one mitigation the model names is **log tiering**: route high-volume, low-fidelity agent trace spans to a cheaper data-lake or auxiliary tier and reserve the expensive analytics tier for the detections and overrides that actually fire. Pin the `gen_ai` convention version while it remains experimental, so a SemConv bump does not become an unbudgeted re-instrumentation cost.

## Open questions

- The [[opentelemetry-gen-ai|OpenTelemetry `gen_ai`]] semantic conventions remain experimental as of mid-2026, with no confirmed stable-release date. A version bump can break oversight and detection wiring, which is the known limitation hanging over the upper levels: an L3-and-above program is building on a moving standard and must pin the convention version defensively.
- Native intent attribution depends on broad tool-vendor adoption of agent-hook emission. Until most tools emit tool-call context natively, the SOC falls back on the brittle parent-process-name heuristic, and end-to-end attribution across chained tool calls (and across separate sessions or workspaces) is unsolved.
- The boundary between D5 (observing and overriding the agent) and D4 (authority to take the action in the first place) is clean in principle but blurs in tooling, where the same response platform often carries both the approval gate and the oversight console. The two are scored separately; an implementation may satisfy them with one product.
- The boundary between D5 (seeing and overriding) and D3 (scoring against ground truth) is likewise clean in principle, but both read overlapping telemetry; an eval harness and an oversight surface may be built over the same trace backend.
- The L3 gating threshold — that D5 must support a function before it may run at autonomy L3 — is anchored to the CMM's MDPI ↔ [[soc-cmm|SOC-CMM]] correspondence and is calibratable, not a fixed constant.

## Relations

- The [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|Agentic SOC CMM]] main page (domain D5) and its [[agentic-soc-cmm#the-gating-rule|gating rule]] — D5 is one of the two L3 autonomy gates, paired with D3 (Evaluation & Ground-Truth).
- The [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] — D5 scores its Observability & Evaluation plane (the observability half; D3 scores the evaluation half) and the human-authority boundary where oversight is exercised.
- Shared layer: the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] (its D7 Observability & Detection) and the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] observability plane. The SOC's own agents are an observed surface secured like any other non-human identity; D5 adds the defender-and-defended duality — the SOC both consumes and emits agent telemetry — which the application-security pair does not model.
- Sibling autonomy gates: D1 (Telemetry & Data Readiness) and D4 (Identity & Action-Authority, the L2 gates), D3 (Evaluation & Ground-Truth, the L3 partner), D7 (Resilience & Agent Supply Chain) and D8 (People & Governance, the L4 partners).
- Domain grounding and concepts: [[genai-endpoint-observability-talk|GenAI Endpoint Observability]] (broken intent attribution and the hooks-to-OTel fix, the load-bearing source), [[opentelemetry-gen-ai|OpenTelemetry `gen_ai` Semantic Conventions]], [[agent-observability|Agent Observability]], [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection for Agents]], [[llm-as-a-judge|LLM-as-a-Judge]] (anomaly detection on agent reasoning), [[oversight-layer|Oversight Layer]], and [[non-human-identity|Non-Human Identity]].
