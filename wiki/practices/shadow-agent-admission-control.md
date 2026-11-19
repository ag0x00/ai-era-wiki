---
type: practice
title: "Shadow Agent Admission Control"
created: 2026-10-06
updated: 2026-10-06
tags:
  - practices
  - identity
  - agentic-ai
  - governance
  - a2a
  - spiffe
status: developing
scope_axis:
  - sec-of-ai
origin: produced
maturity: emerging
addresses_threat: "Unregistered AI agents that authenticate and act outside the agent inventory; registry entries whose traffic bypasses the enforcement point"
related:
  - "[[shadow-automation]]"
  - "[[spiffe]]"
  - "[[a2a-protocol]]"
  - "[[agent-catalog]]"
  - "[[nhi-governance-for-agents]]"
  - "[[agent-identity-architecture]]"
  - "[[non-human-identity]]"
  - "[[microsoft-agent-365]]"
  - "[[okta-for-ai-agents]]"
  - "[[falcon-guardian]]"
  - "[[agentdesktop]]"
  - "[[ai-spm]]"
  - "[[credential-proxy-pattern]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-ra-gaps]]"
sources:
  - "[[.raw/articles/a2a-protocol-specification-v1-0-0-2026-10-06.md]]"
  - "[[.raw/articles/a2a-protocol-agent-discovery-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-agent-registry-register-agents-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-agent-registry-automatic-registration-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-agent-registry-manual-registration-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-agent-registry-release-notes-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-introducing-gemini-enterprise-agent-platform-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-gemini-enterprise-register-a2a-agent-2026-10-06.md]]"
  - "[[.raw/articles/google-cloud-iam-agent-identity-overview-2026-10-06.md]]"
  - "[[.raw/articles/aws-agent-registry-generally-available-2026-10-06.md]]"
  - "[[.raw/articles/aws-agent-registry-sync-records-2026-10-06.md]]"
  - "[[.raw/articles/microsoft-agent-registry-admin-center-2026-09-18.md]]"
  - "[[.raw/articles/spiffe-spire-concepts-2026-10-06.md]]"
  - "[[.raw/articles/spiffe-spire-adopters-2026-10-06.md]]"
  - "[[.raw/articles/microsoft-entra-workload-identity-federation-spiffe-spire-2026-10-06.md]]"
  - "[[.raw/articles/anthropic-claude-api-wif-spiffe-2026-10-06.md]]"
  - "[[.raw/articles/csa-survey-unknown-ai-agents-2026-10-06.md]]"
  - "[[.raw/articles/ietf-draft-ietf-wimse-aims-00-2026-10-06.md]]"
  - "[[.raw/articles/ietf-wimse-wg-documents-2026-10-06.md]]"
  - "[[.raw/articles/nist-nccoe-software-ai-agent-identity-authorization-concept-paper-2026-10-06.md]]"
  - "https://www.hashicorp.com/en/blog/vault-enterprise-1-21-spiffe-auth-fips-140-3-level-1-compliance-granular-secret-recovery"
  - "https://www.hashicorp.com/en/blog/hashicorp-vault-agentic-iam-is-now-generally-available"
verified: 2026-10-06
verified_against:
  - ".raw/articles/a2a-protocol-agent-discovery-2026-10-06.md"
  - ".raw/articles/a2a-protocol-specification-v1-0-0-2026-10-06.md"
  - ".raw/articles/anthropic-claude-api-wif-spiffe-2026-10-06.md"
  - ".raw/articles/aws-agent-registry-generally-available-2026-10-06.md"
  - ".raw/articles/aws-agent-registry-sync-records-2026-10-06.md"
  - ".raw/articles/csa-survey-unknown-ai-agents-2026-10-06.md"
  - ".raw/articles/google-cloud-agent-registry-automatic-registration-2026-10-06.md"
  - ".raw/articles/google-cloud-agent-registry-manual-registration-2026-10-06.md"
  - ".raw/articles/google-cloud-agent-registry-register-agents-2026-10-06.md"
  - ".raw/articles/google-cloud-agent-registry-release-notes-2026-10-06.md"
  - ".raw/articles/google-cloud-gemini-enterprise-register-a2a-agent-2026-10-06.md"
  - ".raw/articles/google-cloud-iam-agent-identity-overview-2026-10-06.md"
  - ".raw/articles/google-cloud-introducing-gemini-enterprise-agent-platform-2026-10-06.md"
  - ".raw/articles/ietf-draft-ietf-wimse-aims-00-2026-10-06.md"
  - ".raw/articles/ietf-wimse-wg-documents-2026-10-06.md"
  - ".raw/articles/microsoft-agent-registry-admin-center-2026-09-18.md"
  - ".raw/articles/microsoft-entra-workload-identity-federation-spiffe-spire-2026-10-06.md"
  - ".raw/articles/nist-nccoe-software-ai-agent-identity-authorization-concept-paper-2026-10-06.md"
  - ".raw/articles/spiffe-spire-adopters-2026-10-06.md"
  - ".raw/articles/spiffe-spire-concepts-2026-10-06.md"
