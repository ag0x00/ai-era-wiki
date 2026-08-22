---
type: product
title: "Cedar"
created: 2026-05-03
updated: 2026-08-21
tags:
  - products
  - policy-language
  - authorization
  - control-plane
  - coding-agents
  - reference-monitor
status: developing
scope_axis:
  - sec-of-ai
license: Apache 2.0
org: AWS (Amazon Web Services)
related:
  - "[[xacml|XACML]]"
  - "[[opa]]"
  - "[[oversight-layer]]"
  - "[[least-agency-principle]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[tenuo-warrant]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[sondera]]"
  - "[[capability-based-authorization-talk]]"
  - "[[agentcordon]]"
---

# Cedar

Cedar is an open-source policy language and evaluation engine designed for **fine-grained, application-level authorization**. Created by AWS and open-sourced under Apache 2.0 (2023). Chosen as the primary policy language in the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] Control Plane (PDP layer). AWS released an AI governance-specific tooling update in March 2026.

## Basis for Cedar in AI agent authorization

Cedar's design maps well onto the agentic-AI PDP problem:

- **Entities and attributes** — Cedar models agents, tools, resources, and users as typed entities with attributes. This makes it natural to write policies like "agent with role X may call tool Y only when attribute Z is present."
- **Policies are data, not code** — Cedar policies are stored separately from application logic and can be updated without redeployment. For an agent platform, this means the policy layer is decoupled from the model layer.
- **Formal verification** — Cedar's semantics are mathematically specified, enabling policy analysis tools (e.g., "can this policy ever allow X?"). AWS provides the Cedar policy analyzer.
- **Deny-by-default** — Cedar is deny-by-default; any action not explicitly permitted is denied. This aligns with the [[least-agency-principle|Least Agency Principle]].
- **Performance** — Cedar is written in Rust; evaluation is typically sub-millisecond even for complex policy sets. Suitable for inline PDP use.

## Cedar vs OPA/Rego

Cedar and [[opa|OPA/Rego]] are the two dominant OSS policy engines in this space:

| Dimension | Cedar | OPA/Rego |
|---|---|---|
| Language style | Declarative, SQL-like | Datalog-inspired, Turing-complete |
| Formal verification | Yes (algebraic semantics) | Limited |
| Performance | Sub-ms (Rust) | Sub-ms (Go) |
| Ecosystem | AWS-led, newer | CNCF, broad; Kubernetes-native |
| Best for | Application-level authz (entity/action/resource model) | Infrastructure-level policy (Kubernetes admission, Terraform) |
| AI governance use | March 2026 AWS AI governance release | Via general policy primitives |

For pure agentic-AI control-plane use (PDP for tool calls, capability enforcement), Cedar's entity model is often more natural. For teams already running OPA for Kubernetes or Terraform admission control, OPA may be the lower-friction choice.

## In the RA / CMM

- **RA Control Plane (PDP):** Cedar is the reference implementation for policy-language-based PDPs, alongside OPA/Rego.
- **CMM D3 L3:** Cedar or OPA policy repository is the evidence artifact for the criterion that the four least-agency action-risk tiers (auto / notify / confirm / block) are implemented.
- **CMM D3 L5:** Capability tokens with cryptographic binding extend Cedar's policy model; [[tenuo-warrant|Tenuo Warrants]] can use Cedar as the constraint language.
- **D1 L4:** Standards crosswalk matrix is maintained in Cedar policies (or OPA equivalent).

AWS offers Cedar as a managed service for AI agent authorization (March 2026 release), enabling policy evaluation without self-hosting the engine.

## Cedar in the coding-agent hook harness (Sondera, 2026)

[[matt-maisel|Matt Maisel]] at [[sondera|Sondera]] presented an open-source **hook-based Cedar harness** for coding agents at Unprompted March 2026. The harness wires per-agent lifecycle hooks (Cursor, Claude Code, Gemini CLI) to a Cedar PDP that mediates every trajectory event (action, observation, control, state). Cedar was chosen specifically because:

- Its **formal analyzability** (Lean symbolic compiler) enables a *policy agent* to generate Cedar policies and then verify them for contradictions, vacuity, and shadow subsets — using Cedar's own tools over MCP.
- Its **ABAC entity model** maps naturally to coding-agent trajectory entities (agent, user, trajectory resource) with attributes for IFC sensitivity labels, YARA signature matches, and safety-model classifications.

This is the primary production-grade, open-source reference implementation of Cedar for agentic AI enforcement. See [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]] for the full architecture.

### Cedar's statelessness as a constraint

Cedar is inherently stateless — each policy evaluation has no memory of previous evaluations. The Sondera harness compensates with entity and trajectory stores that track IFC taint labels across turns. For long-running agents, this creates an integrity dependency on the store. Temporal logic systems (linear temporal logic, others) may be a better fit for policies that need to reason over the full trajectory; this is an open research area.

## Cedar in the AgentCordon credential broker (2026)

[[agentcordon|AgentCordon]] — a self-hostable open-source Agentic IDP and credential broker — uses Cedar as the PDP for **credential vending and MCP tool-call authorization**. This is a different application from Sondera's coding-agent harness: instead of mediating coding-agent trajectory events, Cedar in AgentCordon evaluates whether a given workspace identity (Ed25519-keyed) may vend a given credential or invoke a given MCP tool. Same policy substrate, different enforcement surface — strengthening the case that Cedar is becoming the de-facto policy language across the agentic-AI control plane.

## See also

- [[opa|OPA/Rego]] — the primary alternative
- [[tenuo-warrant|Tenuo Warrant]] — capability tokens that can use Cedar-syntax constraints
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — the architectural context
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] §Control plane
- [[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar — Maisel, Sondera]] — the coding-agent harness built on Cedar
- [[capability-based-authorization-talk|Capability-Based Authorization — Niyikiza, Tenuo]] — delegation-aware Cedar application
- [[agentcordon|AgentCordon]] — Cedar PDP applied to credential vending and MCP authorization

- [[xacml|XACML]] — Cedar supersedes its XML policy language, but deploys the same PDP/PEP/PIP/PAP separation. The architecture outlived the language, which is why XACML remains the vocabulary citation even where Cedar is the implementation.
