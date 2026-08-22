---
type: product
title: "OPA / Rego (Open Policy Agent)"
created: 2026-05-03
updated: 2026-08-21
tags:
  - products
  - policy-language
  - authorization
  - control-plane
status: stub
scope_axis:
  - sec-of-ai
license: Apache 2.0
org: CNCF (Cloud Native Computing Foundation)
related:
  - "[[xacml|XACML]]"
  - "[[cedar]]"
  - "[[oversight-layer]]"
  - "[[least-agency-principle]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
---

# OPA / Rego (Open Policy Agent)

**Open Policy Agent (OPA)** is a CNCF-graduated open-source policy engine (Apache 2.0). **Rego** is its policy language — a Datalog-inspired declarative language. OPA decouples policy from application code: it receives a query (e.g., "is this agent allowed to call this tool?"), evaluates it against loaded policies and data, and returns a decision.

OPA is the dominant policy engine for Kubernetes admission control and infrastructure policy (Terraform, Envoy), and is widely deployed in organizations that already have OPA infrastructure. Its general-purpose design makes it applicable to agent authorization, though with less domain-specific syntax than [[cedar|Cedar]] for entity/action/resource models.

## Basis for OPA/Rego in the RA

- **CNCF-graduated** — mature, production-proven, with a broad ecosystem of integrations (Kubernetes, Envoy, Istio, Terraform, Argo, etc.)
- **Unified policy layer** — organizations already running OPA for infrastructure policy can extend the same engine to AI agent authorization, avoiding a second policy system
- **Flexible data model** — OPA operates on arbitrary JSON; agent metadata, tool inventories, and trust levels can all be modeled as policy data
- **Existing tooling** — Conftest (OPA for file-based config), Styra DAS (commercial management layer), Gatekeeper (Kubernetes webhook) are all OPA-native
- **Policy-as-code** — Rego policies live in version control alongside application code; CI/CD can test policies before deployment

## OPA vs Cedar

See [[cedar|Cedar]] §Cedar vs OPA/Rego for the full comparison table.

In brief: OPA is the better choice when (a) you already have OPA in your infrastructure stack, (b) you need Kubernetes or Terraform admission control alongside agent authorization, or (c) you want maximum flexibility in policy language expressiveness. Cedar is the better choice when (a) you want formal policy verification, (b) you're building a greenfield agent control plane, or (c) you prefer a more readable entity/action/resource syntax.

## In the RA / CMM

- **RA Control Plane (PDP):** OPA/Rego is listed alongside Cedar as the reference implementation for policy-language-based PDPs.
- **CMM D3 L3:** OPA policy repository is an acceptable evidence artifact for the criterion that the four least-agency action-risk tiers (auto / notify / confirm / block) are implemented (Cedar or OPA both satisfy it).
- **CMM D1 L4:** Standards crosswalk matrix can be maintained in OPA policies.

Styra DAS provides a commercial management layer over OPA with governance workflows, audit logs, and pre-built policy libraries — a common enterprise deployment.

## See also

- [[cedar|Cedar]] — the primary alternative for pure entity/action/resource authorization
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — architectural context
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] §Control plane

- [[xacml|XACML]] — OPA/Rego supersedes its policy language while inheriting its PDP/PEP architecture. The separation of decision point from enforcement point is XACML's durable contribution, not the XML.