verified_findings: 0
verified_note: "2 medium, 9 low and 4 wiki-fit findings fixed; Vault 1.21 and 2.1 claims and SPIRE v1.15.3 checked on the live web, not archived"
---

# Shadow Agent Admission Control

Shadow agent admission control makes registration the only route by which an AI agent obtains a working identity and a callable endpoint, then reconciles the registry against telemetry that observes agents without their cooperation. It combines three mechanisms an enterprise may already run: SPIFFE identities issued by SPIRE or a cloud platform ([[spiffe|SPIFFE / SPIRE]]), A2A Agent Cards that describe each agent ([[a2a-protocol|A2A Protocol (Agent-to-Agent)]]), and the agent registries that Google Cloud, AWS and Microsoft operate. Each of these mechanisms records or authenticates an agent only after its owner enrols it or deploys it on the vendor's own runtime. The practice therefore uses them to control admission and assigns discovery to independent telemetry.

**A2A and SPIFFE turn an unregistered agent from a call that quietly succeeds into a call that is refused and logged; finding the agent remains the work of telemetry.**

## Applicability

- The estate runs, or plans, A2A between agents, SPIFFE identities for workloads, or a cloud agent registry.
- Agents are created and retired by code deployment, so a request-and-approve process misses them; [[nhi-governance-for-agents|NHI Governance for AI Agents]] binds the identity lifecycle to deployment for this case.
- A registry is meant to serve as the agent inventory of record.

The population this practice addresses is [[shadow-automation|Shadow Automation]], whose survey evidence finds unknown agents emerging most often in internal scripting environments, LLM platforms and SaaS products with built-in automation.[^csa] Agents built there are created outside the deployment pipeline that would issue them an SVID or publish their Card, which is why the method separates admission from discovery.

## Method

### Identity issued only at registration

SPIRE issues an SVID to a workload only when the workload's attested properties match a registration entry. The SPIRE server "stores registration entries (which specify the selectors that determine the conditions under which a particular SPIFFE ID should be issued)", and the SPIRE agent returns an SVID after it compares the selectors discovered for a workload with those entries.[^spire] A workload without a matching entry receives no SVID, so each entry is an inventory record. An entry names one agent only when its selectors are specific to that agent's workload. Selectors can describe a process by its Kubernetes namespace or service account,[^spire] and an entry keyed on a namespace alone issues its SPIFFE ID to every workload deployed there, an unregistered agent included. The control writes registration entries from the deployment pipeline, with selectors that identify one agent's workload and the owner field the pipeline already requires. Only the pipeline's identity holds the right to create entries on the SPIRE server, and each scheduled reconciliation compares an export of the server's entries with the pipeline's records.

Google's Agent Identity applies the same rule on Google's own runtimes. When an agent is deployed, Google Cloud "assigns it a unique SPIFFE identity and an X.509 certificate", valid for 24 hours and renewed automatically.[^g-aid] HashiCorp Vault Enterprise 1.21 authenticates AI agents and other non-human workloads with SPIFFE and issues X.509-SVIDs to workloads that Vault has authenticated, which brings clients outside a SPIRE deployment into a SPIFFE trust domain.[^vault121] Vault Enterprise 2.1 made its agentic identity features generally available on 2026-09-01, including a registry view of agent identities, authentication activity and policies.[^vault21] Each issuer is an inventory source: its entries or identities are exported and compared with the agent registry at reconciliation.

### Agent Card as the registry record

