---
type: maturity-model
title: "Agentic AI Security Capability Maturity Model"
address: c-000156
created: 2026-04-30
updated: 2026-09-29
tags:
  - maturity-models
  - agentic-ai
  - cmm
  - 2026-proposal
status: developing
origin: produced
scope_axis:
  - sec-of-ai
adoption_signal: proposed
last_substantive_update: 2026-09-29
tier_count: 5
audience: "Enterprise security architects, AI platform owners, internal assessors, investment decision makers"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Targeted cross-instrument and live NIST, DOE, OWASP source check; no archived source opened. Archived source coverage remains incomplete."
---
# Agentic AI Security Capability Maturity Model

## Purpose and assessment unit

This model helps a security architect determine the present capability of one agentic AI deployment and justify a risk-selected improvement target. It describes security and governance outcomes across nine domains. Each result belongs to a defined **deployment shape**: the agent configuration, workflow, authority, hosting environment, data, tools, people, and suppliers that together produce an action or answer. A product name alone does not define the unit. A local coding agent that can write to a repository and a hosted assistant that only drafts answers need separate assessments even if both call the same model. The same service deployed under different grants or oversight can have different profiles. The [[claude-code-control-sheet|Claude Code Control Sheet]] illustrates how several coding-agent configurations produce different evidence and target decisions.

The assessment record names every in-scope agent configuration, entry channel, reachable tool and data source, approval route, external service, and material environment. Replicas under the same configuration may share one assessment. Materially different authority or topology calls for another. State excluded agents and interfaces with a reason. The boundary follows the complete path from input to effect, including supplier-operated steps. A shared organization control may be reused across deployments when its current evidence and scope cover each one. A supplier-held control requires relevant supplier evidence or a customer-visible test. Supplier ownership does not erase an applicable condition. The [DOE C2M2 scope discussion](https://www.energy.gov/sites/default/files/2022-06/C2M2%20Version%202.1%20June%202022.pdf#page=17) likewise treats externally managed assets and the selected operating environment as part of the evaluated function.

Governance authority and technical enforcement are assessed separately. A well-documented approval cannot establish that a tool call is mediated; a strong gateway cannot establish who accepted a residual risk. The report keeps nine domain results and the failed criteria visible so that one strong area cannot hide another's exposure. This is an internal capability assessment, not a certificate or a substitute for deployment risk analysis.

## Using the model

1. Define the deployment, decision owner, and risk question above. Draw its boundaries and flows with the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]].
2. Select the applicable domain criteria below. The linked domain deep dives give each criterion's full pass condition, negative test, minimum evidence, and applicability rule. Their identifiers are stable within this version.
3. Use the [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]] to plan document review, interviews, and tests, record each verdict, and review evidence confidence. Select applicable external standards and regulatory sources directly for the deployment; check each cited clause against its current edition and scope. The [[agentic-ai-security-cmm-crosswalk|archived standards matrix]] preserves an earlier mapping but does not govern this version.
4. Report the current and target domain profile, blockers, work packages, residual risk, and decision. Reassess when the deployment or evidence materially changes.

The core gives the progression and concise criterion restatements. The deep dives own the definitions. The architecture locates trust boundaries and enforcement points. The handbook owns the evidence and reporting procedure. NIST SP 800-53A describes [assessment objectives, methods, objects, depth, and coverage](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf#page=35); those ideas inform the method here, while the agent-specific criteria are this model's synthesis.

## Levels and determinations

| Level | General observable state |
|---|---|
| L1 — Initial | Relevant activity exists, but control and records remain local or incomplete. |
| L2 — Developing | Basic ownership, inventory, and first boundaries are documented and operate. |
| L3 — Defined | The deployment follows repeatable, enforced rules across its material paths. |
| L4 — Managed | Joined evidence, scoped testing, and measured controls support reliable operation. |
| L5 — Optimizing | Deployed controls close findings and adapt on measured, sustained evidence. |

Levels are **cumulative within each domain**. L1 describes an observed state and defines no named criterion. For L2 through L5, the observed level is the highest for which every applicable criterion at that level and below is met. A failed or unanswerable criterion blocks that level and higher levels in its domain. The report names the blocking criterion; it does not convert a lack of evidence into a failed control. If evidence cannot establish even the L1 state or the deployment boundary, report the domain as unanswerable rather than inventing a level. A domain whose governed objects are genuinely absent may be marked not applicable in full, with the absence shown.

| Criterion determination | Assessment rule |
|---|---|
| Met | The full pass condition operates for the applicable population and period, supported by inspectable evidence. |
| Not met | An applicable facet fails, including an uncovered path or a negative test that succeeds. |
| Not applicable | The governed object or activity is genuinely absent under the deep dive's rule, with evidence of absence. |
| Unanswerable | The object exists, but available evidence cannot decide the pass condition, including a material supplier-held step. |

An absent control on an existing path is **not met**, not inapplicable. A missing record may make a condition unanswerable when the activity could have occurred. The assessor states what was inspected and what remains unknown. Evidence confidence is a separate finding about coverage, freshness, independence, and test depth. A level with limited confidence must say why. Confidence does not silently change the level. Applicable L5 outcomes are intended to be implementable in a present deployment. A criterion requiring months of operating history cannot be claimed before that history exists, even if its control was released recently.

## Domain map

| Domain deep dive | Assessment decision | Lead evidence owner | Applicability boundary |
|---|---|---|---|
| [[agentic-ai-security-cmm-d1-governance\|CMM D1: Governance and Accountability]] | Approval and residual-risk ownership | AI risk authority | Organization decisions and each deployment approval |
| [[agentic-ai-security-cmm-d2-identity\|CMM D2: Identity and Authorization]] | Identity, grant, and delegation authority | Identity and platform owner | Every agent identity, grant, and delegation |
| [[agentic-ai-security-cmm-d3-control-least-agency\|CMM D3: Control and Least-Agency]] | Permitted action at call time | Authorization policy owner | Callable actions; delegation rules where agents delegate |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|CMM D4: Runtime and Guardrails]] | Prompt, response, and execution protection | Agent platform owner | Model paths; execution confinement where code runs |
| [[agentic-ai-security-cmm-d5-egress-network\|CMM D5: Egress and Network]] | Permitted network and peer reach | Network and gateway owner | Agent connections; MCP and peer paths where present |
| [[agentic-ai-security-cmm-d6-data-rag\|CMM D6: Data, Memory and RAG]] | Data entitlement and memory integrity | Data and retrieval owner | Corpora, tool-read data, fine-tuning data, and agent memory as used |
| [[agentic-ai-security-cmm-d7-observability\|CMM D7: Observability and Detection]] | Action reconstruction and detection | Security monitoring owner | Production actions and applicable workflow paths |
| [[agentic-ai-security-cmm-d8-supply-chain\|CMM D8: Engineering and Supply Assurance]] | Release assurance and component integrity | Engineering and supply owner | Release, components, suppliers, and producer duties where present |
| [[agentic-ai-security-cmm-d9-operations\|CMM D9: Operations and Human Factors]] | Human review and operating response | Service and incident owner | Human review, incidents, leavers, and retirement |

