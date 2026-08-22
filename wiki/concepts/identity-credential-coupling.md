---
type: concept
title: "Identity-Credential Coupling"
address: c-000190
created: 2026-04-30
updated: 2026-08-21
tags:
  - concepts
  - identity
  - nhi
  - credentials
  - rotation
status: developing
no_public_url: "Wiki-original synthesis derived from NHI material; no single external canonical source"
scope_axis:
  - sec-of-ai
related:
  - "[[non-human-identity]]"
  - "[[credential-proxy-pattern]]"
  - "[[what-are-non-human-identities]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[anti-patterns-and-failure-modes|RA and CMM Anti-Patterns and Failure Modes]]"
sources:
  - "[[what-are-non-human-identities]]"
---

# Identity-Credential Coupling

A property of certain Non-Human Identities where **the credential string IS the identity** — the authentication material is not separable from the principal it represents. Rotation cannot be performed independently of identity change.

The framing comes from [[what-are-non-human-identities|Oasis Security]]:

> "Special considerations arise in scenarios where identities are inseparable from the authentication string, as seen in Storage account access keys, Shared Access Signatures (SAS) tokens, and API keys for Software as a Service (SaaS) applications like Snowflake. In such instances, the authentication method encapsulates permissions configuration, complicating IAM and IGA."

## Coupled vs decoupled identity-credential pairs

| Decoupled (rotatable) | Coupled (rotation = identity rotation) |
|---|---|
| Service Principal with separate client secret | Storage Access Keys |
| OAuth client_id with separate client_secret | SAS tokens |
| Username + password | Snowflake-class API keys |
| [[spiffe\|SPIFFE]] SVID + private key | Personal Access Tokens (PATs) where the token IS the access |
| IAM role assumed via short-lived STS credentials | Shared bearer tokens with no separate principal record |

For decoupled pairs, the [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] works as designed: the proxy holds the secret, the agent never sees it, rotation is an internal concern. For coupled pairs, the proxy can intermediate access but **cannot prevent the underlying identity from changing on rotation** — every consumer must be updated.

## Operational consequences

Three consequences flow from coupling:

1. **Rotation breaks consumers.** Every system that has the old key wired in must be updated atomically. This is why SAS tokens and storage access keys are rarely rotated in practice — the operational risk is high.
2. **Permissions are encoded in the credential.** A SAS token's permissions are defined at issuance; you cannot adjust scope without issuing a new token (= new identity). This conflicts with continuous-least-privilege programs.
3. **Audit becomes harder.** A rotated SAS token is observably different from the prior one; correlation across rotation events requires explicit lineage tracking that most systems do not provide.

## Consequences for the CMM

The [[agentic-ai-security-cmm-d2-identity|D2 Identity deep dive]] folds coupling directly into the identity ladder, and the [[agentic-ai-security-reference-architecture|RA]] lists coupled-credential migration as an Identity-plane capability:

- **D2 L3** inventory must distinguish coupled from decoupled NHIs. Every coupled credential is a candidate for migration to a decoupled alternative — SAS token or storage access key → Azure Managed Identity + RBAC; long-term cloud keys → AWS IAM Roles Anywhere (X.509 → short-lived STS) or GCP Workload Identity Federation.
- **D2 L4** keeps an *active migration plan* off coupled credentials and ties rotation to a documented consumer-dependency map; rotating a coupled credential without one breaks production (cf. [[what-are-non-human-identities|What Are Non-Human Identities? (Oasis Security)]] on operational rotation risk).
- **D9 L4** dependency-mapping applies acutely here: before rotating any coupled credential, the consumer graph must be known.

[[anti-patterns-and-failure-modes|RA and CMM Anti-Patterns and Failure Modes]] records the assessment failure this creates. An organization can hold a credential proxy at the workflow boundary and state a true D2 L4 claim while SAS tokens, storage access keys, and PATs in production route around that boundary, because the proxy intermediates access without decoupling the credential from the principal. A D2 L4 audit therefore has to name which credentials remain coupled and what the migration plan for each is, rather than scoring the presence of the proxy.

## Real-world consequence

[[what-are-non-human-identities|What Are Non-Human Identities? (Oasis Security)]] cites the **Microsoft AI Storage Breach** — a misconfigured SAS token exposed 38TB of internal data. SAS tokens are a canonical coupled-identity case: the token is the identity, its permissions are fixed at issuance, and rotation requires updating every consumer.

## Relations

- Concept anchor: [[non-human-identity|Non-Human Identity (NHI)]] (parent concept)
- Defensive control: [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] (works for decoupled, partial for coupled)
- Practice: [[nhi-governance-for-agents|NHI Governance for AI Agents]] (rotation cadence varies by coupling)
- Source: [[what-are-non-human-identities|What Are Non-Human Identities? (Oasis Security)]]
