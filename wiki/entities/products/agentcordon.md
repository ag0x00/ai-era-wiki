---
type: entity
entity_type: product
title: "AgentCordon"
created: 2026-05-04
updated: 2026-08-21
tags:
  - products
  - credential-proxy
  - identity-plane
  - mcp-security
  - oss
  - rust
  - agentic-idp
  - oauth
  - cedar
status: developing
scope_axis:
  - sec-of-ai
license: GPL-3.0
vendor: "AgentCordon (open-source project)"
homepage: https://agentcordon.dev
related:
  - "[[credential-proxy-pattern]]"
  - "[[cedar]]"
  - "[[mcp-security]]"
  - "[[non-human-identity]]"
  - "[[nhi-governance-for-agents]]"
  - "[[cyberark-conjur]]"
  - "[[okta-for-ai-agents]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[keycard]]"
  - "[[agentgateway]]"
  - "[[inline-gateway-vs-runtime-instrumentation]]"
  - "[[lethal-trifecta]]"
  - "[[agentcordon-project|AgentCordon (project)]]"
sources:
  - "https://agentcordon.dev"
  - "https://github.com/agentcordon/agentcordon"
  - "[[.raw/articles/agentcordon-readme-2026-05-04.md]]"
---

# AgentCordon

**Sources:** [Homepage](https://agentcordon.dev) · [GitHub repo](https://github.com/agentcordon/agentcordon) · [Docs](https://agentcordon.dev/docs) · [README snapshot](.raw/articles/agentcordon-readme-2026-05-04.md)

## Description

**AgentCordon** is a self-hostable open-source **Agentic Identity Provider (Agentic IDP) and credential broker** for AI agents. Written in Rust (GPL-3.0), it combines four primitives in a single deployment: an AES-256-GCM encrypted credential vault, a [[cedar|Cedar]] policy engine for authorization, a credential proxy that injects secrets server-side so agents never see them, and an [[mcp-security|MCP gateway]] that mediates tool calls with policy enforcement and response-leak scanning.

The project's positioning is explicit: "the open-source alternative to hardcoded API keys in AI agent workflows." It cites GitGuardian's 24,000+ secrets-leaked-in-MCP-config-files figure as the threat-model anchor.

## Three-tier architecture

AgentCordon's distinguishing structural choice is a **three-tier split** that other credential products in this space typically collapse into one or two tiers:

```
[Agent Host]   agentcordon CLI (Ed25519)              ── thin client; signs requests
        │
        ▼
[Broker]       agentcordon-broker (port 3141)         ── per-user daemon; holds OAuth tokens; injects creds
        │
        ▼
[Server]       agent-cordon-server (port 3140)        ── Cedar PDP + encrypted vault + OAuth AS
        │
        ▼
[External]     Target API / MCP server                ── credential never seen by agent
```

- **CLI** has only Ed25519 keypair identity. No secrets, no OAuth tokens.
- **Broker** is a per-user daemon that holds OAuth tokens and proxies upstream HTTP. It is the only component that ever holds plaintext credential material outside the server boundary.
- **Server** runs the Cedar PDP, the encrypted vault (AES-256-GCM with per-credential HKDF-derived keys), the OAuth 2.0 authorization server (auth code + PKCE/S256, client credentials, consent page, refresh), and the audit pipeline.

This is the [[credential-proxy-pattern|credential proxy pattern]] taken further than the [[cyberark-conjur|Conjur]]-style design: the broker tier exists specifically so that the *agent host* never holds OAuth tokens, only Ed25519 keypair material. The broker can run on a remote host, in a container, or on a headless server — enrollment uses [RFC 8628 OAuth 2.0 Device Authorization Grant](https://datatracker.ietf.org/doc/html/rfc8628) (same flow as `gh auth login`, `aws sso login`), so there is no loopback callback and no port forwarding.

## Cedar at the policy layer

AgentCordon uses [[cedar|Cedar]] for the PDP — the same policy language [[matt-maisel|Maisel]]/[[sondera|Sondera]] use in the [[hooking-coding-agents-with-cedar-talk|coding-agent hook harness]]. Defaults are deny-by-default; every credential vend, every MCP tool call, and every OAuth scope check is evaluated against a Cedar policy with the agent's workspace identity, the requested resource, and the action.

This places AgentCordon in the **same Cedar-PDP cohort** as Sondera's harness and the [[agentic-ai-security-reference-architecture|RA Control Plane]] reference design — but applied to credential vending rather than coding-agent trajectory enforcement. Cedar's formal analyzability and entity model are well-suited to the per-agent / per-credential / per-action evaluation that this domain requires.

## MCP gateway with credential binding

The MCP gateway is the second distinguishing primitive. AgentCordon ships an **MCP Marketplace** for one-click installation of popular MCP servers (GitHub, Slack, Linear, etc.) with **automatic credential binding** — install once, share the same MCP across workspaces; one record, many bindings. The gateway:

- Proxies MCP tool calls
- Injects credentials server-side per Cedar policy
- Scans outbound responses for credential exposure before they reach the agent (response leak scanning)
- Supports OAuth 2.0 authorization code flow for MCP servers that require OAuth (not just static API keys)

Concretely, this is an [[inline-gateway-vs-runtime-instrumentation|inline-gateway]] approach to the MCP-security problem, sitting alongside [[runlayer|Runlayer]], [[helmet-security|Helmet]], and [[agentgateway|AgentGateway]] in the gateway camp — but with the credential vault and IDP collapsed into the same deployment rather than integrated separately.

## Agent identity model

Workspace identity is Ed25519 keypair, passwordless, per-project isolation. The broker enrolls workspaces via signed registration; there is no shared secret between CLI and broker. Upstream identity (the human user) is OAuth 2.0 with OIDC/SSO (Google, Azure AD, Okta, any OpenID Connect provider).

This is structurally similar to what [[keycard|Keycard]], [[okta-for-ai-agents|Okta for AI Agents]], and [[microsoft-entra-agent-id|Microsoft Entra Agent ID]] are doing in the commercial Identity-plane cohort — but as a single-binary OSS deployment rather than an integration into an existing IDaaS.

## AWS SigV4 + response-leak scanning

Two operational details worth flagging:

- **AWS SigV4 signing at the proxy** — the broker signs AWS requests on behalf of the agent so the agent never holds AWS access keys. This is the AWS-specific specialization of the credential-proxy pattern, mirroring what [[stripe|Stripe]] and Cloudflare do internally.
- **Response leak scanning** — outbound API responses are inspected for credential material *before* they are returned to the agent. This addresses the often-overlooked failure mode where the agent's own request triggers a remote echo (e.g. an API that returns the auth header in a debug field, or an MCP tool that returns an entire env dump). Without leak scanning, a credential-proxy architecture leaks anyway via response payloads.

## Scope (what it does NOT do)

AgentCordon is not a runtime defense layer in the [[capsule-security|Capsule]] / [[miggo-security|Miggo]] sense — it does not hook the agent runtime, instrument tool-call surfaces, or run behavioral classifiers. It does not perform CART/red-teaming. It does not evaluate model outputs for prompt injection inline (that responsibility lives at the gateway boundary as policy/response-scanning, not as a content-safety layer like [[lakera-guard|Lakera Guard]] or [[llamafirewall|LlamaFirewall]]).

The product's center of gravity is **identity, credentials, and policy** — RA Identity plane + Control plane PDP — not Runtime, Egress, or Observability.

## In the RA / CMM

| Slot | Role |
|---|---|
| **RA Identity plane — credential vault** | Open-source alternative to [[cyberark-conjur\|CyberArk Conjur]] for orgs that prefer self-hosted; pairs naturally with [[okta-for-ai-agents\|Okta]] / [[microsoft-entra-agent-id\|Entra]] / [[keycard\|Keycard]] as the agent-identity layer above |
| **RA Identity plane — credential proxy** | Reference OSS implementation of the [[credential-proxy-pattern\|credential proxy pattern]]; agent never sees the secret |
| **RA Control plane — PDP** | [[cedar\|Cedar]] PDP for credential and tool-call authorization; same policy substrate as the Sondera coding-agent harness |
| **RA Egress plane — MCP gateway** | Inline gateway for MCP traffic with policy enforcement and response-leak scanning |
| **CMM D2 (Identity & Authorization) L3–L4** | Per-agent credentials, programmatic rotation, no shared credentials, audit trail with correlation IDs |
| **CMM D3 (Control & Least-Agency) L3** | MCP-aware policy enforcement on tool calls |
| **CMM D7 (Observability & Detection) L3** | Every access decision logged; SOC/IR-ready |

## Comparison with adjacent products

| Vendor / Product | Center of gravity | Relation to AgentCordon |
|---|---|---|
| [[cyberark-conjur\|CyberArk Conjur]] | Enterprise vault + governance | Conjur is the incumbent enterprise vault; AgentCordon is a self-hosted OSS alternative for teams that don't want a CyberArk dependency |
| [[okta-for-ai-agents\|Okta for AI Agents]] | Agent identity + lifecycle (IDaaS) | Okta provides the agent-identity layer; AgentCordon's IDP is workspace-scoped Ed25519 + OAuth, not a full enterprise IDaaS |
| [[microsoft-entra-agent-id\|Microsoft Entra Agent ID]] | Agent identity + lifecycle (M365/Azure) | Same relation as Okta; Entra is the IDaaS layer, AgentCordon is the credential-broker tier beneath |
| [[keycard\|Keycard]] | Identity for AI agents (commercial) | Both occupy the agent-identity layer; AgentCordon's model is Ed25519 + OAuth and is OSS, Keycard's specifics are not yet public |
| [[agentgateway\|AgentGateway]] | OSS MCP gateway | Both inline-gateway-camp; AgentCordon bundles credential vault + IDP into the same deployment |
| [[runlayer\|Runlayer]] / [[helmet-security\|Helmet]] | Commercial MCP gateway | Same camp; commercial vs OSS |

The closest single-product analog is **Conjur + Okta + AgentGateway in one self-hostable Rust binary**, with the device-flow enrollment substituted for traditional IDaaS plumbing.

## Maturity signal

As of 2026-05-04 the GitHub repo shows 4 stars, 0 forks, and 7 releases. This is **early-stage / pre-traction** — the README is polished and the architecture is coherent, but real-world deployment evidence is not yet visible. Treat the page as a credible technical-design reference; do not treat it as a battle-tested production option without further validation.

## See also

- [[agentcordon-project|AgentCordon (project)]] — the open-source project / organization that ships this product
- [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] — the underlying defensive control
- [[cedar|Cedar]] — the policy substrate
- [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar (Maisel, Sondera)]] — the other 2026 Cedar-on-agents reference implementation
- [[mcp-security|MCP Security]] — the broader gateway problem
- [[non-human-identity|Non-Human Identity (NHI)]] — concept anchor
- [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] — the architectural fork
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]