The lead owner supplies evidence, not exclusive responsibility. D1 organization records can be inherited by each covered deployment; D8 supplier records can support a customer release. The assessor still checks the deployment-specific effect. Conditional paths such as inter-agent exchange, model-file loading, fine-tuning, and externally published components are graded only where they exist under the owning criterion's applicability rule.

## Domain progression

Each level sentence states the observable outcome. The nested identifiers are compact restatements for navigation; the corresponding deep dive controls the pass and failure tests. A criterion listed at L5 remains dependent on its applicable lower-level criteria. An applicability condition attached to one criterion does not waive its neighbors.

### D1. Governance and Accountability

- **D1-L1 (Initial):** People make local decisions about agents, with incomplete organization records.
- **D1-L2 (Developing):** An accountable role, AI policy, risk-tier rule, RACI, and deployment register establish a decision owner and scope.
    - **Capability.**
        - **D1-ACCOUNTABLE.** One approved governance role has a current named holder.
        - **D1-POLICY.** An approved AI policy covers built, bought, and used systems and reaches its subjects.
        - **D1-RACI.** An approved RACI assigns one accountable party for model, data, and risk governance.
        - **D1-REGISTER.** The deployment register covers every agent and names a current accountable person.
        - **D1-REGISTER-TIER.** The register tier follows scored inputs of the approved scheme.
        - **D1-SCHEME.** An approved tier rule covers agents and reads their degree of autonomy.
- **D1-L3 (Defined):** A standing risk body, tiered production gate, boundary and supplier decisions, disclosure review, and shadow-agent handling operate on records.
    - **Capability.**
        - **D1-ALLOCATE.** A matrix allocates every identified deployment threat across the organization and named suppliers.
        - **D1-ALLOCATE-RESIDUE.** An authorized party records a disposition for each allocated residual threat.
        - **D1-BODY.** A chartered risk body seats security, legal, privacy, and engineering members.
        - **D1-BODY-CADENCE.** The body meets at its chartered cadence throughout the applicable lookback.
        - **D1-BOUNDARY.** Each agent type has a dated autonomous, approval, and prohibited action boundary.
        - **D1-BOUNDARY-DATA.** Each agent type has defined readable classes and permitted destinations for them.
        - **D1-DETAIL.** The asset inventory classifies model details and technical publications about the deployment.
        - **D1-DETAIL-REVIEW.** Each technical publication receives a prior review recording disclosures and withholdings.
        - **D1-DETAIL-RULE.** An approved publication rule requires that review for AI technical details.
        - **D1-GATE.** The tier-authorized approver approves the deployment before production.
        - **D1-GATE-CONDITION.** Each approval condition is met on time or changed by its approver.
        - **D1-GATE-RULE.** An approved per-tier rule names production approvers and the highest-tier risk body.
        - **D1-SHADOW.** Scheduled discovery covers endpoints, service tenants, clouds, and code hosts.
        - **D1-SHADOW-REAP.** Discovered shadow agents are registered or removed within the stated period.
- **D1-L4 (Managed):** The board receives substantive measures; the organization maintains its requirements crosswalk and independently tests readiness for assurance.
    - **Capability.**
        - **D1-CROSSWALK.** A current crosswalk maps each active AI policy and standard to named frameworks.
        - **D1-METRICS.** The board receives quarterly AI incidents, risk escalations, and anchored findings.
        - **D1-READINESS.** An independent, recent assurance readiness assessment covers the deployment and names gaps.
- **D1-L5 (Optimizing):** Current third-party assurance covers the deployment, while the body, crosswalk, and board attestations show sustained operation.
    - **Capability.**
        - **D1-ASSURE.** Current third-party assessment of the operating AI governance program includes the deployment; a crosswalk review alone does not count.
        - **D1-BODY-HISTORY.** Risk-body minutes span twelve months and record its decisions.
        - **D1-CROSSWALK-REFRESH.** The crosswalk has a revision in each of the last four quarters.
        - **D1-METRICS-ATTEST.** The board attests the AI report in each of the last four quarters.

### D2. Identity and Authorization

- **D2-L1 (Initial):** Agents use local or shared identities with incomplete ownership and credential records.
- **D2-L2 (Developing):** Agents and non-human identities are inventoried; human-delegated access stays within the human's authority.
    - **Capability.**
        - **D2-DELEGATE.** An agent acting for a human cannot exceed that human's business authority, including through a specialist or gateway.
        - **D2-IDENTITY.** Each agent has an exclusive non-human identity and authentication credential.
        - **D2-INVENTORY.** An inventory links every agent to each non-human identity it holds.
- **D2-L3 (Defined):** Issued identities are verified, owned, traceable to a human, and tied to deployment lifecycle and explicit delegation.
    - **Capability.**
        - **D2-COUPLING.** Every direct and brokered credential is classified as coupled or decoupled.
        - **D2-DELEGATE-EXCHANGE.** Each delegation hop exchanges a token naming both parties without copying credentials.
        - **D2-IDENTITY-VERIFY.** Services verify provider-signed assertions for each agent identity.
        - **D2-LIFECYCLE.** The deployment pipeline issues identities, schedules credential rotation, and revokes retirement.
        - **D2-OWNER.** Every agent and non-human identity has a current human owner.
        - **D2-TRACE.** Each action traces to its agent and accountable human.
- **D2-L4 (Managed):** Per-agent authorization, task binding, credential isolation, rotation, chain links, and a tested revocation path constrain standing authority.
    - **Capability.**
        - **D2-AUTHZ.** The decision point grants access by named agent identity or agent group.
        - **D2-COUPLING-MIGRATE.** Every coupled credential has an owner and an active migration date.
        - **D2-DELEGATE-LINK.** Delegated credentials link to their chain step and fail at another step.
        - **D2-DELEGATE-TOKEN.** Signed delegation credentials bind delegator, delegatee, scope, task, and expiry.
        - **D2-KILL.** One operation stops one orphaned agent and revokes its credentials, tokens, and sessions.
        - **D2-MUTUAL.** Each service call verifies its endpoint and authenticates its caller cryptographically.
        - **D2-NOCRED.** No credential is readable in agent context; a broker or credential-less flow supplies access.
        - **D2-ROTATE.** Automation rotates every stored credential class on its configured schedule.
        - **D2-ROTATE-MAP.** Each stored credential has a mapped consumer set that takes its replacement value.
        - **D2-TASKBIND.** Every authenticated access binds agent identity and current task through session, token, or comprehensive independent mediation; wrong-task and post-task calls fail.
