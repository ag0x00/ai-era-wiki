---
type: concept
title: "Non-Human Identity (NHI)"
address: c-000187
created: 2026-04-30
updated: 2026-09-29
tags:
  - concepts
  - identity
  - agentic-ai
  - iam
  - nhi
status: developing
scope_axis:
  - sec-of-ai
complexity: intermediate
domain: identity-and-access-management
source_url: "https://owasp.org/www-project-non-human-identities-top-10/"
aliases:
  - "NHI"
  - "machine identity"
  - "workload identity"
related:
  - "[[agent-identity-architecture]]"
  - "[[nhi-governance-for-agents]]"
  - "[[credential-proxy-pattern]]"
  - "[[identity-credential-coupling]]"
  - "[[what-are-non-human-identities]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[capability-based-authorization]]"
  - "[[tenuo-warrant]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[okta-for-ai-agents]]"
  - "[[oasis-security]]"
  - "[[shadow-automation]]"
  - "[[agent-observability]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-zt4ai]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[falcon-guardian]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[agentdesktop]]"
  - "[[agent-catalog]]"
sources:
  - "https://owasp.org/www-project-non-human-identities-top-10/"
  - "https://spiffe.io/docs/latest/spiffe-specs/spiffe-id/"
  - "https://spiffe.io/docs/latest/spire-about/spire-concepts/"
  - "https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current SPIFFE and SPIRE documentation plus primary Wiz incident report checked 2026-09-29."
---

# Non-Human Identity (NHI)

A non-human identity is a principal assigned to software, such as a workload, service, or agent. The principal is distinct from the credential that authenticates it and from the human responsible for the work. An assessment must identify all three before it can determine an agent's effective authority.

## Identity model

| Element | Assessment question |
|---|---|
| Agent or workload principal | Which identity does the receiving system authenticate? Is it unique to the assessed agent or shared with other software? |
| Authentication material | Which credential proves the principal, and who can use it? |
| Authorized session | What task scope and expiry does the current grant cover? Can it be reused on another route? |
| Represented human | Who started the task, or who owns an event-driven run? Does the downstream decision preserve that relationship? |

A service account can be a principal. Its password is a credential. A [SPIFFE ID](https://spiffe.io/docs/latest/spiffe-specs/spiffe-id/) names a workload identity. An SVID is a verifiable document that presents it. A shared workload identity does not distinguish agents that run under the same workload. An API key may identify only an application or account, even when one agent happens to hold it. The authenticated name must be checked against the agent inventory before an action is attributed to that agent.

## Lifecycle and delegation

The lifecycle record covers the following:

- Register each production agent and its principal under an accountable owner.
- Record everyone who can use each credential.
- Issue a scoped grant for the current task, including the represented human when there is one.
- Rotate each credential according to its class.
- Revoke the identity and usable sessions when the agent retires.
- Transfer accountability when its owner changes.
- Record task authority and action outcome to reconstruct a delegated call.

A short-lived credential limits exposure but does not establish least privilege. A task-scoped grant can still be overbroad. The receiving service or a complete mediation path must enforce the intended action and resource limits.

## Implementation variants

| Pattern | What it establishes | Limit to test |
|---|---|---|
| Dedicated agent principal with a managed credential | The agent can authenticate under its own name. | Other workloads must not be able to use the same material or assume the same role. |
| Attested workload identity | [SPIRE](https://spiffe.io/docs/latest/spire-about/spire-concepts/) can attest a workload and deliver an SVID through its Workload API. | The workload boundary may contain several agents; attestation alone does not authorize a task. |
| Credential broker or proxy | The agent requests an action while the broker holds the downstream secret. | The broker must bind its decision and logs to the agent's task authority and accountable owner. |

The [Wiz report on a Microsoft AI research repository](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers) documents a broadly scoped storage access token exposed through a public link. It illustrates the consequence of credential scope and disclosure; it is not evidence of an autonomous agent compromise.

## Assessment use

[[agentic-ai-security-cmm-d2-identity|CMM D2]] owns principal and credential governance, including task binding. [[agentic-ai-security-cmm-d3-control-least-agency|D3]] owns the call-time authorization decision even when the principal is valid. [[agentic-ai-security-cmm-d7-observability|D7]] owns the action record that attributes a call to an agent and accountable human. The [[agentic-ai-security-reference-architecture|Reference Architecture]] places identity issuance and credential use at different trust boundaries; neither a certificate nor a vendor's “agent identity” label substitutes for evidence of the deployed principal and its effective grants.

<!-- sources:auto -->
## Sources

- [Non-Human Identity (NHI)](https://owasp.org/www-project-non-human-identities-top-10/)
- [spiffe.io](https://spiffe.io/docs/latest/spiffe-specs/spiffe-id/)
- [spiffe.io](https://spiffe.io/docs/latest/spire-about/spire-concepts/)
- [wiz.io](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
<!-- /sources -->
