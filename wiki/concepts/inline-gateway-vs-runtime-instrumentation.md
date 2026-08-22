---
type: concept
title: "Inline Gateway vs Runtime Instrumentation"
created: 2026-05-03
updated: 2026-05-03
tags:
  - concepts
  - architecture
  - egress
  - runtime
  - mcp-security
status: developing
related:
  - "[[runlayer]]"
  - "[[helmet-security]]"
  - "[[capsule-security]]"
  - "[[agentgateway]]"
  - "[[lakera-guard]]"
  - "[[miggo-security]]"
  - "[[mcp-security]]"
  - "[[adr-agentic-detection-system]]"
sources:
  - "[[comprehensive-agentic-ai-security-landscape-2026]]"
  - "https://arxiv.org/abs/2605.17380"
---

# Inline Gateway vs Runtime Instrumentation

## Description

Two architectural strategies for enforcing security policy on agentic AI in production. Both ship at the seed stage of the agentic-AI-security market in Q4 2025 / Q1 2026 — the funded category has effectively split into two camps with the same security goals and incompatible deployment models.

| Dimension | Inline Gateway / Proxy | Runtime Instrumentation |
|-----------|------------------------|--------------------------|
| Sits where | In the data path between agent and tool/MCP/network | Inside the agent runtime, hooked into tool-call or syscall surfaces |
| Visibility | Every request that goes through (full audit trail) | Every action the agent attempts (full action trail) |
| Enforcement | Block / rewrite / quarantine inline | Block / pause / require confirmation at action time |
| Deployment | Network change, DNS / routing, mTLS termination | Runtime hook, agent SDK, or platform integration (Cursor, Copilot, Agentforce, etc.) |
| Latency | Adds at-least-one network hop | Negligible (in-process) |
| Bypass risk | Low if traffic is forced through, but agents can take direct paths | Lower if the runtime is enforced; higher if the agent can call OS-level paths |
| Vendor pattern (May 2026) | [[runlayer\|Runlayer]] (gateway), [[helmet-security\|Helmet]] (discovery+monitoring), [[agentgateway\|AgentGateway]] (OSS), [[agentcordon\|AgentCordon]] (OSS, gateway+vault+IDP combined), Operant, Natoma, Cloudflare AI Gateway | [[capsule-security\|Capsule]] (no proxy/gateway/SDK), [[miggo-security\|Miggo]] (DeepTracing), [[lakera-guard\|Lakera Guard]] (content layer in-process) |

## Basis for its load-bearing status

The seed-cohort startups in the 2025–2026 agentic-AI-security funding wave are explicitly architected against each other. Runlayer and Helmet pitch the **MCP gateway** as the way to get visibility and control over agent communications. Capsule pitches **runtime instrumentation** with the explicit anti-gateway claim that proxies, gateways, SDKs, and browser extensions are not required. More seed capital has gone to the gateway camp than to the instrumentation camp, but the architectural choice is being decided in production-deployment outcomes through 2026.

## Production evidence: the ADR sensor

The first large-scale, production-proven data point for the instrumentation camp is Uber's [[adr-agentic-detection-system|ADR]] (ten months, 7,200+ hosts, 10,000+ sessions/day). Its **ADR Sensor** is runtime instrumentation by another route: rather than hooking a live syscall or tool-call surface, it parses the local SQLite/JSONL caches that Cursor, Cline, and Claude Code already write, reconstructing the full prompt → reasoning → tool-call → outcome chain at ~0.182 s per run.[^adr] The paper makes the architectural argument this page frames explicit, and lands on instrumentation: an LLM/MCP **gateway** was evaluated and rejected for observability because it requires MCP-host changes, is incompatible with streaming responses, and captures only partial information, omitting environmental context.[^adr]

The nuance worth recording is that ADR is **hybrid for enforcement**: the sensor does deep forensics (detective, after-the-fact), while inline **Hooks** in Cursor and Claude Code do real-time blocking of high-severity credential leakage (preventative, in-path).[^adr] That matches this page's "defense-in-depth across the fork" recommendation rather than a pure-instrumentation stance — the cheap, certain control (regex+entropy secret-blocking) sits inline at the chokepoint, while the expensive, semantic control (reasoning over the reconstructed session) runs off the hot path.

> [!check] Partial resolution of the enforceability gap
> The gap below asked whether runtime instrumentation is viable as primary detection at scale for real agents. For **local-process coding agents** (Cursor / Cline / Claude Code), ADR answers yes in production: cache-parsing reconstruction sustains detection across 7,200+ hosts. It does **not** resolve the hosted-LLM-agent case, and the sensor is detective, not tamper-proof enforcement — a prompt-injected agent calling a path the sensor does not reconstruct remains the open risk.

## Historical analogues

Same fork played out a decade ago between **API Gateways** (Apigee, Kong, Tyk — sit in the path) and **Runtime APM** (Datadog, New Relic, AppDynamics — instrument in-process). Both categories survived; they coexist in the same enterprise but for different jobs. Gateways won policy enforcement and contracts at the boundary. APM won deep latency-and-error visibility inside services. The agentic-AI-security analog is likely to settle similarly: gateways for policy at the agent/tool boundary; instrumentation for behavioral observability inside the agent runtime.

