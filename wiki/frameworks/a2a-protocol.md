---
type: framework
title: "A2A Protocol (Agent-to-Agent)"
created: 2026-04-30
updated: 2026-06-22
tags:
  - frameworks
  - protocols
  - agent-to-agent
  - interoperability
  - linux-foundation
status: developing
scope_axis:
  - sec-of-ai
adoption_signal: active
last_substantive_update: 2026-03-12
governance: "Linux Foundation (Agentic AI Foundation)"
contributed_by: "[[google|Google]]"
current_version: "v1.0.0 (released 2026-03-12)"
canonical_spec: "https://a2a-protocol.org/latest/specification/"
canonical_repo: "https://github.com/a2aproject/A2A"
scope: "Open protocol for agent-to-agent communication; signed Agent Cards; opacity principle"
audience: "AI platform builders, agent framework authors, security architects"
aliases:
  - "A2A"
  - "Agent2Agent"
related:
  - "[[google]]"
  - "[[mcp-security]]"
  - "[[agent-identity-architecture]]"
  - "[[multi-agent-runtime-security]]"
  - "[[csa-maestro]]"
  - "[[cosai]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
sources:
  - "https://a2a-protocol.org/latest/specification/"
  - "https://github.com/a2aproject/A2A"
  - "https://github.com/a2aproject/A2A/releases"
  - "https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents"
  - "https://developers.googleblog.com/en/google-cloud-donates-a2a-to-linux-foundation/"
  - "https://cloudsecurityalliance.org/blog/2025/04/30/threat-modeling-google-s-a2a-protocol-with-the-maestro-framework"
  - "https://github.com/a2aproject/A2A/issues/1575"
---

# A2A Protocol — Agent-to-Agent

The **Agent-to-Agent (A2A) Protocol** is an open standard for communication between AI agents across organizational and platform boundaries. Originally contributed by [[google|Google]] in April 2025, **donated to the Linux Foundation on June 23, 2025**, and now hosted under the LF's Agentic AI Foundation umbrella. **Spec v1.0.0 shipped 2026-03-12.**

## Spec at a glance