An A2A Agent Card describes an agent's interface, skills and authentication requirements. The specification names three ways a client finds a Card: a well-known URI, a curated registry, or direct configuration.[^a2a-discovery-spec] The A2A discovery guide states that servers publish their Cards to a registry, and that the specification "does not prescribe a standard API for curated registries".[^a2a-disc] Each registry therefore defines its own intake:

- **Google Cloud Agent Registry**, announced at Google Cloud Next on 2026-04-22[^g-next] and generally available since 2026-06-18 with A2A 1.0 support.[^g-rn] It registers agents automatically on Agent Runtime, for built-in Google agents, on GKE when a deployment carries the agent functional-type label, and on Cloud Run when a service is deployed with the agent flags. On GKE an Agent Card annotation adds the agent's A2A skills to the entry. Automatic registration "operates at the project level and doesn't automatically discover agents deployed in other Google Cloud projects".[^g-reg] Agents hosted elsewhere or in another project enter by Card URL or pasted Card JSON, and an agent without a Card enters by its endpoint URL.[^g-man]
- **AWS Agent Registry**, generally available since 2026-08-31. It detects agents on AgentCore runtime and AgentCore gateways across an AWS organization, and other records are created by hand or synchronized from a URL.[^aws-ga] An A2A Card URL is one such source.[^aws-sync]
- **Microsoft's Agent Registry** in the Microsoft 365 admin center. It lists the agents available in a tenant in four types, accepts a custom agent as an uploaded ZIP package, and counts "Unmanaged agents", which it defines as agents "created or managed outside of Agent 365".[^ms-reg] The registry page describes no import of A2A Cards. [[microsoft-agent-365|Microsoft Agent 365]] covers its licensing and the preview registry sync with other clouds.

The control requires a registry entry, written by the pipeline from the agent's Card, before any consumer is configured to call the agent. Card signatures are optional in A2A, and "Clients SHOULD verify at least one signature before trusting an Agent Card".[^a2a-sign] Where publishers sign, consumers verify, and the stricter profile in which an unsigned Card is refused is graded by D5-A2A-SIGN.

### Refusal of unattested callers

A2A delegates identity to the transport and to the security scheme an agent declares. The specification states that "Identity information is handled at the protocol layer, not within A2A semantics", and that a server "MUST authenticate every incoming request based on the provided credentials and its declared authentication requirements".[^a2a-auth] A receiving agent that declares mutual TLS, OAuth 2.0 or OpenID Connect, and that trusts only certificates or tokens chained to the enterprise's SPIFFE trust domains, refuses a caller that was never registered, because that caller holds no SVID. The refusal is a detection event: the receiver logs the caller's address and presented credential, and the log reaches the receiving agent's owner. The rule also applies in the other direction. A2A clients "SHOULD verify the A2A Server's identity by validating its TLS certificate",[^a2a-auth] and a registered client that accepts only server certificates chained to the same trust domains refuses to call an unregistered agent and logs the endpoint it tried.

Relying parties outside the enterprise trust domain accept SVIDs on narrower terms, and the registry entry records which format each one takes:

- Microsoft Entra Workload ID exchanges a SPIRE JWT-SVID for an Entra access token. Microsoft supports external issuers whose tokens are signed with RS256, while SPIRE signs JWT-SVIDs with ES256 by default, so the SPIRE server is reconfigured to issue RS256-signed JWT-SVIDs.[^entra-spiffe]
- The Claude API's workload identity federation "accepts JWT-SVIDs only, not X.509-SVIDs".[^claude-wif]

Every route to a receiver is tested, because a registry entry does not establish which enforcement point sees the traffic. Gemini Enterprise lets an administrator register an A2A agent from its Card JSON, a Preview feature, and states that "Traffic to A2A Agents added through this method do not go through Agent Gateway and Agent Gateway policies do not apply".[^ge-a2a] An agent registered that way appears in the catalog while its calls bypass the gateway's policy. The route test calls the agent through each registered path and records which enforcement point observed the call.

### Reconciliation against independent telemetry

Registries and identity issuers hold enrolled agents. Sources that observe an agent whether or not its owner enrolled it find the rest; [[shadow-automation|Shadow Automation]] describes the vantage points and [[agent-catalog|AI Agent Catalog]] the products:

- Identity-provider telemetry, such as the Microsoft registry's unmanaged-agent count and [[okta-for-ai-agents|Okta for AI Agents]] discovery.
- Endpoint process telemetry, such as [[falcon-guardian|Falcon Guardian]].
- Harness configuration on developer machines, such as [[agentdesktop|agentdesktop]].
- Cloud inventory of AI services, through [[ai-spm|AI Security Posture Management (AI-SPM)]].
- Egress records of calls to model providers and agent endpoints, from the outbound proxy graded in D5.

Each source yields candidates. Reconciliation runs on a schedule and matches each candidate to a registry entry and an issued identity, using the key the candidate carries:

- A process on a host matches the workload whose registration-entry selectors it satisfies.
- An OAuth client or service principal matches the identity the registry entry lists for the agent.
- A cloud resource matches the registry entry that names its resource ID.
- An egress record matches the registered workload that made the call, and an agent endpoint it reaches matches the URL in a registered Card or registry record.

A candidate endpoint can be queried at the well-known Card path to read what the agent declares; the Card describes the agent, and its operator is established from the telemetry and ownership records. A candidate without a match becomes a finding.

### Closure of each finding

Each finding ends in one of two states. The agent is registered, with a Card, an identity entry and a named owner, or it is removed, with its credentials revoked and its endpoint withdrawn. The finding record keeps the source that found the agent and the decision taken; [[agent-catalog|AI Agent Catalog]] defines the discovery-state labels. D2-DISCOVER grades this scheduled loop from discovery to closure.

## Mechanism

An unregistered agent persists as long as it works. Admission control changes the result of its calls to protected receivers: an agent with no registry entry holds no SVID and no Card a consumer trusts, so its calls fail and each failure is logged against the receiving agent's owner. The cost moves from the security team, which would otherwise search for the agent, to the agent's builder, who must register it to make it work. Reconciliation then covers the agents that call no protected receiver, which admission never sees.

## Coverage by inventory source

Discovery comes from the rows that need no action by the agent's owner. The registry and identity rows of that kind cover only the vendor's own runtimes or surfaces, so an agent hosted elsewhere reaches a registry only when someone enters it, and telemetry carries discovery for the rest of the estate.

| Mechanism | Agents it records | Owner action needed | Status |
|---|---|---|---|
| A2A well-known Card | Agents whose owner publishes a Card | Yes | A2A 1.0.0 |
| Google Agent Registry, automatic | Agent Runtime, built-in, labelled GKE and flagged Cloud Run agents, per project | Label or flag on GKE and Cloud Run | GA 2026-06-18 |
| Google Agent Registry, manual | Agents entered by Card URL, Card JSON or endpoint URL | Yes | GA 2026-06-18 |
| AWS Agent Registry, automatic | Agents on AgentCore runtime and gateways | No | GA 2026-08-31 |
| AWS Agent Registry, URL sync | Agents whose Card or MCP URL is entered | Yes | GA 2026-08-31 |
| Microsoft Agent Registry | Microsoft-surface agents and uploaded packages; counts unmanaged agents | No, on Microsoft surfaces | Graph API in preview |
| SPIRE registration entry | Workloads that match an operator-written entry | Yes, by the operator | SPIRE v1.15.3[^spire-rel] |
| Google Agent Identity | Agents deployed on Agent Runtime, Gemini Enterprise and Cloud Run | Identity-type flag on Cloud Run | Launch stage not stated |
| Identity, endpoint, desktop and egress telemetry | Agents that authenticate, run, are configured or call out | No | Varies by product |

## Limits