- **D2-L5 (Optimizing):** The registry, scoped administration, audit, discovery, owner transfer, and conditional issuance operate across the deployment.
    - **Capability.**
        - **D2-ADMIN.** Administrative roles confine operators to named agents and identities.
        - **D2-AUDIT.** Agent, identity, credential, and grant life-cycle changes reach the audit log.
        - **D2-CONDITIONAL.** An identity service or credential broker evaluates a changing agent-specific condition at issuance and refuses failed requests; opaque supplier issuance is unanswerable.
        - **D2-COUPLING-ZERO.** No coupled credential remains in direct or brokered use.
        - **D2-DISCOVER.** Scheduled discovery closes every unregistered agent finding by registration or removal.
        - **D2-REGISTRY.** One inspectable registry joins agents, identities, credentials, owners, and grants.
        - **D2-IDENTITY-ATTEST.** Credential issuance verifies workload evidence binding the agent to its approved deployment.
        - **D2-OWNER-TRANSFER.** Owner departure or role change transfers each agent and identity to a successor.

### D3. Control and Least Agency

- **D3-L1 (Initial):** Tool use and human confirmation are local to the agent's operation, without a complete policy record.
- **D3-L2 (Developing):** The callable surface is bounded and destructive actions wait for a person or are refused.
    - **Capability.**
        - **D3-ALLOW.** An external component refuses or holds calls to tools outside each agent's allowlist.
        - **D3-APPROVE.** Destructive actions require prior human approval or are externally refused.
- **D3-L3 (Defined):** Every call reaches a locked, outside-model decision and follows the tier and decision-rights record; high-risk routes and uncertain external writes have defined handling.
    - **Capability.**
        - **D3-APPROVE-GATE.** A confirm-tier action cannot run before an approver acts.
        - **D3-DENY.** Unmatched tool calls are denied or held for approval, never silently allowed.
        - **D3-MEDIATE.** Every tool call reaches a decision point before execution in every permitted mode.
        - **D3-MEDIATE-EXACT.** Decisions use the executor's canonical action, including resolved commands and paths.
        - **D3-MEDIATE-FAILCLOSED.** Errors, timeouts, unreachable decisions, and invalid policy deny the call.
        - **D3-MEDIATE-OUTSIDE.** Model content cannot change policy; exposed decision interfaces refuse direct bypass.
        - **D3-MEDIATE-SYNC.** Execution waits for an explicit permit from the decision point.
        - **D3-POLICY-LOCK.** Only policy administration can widen decision rules, never the agent or principal.
        - **D3-RIGHTS.** Every action class has an enforced decision right, approver, justification, and time bound.
        - **D3-TIER.** Every callable action has a recorded auto, notify, confirm, or block tier.
        - **D3-TIER-DESTRUCT.** Destructive actions occupy confirm or block, never auto or notify.
        - **D3-TIER-ENFORCE.** The decision point enforces every recorded tier, including notification and refusal.
        - **D3-HIGHRISK.** Write-action risk categories use impact dimensions and map to authorization routes.
        - **D3-HIGHRISK-SOD.** Critical actions require two approvers in different fixed roles.
        - **D3-WRITE-RECONCILE.** Trusted request IDs prevent blind replay of uncertain external writes; provider state and a compensation owner resolve them.
- **D3-L4 (Managed):** Task, session, delegation, elevation, and separation-of-duties state constrain permitted actions over time.
    - **Capability.**
        - **D3-CHAIN.** The decision point validates complete delegation scope and rejects a misbound chain.
        - **D3-CHAIN-DEPTH.** Policy enforces a maximum delegation depth outside agent code.
        - **D3-CHAIN-SUBSET.** Each delegated hop intersects specialist grants with task authorization fixed outside model context; further hops cannot widen it.
        - **D3-ELEVATE.** Extra grants are task-bound, expire, and never become standing permission.
        - **D3-LEDGER.** An external session ledger denies writes beyond per-class aggregate limits.
        - **D3-NOREPLAY.** Authorization for one agent fails when another agent or delegate presents it.
        - **D3-ORCHESTRATE.** An orchestrator holds coordination grants without direct high-impact or external-action grants.
        - **D3-PROMOTE.** Each agent has a recorded autonomy stage changed only against promotion criteria.
        - **D3-SOD.** Agent-proposed production changes pass distinct proposer, approver, and deployer principals.
        - **D3-TASKSCOPE.** Every call is checked against trusted current task scope and agent role.
        - **D3-TRIFECTA.** Decision rules downgrade outbound actions when private data, untrusted content, and external reach coexist.
- **D3-L5 (Optimizing):** Approval binding, adaptive response, release checks, policy integrity, and security-consequential action order are tested and sustained.
    - **Capability.**
        - **D3-ADAPT.** Anomaly signals change action tiers and grants at call time, then restore appropriately.
        - **D3-APPROVE-TOKEN.** Single-request approval tokens bind approver, exact parameters, and expiry cryptographically.
        - **D3-LEDGER-SEQUENCE.** Consequential writes obey history-dependent order and time rules on a stable session identity.
        - **D3-POLICY-COMPILE.** Structural policy validation blocks invalid releases.
        - **D3-POLICY-DRIFT.** Loaded policy is compared with reviewed revisions and differences are resolved.
        - **D3-POLICY-SEMANTIC.** Pre-release decision tests find consequential authorization defects and block unresolved ones.
        - **D3-POLICY-REVIEW.** A different person reviews every policy release before production.
        - **D3-SOD-CRYPTO.** Separate role-held keys sign approval and deployment, preventing one key from doing both.

### D4. Runtime and Guardrails

- **D4-L1 (Initial):** Runtime protection is ad hoc or limited to instructions in a prompt.
- **D4-L2 (Developing):** Human prompts and all model responses pass configured safety screens.
    - **Capability.**
        - **D4-INPUT.** Every human prompt receives configured safety screening before the model reads it.
        - **D4-OUTPUT.** Every human or tool-directed model response receives configured content screening.
