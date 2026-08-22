---
type: framework
title: "OpenTelemetry gen_ai.* Semantic Conventions"
created: 2026-05-03
updated: 2026-06-22
tags:
  - frameworks
  - observability
  - standards
  - opentelemetry
status: stub
source_url: "https://opentelemetry.io/docs/specs/semconv/gen-ai/"
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
license: Apache 2.0 (CNCF)
org: CNCF (Cloud Native Computing Foundation)
related:
  - "[[agent-observability]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[genai-endpoint-observability-talk]]"
  - "[[nist-ai-800-4]]"
---

# OpenTelemetry gen_ai.* Semantic Conventions

**OpenTelemetry (OTel)** is the CNCF-graduated observability standard: a vendor-neutral API, SDK, and protocol (OTLP) for distributed tracing, metrics, and logs. The **`gen_ai.*` semantic conventions** are a set of standardized attribute names and span schemas for observing AI/LLM workloads — the extension of OTel into the agentic-AI observability space.

## Scope of the gen_ai.* conventions

`gen_ai.*` SemConv (v1.37+ as of May 2026; status: experimental but broadly adopted) specifies:

- **Span names** — `gen_ai.client.request`, `gen_ai.client.response`, `gen_ai.tool.call`, `gen_ai.retrieval`
- **Standard attributes** — `gen_ai.system`, `gen_ai.request.model`, `gen_ai.usage.input_tokens`, `gen_ai.usage.output_tokens`, `gen_ai.tool.name`, `gen_ai.tool.call.id`
- **Agent-specific spans** — `gen_ai.agent.step`, `gen_ai.agent.invocation` for multi-step agent traces

SIG contributors as of May 2026: Amazon, Elastic, Google, IBM, Langtrace, Microsoft, OpenLIT, Scorecard, Traceloop. The multi-stakeholder SIG is the primary signal of standard status — no single vendor can capture it.

The `gen_ai.*` conventions are a direct response to the logging-standardization gap [[nist-ai-800-4|NIST AI 800-4]] documents: the report finds the field monitoring deployed AI lacks common terminology and standardized agent identifiers, with logging fragmented across distributed infrastructure. A shared span and attribute vocabulary is the interchange layer that lets a detection rule travel across tools rather than being rewritten per vendor.

## Basis for OTel as the foundational choice

- **Vendor-neutral:** OTel traces can be sent to any backend (Datadog, Grafana, Splunk, Jaeger, Honeycomb, etc.) without changing instrumentation code. Lock-in is at the backend level, not the collection level.
- **Already in the stack:** most organizations already instrument microservices with OTel; adding `gen_ai.*` spans extends existing infrastructure rather than creating a parallel observability silo.
- **No license cost:** the OTel SDKs (Python, JS, Go, Java, etc.) are Apache 2.0. The `gen_ai.*` SemConv is a specification, not software, so adopting it has zero cost.
- **CNCF graduation:** OTel is a CNCF Graduated project, the highest maturity level. It has production adoption at Google, Microsoft, AWS, Meta, Netflix, and others.
- **Agent-specific volume:** agents generate 10–20× the log volume of human users over the same window. OTel's pre-aggregation-at-hook model (span batching, tail sampling) is the right architecture for this volume.

## Agent observability use pattern

In an agentic-AI system, OTel `gen_ai.*` spans flow through the six planes of the [[agentic-ai-security-reference-architecture|RA]]:

```
Agent process
  ├── gen_ai.client.request span → model call
  ├── gen_ai.tool.call span → tool invocation (→ Egress plane)
  ├── gen_ai.retrieval span → RAG retrieval (→ Data plane)
  └── gen_ai.agent.step span → per-step trace (→ Observability plane)
```

Each span carries `agent_id`, `user_id`, `session_id` attributes (via the [[agent-observability|Agent Observability §3 identity-multiplexing pattern]]), making every action traceable to a human principal.

## In the RA / CMM

- **RA Observability Plane:** OTel `gen_ai.*` SemConv is the primary reference implementation for the Observability Plane — classified as `Std` (CNCF standard).
- **CMM D7 L3:** "OTel gen_ai.* semantic conventions emitted across agents" is the minimum evidence artifact for Defined observability.
- **CMM D9 L3:** OTel latency/cost spans are used to measure guardrail latency (p50/p95/p99) per agent.
- **FOSS/small-team stack:** OTel is the recommended zero-cost observability foundation; backend can be Langtrace/Traceloop (OSS) or any OTel-compatible SaaS.
- **Enterprise stack:** OTel spans feed into existing SIEM (Splunk, Datadog, Dynatrace, etc.) without replatforming.

## Implementations / backends

| Tool | Type | Role |
|---|---|---|
| Langtrace | OSS | Agent-aware OTel tracing with LLM-specific UI |
| Traceloop | OSS | OpenLLMetry SDK (OTel-native) |
| Helicone | OSS / SaaS | LLM observability gateway; OTel-compatible |
| LangSmith | SaaS | LangChain-native; exports OTel |
| DataDog AI Monitoring | SaaS | OTel-native ingestion; AI-specific dashboards |
| New Relic AI Monitoring | SaaS | OTel-native; gen_ai.* support |

## See also

- [[agent-observability|Agent Observability]] — the wiki's observability practice page, which uses OTel as its foundation
- [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] §Observability plane
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] D7 + D9
- [[genai-endpoint-observability-talk|GenAI Endpoint Observability]] — the practitioner case for extending these conventions to endpoint tool activity via agent hooks routed to the SIEM

<!-- sources:auto -->
## Sources

- [OpenTelemetry gen_ai.* Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/)
<!-- /sources -->