- **Discovery in A2A depends on publication.** An agent that publishes no Card, or publishes it on an unmonitored domain, is absent from the Card-based intakes above: well-known URI discovery, Google manual import and AWS URL sync. The automatic intakes record agents by the runtime they are deployed on, and Google's registers an agent that has no Card, without its skills.[^g-reg] Registry APIs differ by vendor because A2A defines none.[^a2a-disc]
- **An SVID names a workload.** Several agents that run in one workload share its identity, and the registry entry must record that grouping; [[non-human-identity|Non-Human Identity (NHI)]] sets out the assessment.
- **Authentication is separate from authorization.** An SVID establishes which workload calls, and the receiver still decides what that workload may do. The NIST NCCoE draft concept paper lists the standards its project "is currently considering": SPIFFE and SPIRE as "one way in which agent workloads could be identified and authenticated", and OAuth 2.0 and 2.1 for authorization.[^nccoe] [[agent-identity-architecture|AI Agent Identity Architecture]] places the authorization layer.
- **Some agents call no protected receiver.** A desktop agent that uses a person's delegated token against a SaaS API, or automation built into a SaaS product, reaches its targets without crossing an enterprise receiver, so admission control does not apply to it and only telemetry finds it.
- **Cross-vendor agent identity rests on drafts.** The IETF WIMSE working group's draft-ietf-wimse-aims-00 (2026-09-15) requires that an agent "MUST be assigned exactly one WIMSE identifier, which MAY be a SPIFFE ID".[^aims] No WIMSE document had reached RFC status when the working group's document list was read on 2026-10-06.[^wimse-docs]
- **Production evidence is vendor-side.** The reviewed sources document SPIFFE identities for agents in vendor products such as Google Agent Identity and HashiCorp Vault. The SPIRE adopters file names end users such as Bloomberg, GitHub and Uber without describing agent workloads,[^adopters] and no source reviewed names an end-user organization that runs SPIFFE identities for its own agents in production.

## Mapping to the CMM

The practice supplies evidence for three domains of the Agentic AI Security CMM without moving any level:

- [[agentic-ai-security-cmm-d2-identity|D2: Identity and Authorization]]. Registry entries and issuer exports evidence D2-INVENTORY at L2. Pipeline-written registration entries evidence D2-LIFECYCLE at L3. SPIRE workload attestation evidences D2-IDENTITY-ATTEST at L5. A pipeline-written registry evidences D2-REGISTRY at L5 only where it also returns which identity reaches which resource; the reconciliation report corroborates its completeness. The reconciliation loop, run to closure, evidences D2-DISCOVER at L5.
- [[agentic-ai-security-cmm-d5-egress-network|D5: Egress and Network]]. Refusal of an unattested peer evidences D5-A2A-AUTH at L3, and refusal of an unsigned Card evidences D5-A2A-SIGN at L5.
- [[agentic-ai-security-cmm-d7-observability|D7: Observability and Detection]]. Refusal logs and reconciliation findings feed detection, including the peer baseline D7-A2A-BASELINE grades.

## Notes