- **D4-L3 (Defined):** Direct and indirect injections are screened on their actual routes; agent-executed code is confined for the whole run.
    - **Capability.**
        - **D4-INJECT-CANON.** Text-route injection detection resists Unicode, invisible-character, case, and confusable variants; syntactic matchers normalize or detect them before matching.
        - **D4-INJECT-DIRECT.** An in-path classifier blocks or holds direct prompt attacks before model action.
        - **D4-INJECT-INDIRECT.** Each untrusted context route has in-path injection screening before model intake.
        - **D4-OUTPUT-SCOPE.** A dated output-screening record covers reachable restricted classes and safety categories.
        - **D4-SANDBOX.** Agent-chosen code and commands run in a sandbox; run-started processes and agent-directed effects stay there or in a narrower enforced service boundary.
        - **D4-SANDBOX-CLEAN.** One task uses each sandbox; destruction removes state and credentials afterward.
        - **D4-SANDBOX-CONFINE.** Sandbox policy blocks host escape, out-of-workspace access, configuration changes, and host interaction.
        - **D4-SANDBOX-CREDS.** Credentials used by the run are unreadable from within its sandbox.
        - **D4-SANDBOX-FIRST.** Sandboxing precedes loading workspace configuration that could start processes or set environments.
        - **D4-SANDBOX-LIMITS.** The platform enforces sandbox CPU, memory, and elapsed-time limits.
        - **D4-SANDBOX-MAC.** Unchangeable kernel or hypervisor policy confines sandbox processes and unnecessary privilege.
- **D4-L4 (Managed):** External checks compare proposed calls with the task and their effects; sourced answers are checked before delivery.
    - **Capability.**
        - **D4-ALIGN.** An external monitor blocks or holds tool calls misaligned with the authorized task; a semantic judge is optional.
        - **D4-CONTEXT.** After a defined untrusted-content risk signal, an outside-model rule refuses side effects or restricted-data egress beyond trusted task authorization.
        - **D4-GROUND.** Answers below a configured groundedness threshold are blocked, corrected, or marked unsupported.
        - **D4-VALIDATE-DRYRUN.** Preview-capable high-impact tools record proposed changes before execution.
        - **D4-VALIDATE-IMPACT.** External deterministic rules hold or refuse high-impact changes exceeding per-call limits.
- **D4-L5 (Optimizing):** The organization measures misses, treats relevant bypasses, enforces guardrail budgets, and covers applicable agents without owner opt-outs.
    - **Capability.**
        - **D4-BUDGET.** The guardrail runner enforces per-control latency and spending budgets with defined failure behavior.
        - **D4-INJECT-BYPASS.** Current bypass-library tests report each injection classifier's misses by bypass class.
        - **D4-INJECT-LANG.** Attack tests report each injection classifier's misses by relevant input language.
        - **D4-INJECT-REFRESH.** Classifier updates follow a stated cadence and leave versioned receipts.
        - **D4-INJECT-REMEDIATE.** Each relevant missed bypass is retested and mitigated or explicitly accepted by authority.
        - **D4-OUTPUT-LEAK.** Output scanning detects credential leakage and measures encoded as well as literal forms.
        - **D4-PLATFORM.** Applicable L4 controls cover every agent and entry channel beyond owner disablement.
        - **D4-SHARED.** Shared agent services are inventoried, partitioned, or accepted as cross-agent channels.

### D5. Egress and Network

- **D5-L1 (Initial):** Network reach has no effective agent-specific boundary.
- **D5-L2 (Developing):** Every applicable connection passes an externally enforced destination allowlist whose reach is recorded.
    - **Capability.**
        - **D5-ALLOW.** An externally enforced destination allowlist covers every agent connection and permitted mode.
        - **D5-REACH.** A dated record maps each allowed destination to its actual reachable systems and data.
- **D5-L3 (Defined):** Agent-aware mediation, DNS closure, call ceilings, MCP checks, and inter-agent authentication cover their respective paths.
    - **Capability.**
        - **D5-A2A-AUTH.** Both ends of each inter-agent exchange authenticate cryptographically.
        - **D5-A2A-CHAIN.** Delegated messages carry verifiable delegator and accountable-human references downstream.
        - **D5-A2A-REPLAY.** Receivers refuse previously accepted inter-agent messages using nonce or sequence state.
        - **D5-A2A-TLS.** Inter-agent receiving endpoints accept TLS 1.3 and no earlier version.
        - **D5-ALLOW-LOCK.** Agents, read content, and represented principals cannot widen destination allowlists.
        - **D5-CEILING.** An external component enforces per-agent model, tool, and MCP call ceilings.
        - **D5-CEILING-TOKENS.** The gateway enforces per-agent token ceilings over sent and received model traffic.
        - **D5-CEILING-TOOLS.** External counters deny tool and MCP invocations past task or session limits.
        - **D5-DNS.** Agent name lookups resolve only allowlisted names.
        - **D5-DNS-ONLY.** Network policy blocks alternate DNS and encrypted lookup routes.
        - **D5-GATEWAY.** All network model, tool, and MCP calls traverse an agent-aware gateway.
        - **D5-GATEWAY-AUTHZ.** The gateway refuses tool and MCP calls lacking agent-specific authorization.
        - **D5-GATEWAY-SCREEN.** The gateway screens request content and returned model or tool content.
        - **D5-MCP-BROKER.** Remote HTTP MCP calls validate issuer, audience, and caller through a broker; local stdio launches confine executable choice and inherited credentials.
        - **D5-MCP-PIN.** A registry pins approved MCP versions and tool-definition fingerprints at connection.
- **D5-L4 (Managed):** Brokered peer traffic, segmented agents, tool-specific credentials, and validated content and contracts narrow reachable effects.
    - **Capability.**
        - **D5-A2A-BROKER.** An external broker authenticates, schema-validates, and mediates inter-agent messages.
        - **D5-A2A-SCREEN.** Injection and restricted-data screening holds unsafe inter-agent messages before context intake.
        - **D5-CONTRACT.** The gateway rejects supplied-service responses outside pinned response and version contracts.
        - **D5-GATEWAY-EXCHANGE.** Per-tool credential exchange prevents tools accepting an agent's reusable token.
        - **D5-INSPECT.** Agent-run code cannot hide an unapproved origin inside an allowed outbound connection.
        - **D5-MCP-CVE.** MCP versions map to applicable advisories and findings, with disclosure gaps recorded and findings routed for response.
        - **D5-MCP-POISON.** Tool definitions are screened for model-directed instructions before approval and changes.
        - **D5-MCP-RUGPULL.** The gateway refuses changed tool definitions until a person approves new fingerprints.
        - **D5-ORCHESTRATE.** Orchestrator direct calls cannot bypass the delegated path's scope and approval for sensitive writes, protected data, or unapproved egress.
        - **D5-SEGMENT.** Network policy denies sensitive internal services outside each exposed agent's allowed tools.
