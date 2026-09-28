---
type: product
title: "OPA / Rego (Open Policy Agent)"
created: 2026-05-03
updated: 2026-09-25
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
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
sources:
  - https://www.openpolicyagent.org/docs
  - https://www.openpolicyagent.org/docs/policy-language
  - https://www.openpolicyagent.org/docs/policy-testing
  - https://www.openpolicyagent.org/docs/ocp
  - https://www.openpolicyagent.org/blog/note-from-teemu-tim-and-torin-to-the-open-policy-agent-community-2dbbfe494371
  - https://github.com/open-policy-agent/opa/blob/main/LICENSE
verified: 2026-09-24
verified_against: []
verified_findings: 0
verified_note: "Read in full against OPA docs (Introduction, Policy Language incl. Metadata, Policy Testing, OCP), the OPA LICENSE, the 2025-08-20 OPA creators' note, D1, D3 and the RA; fixed the D3-TIER-ENFORCE evidence, the D1 L4 crosswalk claim, the unsourced 'dominant' ranking and the Styra DAS currency. The Basis bullets and the OPA-vs-Cedar comparison stay the vault's own assessment; Argo is not on OPA's ecosystem page."
---

# OPA / Rego (Open Policy Agent)

**Open Policy Agent (OPA)** is an open-source policy engine that decouples policy decision-making from enforcement. The calling software queries OPA with structured input, for example a tool call an agent proposes, and OPA evaluates that input against the policies and data it has loaded and returns a decision.[^opa-intro] OPA is a graduated CNCF project released under the Apache 2.0 license,[^opa-intro][^opa-license] and **Rego**, its declarative policy language, was inspired by Datalog.[^opa-rego]

OPA's documentation names microservices, Kubernetes, CI/CD pipelines and API gateways among the places its policies are enforced, and carries use-case guides for Kubernetes, Envoy and Terraform among others.[^opa-intro] Its general-purpose design makes it applicable to agent authorization, though with less domain-specific syntax than [[cedar|Cedar]] for entity/action/resource models.

## Basis for OPA/Rego in the RA

- **CNCF-graduated** — mature, production-proven, with a broad ecosystem of integrations (Kubernetes, Envoy, Istio, Terraform, Argo, etc.)
- **Unified policy layer** — organizations already running OPA for infrastructure policy can extend the same engine to AI agent authorization, avoiding a second policy system
- **Flexible data model** — OPA operates on arbitrary JSON; agent metadata, tool inventories, and trust levels can all be modeled as policy data
- **Existing tooling** — Conftest (OPA for file-based config), Gatekeeper (Kubernetes webhook) and the OPA Control Plane (policy bundle management) are all OPA-native
- **Policy-as-code** — Rego policies live in version control alongside application code; CI/CD can test them before deployment with OPA's own test framework[^opa-testing]

## OPA vs Cedar

See [[cedar|Cedar]] §Cedar vs OPA/Rego for the full comparison table.

In brief: OPA is the better choice when (a) you already have OPA in your infrastructure stack, (b) you need Kubernetes or Terraform admission control alongside agent authorization, or (c) you want maximum flexibility in policy language expressiveness. Cedar is the better choice when (a) you want formal policy verification, (b) you're building a greenfield agent control plane, or (c) you prefer a more readable entity/action/resource syntax.

## In the RA / CMM

- **RA Control Plane (PDP):** OPA/Rego is listed alongside Cedar as the reference implementation for policy-language-based PDPs.
- **CMM [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3:** OPA policy rules, compared with the tier record D3-TIER grades, are acceptable policy evidence for D3-TIER-ENFORCE, the criterion that the decision point enforces the four least-agency action-risk tiers (auto / notify / confirm / block), and a decision-log entry for each tier in use completes that evidence. Cedar rules serve equally, because the criterion names no policy language.
- **CMM [[agentic-ai-security-cmm-d1-governance|D1]] L4:** Rego metadata annotations can link each rule to the controls it implements, and OPA's inspect command lists them,[^opa-metadata] so the organization's crosswalk that D1-CROSSWALK grades at this level can cite the rules that enforce a mapped control.

Styra, the company behind OPA's commercial tooling, built Styra DAS as a management layer over OPA. In August 2025 OPA's creators and many Styra staff joined Apple, and Styra's commercial OPA distribution, the OPA Control Plane, its SDKs and its Regal linter entered the process for inclusion in the CNCF OPA organization.[^opa-note] The OPA documentation now covers the OPA Control Plane, which builds policy bundles from Git repositories and serves them to OPA instances from cloud object storage.[^opa-ocp]

## See also

- [[cedar|Cedar]] — the primary alternative for pure entity/action/resource authorization
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — architectural context
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] §Control plane

- [[xacml|XACML]] — OPA/Rego supersedes its policy language while inheriting its PDP/PEP architecture. The separation of decision point from enforcement point is XACML's durable contribution, not the XML.

## Notes

[^opa-intro]: [Open Policy Agent — Introduction](https://www.openpolicyagent.org/docs), retrieved 2026-09-24. The page describes OPA as "an open source, general-purpose policy engine" and "a graduated Cloud Native Computing Foundation (CNCF) project" that "decouples policy decision-making from policy enforcement" and "generates policy decisions by evaluating the query input against policies and data". It names "microservices, Kubernetes, CI/CD pipelines, API gateways, and more", and the documentation's use cases include Kubernetes, Envoy and Istio, and Terraform.
[^opa-license]: [Open Policy Agent — LICENSE](https://github.com/open-policy-agent/opa/blob/main/LICENSE), retrieved 2026-09-24. Apache License, Version 2.0.
[^opa-rego]: [Open Policy Agent — Policy Language](https://www.openpolicyagent.org/docs/policy-language), retrieved 2026-09-24. "Rego was inspired by Datalog and extends it to support structured document models such as JSON", and "Rego is declarative".
[^opa-testing]: [Open Policy Agent — Policy Testing](https://www.openpolicyagent.org/docs/policy-testing), retrieved 2026-09-24. OPA supplies "a framework that you can use to write tests for your policies", run from the OPA command line.
[^opa-metadata]: [Open Policy Agent — Policy Language, §Metadata](https://www.openpolicyagent.org/docs/policy-language#metadata), retrieved 2026-09-24. A rule or package carries YAML annotations, among them related-resource links to "some related external resource" and a custom map of user-defined data, and "Annotations can be listed through the inspect command".
[^opa-note]: [Open Policy Agent — Note from Teemu, Tim, and Torin to the Open Policy Agent community](https://www.openpolicyagent.org/blog/note-from-teemu-tim-and-torin-to-the-open-policy-agent-community-2dbbfe494371), 2025-08-20, retrieved 2026-09-24. The note announces that "the creators of Open Policy Agent (along with many team members from Styra) have joined Apple", states that OPA "remains a CNCF graduated open source project" with no change to its governance or licensing, and records that its authors "initiated the community process" for Styra's EOPA distribution, the OPA Control Plane, the SDKs and Regal to join the CNCF OPA GitHub organization.
[^opa-ocp]: [Open Policy Agent — OPA Control Plane: Overview](https://www.openpolicyagent.org/docs/ocp), retrieved 2026-09-24. Git-based policy management that builds bundles from multiple Git repositories and distributes them to AWS S3, Google Cloud Storage or Azure Blob Storage.
