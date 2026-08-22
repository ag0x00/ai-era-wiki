---
type: practice
title: "NHI Governance for AI Agents"
address: c-000189
created: 2026-04-30
updated: 2026-06-03
tags:
  - practices
  - identity
  - nhi
  - agentic-ai
  - governance
status: developing
scope_axis:
  - sec-of-ai
maturity: emerging
addresses_threat: "Credential sprawl, unmanaged machine identities, lateral movement via over-provisioned agent credentials, liability gaps when agent actions cannot be attributed"
related:
  - "[[non-human-identity]]"
  - "[[agent-identity-architecture]]"
  - "[[agent-observability]]"
  - "[[identity-credential-coupling]]"
  - "[[what-are-non-human-identities]]"
  - "[[credential-proxy-pattern]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[okta-for-ai-agents]]"
  - "[[tenuo-warrant]]"
  - "[[shadow-automation]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
sources:
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "[[what-are-non-human-identities]]"
---

# NHI Governance for AI Agents

**Non-Human Identity (NHI) governance for AI agents** is the discipline of managing the lifecycle of every credential, token, certificate, and service account assigned to AI agents — inventoried, least-privileged, rotated, and revocable — at the scale agentic deployments demand. It is the operational practice behind the [[agentic-ai-security-reference-architecture|RA]] Identity plane and is graded by the [[agentic-ai-security-cmm-d2-identity|CMM D2 Identity & Authorization]] ladder; the steps below are annotated with the D2 level each satisfies.

## On this page