| Property | Value |
|---|---|
| Current version | **v1.0.0** (2026-03-12) |
| Governance | **Linux Foundation** (since 2025-06-23) |
| Canonical spec | [a2a-protocol.org/latest/specification](https://a2a-protocol.org/latest/specification/) |
| Canonical repo | [github.com/a2aproject/A2A](https://github.com/a2aproject/A2A) (the `google-a2a/A2A` mirror is superseded) |
| Wire transports | JSON-RPC 2.0 over HTTP(S); Server-Sent Events; gRPC |
| Discovery | `https://<base_url>/.well-known/agent-card.json` |
| Complementary to | [[mcp-security\|MCP]] (agent ↔ tool); A2A is agent ↔ agent |

> [!note] Wiki correction
> Earlier wiki references cited "v0.3" with "no standalone security spec." That was accurate for the pre-LF Google snapshot. **The current state is v1.0.0 (Mar 2026), Linux Foundation-governed.** Pages still citing v0.3 are stale and have been updated as part of peer-review readiness #4.

## Security model — what's in the spec

The v1.0 spec covers transport security and Agent Card signing. There is no separate `security.md` in the spec; security is woven through §7 (transport/auth) and §8.4 (Card signing) plus the [Enterprise-Ready topic page](https://a2a-protocol.org/latest/topics/enterprise-ready/).

| Layer | What v1.0 specifies |
|---|---|
| **Transport** | *"Production deployments MUST use HTTPS"* with TLS 1.3 recommended; clients SHOULD validate server certs |
| **Authentication** | Delegates to OpenAPI-style schemes — API keys, HTTP auth, OAuth 2.0/OIDC, mTLS. *"A2A Servers MUST authenticate every incoming request based on the provided HTTP credentials and its declared authentication requirements from its Agent Card."* Credentials live in HTTP headers, not protocol payloads |
| **Authorization** | Per-skill access control, advertised via Agent Card; least-privilege enforcement is the **server's** responsibility |
| **Agent Card signing** | §8.4 specifies the framework — Canonicalization Requirements, Signature Format, Signature Verification — but **algorithm choice is implementation-defined** |
| **Opacity principle** | *"Agents collaborate based on declared capabilities and exchanged information, without needing to share their internal thoughts, plans, or tool implementations."* Agents MAY use `contextId` for internal state, *"this internal state is not exposed to other agents."* |

## Omissions in v1.0

A peer reviewer should know exactly what is missing:

- **No replay-protection section.** Implementations must layer it themselves (timestamps + nonces).
- **No mandatory cryptographic algorithm for Agent Card signing.** Ed25519 is the *de facto* vendor pick (see Oktsec below) but spec is algorithm-agnostic.
- **No multi-hop trust chain or cross-agent delegation primitive.** Delegation is the open work.
- **No formal-methods review** of v1.0 has been published as of mid-2026.
- **No CVEs** assigned to A2A in NVD as of 2026-05.

This means an L3+ CMM claim depending on A2A security must specify the *org's own enforcement profile* — the wiki's [[agentic-ai-security-cmm-2026|CMM]] D5 L3 already requires this.

## Agent Cards

The Agent Card is A2A's identity-and-capability metadata document, served from `/.well-known/agent-card.json`. It carries:

- Identity (agent name, owner, contact)
- Capabilities and skills (what the agent can be asked to do)
- Endpoints (where to talk to it)
- Supported transports
- `securitySchemes` (auth schemes the server expects)
- Optional signature block per §8.4

Cards are how peer agents discover what this agent does and how to authenticate to it. Mandatory vs optional field details should be pulled directly from the live spec (`a2a-protocol.org/latest/specification/#agent-card-object`) since these evolve faster than the wiki refresh cadence.

Signed Agent Cards (§8.4) anchor the CMM's [[agentic-ai-security-cmm-2026|D2 Identity & Authorization]] domain alongside CoSAI's Agentic Identity and Access Management (2026-04-17), per [[standards-review-saif-cosai-2026-Q2|the 2026-Q2 SAIF/CoSAI standards review]], which found this the strongest single identity-content area of the SAIF/CoSAI pair.

## Vendor-side and proposal-side enforcement

The spec is intentionally minimal. Production hardening lives in vendor implementations and active proposals:

| Source | What it adds |
|---|---|
| **Oktsec** ([github.com/oktsec/oktsec](https://github.com/oktsec/oktsec), v0.15.2 / 2026-04-24) | Per-agent **Ed25519 keypairs** issued at init; signatures over `from + to + content + timestamp`; **default-deny ACLs**; per-agent sliding-window rate limit; tamper-evident audit chain v2; OpenTelemetry tracing; **268 detection rules** across categories like prompt-injection, credential-leak, exfiltration, command-execution, MCP-attack, MCP-config, supply-chain, SSRF/cloud, indirect-injection, unicode-attack, third-party-content, external-download, inter-agent communication, container escape, tool-call, memory poisoning, openclaw-config |
| **A2A repo Issue [#1575 — "Agent Passport System"](https://github.com/a2aproject/A2A/issues/1575)** | Open proposal: Ed25519 keypairs for agents, scoped delegation with cascade revocation, 3-signature execution chain (intent → policy eval → signed receipt), and a "values floor" (traceability, honest identity, scoped authority, revocability, auditability, non-deception, proportionality). Working TS implementation, **not merged** as of mid-2026 |
| **Red Hat A2A hardening guide** ([developers.redhat.com](https://developers.redhat.com/articles/2025/08/19/how-enhance-agent2agent-security)) | Platform-side hardening recipes (mTLS-everywhere, gateway-side scanning, identity-bridging) |
| **IETF Agent Identity Protocol (AIP)** ([draft-prakash-aip-00](https://www.ietf.org/archive/id/draft-prakash-aip-00.html)) | Verifiable delegation primitives across MCP and A2A. Adjacent IETF work, not part of A2A |

> [!note] Wiki correction — "175 detection rules"
> Earlier wiki pages cited Oktsec's "175-rule" content scanning. **Oktsec ships 268 rules at v0.15.2.** "175" reflects an older release. Pages updated as part of this stub-fill.

## Threat model — independent reviews

- **CSA "Threat Modeling Google's A2A Protocol with the MAESTRO Framework"** (April 2025) — pre-LF, pre-v1.0; identifies message-injection, agent-impersonation, and foundation-model-level risks. [CSA blog](https://cloudsecurityalliance.org/blog/2025/04/30/threat-modeling-google-s-a2a-protocol-with-the-maestro-framework). The MAESTRO 7-layer mapping is the canonical starting point.
- The wiki's [[agentic-ai-threat-classes-2026|Threat Classes 2026]] page covers A2A-relevant classes (multi-agent collusion, insider amplifier, jurisdictional adversaries) at the architecture level.

## Use in this wiki

| Use | Where |
|---|---|
| Egress-plane PEP between agents | [[agentic-ai-security-reference-architecture\|RA]] §Egress + §Multi-agent mesh deployment shape |
| D5 L3 evidence (mTLS for A2A; org-authored enforcement profile) | [[agentic-ai-security-cmm-2026\|CMM]] |
| D5 L4 evidence (signed Card validation + content scanning) | [[agentic-ai-security-cmm-2026\|CMM]] |
| Multi-agent runtime threats | [[multi-agent-runtime-security\|Multi-Agent Runtime Security]] — cascade detection, behavioral baselines, inter-agent IR |
| ASI07 (Insecure Inter-Agent Comms) anchor | [[agentic-ai-security-reference-architecture\|RA]] threat-control matrix |

## Maturity ladder for A2A enforcement

A practical layering for orgs adopting A2A:

| Tier | What's in place |
|---|---|
| L1 — Spec minimum | HTTPS + OAuth2 / API key per §7; no signed Cards, no rate limits |
| L2 — Card discipline | Signed Agent Cards (any algorithm) + per-skill authorization advertised; `securitySchemes` enforced server-side |
| L3 — Vendor-grade | Ed25519 message signing (Oktsec-class) + scoped per-agent rate limits + content scanning + tamper-evident audit chain |
| L4 — Trust framework | Issue #1575 / AIP-class scoped delegation with cascade revocation + 3-signature execution chain + policy-engine enforcement |
| L5 — Formal | Formally specified mesh invariants + automated containment doctrine — **does not exist in production as of mid-2026** |

This ladder is consistent with the wiki's [[agentic-ai-security-cmm-2026|CMM]] D5 levels.

## See Also

- [[mcp-security|MCP Security]] — the agent-to-tool counterpart
- [[multi-agent-runtime-security|Multi-Agent Runtime Security]] — cascade detection, behavioral baselines, inter-agent IR
- [[csa-maestro|CSA MAESTRO]] — primary independent threat model
- [[cosai|CoSAI]] — broader agentic-AI security ecosystem context
- [[agent-identity-architecture|AI Agent Identity Architecture]] — identity primitives consumed by A2A
- [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes 2026]] — collusion, insider amplifier, jurisdictional adversary classes

<!-- sources:auto -->
## Sources

- [a2a-protocol.org](https://a2a-protocol.org/latest/specification/)
- [github.com](https://github.com/a2aproject/A2A)
- [github.com](https://github.com/a2aproject/A2A/releases)
- [linuxfoundation.org](https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents)
- [developers.googleblog.com](https://developers.googleblog.com/en/google-cloud-donates-a2a-to-linux-foundation/)
- [cloudsecurityalliance.org](https://cloudsecurityalliance.org/blog/2025/04/30/threat-modeling-google-s-a2a-protocol-with-the-maestro-framework)
- [github.com](https://github.com/a2aproject/A2A/issues/1575)
<!-- /sources -->
