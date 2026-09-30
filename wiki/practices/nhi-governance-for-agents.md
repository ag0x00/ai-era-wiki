---
type: practice
title: "NHI Governance for AI Agents"
address: c-000189
created: 2026-04-30
updated: 2026-09-29
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
  - "[[falcon-guardian]]"
  - "[[agentdesktop]]"
sources:
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "[[what-are-non-human-identities]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current Microsoft identity documentation and CMM criteria checked; archived source set was not verified in full."
---

# NHI Governance for AI Agents

**Non-Human Identity (NHI) governance for AI agents** manages the lifecycle of agent identities and credentials: inventory, ownership, grants, rotation, and revocation. The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] places identity and credential mediation on the action path. The [[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]] deep dive grades the relevant outcomes for one deployment.

## On this page

- [[#Applicability]]
- [[#Method]]
- [[#Mechanism]]
- [[#Limits]]
- [[#Mapping to the CMM]]

## Applicability

- When an agent uses its own identity or a delegated credential to call another service; the inventory and lifecycle work grows as agents multiply.
- When agents are ephemeral (spun up per task) and credentials risk not being cleaned up after the task completes.
- When regulated environments require audit trails proving that sensitive access is attributed to a specific identity.
- When agent creation and retirement occur through code or platform deployment rather than a human joiner/mover/leaver process.

## Method

### 1. Inventory and discovery — D2 L2, L3 and L5

Enumerate every service account, API key, JWT, OAuth token, and certificate assigned to agents, and tag each with owning agent, purpose, creation date, and expiry. Two refinements distinguish a mature inventory:

- **Distinguish coupled from decoupled credentials** ([[identity-credential-coupling|identity-credential coupling]]). Coupled classes (SAS tokens, storage access keys, SaaS API keys) rotate as identity rotation and need a separate migration track.
- **Find the agents that were never enrolled.** Shadow-agent discovery is now a platform feature at three vantage points: [[microsoft-agent-365|the Agent Registry]] and [[okta-for-ai-agents|Okta Agent Discovery]] surface agents from identity telemetry, [[falcon-guardian|CrowdStrike Falcon Guardian]] enumerates them from endpoint process telemetry, and [[agentdesktop|agentdesktop]] reads the harness configuration on a developer's machine. The last two reach agents that authenticate to no directory. Ungoverned agents at developer pace are the [[shadow-automation|shadow automation]] problem.

### 2. Adopt workload identity for internal calls — D2 L2 to L3

Give each agent an identity issued by an identity provider and verify its signed assertion at the services it calls. A workload can use [[spiffe|SPIFFE]] Verifiable Identity Documents (SVIDs) and mTLS; platform-managed identities can supply another credential-less route. The CMM grades a separate agent identity at D2 L2 and issuer verification at D2 L3, without requiring a particular identity protocol.

### 3. Keep external credentials out of agent context — D2 L4

For external service access (SaaS APIs, external MCP servers), retrieve short-lived tokens through a vault or the [[credential-proxy-pattern|credential proxy pattern]], scoped to the minimum the task needs. Never embed static credentials in agent code or container images. A platform-managed identity or mediated token vault can keep credentials out of the agent's working context without a separate proxy.

### 4. Enforce least privilege and task scope — D2 L4 to L5, D3 L4 and D5 L5

- Review OAuth and API scopes per agent on a cadence (Identity Security Posture Management, ISPM); revoke unused or over-broad scopes.
- Apply conditional access where the platform and tenant license support it; for example, Entra Agent ID policies can block a selected agent or an agent reported as risky.
- Bind sessions and delegated credentials to the current task and enforce that scope on each tool call. A [[tenuo-warrant|capability token]] with [[monotonic-attenuation|attenuating authority]] is one implementation; it is not a required CMM token format. D2-TASKBIND and D2-DELEGATE-TOKEN grade token fields, D3-TASKSCOPE grades the call-time decision, and D5-TASK-EGRESS grades agent-task-destination binding where the applicable outbound path exists.

### 5. Attribute actions to an agent and owner — D2 L3 (audit at D7)

Log each action alongside the agent identity and its triggering context (human instruction or autonomous decision). This feeds [[agent-observability|Agent Observability]] and forensic attribution. The accountability record also needs a named human owner. [Microsoft Entra Agent ID sponsorship](https://learn.microsoft.com/en-us/entra/agent-id/manage-agent-identities-admin) is one implementation: its lifecycle workflow can transfer sponsorship to a departing sponsor's manager. [[microsoft-agent-365|Microsoft Agent 365]] writes agent activity to Purview, and the Anthropic Compliance API attributes Claude-generated actions to a deployment identity. The NIST CAISI Concept Paper explores OAuth 2.1 / OIDC extensions for delegation-chain capture at the protocol layer. A [[tenuo-warrant|warrant]] can carry such a chain cryptographically.

### 6. Automate rotation and revocation — D2 L4

Short-lived credentials (JWTs, short-TTL API keys) shrink the exposure window. Test revocation: know how long it takes from "agent compromised" to "all credentials revoked." **Map dependencies before rotating** — per [[what-are-non-human-identities|Oasis Security]], "where rotation is operationally risky, invest in dependency mapping to understand what will break before making changes." Without a per-credential consumer graph, automated rotation breaks production, acutely so for [[identity-credential-coupling|coupled credentials]] where rotation is identity rotation.

### 7. Bind the lifecycle to code-pace — D2 L3

Human-identity workflows often start from HR joiner, mover, and leaver events. Agent identities may instead be created and retired with code and deployments, so an HR-only workflow misses them ([[what-are-non-human-identities|Oasis Security]]). Bind the NHI lifecycle to the deploy pipeline:

- A new NHI requires a registration step in CI/CD before the deploy succeeds.
- The owner field is mandatory — deploys without an owner are blocked.
- Decommission is automatic when the application is retired (CI/CD triggers a reaper).
- Ownership transfers automatically when the application changes hands.

This aligns governance with the actual rate of NHI creation and prevents the pace-mismatch that produces [[shadow-automation|shadow automation]].

## Mechanism

Over-provisioning, stale credentials, and poor discovery expose agent credentials. NHI governance makes those lifecycle failures visible and correctable.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] treats the NHI inventory and least-privilege baseline as the foundation safe scaling rests on: as agent counts climb, machine identities come to outnumber human users by orders of magnitude, and an estate that cannot be inventoried or scoped cannot be governed at any higher tier. The report distinguishes this NHI authentication layer from the Agent Identity governance layer above it (see [[non-human-identity|Non-Human Identity]] and [[agent-identity-architecture|AI Agent Identity Architecture]]), and the discipline below is the practitioner form of the former.

## Limits

- **Governance lags deployment.** Organizations that deploy agents faster than they onboard them to identity governance are the target demographic; governance is reactive unless built into the deploy pipeline (step 7).
- **Incumbent tools need adaptation.** IAM/PAM/IGA built for human lifecycles carry real configuration overhead for ephemeral, high-volume agent identities, though platform-native agent identity has reduced this overhead since 2026.
- **Per-task capability-token implementations vary.** A warrant can carry the task and holder binding, but the assessor tests the deployed session, delegation, policy, and egress decisions rather than assuming that a token format establishes them.

## Mapping to the CMM

The seven steps above support the [[agentic-ai-security-cmm-d2-identity|D2 Identity and Authorization]] ladder. L2 establishes separate identities and an inventory. L3 adds owner, pipeline lifecycle, credential classification, issuer verification, and action attribution. L4 adds task-bound sessions, scoped authorization, rotation and dependency mapping, credentials kept outside agent context, and a tested kill switch. L5 adds a maintained identity graph, shadow discovery, ownership transfer, and applicable conditional access. Task-scope authorization is graded separately in [[agentic-ai-security-cmm-d3-control-least-agency|D3]], task-bound outbound decisions in [[agentic-ai-security-cmm-d5-egress-network|D5]], and identity-activity detection in [[agentic-ai-security-cmm-d7-observability|D7]]. Missing D2 identity evidence blocks the particular downstream criterion it prevents the assessor from proving; it does not numerically cap another domain.
