---
type: concept
title: "Network-Layer Prompt Injection Containment"
address: c-000016
created: 2026-05-07
updated: 2026-08-21
origin: aggregated
tags:
  - concepts
  - prompt-injection
  - network-security
  - containment
  - agentic-ai
  - zero-trust
status: developing
scope_axis:
  - sec-of-ai
aliases:
  - "Network-Layer PI Defense"
  - "PI Network Filtering"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[microsoft-zt4ai|Microsoft ZT4AI]]"
  - "[[microsoft-secure-agentic-ai-end-to-end|Secure Agentic AI End-to-End]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[smokescreen|Smokescreen]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
sources:
  - "[[.raw/articles/microsoft-secure-agentic-ai-end-to-end-2026-05-07.md]]"
---

# Network-Layer Prompt Injection Containment

A defensive primitive that intercepts and filters [[indirect-prompt-injection|indirect prompt injection]] payloads at the **network egress / ingress layer** rather than at the application layer (LLM input filter / prompt-shield). The traffic is inspected as it traverses an enterprise's network controls — specifically the secure-web-gateway / forward-proxy / SASE tier that already exists for human-user policy enforcement — and prompt-injection-bearing content is blocked or rewritten before it reaches the LLM.

The category was first shipped at major-vendor scale by Microsoft as **Entra Internet Access Prompt Injection Protection** (announced in [[microsoft-secure-agentic-ai-end-to-end|Vasu Jakkal's pre-RSAC 2026 post]], GA March 31, 2026). It surfaces a third architectural layer in the [[prompt-injection-containment|prompt-injection-containment]] stack distinct from input-detection and execution-containment.

## Placement in the containment stack

The wiki's [[prompt-injection-containment|Prompt Injection Containment]] practice page carries a three-layer model, of which this concept is Layer 0:

- **Layer 0: Network-Layer Detection / Containment** — secure-web-gateway / forward-proxy inspects the agent's outbound + inbound traffic. Detects PI patterns in retrieved web content, blocks known-bad domains, scrubs or rewrites injected content. Operates at network / DNS / TLS-introspected HTTP layers. It sits **before** the application, outside the agent's process boundary.
- **Layer 1: Input Detection** — application-layer filters (LlamaFirewall, PromptGuard 2, NeMo Guardrails, Microsoft Prompt Shields). Run inside or alongside the agent runtime; inspect prompt content before it reaches the model.
- **Layer 2: Execution Containment** — runtime controls limiting the blast radius of a successful injection (credential proxy, tool-call interception, sandboxing, least-agency tiers).

The numbering (0 / 1 / 2) reflects layer order along the request path, not security priority. All three are complementary; production deployments should run all three. The [[owasp-ai-exchange|OWASP AI Exchange]]'s seven layers of protection, carried on the same practice page, number a different axis — precedence. Its layer 2, prompt-injection I/O handling, covers the detection work this control performs and states no placement for it, so the Exchange's list names no network tier and this control falls under its layer 2 by function.

## Architectural placement

Network-layer PI containment lives in the same operational tier as:

- Secure-web-gateway / SASE (Zscaler, Netskope, Palo Alto Prisma Access, Microsoft Entra Internet Access).
- Cloud-native egress proxies ([[smokescreen|Smokescreen]]).
- Agent-aware egress brokers ([[agentgateway|AgentGateway]]).

The defining characteristic: the inspection point is **outside the agent's process boundary** and is enforced via network policy rather than per-agent SDK instrumentation. This means:

- It applies even to agents that have **bypassed** application-layer guardrails (compromised agent, missing instrumentation, custom agent framework).
- It applies even to agents the **enterprise doesn't control** (e.g., an unsanctioned shadow-agent calling out to internet APIs).
- It applies as a **policy boundary** that scales independently of the number of agents.

## Defensive surface

Three categories of injection traffic are addressable at this layer:

| Traffic class | Where injection lives | Network-layer treatment |
|---|---|---|
| **Outbound retrieval responses** — agent calls a search API or web fetcher and the response includes injected instructions | Inside the response body | Inspect response, detect PI patterns, block / rewrite / annotate before it returns to agent |
| **Inbound instructions to a self-hosted agent endpoint** — attacker submits a request whose prompt body contains injected instructions | Inside the request body | Inspect request body, drop on PI signal, log for analyst review |
| **Cross-agent instructions in [[a2a-protocol\|A2A]] traffic** — peer agent attempts to coerce another agent | Inside A2A message body | Inspect A2A traffic at the policy enforcement point; same controls as outbound retrieval |

For all three, the network layer's signal is the **payload pattern** (suspicious instruction-like content; unusual control-token sequences; canary-tripwire phrases). The signal is **not** the originating agent's intent, which only the application layer can assess.

## Tradeoffs vs. application-layer containment

| Property | Application-layer (Layer 1) | Network-layer (Layer 0) |
|---|---|---|
| Visibility into agent intent | High | None |
| Visibility into payload content | Same | Same |
| Bypassability via agent compromise | High | Low (independent control plane) |
| Coverage of unsanctioned / shadow agents | None (requires SDK integration) | High (network policy applies regardless) |
| False-positive blast radius | Single agent / session | Whole network policy scope |
| Latency cost | ~10–100ms per inference | ~5–20ms per network round-trip |
| TLS introspection requirement | None | **Required** (or visibility limited to metadata) |

The two layers' strengths are complementary: application-layer can assess intent but is bypassable; network-layer is unbypassable from the agent's perspective but blind to intent.

## Limitations

- **TLS introspection is required.** Without TLS termination at the secure-web-gateway, the network layer sees only TLS metadata (SNI, cert issuer, packet sizes). PI payload inspection requires breaking and re-establishing TLS, which is non-trivial in many enterprise environments.
- **High false-positive risk on retrieval traffic.** Web content is full of instruction-like text. Aggressive PI detection at the network layer can break legitimate retrievals; calibration is required.
- **Miss rate is not uniform across languages or modalities.** The [[owasp-ai-exchange|OWASP AI Exchange]] states that prompt-injection detection accuracy differs across languages, modalities, and levels of attacker sophistication, and directs that per-language miss rate be measured rather than assumed.[^aix-piioh] The measurement matters more at this layer than at the application layer, because one policy covers every agent behind it and a per-language gap is a gap for the whole network.
- **Doesn't defend against direct user input.** A user typing an injection into a chat interface is on the application-layer surface, not network-layer. (Unless the chat interface itself is a remote service the enterprise routes through its proxy.)
- **Vendor-specific implementations.** Microsoft's Entra Internet Access PI Protection is the first major-vendor example; others (Zscaler, Netskope, Palo Alto) have not announced equivalent products as of May 2026. Cross-vendor pattern definitions don't yet exist.
- **Network-layer == [[oversight-layer|policy enforcement point]].** The control benefits from being co-located with existing identity / access policy enforcement, which usually means specific vendor stacks (Microsoft Entra, Zscaler ZIA, Palo Alto Prisma Access). Without that alignment, deploying network-layer PI containment is harder than deploying application-layer Prompt Shields.

## Relation to wiki

- **CMM D5 (Egress & Network)** — network-layer PI containment belongs as an L3+ control; pairs with the existing egress-proxy / domain-allowlist / agent-tag controls.
- **CMM D4 (Runtime & Guardrails)** — note that application-layer Prompt Shields (Layer 1 in the containment stack) live in D4; this concept is upstream of D4.
- **[[prompt-injection-containment|Prompt Injection Containment]]** — carries the three-layer model with Layer 0 citing this page, alongside the Exchange's seven-layer precedence ordering.
- **[[microsoft-zt4ai|Microsoft ZT4AI]]** — the Network pillar names this control; Microsoft is the first major vendor shipping it at scale.
- **[[lethal-trifecta|Lethal Trifecta]]** — network-layer egress control is the canonical break of the trifecta's "external-comms" leg; PI containment at the network layer is a related but distinct primitive (the trifecta lever blocks the *exfil*; network-layer PI blocks the *injection*).

## Provenance

The category as a deployable enterprise control was named and shipped by Microsoft in [[microsoft-secure-agentic-ai-end-to-end|Secure Agentic AI End-to-End]] (2026-03-20). Conceptually the architectural primitive predates Microsoft's specific product — secure-web-gateways have done content inspection for two decades — but applying it to PI at agent-aware scale is new in 2026.

## Notes

[^aix-piioh]: [OWASP AI Exchange — PROMPT INJECTION I/O HANDLING](https://owaspai.org/go/promptinjectioniohandling/), retrieved 2026-08-18. Risk-reduction guidance: detection accuracy differs across languages, modalities, and levels of attacker sophistication, with per-language miss rate to be measured rather than assumed.

<!-- sources:auto -->
## Sources

- [Secure Agentic AI End-to-End](https://www.microsoft.com/en-us/security/blog/2026/03/20/secure-agentic-ai-end-to-end/)
<!-- /sources -->
