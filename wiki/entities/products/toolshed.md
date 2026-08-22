---
type: entity
entity_type: product
title: "Toolshed (Stripe)"
created: 2026-05-02
updated: 2026-06-23
tags:
  - entities
  - products
  - stripe
  - mcp
  - mcp-proxy
  - tool-annotations
status: seed
scope_axis:
  - sec-of-ai
no_public_url: "Stripe-internal product; not open-source and not publicly available. Disclosed in Andrew Bullen's Unprompted March 2026 talk; canonical reference is the talk slides + transcript under .raw/talks/."
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[stripe]]"
  - "[[andrew-bullen]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[mcp-security]]"
  - "[[oversight-layer]]"
  - "[[lethal-trifecta]]"
sources:
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_slides.pdf]]"
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md]]"
---

# Toolshed

**Stripe's central MCP proxy / tool registry.** Internal product (not open source as of Unprompted March 2026). Disclosed publicly in Andrew Bullen's [[breaking-the-lethal-trifecta-talk|"Breaking the Lethal Trifecta"]] talk.

## Function

Two functions on top of [[mcp-security|MCP]]:

1. **Third-party SaaS proxying.** When Stripe wants to hook up an external SaaS app to internal agents, the MCP connection routes through Toolshed. The proxy is the policy-enforcement point — e.g. "don't allow connections to non-Stripe tenants when writing to Google Docs / Figma." This is Stripe's answer to "MCP-as-data-exfiltration-vector": the SaaS connection itself is fine, but it has to go through one chokepoint.
2. **Tool-annotation registration.** Tools registered through Toolshed carry [[breaking-the-lethal-trifecta-talk#enforcement-slide-12-transcript|ToolAnnotations]] (`production_impacting_write`, `data_sensitivity`, `broadcasts_data_internally`). The framework reads annotations and decides whether human review applies. Inline-tool definitions in agent frameworks carry the same annotations.

## In the architecture

Toolshed is the most concrete published example of a **PDP/PEP for MCP**: PDP = the annotation policy (declarative; can be evaluated outside the agent loop), PEP = the proxy (intercepts the actual tool call). Maps directly onto the [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] / [[guardian-agent|Guardian Agent]] vocabulary.

User-side benefit Bullen emphasizes (transcript): users connect to one MCP server (Toolshed) instead of N — the centralization is operationally easier, not just a security ceiling.

## Limitations (per the speaker)

- Does **not** cover "deep agents" that bypass declared tools (Claude Code-style agents writing arbitrary code that hits arbitrary internal APIs). The work-in-progress answer is to additionally proxy raw network egress out of agent sandboxes and annotate the API endpoints themselves.

## See also

- [[smokescreen|Smokescreen]] — the egress-network-side counterpart (open source).
- [[mcp-security|MCP Security]] — the broader category Toolshed sits inside.
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — generic PDP/PEP language for what Toolshed implements.
