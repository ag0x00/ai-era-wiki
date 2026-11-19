---
type: framework
title: "A2A Protocol (Agent-to-Agent)"
created: 2026-04-30
updated: 2026-10-06
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
last_substantive_update: 2026-05-26
governance: "Linux Foundation (Agentic AI Foundation)"
contributed_by: "[[google|Google]]"
current_version: "Repository release v1.0.1; published specification labels v1.0.0 (checked 2026-09-29)"
canonical_spec: "https://a2a-protocol.org/latest/specification/"
canonical_repo: "https://github.com/a2aproject/A2A"
scope: "Open protocol for agent discovery, task exchange, and agent-to-agent communication"
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
  - "[[owasp-agentic-ai-top-10]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[agentic-ai-security-ra-gaps]]"
  - "[[spiffe]]"
  - "[[agent-catalog]]"
  - "[[shadow-agent-admission-control]]"
  - "[[shadow-automation]]"
sources:
  - "https://github.com/a2aproject/A2A/releases"
  - "https://a2a-protocol.org/latest/specification/"
  - "https://a2a-protocol.org/latest/topics/agent-discovery/"
  - "https://a2a-protocol.org/latest/blog/2026/08/27/a-new-chapter-for-a2a-joining-the-agentic-ai-foundation/"
  - "https://docs.cloud.google.com/agent-registry/manual-registration"
  - "https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/registry-sync-records.html"
  - "https://docs.cloud.google.com/agent-registry/register-agents"
  - "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-agent-registry-generally-available/"
verified: 2026-10-06
verified_against:
  - ".raw/articles/a2a-protocol-agent-discovery-2026-10-06.md"
  - ".raw/articles/a2a-protocol-joining-agentic-ai-foundation-2026-10-06.md"
  - ".raw/articles/a2a-protocol-specification-v1-0-0-2026-10-06.md"
  - ".raw/articles/aws-agent-registry-sync-records-2026-10-06.md"
  - ".raw/articles/google-cloud-agent-registry-manual-registration-2026-10-06.md"
verified_findings: 0
verified_note: "Whole page read against the A2A 1.0.0 spec (§7, §8, §13), discovery guide, AAIF blog and the Google and AWS registry captures (register-agents and AWS GA notice also opened; the manifest does not map them to this page). Fixed the discovery-route overstatement and the AWS JSON/auto-detection attribution, and cited the AAIF acceptance in the body. K5/K6 signing corrections confirmed."
---

# A2A Protocol (Agent-to-Agent)

The Agent-to-Agent (A2A) Protocol defines how an agent discovers and exchanges work with another agent. It supplies message and task formats and protocol bindings. The receiving service still decides who may invoke each operation and what the caller may see.

## Version and scope

