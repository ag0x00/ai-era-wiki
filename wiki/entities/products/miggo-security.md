---
type: entity
entity_type: product
homepage: "https://miggo.io"
title: "Miggo Security"
created: 2026-05-03
updated: 2026-06-21
tags:
  - products
  - runtime-security
  - behavioral-monitoring
  - ai-bom
  - cots
status: developing
vendor: "Miggo Security"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agent-observability]]"
  - "[[ai-spm]]"
  - "[[multi-agent-runtime-security]]"
sources: []
---

# Miggo Security

**Sources:** [Miggo Security (homepage)](https://miggo.io)

Miggo Security is an application detection and response (ADR) vendor that has extended its platform to cover agentic AI workloads. It appears in three planes of the [[agentic-ai-security-reference-architecture|Agentic AI Security RA]]: Runtime (proof-of-guardrail attestation), Data (runtime AI-BOM), and Observability (agent behavioral drift detection).

## Core capabilities relevant to agentic AI

**DeepTracing (runtime AI-BOM):** Miggo's runtime tracing capability generates a behavioral baseline of an agent or model at deployment time — a record of what API calls, data flows, and tool invocations the model produces under normal conditions. This baseline serves as a dynamic AI Bill of Materials: rather than a static manifest of components, DeepTracing captures *how the system behaves*, making it sensitive to supply-chain changes that alter behavior without changing package versions.

**Behavioral drift detection:** By comparing runtime behavior against the DeepTracing baseline, Miggo flags deviations — new tool calls, unusual data access patterns, unexpected egress destinations. This is the Observability plane's "agent behavioral monitoring" control.

**Proof-of-Guardrail attestation:** Miggo partnered with AWS Nitro Enclaves to implement attestation that guardrails are actually running and have not been bypassed at the runtime layer. The pattern uses Nitro's cryptographic attestation to produce a verifiable proof that a given guardrail (e.g., LlamaFirewall PromptGuard) was executed for a given model response — addressing the "how do you know the guardrail ran?" audit question.

## RA plane coverage

| Plane | Capability | Role |
|---|---|---|
| **Runtime** | Proof-of-Guardrail attestation | With AWS Nitro Enclaves; research-stage |
| **Data** | Runtime AI-BOM (DeepTracing) | Behavioral baseline as dynamic BOM; developing |
| **Observability** | Agent behavioral drift detection | Compare runtime behavior vs. baseline; developing |

## Positioning

Miggo sits between traditional ADR (application runtime security) and the emerging AI-SPM category. Unlike pure AI-SPM tools ([[ai-spm|Wiz AI-SPM]], Palo Alto Prisma AIRS) that focus on posture and configuration, Miggo's value is in *runtime behavioral evidence* — what the agent is actually doing, not what its configuration says it should do.

> [!gap]
> Miggo's public documentation on agentic AI-specific capabilities is limited. The DeepTracing capability and Nitro Enclaves proof-of-guardrail work is referenced in industry coverage but not fully specified in public technical documentation. Treat as "Developing — novel primitive" rather than proven production control.