[^spire]: SPIFFE project, [*SPIRE Concepts*](https://spiffe.io/docs/latest/spire-about/spire-concepts/), retrieved 2026-10-06. Server registration entries, selectors and workload attestation.
[^spire-rel]: SPIFFE project, [*SPIRE releases*](https://github.com/spiffe/spire/releases), read 2026-10-06; v1.15.3, dated 2026-08-21, is the newest tag listed.
[^adopters]: SPIFFE project, [*SPIRE ADOPTERS.md*](https://github.com/spiffe/spire/blob/main/ADOPTERS.md), retrieved 2026-10-06, End users section.
[^g-aid]: Google Cloud, [*Agent Identity overview*](https://docs.cloud.google.com/iam/docs/agent-identity-overview), retrieved 2026-10-06. SPIFFE-based identity and 24-hour X.509 certificate per deployed agent, on Agent Runtime, Gemini Enterprise and Cloud Run; the page states no launch stage.
[^vault121]: HashiCorp, [*Vault Enterprise 1.21: SPIFFE auth, FIPS 140-3 level 1 compliance, granular secret recovery*](https://www.hashicorp.com/en/blog/vault-enterprise-1-21-spiffe-auth-fips-140-3-level-1-compliance-granular-secret-recovery), 2025-10-23.
[^vault21]: HashiCorp, [*HashiCorp Vault agentic IAM is now generally available*](https://www.hashicorp.com/en/blog/hashicorp-vault-agentic-iam-is-now-generally-available), 2026-09-01.
[^a2a-discovery-spec]: A2A Project, [*A2A Protocol Specification*, §8.2 Discovery Mechanisms](https://a2a-protocol.org/latest/specification/#82-discovery-mechanisms), latest released version 1.0.0, retrieved 2026-10-06.
[^a2a-disc]: A2A Project, [*Agent Discovery in A2A*, Curated Registries](https://a2a-protocol.org/latest/topics/agent-discovery/), retrieved 2026-10-06.
[^g-next]: Google Cloud, [*Introducing Gemini Enterprise Agent Platform*](https://cloud.google.com/blog/products/ai-machine-learning/introducing-gemini-enterprise-agent-platform), 2026-04-22. Announces Agent Identity, Agent Registry and Agent Gateway.
[^g-rn]: Google Cloud, [*Agent Registry release notes*](https://docs.cloud.google.com/agent-registry/release-notes), entries of 2026-04-22 (Preview) and 2026-06-18 (general availability, A2A 1.0 support).
[^g-reg]: Google Cloud, [*Register agents*](https://docs.cloud.google.com/agent-registry/register-agents), retrieved 2026-10-06; the GKE label, the annotation's role in skill discovery and the Cloud Run flags are on [*Use automatic registration*](https://docs.cloud.google.com/agent-registry/automatic-registration).
[^g-man]: Google Cloud, [*Use manual registration*](https://docs.cloud.google.com/agent-registry/manual-registration), retrieved 2026-10-06.
[^aws-ga]: AWS, [*AWS Agent Registry for centralized agent discovery and governance is now generally available*](https://aws.amazon.com/about-aws/whats-new/2026/08/aws-agent-registry-generally-available/), 2026-08-31.
[^aws-sync]: AWS, [*Synchronize records from external sources*, Synchronize from an A2A agent card](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/registry-sync-records.html), retrieved 2026-10-06.
[^ms-reg]: Microsoft, [*Manage agent registry in Microsoft 365 admin center*](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/agent-registry), ms.date 2026-09-21, retrieved 2026-10-06.
[^a2a-sign]: A2A Project, [*A2A Protocol Specification*, §8.4 Agent Card Signing](https://a2a-protocol.org/latest/specification/#84-agent-card-signing), version 1.0.0, retrieved 2026-10-06; the optional signature ("MAY") is in §8.4 and the quoted verification rule in §8.4.3.
[^a2a-auth]: A2A Project, [*A2A Protocol Specification*, §7 Authentication and Authorization](https://a2a-protocol.org/latest/specification/#7-authentication-and-authorization), version 1.0.0, retrieved 2026-10-06; the server-authentication rule is in §7.4 and the client's verification of the server in §7.2.
[^csa]: Cloud Security Alliance, [*New Cloud Security Alliance Survey Reveals 82% of Enterprises Have Unknown AI Agents in Their Environments*](https://cloudsecurityalliance.org/press-releases/2026/04/21/new-cloud-security-alliance-survey-reveals-82-of-enterprises-have-unknown-ai-agents-in-their-environments), 2026-04-21; survey commissioned by Token Security.
[^entra-spiffe]: Microsoft, [*Federate SPIFFE/SPIRE workloads with Microsoft Entra Workload ID*](https://learn.microsoft.com/en-us/entra/workload-id/workload-identity-federation-spiffe-spire), ms.date 2026-08-12.
[^claude-wif]: Anthropic, [*SPIFFE* workload identity federation provider](https://platform.claude.com/docs/en/manage-claude/wif-providers/spiffe), retrieved 2026-10-06.
[^ge-a2a]: Google Cloud, [*Register and manage A2A agents*](https://docs.cloud.google.com/gemini/enterprise/docs/register-and-manage-an-a2a-agent), Gemini Enterprise, Preview, retrieved 2026-10-06.
[^nccoe]: NIST NCCoE, [*Accelerating the Adoption of Software and AI Agent Identity and Authorization*, concept paper, draft](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf), February 2026, §2 Relevant Standards and Guidelines.
[^aims]: IETF WIMSE WG, [*draft-ietf-wimse-aims-00: AI Identity Management System*](https://datatracker.ietf.org/doc/draft-ietf-wimse-aims/), 2026-09-15, WG Document; replaces draft-klrc-aiagent-auth.
[^wimse-docs]: IETF Datatracker, [*WIMSE working group documents*](https://datatracker.ietf.org/wg/wimse/documents/), read 2026-10-06; draft-ietf-wimse-workload-identity-practices-07 had been submitted to the IESG for publication and was in state "AD Evaluation::AD Followup", and the other WG documents were in state "I-D Exists".