As checked on 2026-09-29, the [A2A repository release list](https://github.com/a2aproject/A2A/releases) identifies **v1.0.1** as its latest release. The [published specification](https://a2a-protocol.org/latest/specification/) still labels its latest released version **1.0.0**. An assessment records the version and binding implemented by each peer; the repository tag alone does not establish the deployed wire profile. On 2026-08-27 the [A2A project announced](https://a2a-protocol.org/latest/blog/2026/08/27/a-new-chapter-for-a2a-joining-the-agentic-ai-foundation/) that A2A had been accepted as a Growth Stage project at the Linux Foundation's Agentic AI Foundation (AAIF), alongside MCP.

A2A covers agent-to-agent exchange. [[mcp-security|Model Context Protocol security]] covers an agent's use of MCP tools. An agent may use both, with separate trust and authorization decisions at each boundary.

## Discovery and communication

Each server makes an [Agent Card](https://a2a-protocol.org/latest/topics/agent-discovery/) available. It describes the service's interface, advertised abilities, and authentication requirements. Discovery may use a well-known URL, a registry, or configured information. In the [discovery guide](https://a2a-protocol.org/latest/topics/agent-discovery/), a server hosts its Card at the well-known path or publishes it to a registry, and an operator configures a client with the Card directly; the guide states that the specification "does not prescribe a standard API for curated registries". [Google Cloud Agent Registry](https://docs.cloud.google.com/agent-registry/manual-registration) imports a Card by URL or pasted JSON, and [AWS Agent Registry](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/registry-sync-records.html) synchronizes a record from a Card URL. Google registers agents automatically only on supported Google Cloud runtimes within one project ([Register agents](https://docs.cloud.google.com/agent-registry/register-agents)), and AWS detects agents automatically on AgentCore runtime and gateways ([general availability notice](https://aws.amazon.com/about-aws/whats-new/2026/08/aws-agent-registry-generally-available/)). [[shadow-agent-admission-control|Shadow Agent Admission Control]] applies these registries to agents that were never registered, the population [[shadow-automation|Shadow Automation]] describes. Item 13 of [[agentic-ai-security-ra-gaps|Agentic AI Security RA Gaps]] tracks registry interoperability and cross-registry reconciliation as open questions. A Card's name and capability claims are metadata; they do not by themselves authenticate a principal or grant access.

The specification supports task and message exchange through these core bindings:

- JSON-RPC
- gRPC
- HTTP+JSON

The selected binding and task operations determine the exposed request and callback routes. Where the Card declares an authentication scheme, such as OAuth 2.0 or mutual TLS, the implementation must enforce it. The [specification](https://a2a-protocol.org/latest/specification/#7-authentication-and-authorization) handles identity "at the protocol layer, not within A2A semantics", so a receiver that declares mutual TLS can accept X.509-SVIDs issued in an enterprise [[spiffe|SPIFFE / SPIRE]] trust domain. [[agent-identity-architecture|AI Agent Identity Architecture]] places that peer identity beneath the task authorization the receiver still applies.

## Security boundary

The [specification's §13 security considerations](https://a2a-protocol.org/latest/specification/#13-security-considerations) require an A2A server to authorize every protocol-operation request and scope returned resources to the caller, including requests without a context identifier. Production transport uses encryption. The specification recommends TLS 1.3; the CMM's D5-A2A-TLS separately requires an older-version handshake to fail. Authentication of a connection does not replace authorization of a task operation or a downstream tool call.

[§8.4](https://a2a-protocol.org/latest/specification/#84-agent-card-signing) defines optional Agent Card signatures. A signature can authenticate Card content only when the client verifies it against an accepted signer and binds that signer to the expected peer. The specification does not make every Card signed. Where discovery relies on a registry or direct configuration instead, the assessor records how peer identity and Card changes are checked.

A2A task and context identifiers do not establish the represented human's delegated authority. A deployment that delegates work must bind the receiving agent's effective permission to the original task and caller. It must also define how it refuses duplicate or stale requests on consequential operations. Push notifications add a callback boundary; [§13](https://a2a-protocol.org/latest/specification/#13-security-considerations) addresses callback authentication and server-side request forgery. These deployment controls require evidence beyond protocol conformance.

## Assessment use

| CMM owner | Evidence to inspect |
|---|---|
| [[agentic-ai-security-cmm-d2-identity\|D2 — identity]] | Binding of peer identity to the original caller's task authority. |
| [[agentic-ai-security-cmm-d5-egress-network\|D5 — network reach]] | Enforcement of the selected inter-agent trust profile. |
| [[agentic-ai-security-cmm-d7-observability\|D7 — observability]] | Attributed trail from task receipt to downstream action. |
| [[agentic-ai-security-cmm-d8-supply-chain\|D8 — release assurance]] | Release-linked test of the approved Card signing profile. |

The [[agentic-ai-security-reference-architecture|Reference Architecture]] locates the inter-agent boundary. D5-A2A-REPLAY requires a receiver refusal test. D5-A2A-SIGN tests Card verification at runtime. D8-A2A-SIGN-TEST checks it before release. Those outcomes are CMM choices; A2A supplies mechanisms and interfaces, not a maturity score.

<!-- sources:auto -->
## Sources

- [github.com](https://github.com/a2aproject/A2A/releases)
- [a2a-protocol.org](https://a2a-protocol.org/latest/specification/)
- [a2a-protocol.org](https://a2a-protocol.org/latest/topics/agent-discovery/)
- [a2a-protocol.org](https://a2a-protocol.org/latest/blog/2026/08/27/a-new-chapter-for-a2a-joining-the-agentic-ai-foundation/)
- [docs.cloud.google.com](https://docs.cloud.google.com/agent-registry/manual-registration)
- [docs.aws.amazon.com](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/registry-sync-records.html)
- [docs.cloud.google.com](https://docs.cloud.google.com/agent-registry/register-agents)
- [aws.amazon.com](https://aws.amazon.com/about-aws/whats-new/2026/08/aws-agent-registry-generally-available/)
<!-- /sources -->
