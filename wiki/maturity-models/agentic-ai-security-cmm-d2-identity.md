---
type: maturity-model
title: "CMM D2: Identity and Authorization"
address: c-000137
created: 2026-05-25
updated: 2026-08-25
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
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[identity-credential-coupling]]"
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
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[identity-credential-coupling]]"
---

# Agentic AI Security CMM — D2 Identity & Authorization (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D2 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D2 assigns every agent a per-agent non-human identity and governs its credential lifecycle. The threats this domain answers are Privilege Compromise (T3) and Identity Spoofing and Impersonation (T9) in [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]], whose Playbook 4 (authentication, identity, and privilege controls) calls for per-agent mutual authentication and short-lived credentials — the operational form of the L3–L5 ladder below. Three things change in the recalibration: a stale GA assertion is corrected (Okta for AI Agents is not yet GA), per-agent identity is now GA platform-native on all three hyperscalers, and per-task capability tokens move to L5+ because no platform ships them.

> [!gap] Single-source grounding
> Levels and cost model synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation. Tooling status is a May 2026 snapshot.

**D2-L3 raises the ceiling on three domains at once.** The active dependency caps are **D2→D5** and **D2→D7**, so one rollout of verifiable per-agent identity lifts D2, D5, and D7 together, and no other rung in the model reaches that far for the same spend. Egress and observability cannot exceed D2's level, because both per-agent egress policy and per-agent behavioral baselining bind to a principal that D2-L3 verifies. For a Microsoft incumbent, D2-L3 costs near-zero licensing: Entra Agent ID rides the directory. Spend the first identity dollar here.

The [[microsoft-zt4ai|Microsoft ZT4AI]] Identity pillar (verify explicitly / least privilege) supplies the named controls behind these rungs: [[microsoft-entra-agent-id|Entra Agent ID]] per-agent identity, Conditional Access for Agent Identities, and ID Protection for agents, all crosswalked to the D2 ladder in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]]. [[microsoft-entra-agent-id|Agent 365]] sits above these controls as a **management plane** — the [[agent-catalog|agent registry]] and admin surface that aggregates and operates them — rather than as a new enforcement control or a portable standard. It is vendor-bound and per-user-licensed, and an organization cannot implement it on another stack. The management-plane-versus-catalogue distinction is set out in [[standards-review-microsoft-rai-agent-365-2026-Q2|the 2026-Q2 RAI / Agent 365 review]].

## Threat coverage

D2 is the primary domain for **ASI03 (Identity & Privilege Abuse)** and **ASI10 (Rogue Agents)**, and the entry point for **Class 1 (AI-aware insider)** through the identities and grants an insider uses against the running system. Because per-agent egress (D5) and behavioral baselining (D7) are capped by it, D2 is the prerequisite for containing any per-agent threat. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