- [[#When to apply]]
- [[#How]]
- [[#Why it works]]
- [[#Limits]]
- [[#How this maps to the CMM]]

## Applicability

- When an organization runs more than a handful of autonomous or semi-autonomous agents with their own service credentials.
- When agents are ephemeral (spun up per task) and credentials risk not being cleaned up after the task completes.
- When regulated environments require audit trails proving that sensitive access is attributed to a specific identity.
- Proactively — before the NHI estate becomes unmanageable, analogous to [[dspm|DSPM]] adoption before data sprawl reaches crisis levels.

## Method

### 1. Inventory and discovery — D2 L2→L3

Enumerate every service account, API key, JWT, OAuth token, and certificate assigned to agents, and tag each with owning agent, purpose, creation date, and expiry. Two refinements distinguish a mature inventory:

- **Distinguish coupled from decoupled credentials** ([[identity-credential-coupling|identity-credential coupling]]). Coupled classes (SAS tokens, storage access keys, SaaS API keys) rotate as identity rotation and need a separate migration track.
- **Find the agents you did not enroll.** Shadow-agent discovery is now a platform feature: the [[microsoft-entra-agent-id|Agent 365 Registry]] and [[okta-for-ai-agents|Okta Agent Discovery]] both surface agents not explicitly registered. Ungoverned agents at developer pace are the [[shadow-automation|shadow automation]] problem.

### 2. Adopt workload identity for internal calls — D2 L3

Issue [[spiffe|SPIFFE]] Verifiable Identity Documents (SVIDs) to each agent workload at deploy time, rotate certificates on short TTLs, and use SPIFFE/mTLS instead of static API keys for machine-to-machine calls. This eliminates the **Credential Zero** bootstrap problem: agents need no pre-stored secret to authenticate. Platform-native agent identities deliver the same property: GCP Agent Identity is SPIFFE-based, and Azure Managed Identities provide a credential-less equivalent.

### 3. Keep external credentials out of agent context — D2 L4

For external service access (SaaS APIs, external MCP servers), retrieve short-lived tokens through a vault or the [[credential-proxy-pattern|credential proxy pattern]], scoped to the minimum the task needs. Never embed static credentials in agent code or container images. Where the platform offers a credential-less identity model (Managed Identities, AWS Bedrock AgentCore token vault, GCP auth-manager), prefer it — it reaches the same zero-credentials-in-context state without operating a separate proxy.

### 4. Enforce least privilege and scope governance — D2 L4→L5+

- Review OAuth and API scopes per agent on a cadence (Identity Security Posture Management, ISPM); revoke unused or over-broad scopes.
- Apply risk-based / conditional access where the platform supports it — Conditional Access for Agent Identities (Entra ID P1) can block high-risk agents automatically.
- For high-autonomy multi-agent meshes, bound *authority per task* rather than per workload. Workload identity is task-blind; a [[tenuo-warrant|capability token]] ([[capability-based-authorization|capability-based authorization]] with [[monotonic-attenuation|monotonic attenuation]]) carries task-scoped, attenuating authority so a compromised sub-agent cannot exceed the scope minted for it. No hyperscaler ships per-task holder-bound tokens yet, so this is the L5+ frontier.

### 5. Trace every action to a human — D2 L3 (audit at D8)

Log every action alongside the agent identity and the triggering context (human instruction versus autonomous decision); this feeds [[agent-observability|Agent Observability]] and forensic attribution. The accountability primitive has matured into a named human owner: Entra Agent ID **sponsors** bind each agent to a person whose accountability transfers automatically to their manager on departure, [[microsoft-entra-agent-id|Agent 365]] writes the trail to Purview, and the Anthropic Compliance API attributes Claude-generated actions to a deployment identity. The standards gap — capturing the *delegation chain* at the protocol layer, not only in audit logs — is the subject of the NIST CAISI Concept Paper's OAuth 2.1 / OIDC extensions and is met cryptographically by a warrant's embedded chain.

### 6. Automate rotation and revocation — D2 L4

Short-lived credentials (JWTs, short-TTL API keys) shrink the exposure window. Test revocation: know how long it takes from "agent compromised" to "all credentials revoked." **Map dependencies before rotating** — per [[what-are-non-human-identities|Oasis Security]], "where rotation is operationally risky, invest in dependency mapping to understand what will break before making changes." Without a per-credential consumer graph, automated rotation breaks production, acutely so for [[identity-credential-coupling|coupled credentials]] where rotation is identity rotation.

### 7. Bind the lifecycle to code-pace, not HR-pace — D2 L3

Legacy IAM is built around HR-driven joiner/mover/leaver events. NHIs have no HR events; they have code commits and deploys, and that mismatch is why legacy IAM and PAM fail for them at scale ([[what-are-non-human-identities|Oasis Security]]). Bind the NHI lifecycle to the deploy pipeline instead:

- A new NHI requires a registration step in CI/CD before the deploy succeeds.
- The owner field is mandatory — deploys without an owner are blocked.
- Decommission is automatic when the application is retired (CI/CD triggers a reaper).
- Ownership transfers automatically when the application changes hands.

This aligns governance with the actual rate of NHI creation and prevents the pace-mismatch that produces [[shadow-automation|shadow automation]].

## Mechanism

Most credential incidents involving AI agents trace to over-provisioning, stale credentials, or poor discovery rather than sophisticated cryptographic attacks. NHI governance addresses the **people-and-process root cause** (Insight Partners' framing) by creating systematic visibility and lifecycle controls, making mismanagement auditable and correctable.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] treats the NHI inventory and least-privilege baseline as the foundation safe scaling rests on: as agent counts climb, machine identities come to outnumber human users by orders of magnitude, and an estate that cannot be inventoried or scoped cannot be governed at any higher tier. The report distinguishes this NHI authentication layer from the Agent Identity governance layer above it (see [[non-human-identity|Non-Human Identity]] and [[agent-identity-architecture|AI Agent Identity Architecture]]), and the discipline below is the practitioner form of the former.

## Limits

- **Governance lags deployment.** Organizations that deploy agents faster than they onboard them to identity governance are the target demographic; governance is reactive unless built into the deploy pipeline (step 7).
- **Incumbent tools need adaptation.** IAM/PAM/IGA built for human lifecycles carry real configuration overhead for ephemeral, high-volume agent identities, though platform-native agent identity has reduced this overhead since 2026.
- **Per-task authority is still maturing.** No platform ships per-task holder-bound capability tokens; the only implementation is an early-stage OSS primitive, so step 4's L5+ tail is genuinely leading-edge.

## Mapping to the CMM

The seven steps above are the practitioner view of the [[agentic-ai-security-cmm-d2-identity|D2 Identity & Authorization]] ladder. Mapped to levels: per-agent identity + owner + deploy-pipeline lifecycle + coupled/decoupled inventory at **L3**; zero-credentials-in-context + automated rotation against a dependency map + kill switch at **L4**; a unified governance program with shadow-agent discovery and conditional access at **L5**; per-task attenuating capability tokens at **L5+**. D2-L3 is the highest-leverage rung in the whole CMM: per-agent egress policy and per-agent behavioral baselining cannot be applied below it.
