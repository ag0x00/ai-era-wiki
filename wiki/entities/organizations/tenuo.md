---
type: organization
entity_type: organization
org_type: vendor
title: "Tenuo"
created: 2026-05-03
updated: 2026-05-23
tags:
  - organizations
  - vendors
  - capabilities
  - authorization
  - open-source
  - tenuo
status: developing
founders:
  - "[[niki-aimable-niyikiza]]"
website: "https://tenuo.ai"
github: "https://github.com/tenuo-ai/tenuo/"
core_product: "Tenuo Warrant — capability-based authorization primitive for AI agents"
implementation: "Rust core + Python bindings"
license: "Open source"
related:
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[monotonic-attenuation]]"
  - "[[ambient-vs-derived-authority]]"
  - "[[capability-based-authorization-talk]]"
  - "[[niki-aimable-niyikiza]]"
sources:
  - "[[capability-based-authorization-talk]]"
---

# Tenuo

Startup founded by [[niki-aimable-niyikiza|Niki Aimable Niyikiza]] (also Security Engineering at [[snap|Snap]]) building **capability-based authorization** for AI agents. The core primitive is the **[[tenuo-warrant|Tenuo Warrant]]**, a six-property cryptographic capability artifact (signed / scoped / ephemeral / holder-bound / verifiable offline / delegation-aware). The implementation is a Rust core with Python bindings, open-sourced at `github.com/tenuo-ai/tenuo`.

Public emergence: **March 4, 2026 at Unprompted** ([[capability-based-authorization-talk|talk page]]). Tenuo's positioning is that the missing primitive in the agentic-AI security stack is **delegation-aware authorization at execution time**, and that warrants productize 60 years of capability-systems research (Dennis & Van Horn 1966 → Macaroons → UCAN → Biscuits → CaMeL) into something that drops into LangGraph, CrewAI, Temporal, or an MCP proxy.

## Product

**Tenuo Core** (Rust + Python bindings; open source). Implements the warrant primitive plus four deployment modes:

| Mode | Where | Best for |
|---|---|---|
| In-process | Interceptor / middleware / drop-in node | LangGraph, CrewAI, Temporal: "one line of code" replacement of the framework's tool node |
| Sidecar | Separate process, same host | Polyglot stacks |
| Gateway | Proxy between agents and tools | Fleet-wide enforcement |
| MCP Proxy | Inside the MCP tool server (client- and/or server-side) | Any MCP-compatible agent: easiest because MCP already has structured tool + arg schemas |

Constraint language supports basic logic, regex, glob, and **CEL** (Common Expression Language) for arbitrary expressivity.

A live decoder/playground for warrants is hosted at `tenuo.ai`.

## Validation results published

| Harness | Number |
|---|---|
| End-to-end authorization (constraints + PoP) | ~55 μs (Rust core, CBOR, Mac M3 Max) |
| Denial fast-path | ~200 ns |
| Constraint + cryptographic-integrity violations rejected | 53 / 53 (5,700 fuzz probes; 0 bypasses) |
| **Multi-agent-delegation baseline ASR: 90% → 0%** | Custom in-house benchmark; **no public benchmark exists** for delegation-aware authorization at scale |

The 90%→0% claim is on Tenuo's own custom harness: credible signal, not yet third-party-replicated. The talk's open call to researchers is explicitly about building a shared benchmark.

## Constraint-design lesson the company has internalized

Slide 11 of [[capability-based-authorization-talk|the talk]] reports a real-world rejection-rate fix from **87.5% → 100%** that was *not* a cryptographic fix; it was constraint-design. The company maintains an opinionated three-layer model:

1. **Map**: logical constraint (regex / glob)
2. **Annotated Map**: constraint + normalization (`Subpath`, `UrlSafe`)
3. **Territory**: execution guard (`path_jail`, `url_jail`, OS sandbox)

Tenuo ships open-source execution guards for filesystem and process running.

## Positioning against adjacent vendors

| Adjacent capability vendor / project | Tenuo's framing |
|---|---|
| [[toolshed\|Stripe Toolshed]] | Centralized PDP via `ToolAnnotations`; Tenuo is decentralized via in-artifact policy |
| Sondera (Cedar reference monitor) | Cedar policy language for declared-tool actions; Tenuo carries the policy in the artifact and adds the delegation chain |
| Macaroons / UCAN / Biscuits | Prior art Tenuo cites; Tenuo's contribution is the agent-runtime productization (LangGraph, MCP, OAuth/IAM stacking) |
| Google DeepMind CaMeL | Model-layer privileged/quarantined split; complementary, not redundant |

## Open questions about Tenuo specifically

1. **Operational composition with existing PEPs**: Tenuo's verification is offline, but a fleet-scale deployment still needs a place to issue root warrants and collect receipts. How that composes with Stripe-/Block-/Salesforce-scale infra is not yet publicly disclosed.
2. **Real-world adoption beyond LangGraph demos**: public examples to date are demo-shaped (SOC triage pipeline, two LangGraph agents). Production case studies are pending.
3. **Public benchmark contribution**: whether Tenuo's harness becomes (or is contributed to) a community benchmark or stays vendor-internal.

## See also

- [[capability-based-authorization-talk|Capability-Based Authorization for AI Agents — Warrants That Survive Prompt Injection]] (Niyikiza, Unprompted March 2026)
- [[tenuo-warrant|Tenuo Warrant]]: the six-property primitive
- [[capability-based-authorization|Capability-Based Authorization]]: broader concept and prior art
- [[monotonic-attenuation|Monotonic Attenuation]]: the W₂ ⊆ W₁ ⊆ W₀ invariant
- [[ambient-vs-derived-authority|Ambient vs Derived Authority]]: the structural framing Niyikiza uses
- [[niki-aimable-niyikiza|Niki Aimable Niyikiza]]: founder