## Selection between the two primitives

### Gateway is the right primitive when
- The boundary is well-defined (MCP, A2A, tool registry)
- The agent population is heterogeneous (the gateway becomes the only common chokepoint)
- Policy must be *enforced*, not advisory (the gateway can deny)
- The audit trail needs to live outside the agent runtime for compliance reasons

### Runtime instrumentation is the right primitive when
- The agent population is homogeneous (Cursor + Copilot + Agentforce — small set of supported runtimes)
- Network interception is impractical (managed SaaS, mobile, hosted runtime)
- Latency budget is tight
- The interesting events happen *inside* the agent (planning steps, memory writes, code generation), not at the network boundary

## Tradeoffs that don't have a clean answer yet

> [!gap]
> **Gateway bypass via the [[lethal-trifecta\|Lethal Trifecta]]'s third leg.** Agents that exfiltrate via image rendering, markdown URLs, DNS, or direct browser fetch can route around an MCP-only gateway. Runlayer/Helmet/AgentGateway handle the MCP surface; the rest needs [[smokescreen\|Smokescreen]]-shaped SSRF/egress control. A gateway-only architecture is necessary-but-not-sufficient.

> [!gap]
> **Runtime-instrumentation enforceability under hostile model behavior.** A misaligned or [[indirect-prompt-injection\|prompt-injected]] agent can in principle call APIs directly without going through the instrumented hook. The instrumentation camp's claim — that the runtime hooks are tight enough to be unbypassable — is unproven in the public literature for hosted-LLM agents (vs. local-process agents). [[miggo-security\|Miggo]]'s AWS Nitro Enclaves attestation is the closest production approach to making the hook tamper-resistant; this is not yet a category norm.

> [!gap]
> **Where does identity coupling live?** Both camps integrate with [[okta-for-ai-agents\|Okta]] / [[microsoft-entra-agent-id\|Entra]] / [[keycard\|Keycard]] for the principal-and-permissions side. The PDP-vs-PEP split (per [[oversight-layer\|Oversight Layer]]) lands differently: gateways naturally are PEPs; runtime instrumentation can be either.

## Implication for the [[agentic-ai-security-cmm-2026|CMM]]

Current CMM **D5 (Egress & Network)** rows are written gateway-first. A revision should:

1. Add a parallel **D5 evidence row for runtime-instrumentation enforcement** — at L3 acceptable as primary enforcement when the agent runtime is homogeneous and tamper-evidence is provided.
2. At L4, require **both** gateway-style enforcement at platform boundaries **and** runtime instrumentation inside the agent runtime — defense-in-depth across the architectural fork rather than choosing one.
3. At L5, require enforcement attestation (e.g. AWS Nitro Enclaves per [[miggo-security|Miggo]], TPM-backed runtime, or signed gateway logs) to harden against the bypass paths above.

## Deepest instrumentation: model forward-pass hooks

The Inline Gateway vs Runtime Instrumentation fork as described above treats the *agent runtime* as the instrumentation boundary. Carl Hurd's [[glass-box-security-talk|Glass-Box Security talk]] at Unprompted March 2026 identifies a third, deeper instrumentation surface: the model's *forward pass* itself — hooks into the residual stream at specific layers to capture activation vectors for intent and strength measurement.

This is not in the current seed-cohort product map — no funded startup as of May 2026 appears to ship production forward-pass activation monitoring. It is [[starseer|Starseer]]'s pre-launch direction and establishes a theoretical stack of:

```
[gateway/network layer] ← Runlayer, Helmet, AgentGateway
[agent runtime layer]   ← Capsule, Miggo, Lakera in-process
[model forward-pass]    ← Starseer (Glass-Box), future vendors
```

Each deeper layer provides higher-fidelity intent signals but requires more infrastructure control (self-hosted inference or canary-model instrumentation). See [[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]] and [[glass-box-security|Glass-Box Security]] for the conceptual basis.

## See Also

- [[comprehensive-agentic-ai-security-landscape-2026|Comprehensive Agentic AI Security Landscape]] — the funding-landscape pass that covers this cohort
- [[agentic-ai-security-reference-architecture|Reference Architecture]] — Egress + Runtime planes
- [[mcp-security|MCP Security]]
- [[glass-box-security|Glass-Box Security]] — deepest instrumentation layer (model forward-pass)
- [[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]] — underlying technique
- [[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]] — production-proven instrumentation (sensor) case, with an explicit gateway rejection

[^adr]: §3.1 *Observability: The ADR Sensor*, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): cache-parsing reconstruction at 0.182 s/run, the rejected LLM/MCP gateway alternative, and the hybrid sensor-plus-inline-Hooks prevention model.

<!-- sources:auto -->
## Sources

- [arxiv.org](https://arxiv.org/abs/2605.17380)
<!-- /sources -->
