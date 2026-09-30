---
type: maturity-model
title: "CMM D2: Identity and Authorization"
address: c-000137
created: 2026-05-25
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - identity
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agent-identity-architecture|Agent Identity Architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[identity-credential-coupling]]"
  - "[[agent-catalog]]"
  - "[[tenuo-warrant]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[endor-labs-ai-code-governance]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[crowdstrike-agentic-identity-provider]]"
  - "[[ping-enterprise-personal-agent-access]]"
  - "[[shadow-ai]]"
  - "[[claude-cowork]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[identity-credential-coupling]]"
  - "[[.raw/articles/microsoft-entra-agent-identities-overview-2026-09-18.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-what-is-2026-09-18.md]]"
  - "[[.raw/articles/microsoft-entra-agent-id-whats-new-2026-09-18.md]]"
verified: 2026-09-29
verified_against:
  - ".raw/articles/microsoft-entra-agent-id-what-is-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-id-whats-new-2026-09-18.md"
  - ".raw/articles/microsoft-entra-agent-identities-overview-2026-09-18.md"
verified_findings: 0
verified_note: "Checked current OWASP identity/least-privilege guidance and vendor documentation; other archived sources remain outside this targeted read."
---

# Agentic AI Security CMM — D2 Identity and Authorization

## Domain decision and boundary

D2 determines whether every agent and broker acting for it has an identifiable principal, bounded delegation, governed credentials, and a revocation path. It grades the deployed agent application or vendor platform named in the assessment scope. An agent is one deployed configuration of code, instructions, tools, and grants; replicas count as one agent. The non-human identities in scope include identities a broker or gateway uses for that agent. [[agentic-ai-security-cmm-2026|The CMM]] defines the five cumulative levels, and [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]] records evidence and applicability.

Identity establishes which principal requests an action. [[agentic-ai-security-cmm-d3-control-least-agency|D3]] grades the tool-call policy and its enforcement. [[agentic-ai-security-cmm-d5-egress-network|D5]] grades the route and inter-agent leg. [[agentic-ai-security-cmm-d7-observability|D7]] grades telemetry and behavioral detection. D2-TRACE requires a downstream record linking an action to agent and accountable human, whereas D7 grades the organization's correlated telemetry. A shared inventory can evidence D2 and [[agentic-ai-security-cmm-d1-governance|D1]] only where it contains both domains' required fields.

A vendor-held identity condition applies when an in-suite agent runs wholly in the vendor service and that service offers no customer-usable agent principal in general availability. The vendor's product-specific identity documentation must establish the condition. It makes only the four explicitly named identity-control criteria below not applicable. Inventory, owner, delegation, trace, and other controls remain assessable. If the vendor does offer a usable principal, missing adoption fails the criterion. An opaque supplier claim cannot turn an existing identity into a nonexistent control instance.

## Failure paths

A shared human token hides which agent acted. A copied delegation credential can cross to another agent or task. Stored credentials in the agent process expose long-lived authority to prompt injection or compromised tools. An orphaned agent can keep running after its owner leaves, and a coupled storage key can remain active after a nominal identity revocation. The [OWASP Agentic AI threat and mitigation guide](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/) identifies identity and privilege abuse as an agentic risk; the CMM's particular level placements are its own synthesis.

## L1–L5 progression

| Level | Observable identity outcome |
|---|---|
| L1 — Initial | Agents use local or shared identities with incomplete ownership and credential records. |
| L2 — Developing | Agents and non-human identities are inventoried; human-delegated access stays within the human's authority. |
| L3 — Defined | Issued identities are verified, owned, traceable to a human, and tied to deployment lifecycle and explicit delegation. |
| L4 — Managed | Per-agent authorization, task binding, credential isolation, rotation, chain links, and a tested revocation path constrain standing authority. |
| L5 — Optimizing | The registry, administration, audit, discovery, ownership transfer, and applicable stronger identity controls operate across the deployment. |

These levels are cumulative. The assessor records each criterion as met, not met, not applicable, or unanswerable with the handbook's evidence method. An unanswerable vendor-controlled instance cannot be credited as met. The table gives no universal target and no cross-domain numeric cap.

## Criterion catalogue

Each bold identifier below has one canonical definition. “Every” means every in-scope instance, and evidence must name the deployed version and scope. A production-capable agent with a production credential or production data is in scope even if labelled a pilot.

### L2 detail

