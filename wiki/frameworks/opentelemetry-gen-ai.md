---
type: framework
title: "OpenTelemetry gen_ai.* Semantic Conventions"
created: 2026-05-03
updated: 2026-09-29
tags:
  - frameworks
  - observability
  - standards
  - opentelemetry
status: stub
source_url: "https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/README.md"
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
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Reviewed current OpenTelemetry GenAI repository docs, semantic conventions changelog and current CMM criteria; no archived source opened."
---

# OpenTelemetry gen_ai.* Semantic Conventions

**OpenTelemetry (OTel)** is the CNCF-graduated observability standard: a vendor-neutral API, SDK, and protocol (OTLP) for distributed tracing, metrics, and logs. The **`gen_ai.*` semantic conventions** are a set of standardized attribute names and span schemas for observing AI/LLM workloads — the extension of OTel into the agentic-AI observability space.

## Scope of the gen_ai.* conventions

The conventions are in Development status. Since semantic conventions v1.42.0 they are deprecated in the main repository and maintained in a dedicated GenAI repository, which carried no release tag on 2026-09-24 ([v1.42.0 release](https://github.com/open-telemetry/semantic-conventions/releases/tag/v1.42.0), [GenAI repository](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/README.md)). Semantic conventions v1.38.0 added a reasoning content message part ([changelog](https://github.com/open-telemetry/semantic-conventions/blob/main/CHANGELOG.md)). The `gen_ai.*` conventions specify:

- **Spans for model and tool operations** — inference spans, named `{gen_ai.operation.name} {gen_ai.request.model}`, and embeddings, retrieval, fetch-response, memory and execute-tool spans
- **Standard attributes** — `gen_ai.provider.name` (formerly `gen_ai.system`), `gen_ai.request.model`, `gen_ai.usage.input_tokens`, `gen_ai.usage.output_tokens`, `gen_ai.tool.name`, `gen_ai.tool.call.id`
- **Agent spans** — create-agent, invoke-agent, invoke-workflow and plan spans for multi-step agent traces

The `gen_ai.*` conventions address part of the logging-standardization gap [[nist-ai-800-4|NIST AI 800-4]] describes: monitoring of deployed AI lacks common terminology and standardized agent identifiers, with logging fragmented across distributed infrastructure. A shared span and attribute vocabulary can let a detection rule travel across tools where those fields are preserved.

## Basis for OTel as the foundational choice

- **Vendor-neutral:** OTel traces can be sent to any backend (Datadog, Grafana, Splunk, Jaeger, Honeycomb, etc.) without changing instrumentation code. Lock-in is at the backend level, not the collection level.
- **Existing instrumentation:** an organization already using OTel can add GenAI spans to its collection path rather than create a separate trace system.
- **Licensing:** OTel SDKs are Apache 2.0 and the conventions are a specification. Collection, storage, retention and analysis still have operating cost.
- **CNCF graduation:** OTel is a CNCF Graduated project; the GenAI conventions have their own development status.
- **Volume control:** batching and collector-side sampling let operators budget trace volume while retaining the security events needed to reconstruct a run. A sampling rule must not discard a path that D7-SPANS requires.

## Agent observability use pattern

In an agentic-AI system, OTel `gen_ai.*` spans record operations across the [[agentic-ai-security-reference-architecture|RA]]'s trust boundaries:

```
Agent process
  ├── inference span (chat) → model call
  ├── execute_tool span → action gateway
  ├── retrieval span → retrieval mediator
  └── invoke_agent span → delegated agent
```

The application must add and preserve a resolvable agent, session and accountable-human reference across the trace (the [[agent-observability|identity-multiplexing pattern]]). The conventions alone do not supply or verify those identities.

## In the RA / CMM

- **RA evidence path:** Correlated traces from the model, retrieval mediator, action gateway, and agent runtime reach an evidence store outside the agent's write authority. OTel is one transport and schema choice.
- **CMM D7 L3:** See [[agentic-ai-security-cmm-d7-observability|D7]] for D7-SPANS, which requires correlated traces on applicable paths. OTel GenAI spans are one implementation. D7-SPANS-HELD requires searchable or exportable records; D7-SPANS-PIN requires version records or a compatibility contract with a field-compatibility test.
- **CMM [[agentic-ai-security-cmm-d9-operations|D9]] L3:** Instrumented guardrail spans can contribute to D9-GUARD-LATENCY and D9-GUARD-COST series for each agent. Token counts alone do not establish guardrail cost without a pricing method and attribution.

## See also

- [[agent-observability|Agent Observability]] — the wiki's observability practice page, which uses OTel as its foundation
- [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] — trust boundaries and the independent evidence store
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] D7 + D9
- [[genai-endpoint-observability-talk|GenAI Endpoint Observability]] — the practitioner case for extending these conventions to endpoint tool activity via agent hooks routed to the SIEM

<!-- sources:auto -->
## Sources

- [OpenTelemetry gen_ai.* Semantic Conventions](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/README.md)
<!-- /sources -->
