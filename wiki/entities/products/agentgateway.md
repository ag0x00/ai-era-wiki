---
type: entity
title: "AgentGateway"
created: 2026-04-30
updated: 2026-09-10
tags:
  - entities
  - products
  - mcp
  - open-source
  - a2a
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
entity_type: product
homepage: "https://github.com/agentgateway/agentgateway"
vendor: "[[solo-io|Solo.io]]"
role: "Rust-based open-source data plane for agentic AI; supports A2A and MCP protocols with RBAC and multi-tenant policy"
related:
  - "[[llamafirewall]]"
  - "[[mcp-security]]"
  - "[[a2a-protocol]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[agentdesktop]]"
  - "[[solo-io]]"
sources:
  - "[[.raw/papers/ai-security-standards-in-q1-2026.md]]"
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
---

# AgentGateway

**Sources:** [AgentGateway (repo)](https://github.com/agentgateway/agentgateway)

AgentGateway is an open-source, **Rust-based data plane** for agentic AI connectivity, created by [[solo-io|Solo.io]] and donated to the Linux Foundation, which announced the project on August 25, 2025. It serves as the network-layer control plane for both A2A (Agent-to-Agent) and MCP (Model Context Protocol) traffic.

## Architecture

- **Language**: Rust (performance-oriented; suits inline traffic interception)
- **Protocols**: A2A Protocol and MCP Protocol, the two primary inter-agent and agent-to-tool communication protocols as of 2026
- **Policy model**: RBAC (Role-Based Access Control) with multi-tenant support
- **Configuration**: dynamic via xDS (the Envoy/Istio configuration API, familiar to service-mesh operators)

## Role in the Security Stack

AgentGateway sits in the **network/protocol layer** of the [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] taxonomy. It provides:
- Centralized control over which agents can communicate with which tools/servers
- Protocol-aware policy enforcement (not generic TCP/HTTP filtering)
- Multi-tenant isolation: different teams / agent deployments can share infrastructure with policy-enforced separation

## Relationship to Other Controls

- Complements [[llamafirewall|LlamaFirewall]] (model-layer detection): AgentGateway enforces access policy; LlamaFirewall detects adversarial inputs.
- Integrates with [[mcp-security|MCP Security]] controls: AgentGateway as MCP broker provides a natural policy enforcement point for MCP tool access.
- The Okta Agent Gateway (enterprise) provides similar centralized control with Okta identity integration; AgentGateway is the open-source analog.

## Endpoint companion

[[agentdesktop|agentdesktop]] (Solo.io, Apache 2.0, September 2026) is the endpoint half of the same stack. It runs on a developer's machine, inventories the agent harnesses and MCP client configurations there, translates policy into each harness's own configuration format rather than into an OS-level policy, and binds user and device identity to each agent instance. It then supplies AgentGateway with short-lived credentials carrying that user, device and tool context, which AgentGateway injects at the traffic layer when it enforces policy on model access, MCP calls and inter-agent communication. Solo.io positions the pair as control plane and data plane: agentdesktop declares and enforces locally, AgentGateway enforces centrally.

## See Also

- [[mcp-security|MCP Security]]: the protocol layer context
- [[a2a-protocol|A2A Protocol — Agent-to-Agent]]: one of the two protocols AgentGateway supports
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]]: network/protocol layer