- **D2-DELEGATE.** An agent acting for a human exercises no business authority beyond that human's own: the human could cause each read, write or external action through an approved human workflow, even if the agent's specialist or gateway holds a different technical credential. An agent acts for a human when that human's request starts the run, in a session or by explicit invocation. A component outside the model enforces this bound through the downstream authorization decision or an equivalent gateway check; delegation to another agent cannot enlarge it. A human's ability to ask an agent for an action does not itself establish the underlying business right. Not applicable to a run that an event or a schedule starts on the agent's own grant. *Evidence:* sampled human entitlements and approved action rights compared with the agent's effective reads and effects, including delegated calls, and a refused action beyond the human's authority.
- **D2-IDENTITY.** Each agent authenticates as a non-human identity of its own, and no person, other agent or other workload can use any credential the agent authenticates with. An organization-wide secret that other workflows can read fails the criterion, and so does a developer's own token. Not applicable under the vendor-held identity condition. *Evidence:* the identity provider's record of each agent identity and, for each credential it holds, the principals able to use it.
- **D2-INVENTORY.** An inventory records each agent in the deployment and every non-human identity it holds, and each identity's entry names the agent it belongs to. An inventory kept by hand meets the criterion, and so does a registry the pipeline maintains. *Evidence:* the inventory export, reconciled against the identity provider's list of the deployment's identities.

### L3 detail