- **D5-L5 (Optimizing):** No uncontrolled direct bypass or agent-controllable relay remains; mediated routes enforce task-bound decisions, with multi-proxy policy reconciliation where that topology exists.
    - **Capability.**
        - **D5-A2A-SIGN.** A2A publishers sign Agent Cards and consumers verify issuer signatures, refusing unsigned, altered, or untrusted cards.
        - **D5-FEDERATE.** For actual multi-cloud proxies, one approved policy revision governs each location.
        - **D5-FEDERATE-RECONCILE.** Applicable proxies reconcile decisions by agent and revision and flag policy violations.
        - **D5-GATEWAY-ONLY.** Network routes leave no agent bypass around the gateway or adjacent proxy.
        - **D5-MCP-QUARANTINE.** Urgent applicable MCP findings trigger automatic containment and authorized restoration.
        - **D5-RELAY.** Internal services with agent-controllable relay inputs are confined to approved downstream destinations.
        - **D5-SSRF.** Redirects, address tricks, and resolution changes cannot escape the destination allowlist.
        - **D5-TASK-EGRESS.** Outbound decisions bind authenticated agent, current task, and approved destination; wrong-actor, wrong-destination, and closed-task calls fail.

### D6. Data, Memory and RAG

- **D6-L1 (Initial):** Data reach and memory state have no consistent assessment.
- **D6-L2 (Developing):** Data classes, retrieval origins, extensions, and corpus reach are recorded.
    - **Capability.**
        - **D6-CLASSIFY.** Each source and memory store has a class at its actual authorization grain.
        - **D6-EXTEND.** A named reviewer approves every retrieval extension before first use.
        - **D6-ORIGIN.** Each retrieved item carries source origin set by the retrieval layer.
        - **D6-REACH.** A dated per-location assessment identifies readers through the agent and oversharing findings.
- **D6-L3 (Defined):** Retrieval follows source authorization, while admission, copies, scope, and validation data have assessable protections matched to the deployment's threats.
    - **Capability.**
        - **D6-ENTITLE.** Every retrieval and copy enforces the asking principal's current source entitlements.
        - **D6-ENTITLE-TASK.** Unattended runs use a trusted task-bound retrieval scope no broader than needed.
        - **D6-HOLDOUT.** Validation corpora are stored and write-isolated from training, models, code, and instructions.
        - **D6-HOLDOUT-TRANSFER.** Validation-corpus transfers name recipients and preserve restrictive access policy.
        - **D6-REACH-REMEDIATE.** Oversharing findings close within a stated period through grants, removal, or owner exception.
        - **D6-SCAN.** A threat-appropriate check treats new items before use, rescans held items on schedule, and routes configured holds, rejects, or alerts to triage.
        - **D6-SCAN-INJECT.** Ingest-path scans hold model-directed instructions before retrieval.
        - **D6-SCAN-TEST.** Route-matched planted and benign tests report poison detection and false holds or alerts by treatment decision.
        - **D6-SCOPE.** Recorded minimum-data scope governs corpora, copies, fine-tuning datasets, and model-request fields.
        - **D6-SCOPE-IDENTIFIERS.** Lifecycle-only identifiers are inventoried and excluded from model training.
        - **D6-STORE-ACCESS.** Direct corpus-store readers have named operational identities and scoped grants.
        - **D6-STORE-ENCRYPT.** Corpus content and embeddings are encrypted at rest in each store.
        - **D6-STORE-RETAIN.** Deleted or excluded source items and embeddings leave stores within stated periods.
        - **D6-TRUST.** An external component attaches recorded source trust levels to retrieved items.
- **D6-L4 (Managed):** Memory has partition, write, integrity, provenance, reset, and rollback controls; labels and poisoning detections act in production.
    - **Capability.**
        - **D6-DETECT.** Production detection alerts on poisoned agent writes to memory or corpora.
        - **D6-DETECT-CLASSES.** Planted tests report sabotage and targeted poisoning coverage separately.
        - **D6-DETECT-PROTECT.** Detection rules and baselines have write separation from data paths and integrity checks.
        - **D6-LABEL-CARRY.** External logic carries highest source class into answers and derived artifacts.
        - **D6-LABEL-GATE.** External policy withholds restricted classes from disallowed principals or channels.
        - **D6-MEMORY-LOG.** An agent-immutable append-only log permits memory-state replay.
        - **D6-MEMORY-PARTITION.** External policy confines memory reads to trusted agent, session, or principal partitions.
        - **D6-MEMORY-PROVENANCE.** Memory entries record source, writer, time, and partition outside model control.
        - **D6-MEMORY-RESET.** Between tasks, an outside-model rule removes unauthorized carryover while retaining permitted memory.
        - **D6-MEMORY-VERIFY.** Integrity checks reject or quarantine changed memory before context intake.
        - **D6-MEMORY-WRITE.** External policy refuses writes outside an agent's allowed memory partitions.
        - **D6-OBFUSCATE.** Restricted fine-tuning fields are obfuscated before training.
        - **D6-OBFUSCATE-RESIDUAL.** Residual quasi-identifiers have recorded treatment or authorized acceptance.
        - **D6-OBFUSCATE-TABLES.** Token reversal tables have access control at least as strict as protected data.
        - **D6-REACH-CADENCE.** Oversharing review repeats on schedule and after corpus or extension changes.
        - **D6-ROLLBACK.** Memory, index, and vector stores have tested restore to recorded earlier states.
        - **D6-SCOPE-MEASURE.** Kept or removed training fields have measured correctness, robustness, or fairness effects.
        - **D6-SCOPE-PROPAGATE.** Source corrections and deletions propagate to derived datasets, copies, and embeddings.
        - **D6-TRUST-WEIGHT.** Retrieval ranking or filtering uses recorded source trust weights.