That entry point stops at the running system. No rung below grades an MLOps role, a human engineer's access to the training environment, or the development-time controls the [[owasp-ai-exchange|OWASP AI Exchange]] groups in §3.0 — `DEV SECURITY`, `SEGREGATE DATA`, `CONF COMPUTE` — so an insider already inside that environment sits outside this domain's graded criteria, and the Exchange names insider access as one route to the confidentiality threat those controls answer ([`/go/devmodelleak/`](https://owaspai.org/go/devmodelleak/)). Least-privilege MLOps role design, which [[agentic-ai-threat-classes-2026|the threat classes]] name as a Class 1 control, is stated in the class and graded in no rung here. [[agentic-ai-security-cmm-crosswalk|The crosswalk]] records the same bound on all four domains it places Class 1 on — D2, D3, D6, and D8 — each of which grades the system in operation.

The prerequisite is necessary and not sufficient, and the [[owasp-ai-exchange|OWASP AI Exchange]] states the insufficiency directly: authentication confirms identity and not intent, so a prompt-injected authenticated agent still passes its auth checks.[^aix-mac] Every control in this domain assumes the principal it names is acting on its own behalf. Where that assumption fails, D2's contribution is that the action carries a nameable principal for [[agentic-ai-security-cmm-d5-egress-network|D5]] to constrain and [[agentic-ai-security-cmm-d7-observability|D7]] to baseline, which is the reason the caps run this direction. A program reporting a high D2 with a low D5 and D7 can name the principal behind an action and can still not constrain what that principal does.

## Control landscape (dated)

| Capability | What ships today | Status | Microsoft | AWS | GCP |
|---|---|---|---|---|---|
| Per-agent non-human identity | [[spiffe\|SPIFFE]]/SPIRE (OSS); OAuth 2.1 / OIDC | GA | Entra Agent ID, GA Apr 2026[^entra] | Bedrock AgentCore workload identities[^awsid] | Agent Identity, SPIFFE-based[^gcpid] |
| Credential lifecycle (issue/scope/rotate/revoke) | OAuth 2.1 token exchange; X.509 + STS | GA | blueprint holds credentials, the agent identity holds none[^entra] | AgentCore token vault[^awsid] | auth-manager credential vault[^gcpid] |
| Zero-credentials-in-agent-context | credential-proxy pattern; SPIFFE SVID; Azure Managed Identities | GA | Managed Identities (GA, long-standing) | native | native |
| Conditional / risk-based access for agent identities | — | **MS only** | Conditional Access for Agent Identities, GA, requires Entra ID P1[^ca] | none agent-specific | none agent-specific |
| Coupled-credential migration (SAS / storage keys → decoupled) | pattern, not a product | — | Managed Identities + RBAC | IAM Roles Anywhere / STS[^iamra] | [[non-human-identity\|Workload Identity]] Federation[^wif] |
| Per-task / capability-scoped tokens (holder-bound, attenuating) | [[tenuo-warrant\|Tenuo Warrant]] (OSS, Ed25519, [[monotonic-attenuation\|monotonic attenuation]]) | **OSS, early-stage** | none — Entra issues per-*resource*, not per-*task*, grants | none | none |

Entra Agent ID mints a credential-less service principal from an agent blueprint, so a Microsoft deployment reaches the zero-credentials row natively and buys no credential proxy.

**Correction.** The current CMM dates "Okta for AI Agents GA Apr 30 2026." That is wrong. Okta's own materials place it at Early Access in FY27 Q1 and GA later in FY27[^okta]; the GA'd product is Auth0 for AI Agents (Oct 2025). The date is removed from D2 and the tooling map. Per-agent identity is well covered platform-native regardless, so nothing in the ladder depends on Okta.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2.

- **L1 — Initial.** Agents share human credentials or service accounts; no inventory.
- **L2 — Developing.** Agents hold distinct non-human identities in a manual inventory; delegation runs only through the human user.
- **L3 — Defined.** Every agent has a verifiable, attested per-agent identity (platform-native agent identity or SPIFFE workload ID); OAuth 2.1 token exchange handles delegation; the NHI lifecycle binds to the deploy pipeline, not HR joiner/mover/leaver; the inventory distinguishes coupled from decoupled credentials ([[identity-credential-coupling|identity-credential coupling]]); every NHI carries a human owner; every action traces to a human.
- **L4 — Managed.** No credential sits in agent context: a broker or a credential-less identity model issues what an agent needs, a PDP authorizes each agent per action, and every session, token, and delegated grant binds to one identity and one task, so nothing carries across a task boundary or a delegation hop. Rotation and an orphaned-agent kill switch are automated and tested, each NHI carries a behavioral baseline, and a migration plan off coupled credentials is active.
- **L5 — Optimizing.** A unified agent-governance program operates in production (registry, lifecycle API, per-agent identity graph, ownership transfer, scoped RBAC, audit-log integration); shadow-agent discovery is operational; risk/conditional access for agent identities is active where the platform supports it; identity binding carries cryptographic attestation (SPIFFE JWT-SVID or platform-attested identity); zero coupled credentials remain for agent-class NHIs.
- **L5+ — Leading Edge.** Per-task capability tokens with cryptographic holder-binding and monotonic attenuation ([[tenuo-warrant|Tenuo Warrant]]-class — OSS-only, no platform-native implementation); multi-vendor agent-identity federation across two or more IDaaS platforms with cross-platform identity-graph reconciliation; named participation in a SPIFFE / OAuth / OIDC agent-extension working group.

The L4 delegation clause supplies the token property that [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L4 consumes. That rung grades the policy engine validating a delegation chain end to end, enforcing a configured maximum depth, and holding a downstream agent to a subset of its delegator's grants. A chain is validatable only where the credentials carry it, so this domain grades the artifact and D3 grades the evaluation, and the capability is scored once in each place. Chain splicing is the failure the parent link closes: a credential correctly signed and correctly scoped, presented at a step of the chain it was never issued for. The [[owasp-ai-exchange|OWASP AI Exchange]] states the residue that survives both halves — a structurally valid token can still be contextually unauthorised, and short lifetimes, revocation, and attenuation carry what token construction cannot.[^aix-leastmodelpriv]

The L4 session clause governs the authentication session and the issued token. Destruction of a sandbox's transient state at task completion is graded in [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] L3, and review and reset of agent memory context at a session boundary in [[agentic-ai-security-cmm-d6-data-rag|D6]] L4; the three cover different artifacts and a program can hold any one without the others. The L4 mutual-authentication clause covers the agent-to-service call. The inter-agent channel, its transport profile, and its message-level replay protection are graded in [[agentic-ai-security-cmm-d5-egress-network|D5]] L3, so an assessor scores the inter-agent leg once and in that domain.

Per-task holder-bound capability tokens leave L5 for L5+, the one structural move this page makes on the current D2. The current L5 implies they are deployable today, but the only implementation is an early-stage OSS primitive, and the platforms issue per-resource tokens, not per-task. This mirrors D6 moving cryptographic attestation up and entitlement enforcement down.

## Assessor detail per level

L1, L2, L3, L5, and L5+ are graded from their statements above. L4 carries criteria an assessor checks item by item.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable states that condition alongside the criterion. The lists below hold criteria only; a paragraph after a list carries maturity or market commentary and states no criterion.

### L4 detail

- **Zero credentials in agent context**, enforced by a credential broker or vault, or by a credential-less identity model.
- **Per-agent authorization at a PDP.**
- **A tested orphaned-agent kill switch.**
- **Task-bound sessions and tokens.** Authentication sessions and issued tokens are bound to both agent identity and the current task, so no session state or token scope carries across a task boundary, and agent-to-service authentication is mutual and cryptographic (mTLS or signed tokens) rather than a shared static key ([[owasp-ai-exchange|OWASP AI Exchange]]).[^aix-mac]
- **Delegated-credential construction.** A delegated credential is signed and names its delegator, its delegatee, the scope it permits, the task it was issued for, and a bounded expiry, and it links to the delegation it descends from, so a credential minted for one step of a chain fails validation when presented at another.[^aix-leastmodelpriv]
- **Automated rotation per credential class**, against a documented consumer-dependency map.
- **A behavioral baseline per NHI.**
- **An active migration plan off coupled credentials.**

## Right-sizing by deployment shape

| Deployment shape | Realistic D2 target | Why |
|---|---|---|
| Internal RAG / support chatbot (no/few tools) | L3 | Per-agent identity + owner + decoupled credentials. [[agentic-ai-security-cmm-recalibration-method-2026\|The persona]]'s bot sits here; per-task tokens and federation are irrelevant |
| Data-science / coding copilot | L3 → L4 | Touches secrets and the SDLC; the credential broker and rotation discipline earn their cost |
| MCP / skill provider serving others | L4 → selective L5 | Third-party blast radius raises the bar; scoped tokens per caller, attestation, shadow discovery |
| High-autonomy multi-agent mesh | L5 (selective L5+) | Delegation chains make per-task attenuating tokens (L5+) genuinely load-bearing — the one shape where Warrant-class authority is worth its immaturity cost |

> [!check] Attribution back to a human is the coding-shape D2 test
> Per-agent identity is necessary and not sufficient here. The assessable question is whether a given commit, tool call, or MCP invocation is traceable to both an agent identity *and* the human accountable for it. Most organizations cannot separate agent-authored from human-authored change once it lands in the repository, which removes the ability to scope a review policy, trace a defect to the tool that produced it, or measure which harness introduced what. No first-party harness feature supplies this across vendors; it currently requires a [[endor-labs-ai-code-governance|third-party control plane]] or in-house instrumentation. Because D2 caps effective D5 and D7, an unattributable coding fleet also caps those domains. See [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] §Fleet and parallel.

The [[lethal-trifecta|lethal-trifecta]] test lowers the target level. An agent with no private-data reach or no external-comms path does not need the per-task-token and federation tail. A sound L3 with the trifecta broken is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field.

## Cost model

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 | ~0.25 FTE: NHI inventory, owner fields | — |
| L3 | ~0 for an E5 incumbent (Entra Agent ID rides the directory; Conditional Access for agents needs Entra ID P1, included in E5)[^ca] | ~0.5 FTE: per-agent identity rollout, coupled/decoupled classification, pipeline binding | identity sign-in log volume |
| L4 | ~0 native (Managed Identities / Entra); credential-broker COTS only if not platform-native | the dominant cost: rotation automation + the coupled-credential migration project | — |
| L5 | ~0 native on the Agent 365 path; some Agent ID / governance features meter on Entra P2 or an add-on | ownership-transfer process, shadow-AI triage, attestation-chain upkeep | audit-log ingestion scales with agent count |
| L5+ | Tenuo OSS ~0 license | high: capability-token design, federation reconciliation, standards participation | verifier compute (negligible per call) |

Licensing is near-zero for the E5 incumbent through L5; the spend is the coupled-credential migration labor and audit-log run-rate (agent logs run roughly 10–20× human volume into the SIEM). "Free in E5" holds for the core but not universally: Conditional Access for agents needs P1, and some Agent ID governance features meter on P2 or an add-on.

## Customer critiques folded in

- *"L5 names a just-GA'd product we cannot procure in 12–18 months."* Addressed: L5 is capability-stated and the production-maturity qualifier applies. Entra Agent ID GA'd April 2026; a buyer satisfies the criterion via an approved-vendor-pipeline product with a documented production date.
- *"Microsoft is thin on per-task tokens."* Confirmed and bounded: the one genuine D2 residual gap is per-task holder-bound tokens, which sit at L5+, so most buyers never need them.
- *"Cost was invisible."* Addressed: the migration labor and SIEM run-rate are named; licensing is near-zero for incumbents.
- *"Zero-credential via Managed Identities is a cheap win."* Preserved: L4 reads "credential-less identity model or broker," so the Azure Managed Identities path counts without a separate credential-proxy purchase.

## Open questions

- Tenuo production-readiness (early-stage OSS, no independent enterprise-deployment evidence) is the variable that would move per-task tokens from L5+ to L5. Re-check quarterly.
- AWS AgentCore Identity and GCP Agent Identity are GA, but precise GA dates and the scope of GCP coverage outside its own agent runtime are not cleanly published.
- Okta for AI Agents GA timing (FY27) is directional; the CMM date is removed regardless.
- D2 maps cleanly to FFIEC/GLBA authentication-and-access expectations and NCUA third-party-NHI scrutiny; the mapping is deferred to the forthcoming FFIEC/GLBA crosswalk.
- Behavioral trust and reputation scoring for agent identities has no standardised method. The [[owasp-ai-exchange|OWASP AI Exchange]] names it as an agentic authentication element with decay and circuit breakers, states that industry scoring methods are not yet standardised, and admits it only as supplementary to identity, policy, and monitoring.[^aix-mac] The L4 per-NHI behavioral baseline is graded on existence for the same reason, and a scoring-quality criterion is unavailable until a method is published.

## Control anchors

[[agentic-ai-security-cmm-crosswalk|The crosswalk]] anchors `MODEL ACCESS CONTROL` to this domain for machine-to-machine and inter-agent calls, and records the qualification that a compromised principal authenticates correctly, so this domain's grade bounds who is calling rather than what the caller intends.

## Notes

[^entra]: [Microsoft Learn — Overview of agent identities](https://learn.microsoft.com/en-us/entra/agent-id/agent-identities), 2026. Entra Agent ID as a credential-less service principal; GA timing.
[^ca]: [Microsoft Learn — Conditional Access for Agent Identities](https://learn.microsoft.com/en-us/entra/identity/conditional-access/agent-id), 2026. Access patterns; per-resource (audience) tokens; Entra ID P1 requirement.
[^awsid]: [AWS — Understanding AgentCore workload identities](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/understanding-agent-identities.html), 2026. Workload identities, token vault, credential providers.
[^gcpid]: [Google Cloud — Agent Identity overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/agent-identity-overview), 2026. SPIFFE-ID agent identity + auth-manager credential vault.
[^iamra]: [AWS — IAM Roles Anywhere](https://docs.aws.amazon.com/rolesanywhere/latest/userguide/introduction.html), 2026. X.509 to short-lived STS, replacing long-term keys.
[^wif]: [Google Cloud — Workload Identity Federation](https://docs.cloud.google.com/iam/docs/workload-identity-federation), 2026. Federated identities replacing service-account keys.
[^okta]: [Okta — Newsroom: securing the AI-driven enterprise](https://www.okta.com/newsroom/press-releases/), 2026. Okta for AI Agents phasing (EA FY27 Q1 / GA FY27); Auth0 for AI Agents GA Oct 2025.
[^aix-mac]: [OWASP AI Exchange — MODEL ACCESS CONTROL](https://owaspai.org/go/modelaccesscontrol/), retrieved 2026-08-18.
[^aix-leastmodelpriv]: [OWASP AI Exchange — LEAST MODEL PRIVILEGE](https://owaspai.org/go/leastmodelprivilege/), retrieved 2026-08-19. Signed delegation tokens and their bound fields, the parent-linkage requirement against chain splicing, and the Limitations block's statement that a structurally valid token can be contextually unauthorised.

The dependency caps this domain imposes on D5 and D7 are grounded architecturally in [[agent-identity-architecture|Agent Identity Architecture]], which states the sequencing directly: a verifiable per-agent identity is the prerequisite for per-agent egress policy and per-agent behavioral baselining, so the identity layer is built first.