- **D2-COUPLING.** The inventory classes every credential in the deployment as coupled or decoupled ([[identity-credential-coupling|identity-credential coupling]]). A coupled credential cannot be separated from the identity it grants, as with a storage access key or a personal access token. *Evidence:* the inventory's coupling field across the credentials the agents use and those a broker uses for them.
- **D2-DELEGATE-EXCHANGE.** Every delegation hop is a token-exchange flow: an agent acting for a human or another agent obtains a new token that names both parties, through [OAuth 2.0 Token Exchange](https://www.rfc-editor.org/rfc/rfc8693.html) or a platform on-behalf-of flow, and passes on no copied or shared credential. Acting for a human is itself a hop. A header or parameter carrying the human's name leaves the hop unmet. Not applicable where the deployment's agents act for no human and delegate to no agent. *Evidence:* the token-exchange configuration and exchanged tokens naming both parties.
- **D2-IDENTITY-VERIFY.** Each agent identity is issued by an identity provider, and each service the agent calls verifies it from an assertion the provider signs, such as a token or certificate. A locally asserted agent name or shared bearer secret fails. The inter-agent leg belongs to [[agentic-ai-security-cmm-d5-egress-network|D5]]. Not applicable under the vendor-held identity condition. *Evidence:* the identity provider's configuration for the agent and signed assertions accepted by sampled services.
- **D2-LIFECYCLE.** The deployment pipeline issues each agent identity when the agent is created, sets how its credential rotates, and revokes the identity at agent retirement. A human personnel change alone does not provide those triggers. Not applicable under the documented vendor-held identity condition. *Evidence:* pipeline definitions and the retirement record of an agent or a production-path test.
- **D2-OWNER.** Every agent and every non-human identity in the deployment names a human owner, and the organization's personnel record shows that owner as current. *Evidence:* the owner field for each agent and identity, checked against the personnel record.
- **D2-TRACE.** Every action an agent takes traces to the agent's identity and to the human accountable for it: the human the agent acted for, or its owner where the run acted on the agent's own grant. A component outside the model supplies both: the agent's identity from its authentication, and the human's from the session, delegated token, or owner record. For a coding agent, a sampled commit and tool call must retain both links after they reach the downstream system. *Evidence:* actions re-traced from downstream records to the agent and human, with the code or configuration that supplies the human's identity.

### L4 detail

- **D2-AUTHZ.** Per-agent authorization: the decision point that authorizes the agent's calls evaluates rules that name the calling agent's identity or a named group of agents, so each agent holds only the grants written for it. [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 grades whether that decision point mediates every call. *Evidence:* the policy export with the identity each rule names, and a decision-log sample recording the calling agent.
- **D2-COUPLING-MIGRATE.** An active migration plan off coupled credentials: the plan names each coupled credential the inventory records, the person accountable for migrating it and a target date, and no target date has passed without the credential migrated or the date reset by a recorded decision. Not applicable where the inventory records no coupled credential in the deployment. *Evidence:* the plan, set against the inventory's coupled entries and the decisions that reset a date.
- **D2-DELEGATE-LINK.** A credential one agent issues to another links to the delegation it descends from, so a credential minted for one step of a chain fails validation when presented at another. A first-hop credential, issued on a human's authority, has no delegation above it, so the criterion is not applicable where no agent delegates to another. *Evidence:* a second-hop credential showing the field that names its parent.
- **D2-DELEGATE-TOKEN.** Delegated-credential construction: each credential an agent receives to act for a human or another agent is signed and names its delegator, its delegatee, the scope it permits, the task it was issued for, and a bounded expiry. Not applicable where the deployment's agents act for no human and delegate to no agent. *Evidence:* a sample of delegated credentials showing each field.
- **D2-KILL.** An orphaned-agent kill switch: one operation revokes every credential, token and session one agent holds and stops the agent acting, while every other agent keeps running. The organization has executed it end to end since the revocation path last changed, against a running agent that uses the production identity provider, broker and revocation path. A tabletop exercise shows the procedure, and the criterion asks for the execution. *Evidence:* the execution record, naming what the operation revoked and stopped and how long it took.
- **D2-MUTUAL.** Each service call by the agent or its broker verifies the service endpoint and authenticates the caller cryptographically; a shared static key alone does not establish both ends. The inter-agent route is assessed in D5. *Evidence:* the deployed channel and endpoint-authentication configuration for sampled services.
- **D2-NOCRED.** Zero credentials in agent context: a broker or a credential-less identity model issues what the agent needs, and no stored credential sits anywhere the agent's process can read. That context covers the model context, the process memory, the environment, and every file or secret mounted into the process. A stored credential is a secret that stays valid until someone rotates it, such as a password, an API key, a client secret or a private key, and a short-lived token issued at run time falls outside the term. *Evidence:* the agent's deployment specification with its environment and mounted secrets, and the broker or vault log or the credential-less identity configuration.
- **D2-ROTATE.** Automated rotation per credential class: automation rotates every stored credential in the deployment on a schedule set for its class, and the rotation log shows each class rotating on schedule. *Evidence:* the rotation schedule per class and the rotation log.
- **D2-ROTATE-MAP.** A documented consumer-dependency map names, for each stored credential, every consumer that must take the new value when it rotates. *Evidence:* the map, and for one recent rotation, the consumers it named and the record that each took the new value.
- **D2-TASKBIND.** Each authenticated access an agent can use binds its identity to the current task, either in the issued session or token or at an independent mediation point that covers every use of a broader credential. A task is one unit of work the agent was started for, such as one user request or scheduled run. A broader credential that the agent can replay through a direct provider call or another reachable path fails. Test a call under the wrong task and after the task closes. *Evidence:* session or token scope, mediation policy and route coverage where used, and both refusal tests.

### L5 detail

- **D2-ADMIN.** Scoped administration: roles grant the administrative rights over agents and their identities, and each role reaches only the agents it names. *Evidence:* the role definitions and their assignments.
- **D2-AUDIT.** Audit-log integration: every creation, change and retirement of an agent, an identity, a credential or a grant writes a record to the organization's audit log. *Evidence:* an audit-log sample covering each event type.
- **D2-CONDITIONAL.** For each agent credential or token request, the issuing identity service or credential broker evaluates a current condition tied to that agent beyond account enablement, such as an agent-risk signal or mutable deployment attribute, and refuses issuance when the condition fails. A destination-only route rule or a check made only after issuance fails. Where a supplier controls issuance, scoped supplier evidence must show the decision and refusal; evidence the assessor cannot obtain is unanswerable, not not applicable. *Evidence:* the issuance rule and a credential or token-request record showing the condition, evaluation, and denial for a sampled agent.
- **D2-COUPLING-ZERO.** No coupled credential remains among the deployment's credentials, whether the agents use them directly or a broker uses them for the agents. *Evidence:* the inventory's coupling field and the coupled-credential migration report.
- **D2-DISCOVER.** Shadow-agent discovery: discovery runs on a schedule across the platforms the deployment's agents run on, reports every agent the registry does not hold, and ends each finding with the agent registered or removed. *Evidence:* the discovery schedule and scope, and recent findings with their outcomes.
- **D2-REGISTRY.** One inspectable registry records every in-scope agent, its identities and credentials, owner, and grants. The deployment pipeline writes creation, change, and retirement through a governed interface. For each agent, the registry returns which identity reaches which resource. Where agents actually use multiple identity platforms, that graph includes each platform's identities. A reconciliation report can corroborate completeness but is not a prerequisite, and no federation protocol is required. A registry that lists agents but cannot answer their effective identity reach fails. *Evidence:* registry export, pipeline write records, a sampled identity graph, and a platform-to-graph sample for each platform in use.
- **D2-IDENTITY-ATTEST.** Identity binding carries cryptographic attestation: the identity provider issues each agent's credential only after verifying evidence that binds the running workload to the claimed identity and approved deployment. Possession of a stored secret or an identity token without workload attestation fails. Not applicable under the vendor-held identity condition. *Evidence:* the issuer's verification rule and attestation chain behind a sampled credential.
- **D2-OWNER-TRANSFER.** Ownership transfer: when an owner leaves or changes role, each agent and non-human identity they own passes to a named successor, and the registry records the transfer. *Evidence:* the personnel trigger and transfer records for sampled owners who left or changed roles.

## Prerequisites and blockers

D2-INVENTORY establishes the agent and identity population for owner, rotation, registry, and discovery checks. The task binding evidenced for D2-TASKBIND, whether in a session, token, or independent mediator, must use the same task boundary D3-TASKSCOPE enforces; otherwise a credential may remain valid while its action is out of scope. D2-DELEGATE-TOKEN and D2-DELEGATE-LINK supply chain data for D3-CHAIN to validate. D2-TRACE supplies identity context to D7 attribution, while D7 owns the identity-activity baseline. A missing prerequisite appears as the failed criterion and a remedy in the handbook, without a cross-domain arithmetic adjustment.

## Deployment-shape differences

An internal retrieval assistant with no external action can have few credential classes, but still needs owner and delegated-access evidence. An in-suite assistant may meet the documented vendor-held condition for identity issuance. Its human-access bound and action trace remain relevant. A local coding agent needs separate agent identity and a way to link a commit or tool execution to its accountable human. A gateway or multi-agent system adds broker identities and delegation hops to the sample frame. Where the deployment spans identity platforms, D2-REGISTRY needs one inspectable graph of the agents actually used. No federation protocol is required.

A useful sample starts at an action in the downstream service. The assessor follows its agent principal to the identity provider, the delegating human or owner, and the credential or broker that made the call. A later sample tests the same link after task closure and another after an owner transfer. The records can reside in different systems, but their identifiers and timestamps must join without relying on a label the model supplied. A platform screenshot showing an agent name without the downstream authorization record does not establish the complete trace.

For a multi-platform deployment, the registry's identity graph must account for the principals the agents actually use on each platform. A sampled platform export can expose an identity missing from the graph. A scheduled reconciliation job is one way to find it, while an operator-controlled comparison with recorded disposition can serve the same assessment purpose. The criterion grades completeness and maintenance of the graph, not a particular exchange protocol. Supplier-held components still need a named responsible operator and product-specific evidence for the portions the customer cannot inspect directly.

## Implementation and effort drivers

Identity-provider integration and owner reconciliation begin the work. Coupled-credential migration and consumer mapping are often the largest L4 projects because rotating a shared key can break every dependent service. A credential broker, workload identity, or equivalent design can keep stored credentials out of agent context. L5 adds registry integrations, transfer triggers, discovery triage, audit volume, and attestation maintenance where applicable. The chosen target depends on privilege, external reach, and delegation complexity; the model does not set a default product or license budget.

Revocation tests have operational cost because they interrupt a real production-path agent. A safe test uses an agent and task selected for the exercise, records the elapsed time to stop activity, and proves that unrelated agents continue. Rotations require a dependency map before automation is reliable. Audit storage grows with identity and grant churn, so evidence retention and query access belong in the operating estimate. These costs are material for a high-autonomy deployment with many short-lived agents even if its identity provider has no new license charge.

## Sources and limits

The [OWASP Agentic AI threat and mitigation guide](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/) and [OWASP model access control](https://owaspai.org/go/modelaccesscontrol/) anchor identity, mutual authentication, and delegated access. [Microsoft Entra Agent ID identity guidance](https://learn.microsoft.com/en-us/entra/agent-id/agent-identities), [AWS AgentCore workload identities](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/understanding-agent-identities.html), and [Google Agent Identity](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/agent-identity-overview) document identity paths. Their coverage and license terms must be checked for the assessed deployment. [Microsoft's agent registry guidance](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/agent-registry) illustrates a deployable registry. Cross-platform reconciliation remains the organization's integration responsibility where the deployment spans identity platforms. Authentication establishes the actor. The [OWASP AI Exchange least-privilege control](https://owaspai.org/go/leastmodelprivilege/) also calls for task- and user-bound authorization at the action boundary. D3, D4, and D5 assess other parts of that path.