- **D6-L5 (Optimizing):** The deployment tests specified cumulative disclosure and poisoning risks, detects drift and conflicting sources, rehearses recovery, and verifies item provenance before context entry.
    - **Capability.**
        - **D6-ATTEST.** Before context intake, retrieved items pass authenticated provenance and integrity verification.
        - **D6-CONTRADICT.** Cross-source contradictions trigger withholding, qualification, or review before answers leave.
        - **D6-ENTITLE-INFER.** Session-aware policy holds specified, tested combinations of individually readable items that disclose a restricted inference.
        - **D6-ROLLBACK-DRILL.** Quarterly restore drills measure recovery time against the stated target.
        - **D6-SCAN-BOUND.** For applicable corpus, memory, and supplied training stores, tested miss and wrongful-treatment limits by threat class support an owner-approved residual decision.
        - **D6-SCAN-DRIFT.** Production detectors alert when corpus content distributions depart from recorded baselines.

### D7. Observability and Detection

- **D7-L1 (Initial):** Agent actions cannot be reconstructed from records the organization holds.
- **D7-L2 (Developing):** Every tool call and answer has a searchable record, time, and accountable human.
    - **Capability.**
        - **D7-LOG.** Searchable organization-held records identify each tool call and answer, time, and accountable human.
- **D7-L3 (Defined):** Attributed action, memory, and relevant guardrail records join into a retained trace with a controlled schema.
    - **Capability.**
        - **D7-ATTRIBUTE.** Every action record identifies its agent and accountable human.
        - **D7-FORWARD-ESCAPE.** Sandbox escape indicators for system calls, forbidden paths, and network reach enter telemetry.
        - **D7-FORWARD-SCAN.** Investigable D6 corpus-screening findings reach telemetry with corpus, run, and affected item identity.
        - **D7-LOG-MEMORY.** Memory writes log writer, session, store, entry, source, time, and content reference.
        - **D7-LOG-SCHEMA.** Tool records add arguments, target, outcome, correlation key, and write reference.
        - **D7-RETAIN.** Each required record class remains searchable for its stated forensic period.
        - **D7-SPANS.** Inference, tool, agent, and retrieval events form attributed correlated traces.
        - **D7-SPANS-HELD.** Required traces reach an exportable backend available to the organization or assessor.
        - **D7-SPANS-PIN.** Versioned instrumentation catches removal of fields needed for reconstruction or detection.
        - **D7-PROMPT-CANARY.** System-prompt canaries are checked against every output and tool argument.
- **D7-L4 (Managed):** Production detections use that evidence, and control changes, workflow steps, evaluation results, and log integrity have inspectable outcomes.
    - **Capability.**
        - **D7-IDENTITY-BASELINE.** Each non-human identity has resource and origin baselines with departure alerts.
        - **D7-ESCAPE-RECORD.** Denied out-of-task calls emit agent, task, tool, and violated-scope events.
        - **D7-BASELINE.** Per-agent tool sequence and frequency baselines drive production departure alerts.
        - **D7-CONTROL-APPROVE.** Detectors alert on missing required approvals and approval steps that lose human action.
        - **D7-CONTROL-RELAX.** Detectors alert on changes that relax any deployment control.
        - **D7-DRIFT.** Production detection scores progressive within-session relaxation and alerts at threshold.
        - **D7-DRIFT-ROUTE.** Drift alerts follow a predetermined human-review or automatic-suspension route.
        - **D7-EVAL.** Quarterly agent evaluations cover multiple threat categories and report coverage gaps.
        - **D7-EVAL-SESSIONS.** Quarterly evaluations test attacks that persist across sessions.
        - **D7-EVAL-TURNS.** Quarterly evaluations separately test multi-turn jailbreak and escape paths.
        - **D7-LOG-OUTSIDE.** An agent-independent writer stores tool-call and answer records outside agent write access.
        - **D7-LOG-TAMPER.** Tests show agent identities cannot suppress, change, or erase required records.
        - **D7-ORCHESTRATE.** External workflow logs record delegated tasks, agents, and returned results.
        - **D7-ORCHESTRATE-RECONCILE.** Workflow reconciliation alerts on delegated actions without logged steps.
        - **D7-POSTURE.** A posture inventory covers hosting accounts, subscriptions, tenants, and agents.
        - **D7-REVIEW-EVASION.** Detectors flag inputs aimed at weak human-review timing or channels.
- **D7-L5 (Optimizing):** Alert handling closes within a stated service level; applicable inter-agent baselines and cascade rules operate against tested paths.
    - **Capability.**
        - **D7-A2A-BASELINE.** Applicable inter-agent traffic is baselined by peers, types, and volume with alerts.
        - **D7-ALERT-ACTIONABLE.** Analyst-actionable alert rate is measured against a documented target and tuned.
        - **D7-ALERT-LOOP.** Every L4 alert reaches a control decision or false-positive tuning within its SLA.
        - **D7-CASCADE.** Tested production rules detect selected harmful cross-agent propagation paths.
        - **D7-JOINT.** Shared-state or interacting agents have joint-behavior baselines and departure alerts.
        - **D7-PLAYBOOK.** Production playbooks identify, attribute, stop, and contain agents for each alert class.

### D8. Engineering and Supply Assurance

- **D8-L1 (Initial):** The deployment lacks a complete component and version record.
- **D8-L2 (Developing):** Sources, versions, model cards, acquired-data provenance, and sensitive engineering records are identified and controlled.
    - **Capability.**
        - **D8-DATASET.** Acquired datasets retain source and transformation provenance, including referenced-entry checksums.
        - **D8-DOCS.** Asset records list agent design, development, model, and experiment documents.
        - **D8-DOCS-ACCESS.** Document stores restrict readers to owner-approved people and roles.
        - **D8-INVENTORY.** A reconciled component inventory records source, version, maintainer, introduction date, and held digest.
        - **D8-MODEL-CARD.** Current supplier model or system cards are held for each model in use.
        - **D8-VERSION.** Released component versions are reconstructable for every period they ran.
        - **D8-PROMPT-SECRETS.** Effective system prompts are reviewed for secrets before use.
- **D8-L3 (Defined):** Material designs and changes receive review, scans, supplier checks, assembly records, and a release-linked prompt or harness check.
    - **Capability.**
        - **D8-DESIGN.** Material releases receive deployment-specific threat and boundary review, with treatment for each material risk.
        - **D8-HARNESS.** Managed settings govern every coding agent and runner without local override.
        - **D8-HARNESS-EXTEND.** Local extensions to managed list keys are inventoried and bounded.
        - **D8-HARNESS-LOCK.** Managed-only locks cover available permission, hook, MCP, and ability controls.
        - **D8-HARNESS-RESTORE.** Changed managed settings return to the published version or a clean image.
        - **D8-HARNESS-REVIEW.** Every security-relevant harness configuration change receives prior documented review.
        - **D8-IMPLEMENT.** Every first-party change, including generated code, receives risk-matched checks and holds blocking findings.
        - **D8-INSTRUCT-BASELINE.** Loaded instructions match reviewed baselines; changes require new reviewed versions.
        - **D8-ABILITY-SCAN.** Installed abilities are scanned for hostile code and instructions at each version.
        - **D8-ABILITY-SOURCE.** Each ability comes from a source approved for its publisher and identity.
        - **D8-AIBOM.** Pipeline-generated machine-readable AI bills cover release components and versions.
        - **D8-DEPS-INSTALL.** Organization-controlled registries check package name, publisher, and age before install.
        - **D8-DEPS-LOCK.** Package installs use exact version and hash pins, refusing mismatches.
        - **D8-DEPS-SCAN.** Release pipelines scan all installed packages and images and hold blocking findings.
        - **D8-MODEL-INSPECT.** Less-trusted model files receive pre-load architecture inspection for unknown execution.
        - **D8-MODEL-PROBE.** Less-trusted model files run isolated probes with resource, call, and network monitoring.
        - **D8-MODEL-SCAN.** Pre-load model-file checks cover format, signature, executable patterns, and corruption.
        - **D8-SIGN.** Every organization-built release artifact carries a signature.
        - **D8-SUPPLIER.** Supplier assessment covers security, disclosure, notice, audit, and applicable provider data-use and retention terms.
        - **D8-PROMPT-PROBE.** Material prompt or model changes receive pre-release sensitive-function leak probes.
- **D8-L4 (Managed):** The assembled deployment is tested before promotion; deployed components, build provenance, advisory response, and exact version admission are verified.
    - **Capability.**
        - **D8-AIBOM-RUNTIME.** Observed runtime components are compared with approved release bills on a schedule.
        - **D8-BUILD.** Build artifacts carry signed platform-generated SLSA Build L2 provenance.
        - **D8-DISCLOSE.** Component advisories enter triage and meet severity-based remediation deadlines.
        - **D8-DISCLOSE-CONTAIN.** Overdue advisories have bounded compensating controls with owners and end conditions.
        - **D8-LINEAGE.** Produced models have versioned lineage across bases, datasets, transformations, code, and build.
        - **D8-REPO-WRITE.** Agent-held identities can write only their own release locations in artifact repositories.
        - **D8-RETIRE.** Periodic component review retires, replaces, or excepts deprecated and unmaintained parts.
        - **D8-VERIFY.** External checks refuse components with missing or mismatched approved provenance or integrity.
        - **D8-VERIFY-BUNDLE.** Loaded model verification covers weights, tokenizer, vocabulary, configuration, and inference code.
        - **D8-VERIFY-INSTRUCT.** Every loaded instruction file passes origin-backed digest verification or is refused.
        - **D8-VEX.** Externally published components carry vulnerability exploitability statements in VEX format.
        - **D8-MODEL-PIN.** Production agents select fixed model versions under release control.
        - **D8-TEST.** The assembled deployment receives risk-selected conventional and agent-specific tests, with findings retested.
        - **D8-RELEASE.** Every promotion has an exact-version security decision; blocking findings require D1-authorized exception.
- **D8-L5 (Optimizing):** Reconciliation, verification gates, supplier-facing feeds where produced, and owned findings have sustained, time-bound outcomes.
    - **Capability.**
        - **D8-A2A-SIGN-TEST.** Applicable A2A releases test signed Agent Card verification and refusal for missing, altered, or untrusted signatures.
        - **D8-AIBOM-DRIFT.** Runtime-to-release component drift has a tolerance and resolution period.
        - **D8-BUILD-HARDENED.** Built artifacts use isolated SLSA Build L3 provenance and protected signing material.
        - **D8-LOOP.** Scheduled production reconciliation assigns owners to inventory, bill, and finding discrepancies.
        - **D8-LOOP-SLA.** Every D8 finding reaches a control change or decision within a published SLA.
        - **D8-VERIFY-GATE.** All promotion routes refuse failed verification or unapproved component sets.
        - **D8-VEX-FEED.** External component consumers receive timely machine-readable VEX updates.
        - **D8-WEIGHTS.** Produced weights are separated, least-privileged, integrity-monitored, and hash-protected.

### D9. Operations and Human Factors

- **D9-L1 (Initial):** Owner departure, guardrail failure, approval review, and incident response have no deployment-specific procedure.
- **D9-L2 (Developing):** Runbooks assign operator steps for guardrail failures, leavers, credentials, and approval paths.
    - **Capability.**
        - **D9-GUARD-RUNBOOK.** Each guardrail failure has an operator step and defined call behavior.
        - **D9-OFFBOARD.** Owner departure transfers or retires each agent under a timed runbook.
        - **D9-OFFBOARD-CREDS.** Leaver runbooks revoke or replace credentials a departing person could use.
        - **D9-QUEUE-RUNBOOK.** Each approval path has a review owner, cadence, record, and response rule.
- **D9-L3 (Defined):** Guardrail effects, orphan resolution, queue measures, notices, and exercised incident duties are recorded.
    - **Capability.**
        - **D9-GUARD-COST.** Guardrail costs are tracked for every agent using them.
        - **D9-GUARD-FAILMODE.** Error and timeout tests confirm each guardrail's declared failure behavior.
        - **D9-GUARD-LATENCY.** Added guardrail latency is measured per agent.
        - **D9-HIGHRISK-RECORD.** High-risk approvals record request, exact parameters, authenticated approver, confirmation, and outcome tamper-evidently.
        - **D9-IR.** An approved AI playbook covers applicable incident classes and response stages.
        - **D9-IR-CONTAIN.** Deployment-specific stop controls, records, and responders appear in containment steps.
        - **D9-IR-EXERCISE.** An annual AI incident exercise yields owned after-action items.
        - **D9-IR-NOTIFY.** The playbook names applicable notification instruments, owners, and clocks.
        - **D9-IR-SCENARIO.** Response plans cover compromised supplier models already in production.
        - **D9-MODEL-DEPRECATE.** External consumers receive version-support and deprecation notice promises.
        - **D9-NOTICE.** Users receive AI-involvement notice at first interaction or content.
        - **D9-NOTICE-PROPERTIES.** User disclosure is checked against model, data, accuracy, robustness, and residual-risk properties.
        - **D9-QUEUE-AGE.** Median approval age and unanswered expirations are tracked per path.
        - **D9-QUEUE-RATE.** Approval rates are tracked per path.
        - **D9-REAP.** Scheduled orphan sweeps close agents and credentials within stated periods.
        - **D9-ROLE.** An approved operations-security role has duties and a current holder.
        - **D9-ROLE-DEPUTY.** Each duty has an approved deputy with necessary access.
- **D9-L4 (Managed):** Approval quality and coverage, drift triage, adversarial oversight tests, and end-to-end retirement are measured.
    - **Capability.**
        - **D9-APPROVE-COVERAGE.** Execution-to-approval joins measure human approval coverage by action class.
        - **D9-DRIFT-TRIAGE.** Every drift finding is classified and adversarial cases reach response.
        - **D9-DRILL.** Recent quarterly drills retire agents through credential and connection closure.
        - **D9-OVERSIGHT-FLAG.** Approver-rate anomalies against historical baseline raise alerts.
        - **D9-OVERSIGHT-INVOLVE.** Each approval path has a stated involvement measure taken each period.
        - **D9-OVERSIGHT-LIMIT.** Per-approver limits hold or reroute excess requests.
        - **D9-OVERSIGHT-TEST.** Annual adversarial approval tests cover urgency, fatigue, normalization, and confusion.
        - **D9-QUEUE-P95.** Approval queue age is tracked at the 95th percentile per path.
        - **D9-QUEUE-STAMP.** Each path tracks rubber-stamp rate by a stated method.
- **D9-L5 (Optimizing):** Incidents and queue excursions drive dated control decisions within stated limits, and deputies demonstrate continuity.
    - **Capability.**
        - **D9-LOOP.** Recent incidents reach timely control changes or explicit no-change decisions.
        - **D9-QUEUE-THRESHOLD.** Queue age, rubber-stamp, and involvement breaches receive timely decisions over two quarters.
        - **D9-ROLE-CONTINUITY.** Deputies perform duties and an end-to-end incident run in quarterly continuity tests.

## Prerequisites and blockers

A dependent criterion is met only when its own outcome can be shown. Record an unmet upstream condition as the dependent criterion's failed or unanswerable facet, name the upstream owner, and carry it into the target plan. Do not apply a numeric ceiling to another domain. Relevant dependency paths include:

- D2 identity and human attribution supporting D3 delegation checks and D7 action attribution. If identity cannot be resolved, the affected D3 or D7 test fails or remains unanswerable on its own terms.
- A trusted task scope and mediated decision from D3 supporting D5-TASK-EGRESS. A destination rule that sees only a bearer credential cannot establish an agent-task-destination decision.
- D6 corpus entitlement and origin supporting an interpretation of D4 grounding and D7 retrieval traces. A grounded answer may still expose content the asker was not entitled to retrieve.
- D8-DESIGN, D8-IMPLEMENT, D8-TEST, and D1 risk authority feeding the exact-version D8-RELEASE decision. A release missing a material test or authorized exception cannot claim that gate.
- An actual multi-cloud proxy topology making D5-FEDERATE and D5-FEDERATE-RECONCILE applicable; a real cross-agent propagation path making D7-CASCADE applicable. Their absence is recorded, not presumed.

These are examples of evidence dependencies, not extra level gates. The domain definition identifies the applicable population and negative tests. The assessor records the specific criterion, missing prerequisite, owner, and work needed to clear it.

## Assessment result

The result is a **current and target domain profile for one deployment**. One row per applicable domain states the observed level, evidence confidence and limitation, risk-selected target, consequential failed or unanswerable criteria, prerequisite owner, and proposed action. A fully inapplicable domain has a reason instead of a level. The nine rows are kept separate. An illustrative row format is:

| Domain | Current | Confidence and basis | Target | Gap and prerequisite | Proposed decision |
|---|---|---|---|---|---|
| D8 | L3 | Limited: supplier test scope unresolved | L4 | D8-TEST unanswerable; request scoped supplier results and test customer integration | Fund integration test; hold release pending D8-RELEASE decision |

This row illustrates the reporting grammar, not a rating of a real deployment. The full report retains the criterion records and evidence identifiers, including not-applicable and unanswerable reasons. It explains why the target fits the deployment's action authority, data sensitivity, exposure, and business consequence. It then compares the expected reduction in those failure paths with one-time engineering effort, recurring operations, supplier cost, and the burden on human approvers. A named decision owner funds, defers, or accepts each package, records residual risk, and sets a reassessment trigger. [NIST CSF 2.0's Current and Target Profile method](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf#page=12) and [DOE C2M2's target-level guidance](https://www.energy.gov/sites/default/files/2022-06/C2M2%20Version%202.1%20June%202022.pdf#page=24) inform this profile-and-plan structure. Neither source prescribes an AAI-S CMM target for every deployment.

A change in tool grants, data source, supplier boundary, model behavior, or release path may invalidate earlier evidence even when the product name is unchanged. Reassess the affected criteria and report a new deployment profile. Organization-wide controls can be reused only while their scope, version, and operating record still cover the changed deployment.

## Limits and provenance

The model grades control capability in the assessed boundary. It does not certify a supplier, prove that every attack is stopped, or score security of non-AI systems against AI-assisted attackers. A high level can coexist with material residual risk, especially where tests have narrow coverage or supplier internals are unavailable. Read the confidence and gap record beside each level.

The domain progression is this wiki's assessment design. [DOE C2M2's cumulative practices and risk-selected targets](https://www.energy.gov/sites/default/files/2022-06/C2M2%20Version%202.1%20June%202022.pdf#page=24), [NIST CSF 2.0's outcome and profile structure](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf#page=11), and [NIST SP 800-53A's assessment methods](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf#page=22) inform its form. [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]] discusses the broader design lineage. Agent-specific threat and control details in the deep dives draw on their cited primary sources, including the [OWASP AI Exchange agentic control guidance](https://owaspai.org/go/agenticaioverview/). The AAI-S CMM level assignments and criterion combinations are not requirements attributed to those publications.

L5 measures deployed outcomes that can be implemented and inspected now, subject to the operating history each criterion demands. Earlier research placeholders are outside the scored criteria. A proposed technique based only on an old paper or talk, without an implemented and developed control path, cannot establish L5. For a criterion change, revise its pass condition, negative test, applicability, and evidence in the owning deep dive. Synchronize its level row here and its question and artifact in the handbook before use. The [[agentic-ai-security-cmm-recalibration-method-2026|May 2026 recalibration]] is historical. The [criterion migration ledger](../../docs/aai-s-cmm-criterion-migration-2026-09.tsv) maps earlier IDs to this version for prior citations and assessments. It is not an assessment method. Dated product examples show possible implementations at the date assessed. No product purchase or specific token format is itself a level condition.
