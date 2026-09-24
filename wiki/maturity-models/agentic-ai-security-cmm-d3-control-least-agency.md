---
type: maturity-model
title: "CMM D3: Control and Least-Agency"
address: c-000138
created: 2026-05-25
updated: 2026-09-24
tags:
  - maturity-models
  - cmm
  - least-agency
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[decision-rights]]"
  - "[[least-agency-principle]]"
  - "[[emerging-cybersecurity-practices-for-agentic-ai-applications]]"
  - "[[owasp-ai-exchange]]"
  - "[[lethal-trifecta]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[guard-canonicalization-gap]]"
  - "[[securing-agentic-coding]]"
  - "[[cmm-known-limitations]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[offensive-agent-collective]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[agent-escape]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[vvah]]"
  - "[[deepsec]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[csa-maestro]]"
  - "[[wiki-novelty-and-counterarguments-2026]]"
  - "[[claude-cowork]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[least-agency-principle]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
verified: 2026-09-19
verified_against: []
verified_findings: 0
verified_note: "Verify-and-fix for the applied #229 research. Read the new FIDES control-landscape row, the rescoped L5+ parenthetical and the [^fides] footnote against the 2026-09-19 primary-source findings (Microsoft Learn Agent Security with FIDES, page dated 2026-09-16; the 2026-05-20 devblog; PyPI agent-framework-core upload times) and against recalibration rule 2; no .raw document opened. Quotation, version and date fidelity confirmed; the rule-2 wording departure preserves the verdict that the capability stays at L5+; the row is 184 characters and inside the cell budget."
---

# Agentic AI Security CMM — D3 Control & Least-Agency (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D3 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D3 is the decide plane: the [[oversight-layer|Policy Decision Point]] (PDP) outside the model context that authorizes agent actions. D4 is the runtime plane that enforces those decisions. The recalibration adds the platform-native PDP options that shipped since the domain was written and grades the leading-edge tier now that formal-verification tooling has reached open source.

> [!gap] Single-source grounding
> Levels and cost model synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation. Tooling status is a May 2026 snapshot.

## Threat coverage

D3 is the primary domain for **ASI02 (Tool Misuse)** and **ASI08 (Cascading Failures)**, and carries **Class 1 (insider, via dual control)** and **Class 3 (collusion, via tiered approval gates and monitor isolation)**. Its policy decision point caps runtime-guardrail effectiveness (D4): a guardrail can only enforce a decision the PDP makes. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

D3 is also the primary domain for the [[agent-escape|agent escape]] threat entry. The [[owasp-ai-exchange|OWASP AI Exchange]] places escape prevention at the infrastructure policy-enforcement layer, through capability-based access control that restricts tool sets, data sources, and action space independently of LLM reasoning, and pairs it with `OVERSIGHT` for session-level jailbreak drift.[^aix-escape] D4 enforces the decision the PDP returns, and [[agent-sandboxing|sandboxing]] bounds what a wrong decision can reach.

The Class 3 coverage claim needs a bound the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] supplies. Neither named control reached the collusion that occurred there.[^bhusa] Tiered approval gates were not bypassed, because no gate was triggered: writing a file to a package repository the workload is authorized to use is not an action class an approval tier covers. Monitor isolation held in the sense the control means it — the evaluation runs were mutually blind by construction — and the agents built a channel through a shared writable dependency anyway. **D3 grades what an agent may do, and both controls assume the coordination risk lives in the action set; in the attested case it lived in a medium two agents could both reach.** The complementary control is per-workload write scoping on shared caches and registries. No CMM domain grades it today: [[agentic-ai-security-cmm-d5-egress-network|D5]] lists it as ungraded and weighs the boundary against D4, and [[agentic-ai-security-cmm-d8-supply-chain|D8]] carries it without a measurement protocol. See Open questions below.

## Control landscape (dated)

| Capability | What ships today | Status (May 2026) | Platform-native (MS / AWS / GCP) |
|---|---|---|---|
| Policy language / PDP engine | Cedar (OSS, Apache-2.0); OPA / Rego (OSS, CNCF) | Stable; both sub-millisecond | — |
| Managed PDP service | AWS Amazon Verified Permissions (Cedar) | GA since 2023[^avp] | **AWS** |
| Agent-runtime PDP intercepting tool calls | AWS Bedrock AgentCore Policy (Cedar; permissive/audit mode; conditional auth); Google IAM Unified Access Policies (CEL; allow + deny per rule; DRY_RUN then ENFORCE) | AgentCore **GA Mar 2026**[^agentcore]; Google states no launch stage[^uap] | **AWS** + **GCP** |
| Agent-runtime PDP (OSS) | Microsoft Agent Governance Toolkit — Agent OS policy engine, YAML + OPA Rego + Cedar, sub-0.1 ms, ToolPolicy approval/justification/rate-limit guards | OSS, MIT, v3.7.0[^agt] | **MS** (bridges to Entra Agent ID) |
| Human-approval gates (HITL) | MS Copilot Studio multistage / AI approvals; AWS Bedrock Return-of-Control; LangGraph `interrupt()` | Copilot Studio approvals GA; computer-use supervision preview[^copilot] | **MS** + **AWS**; GCP thinner |
| Per-task capability tokens (cryptographic binding) | [[tenuo-warrant\|Tenuo Warrant]] (OSS; holder-bound, ephemeral, delegation-aware) | OSS primitive, no platform-native equivalent | None native |
| Privileged/quarantined LLM split (information-flow control) | Microsoft Agent Framework FIDES — integrity and confidentiality labels on content, most-restrictive-wins propagation, a policy check before each sink, a tool-free quarantined model | Experimental and Python-only, .NET stated as coming; ships in `agent-framework-core` from 1.3.0, uploaded 2026-05-08[^fides] | **MS** |
| Least-agency action tiering | Action-risk tiers (auto/notify/confirm/block) from [[emerging-cybersecurity-practices-for-agentic-ai-applications\|Emerging Practices]] §3.2; CSA progressive autonomy | Standards; stable | implemented via any PDP above |
| Agentic authorization framework | [[owasp-ai-exchange\|OWASP AI Exchange]] `LEAST MODEL PRIVILEGE` — the agentic authorization framework set out below the table | Standard; stable as of Aug 2026 | implemented via any PDP above |
| Formal policy verification | Cedar Analysis — SMT/Lean symbolic compiler (inconsistency, redundancy, permission-diff) | **OSS, shipping** (was framed research-only)[^cedaranalysis] | — |

Five capabilities survive the product churn: a PDP mediating every tool call outside the model context, least-agency tiering, per-task scoped authorization with binding, human-approval gates, and auto-downgrade on anomaly. The product names perish, and the tooling map holds them.

The Exchange's agentic authorization framework runs to seven parts, and the levels below grade four of them.[^aix-leastmodelpriv] Agents hold no permissions until granted, and each grant is scoped to a task, a time window, and a resource set, expiring when the task completes, times out, or is cancelled. Evaluation happens at API gateways, service meshes, or tool-execution proxies, through a dedicated policy decision point that returns permit or deny and withholds the policy logic behind it; that decision point and its enforcement point both sit outside the agent execution context, since an agent can reason around a policy expressed in a system prompt. The enumeration names three locations, and none is the runtime that hosts the model, so the L3 criterion below admits a placement the Exchange does not list. It reads the requirement as the two things the Exchange states around it: that authorisation must not live in prompts the agent can reason around, and that the control is an interceptor in the data path validating an action before it reaches a tool.[^aix-leastmodelpriv] An assessor taking the enumeration itself as the criterion reaches a different verdict on the same deployment, and the L3 detail below names the evidence that carries this one.

Each grant binds a context chain — human principal, verified agent identity, tool and operation, target resource classification — and re-authorisation is required when scope expands, a read elevates to a write, a trust domain is crossed, or work is delegated downstream. For dynamic task decomposition the Exchange prefers task-bound capability tokens, narrowable across delegation and subset-only when one agent delegates to another, with ABAC layered over them for time, data classification, input trust, and task stage.

The remaining three parts are graded elsewhere or not at all. Context-aware access control — infrastructure signals feeding graduated tiers, autonomous in low-risk context and gated in elevated-risk context — is the risk-adaptive step-up at L5 below. Agent identity verification, which the framework carries as unique cryptographic per-instance identity with mutual authentication and an active-agent registry, is graded at [[agentic-ai-security-cmm-d2-identity|D2]] and anchored there in [[agentic-ai-security-cmm-crosswalk|the crosswalk]]. Dynamic permission scoping — automatic narrowing the moment untrusted external content enters the flow — is graded at no level in this domain; it is implementation pattern 7 on [[least-agency-principle|least agency]], and [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] L4 records the runtime counterpart as ungraded for the same reason.

The [[microsoft-zt4ai|Microsoft ZT4AI]] least-privilege principle grounds the D3 deny-by-default level in its least-action design guidance ("start with no permitted actions by default"), crosswalked to D3 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]].

Two open-source vulnerability-discovery harnesses ship a per-agent tool allowlist as a product default. [[vvah|VVAH]] runs a tool sandbox with no Bash and executes no code, and [[deepsec|deepsec]] gives its agents read-only tools, per the LLM-generated capability matrix and per-tool summaries in Semgrep's July 2026 survey of nine such harnesses.[^semgrep] The same survey states what the narrowing removes: three of the tabled harnesses reason completely statically while two compile and execute binaries to validate a finding, which Semgrep calls a hybrid analysis much closer to how human vulnerability research looks. A tool set without execution cannot produce the finding class that requires execution. The level definitions below grade the narrowing and say nothing about the workload's output, which is where the consequence of the narrowing lands. [[oss-ai-vuln-discovery-harness-landscape|The open-source harness landscape]] carries the comparison, and [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] grades the sandbox the executing harnesses run in.

Cyera states its Protect phase blocks a risky tool call during execution; the release names no policy language, no deny-by-default configuration, and no risk tier behind that block, so the release adds no entry to the table above ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). Cyera states its Govern phase watches high autonomy as a risk signal, and the release names no permission-narrowing or capability-attenuation mechanism this domain's tooling map would carry.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2. A control implemented through a product in the organization's approved-vendor pipeline with a documented production date satisfies its criterion on that basis alone, and the production date is the date the control entered the organization's production. A planned date satisfies none, and neither does a product the organization does not yet run in production, whatever the vendor has announced.

- **L1 — Initial.** No tool-call policy; agents may call any tool.
- **L2 — Developing.** Per-agent tool allowlist; HITL on destructive actions defined informally.
- **L3 — Defined.** A PDP outside the model context mediates every tool call, deny-by-default, synchronously, and fails closed — no action proceeds until a permit or deny returns, and an unreachable policy engine denies. Behind it sit a documented risk tier for every action and a decision-rights matrix the PDP evaluates at call time. Shipping products meet this level, so it is graded off the PDP's deployed configuration rather than off the policy language named in it.
- **L4 — Managed.** The policy engine decides on state as well as on the action: an agent's current task scope, its session's cumulative activity, its position in a delegation chain, and a continuous lethal-trifecta reading all enter the decision, so a call to an individually authorized tool is denied when the task, the session total, or the chain says otherwise. Autonomy is promoted through documented stages, elevation is time-bounded, and proposer, approver, and deployer stay distinct.
- **L5 — Optimizing.** Authority is carried in cryptography rather than in configuration: approval tokens bind a human decision to the exact parameters approved, and the deploying key cannot approve its own deployment. Policy is compiled and reviewed every release with no drift, and D7 anomaly scores drive the tier up and down in production.
- **L5+ — Leading Edge.** Per-task capability tokens that bind a grant to one task and one holder and attenuate at every delegation hop (the tooling map above carries one early-stage open-source implementation and no platform-native equivalent); a CaMeL-style [[camel-pattern|privileged/quarantined LLM split]] for trifecta-positive workloads (the map above carries one vendor implementation, marked experimental by its vendor and enforcing in tool-call middleware rather than over a generated program, so rule 2 keeps the capability at this level); formal verification of policy contradictions / vacuity / shadow subsets wired over MCP to credential and tool-call PDPs at production scale; temporal-logic, trajectory-aware policy that decides on the executed action history, where the L4 criterion above decides on the declared current scope (open research — Cedar is stateless).

Four changes from the current D3. First, the platform-native PDP answers (AWS AgentCore Policy, Microsoft Agent Governance Toolkit) are named at L3/L4; the current text named only a generic "Cedar/OPA PDP." Second, the L5+ formal-verification line is reframed: Cedar Analysis now ships as OSS, so the analyzer itself is no longer leading-edge. What remains leading-edge is wiring it over MCP and the trajectory-aware (temporal-logic) extension. Third, L4 grades task-scope binding at invocation, which needs session or task state and therefore sits above the stateless action-class PDP L3 requires. The three levels above it stay distinct on mechanism: L4 checks a scope the agent's session declares, L5 binds a human approval into a token the agent cannot mint for itself, and L5+ binds the scope itself into a capability the agent cannot assert, then decides on the executed action history that a declared scope does not carry. Fourth, per-task holder-bound capability tokens leave L5 for L5+ under the cadence qualifier.

**Per-task holder-bound capability tokens move from L5 to L5+.** The tooling map above records no platform-native product for the capability, and the one implementation it carries is an early-stage open-source primitive ([[tenuo-warrant|Tenuo Warrant]]) whose August 2026 reading found no independent enterprise-deployment evidence. [[agentic-ai-security-cmm-recalibration-method-2026|Rule 2]] places such a capability in L5+ until a production-hardened implementation path exists, and it states the reading that satisfies a level criterion: a product in the organization's approved-vendor pipeline with a documented production date. An early-stage primitive with no recorded production deployment supplies neither the path nor the date, so the rule's test is the hardened path, where the earlier D3 reading tested whether a program could assemble the capability from available components. The earlier wording graded the capability at L5 and offered a regulated buyer the option of treating it as L5+, which inverted the rule, because rule 2 exists to keep the levels reachable for a buyer whose adoption cadence is externally constrained. That buyer's reading is now the graded one, and no program records a trade-off for declining a primitive with no platform-native counterpart in that map. [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] reads the qualifier the same way twice. A September canvass of twenty-one runtime-protection vendors found no implementation clearing the two-quarter window and moved no graded status, and a detection product that reached general availability inside the window carries no D4 L5 criterion until it is production-hardened. [[agentic-ai-security-cmm-d2-identity|D2]] already grades the capability at L5+ and [[agentic-ai-security-cmm-d5-egress-network|D5]] moves with this page, so one level answers for all three domains. The capability returns to L5 when a platform ships it, or when the open-source primitive carries production evidence from an organization that did not write it.

**A high D3 score asserts that actions are permitted, and asserts nothing about whether they are wise.** The Exchange states the bound on its own control: authorisation and policy enforcement define what is permitted, agents can cause harm entirely inside their granted scope, and prompt injection landing before a delegation is established or misuse of legitimately delegated authority both survive a correct policy engine.[^aix-leastmodelpriv] Two operational costs travel with the levels above. Per-action evaluation adds latency to every tool call, and the fail-closed property L3 requires converts a policy-engine outage into an agent outage — a deliberate availability trade the deployment records alongside its revocation-latency tolerance.[^aix-leastmodelpriv] Novel action sequences resist complete pre-specification, which leaves short token lifetimes, revocation, and delegation attenuation carrying the residue that policy authoring cannot.

## Assessor detail per level

L1 describes where a deployment starts and carries no criterion, so a deployment that does not meet every L2 criterion scores L1, or 0 under the measurement protocol's rubric where no evidence of the L1 baseline exists. L2 and L5+ are graded from their statements above. The three levels detailed below carry criteria an assessor checks item by item, each list stating what its own level adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a level is met only where every level below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names, whoever operates the control. The assessment records the evidence's assurance class beside the verdict and names the artifact behind it, its issuer and its date, per [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]]:

- **Tested**: the customer or the assessor exercised the control and recorded what it did.
- **Inspected**: vendor tooling the customer can reach, or a record the organization keeps, such as an inventory or a plan, shows the control's state in the deployment.
- **Attested**: the vendor states the control in a document the customer holds, and the document's own scope names the control.

**Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists, the customer can run no test and keeps no record of the control's state, and the vendor supplies neither inspectable output nor an attestation that names the control. An unanswerable criterion never counts as met: its level stays open, and every level above it stays open under cumulative grading. The assessment names what would close it. A criterion that can be not applicable states that condition alongside the criterion. Each level's list below holds criteria only, and a paragraph after a list states no criterion: it carries maturity, market or provenance commentary, or the boundary between this domain and another.

### L3 detail

- **A PDP outside the model context mediating every tool call**, in a deny-by-default policy language: Cedar, OPA/Rego, or the permission rule set of the runtime hosting the model, where that runtime holds the decision. The phrase names one property, that an instruction reaching the model's context cannot rewrite the decision the PDP issues, because a restriction expressed only in a system prompt is not enforced against an injected instruction.[^aix-testing] Where the PDP exposes an interface a tester can reach, that property is established by presenting a crafted invocation directly to the access-control or API gateway layer and observing the deny. Where the PDP is in-process and exposes no such interface, the direct-invocation test alone is recorded **not applicable**, and the criterion is graded from the deployed configuration: the policy as the decision point resolved it, the administrative scope it arrived from, and a deny observed under the most permissive autonomy mode the deployment permits. [[agentic-ai-security-cmm-measurement-protocol|The measurement protocol]] names the full substitute set, the fail-closed reading in the last bullet below, and the two limits that hold the substitution shut.
- **Least-agency action-risk tiering implemented.** The four action-risk tiers — auto, notify, confirm, block — are enforced by the PDP.
- **Each action's risk tier documented.** The tier assignment for every action the agent can invoke is recorded ahead of runtime.
- **A [[decision-rights|decision-rights]] matrix operationalized in the PDP** — action class × decision right × approver × justification × time bound.
- **A synchronous, fail-closed gate.** No action proceeds until a permit or deny returns, and an unreachable policy engine denies.[^aix-leastmodelpriv] Both properties are read off the PDP's deployed configuration, and a deployment whose agents proceed on policy-engine timeout has not met this level whatever its policy language. Where the policy engine runs in-process, the same reading is taken on its policy input, and an enforcement point that proceeds when its policy source is absent or malformed has not met the level either.

The tier scheme graded here is [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]] §3.2's operationalization of the OWASP ASI least-agency principle; ASI names the principle and supplies no tiers. The tier scheme sorts actions by risk and says nothing about who approves them. The human-involvement axis is the [[owasp-ai-exchange|OWASP AI Exchange]] oversight requirement set, graded at L4 through per-action HITL and at L5 through per-request approval tokens; [[least-agency-principle|Least Agency Principle]] carries both tables side by side.

AWS AgentCore Policy (GA), the Microsoft Agent Governance Toolkit (OSS), and Verified Permissions each meet this level platform-native, so no off-stack purchase stands between a buyer and L3.

### L4 detail

- **Four-stage progressive-autonomy promotion with documented criteria**, and per-action-class HITL coverage measured.
- **A continuous lethal-trifecta breaker** that auto-downgrades the tier on private-data + untrusted-content + external-comms, with the external-communications leg evaluated transitively across every permitted dependency rather than from the agent's own network policy.[^bhusa]
- **Time-bounded JIT elevation that auto-reverts.**
- **Segregation of duties** (proposer ≠ approver ≠ deployer).
- **Non-transferable sessions.** An authorization issued to one agent cannot be replayed by another and does not survive a delegation hop. Session non-transferability sits at L4 because it binds a session to an identity D2 already issues, and L5 adds the cryptographic capability-token infrastructure ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/agenticaioverview/`](https://owaspai.org/go/agenticaioverview/)).
- **Task-scope authorization at invocation.** Every tool invocation is authorized against the invoking agent's current task scope and role as well as against the tool's own annotation, so a call to an individually authorized tool made while the agent performs an out-of-scope task is denied and recorded as an [[agent-escape|agent escape]] event.[^aix-escape]
- **A session action ledger alongside the per-call decision**, so the policy engine takes cumulative session activity as a decision input and blocks a sequence of individually permitted steps that aggregates past a threshold — the salami-sliced pattern that passes every per-call check.[^aix-leastmodelpriv]
- **Full delegation-chain validation.** The policy engine validates the whole delegation chain, where the agents in the chain would otherwise each validate their own link, and enforces a configured maximum depth. Delegation is subset-only: a downstream agent holds at most the grants its delegator held, and cumulative privilege above the session's inception authorisation is blocked.[^aix-leastmodelpriv]

CSA's Agentic Trust Framework names five promotion-gate categories — Performance, Security Validation, Business Value, Incident Record, Governance Sign-off — but publishes no threshold or scored rubric behind any of them ([[csa-maestro|CSA MAESTRO / CSA Agentic Trust Framework]]), so the first bullet's "documented criteria" is a rubric the assessed organization writes, not one CSA ships. [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]] tracks this org-authored-rubric dependency as an open contest against the standard this level cites.

### L5 detail

- **Per-request approval tokens on the human-approval path**, each binding the approver's identity, the specific action parameters, and an expiry, with execution rejected where a parameter deviates from the approved value or the token has expired.[^aix-oversight]
- **Risk-adaptive step-up and step-down** driven by D7 anomaly scores.
- **A deny-by-default policy compiled and reviewed every release with no drift.**
- **SoD enforced cryptographically** — the deploying key cannot approve. The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] shows the cost of its absence: 84 of 85 cracked credentials pivoted through SSO into further systems with no additional authentication at the federation boundary.[^taiwan]

## Right-sizing by deployment shape

| Deployment shape | Realistic D3 target |
|---|---|
| Internal RAG / member-facing chatbot (few/no tools) | L2 → L3 |
| In-suite productivity assistant (Gemini for Workspace, Microsoft 365 Copilot) | L3 |
| Desktop-agent productivity assistant ([[claude-cowork\|Claude Cowork]] class) | L3 |
| Data-science / coding copilot | L3 → L4 |
| MCP / skill provider serving others | L4 → selective L5 |
| High-autonomy multi-agent mesh | L5 (+L5+ where resourced) |

**An allowlist, a PDP, and action-risk tiering are the whole of D3 for a chatbot with no tools.** With no external-communications reach the trifecta is broken by architecture, so per-task tokens, JIT elevation, and cryptographic SoD are controls for their own sake. [[agentic-ai-security-cmm-recalibration-method-2026|The persona]]'s bot sits here.

**The tool-call mediation this domain grades runs inside an in-suite productivity assistant's vendor.** What the customer holds is per-organizational-unit enablement and data-loss-prevention rules bounding which sources the assistant reaches, so the evidence is configuration that bounds reach rather than a decision point the customer operates. The decision point is graded on the evidence the vendors produce, and two criteria resolve **not met** on it. Microsoft documents a tool-chain analysis at the grounding stage that evaluates tool usage, validates tool capabilities and decides whether to block or allow a request, and records its output as a signal supporting task adherence rather than as an enforcement point the article places in the call path; the article names no policy language, states no coverage over every tool call and states no default-deny posture, so the mediation criterion is not met on attested evidence.[^msdefense] The synchronous fail-closed criterion is not met on inspected evidence, because the policy gate the customer configures over this shape is Microsoft Purview data-loss prevention, and the interaction audit record carries a `DLPEvaluationDeferred` bitmask marking the prompt, response, grounding or web-grounding stage whose evaluation could not be completed during the original request processing and was deferred for later reevaluation, with a companion reason field whose documented values include a timeout and an unavailable service.[^mscopilotaudit] On the Google side a matching Workspace data-protection rule stops Gemini using the source to generate its response and logs the event, and the page read states no deferral path.[^gdlpgemini] The action-risk tiers and the decision-rights matrix stay customer artifacts, because the agent registry in the administrative console enumerates the tools an agent can invoke at runtime and records no tier against those tools.[^msagentdetails] The row covers the in-suite variant alone.

**A desktop agent mediates its own local tool calls, and the vendor mediates its connector calls server side.** The two variants land on the same level on different evidence. [[claude-cowork|Claude Cowork]] lets an owner set a permission category per connector — always allow, needs approval, or blocked — which is this shape's action-risk tiering and an artifact the assessment can read; it pins permanent file deletion to an explicit approval; and it denies a permission prompt raised by a dispatched child task after ten minutes, with the task continuing without that action, so the gate denies on timeout rather than proceeding. The decision point over local files and the browser sits inside the desktop application, which is the condition the first L3 criterion states: the direct-invocation test alone is recorded not applicable, and the criterion is graded from the deployed configuration, which on a managed deployment is the device profile's allowed workspace folders, disabled built-in tools and managed MCP server list. [[agentic-ai-security-cmm-measurement-protocol|The measurement protocol]] carries the artifacts that grading collects. What is not met is the decision-rights matrix, because the approver is whoever holds the session, and the settings recorded there bind a capability to a role and not an action class to an approver. The row covers the desktop-agent variant alone.

**A copilot touches the SDLC, which makes segregation of duties load-bearing.** Proposer, approver, and deployer must be distinct agents, and JIT elevation stops a maintenance grant from becoming permanent.

**Scoping a grant at issue time bounds third-party blast radius.** An MCP or skill provider is consumed by callers it does not control, so its selective L5 target scopes each grant when it issues it rather than trusting the caller at call time. The holder-bound per-task form of that scoping sits a level above the row, at L5+.

**Delegation-aware capability tokens freeze blast radius at the top of the chain.** Of the four shapes above, only the mesh carries enough exposure for the L5+ tail — those tokens and the [[camel-pattern|CaMeL]] split — to earn its cost.

The [[lethal-trifecta|lethal-trifecta]] test is the primary instrument for lowering the required level: removing the sensitive-action or external-comms capability costs less than buying controls to govern it. Its design-constraint restatement, the [[agents-rule-of-two|Agents Rule of Two]], gives the assessor the same test in actionable form.

For the coding-copilot row, a text-matching command allowlist is not a policy decision point. The [[guard-canonicalization-gap|guard canonicalization gap]] describes why: the guard inspects a string the shell rewrites before executing, and the [[guardfall-shell-injection-audit|GuardFall audit]] found ten of eleven surveyed coding agents bypassable on exactly this construction. An organization scoring D3 on a Bash pattern allowlist has overstated by roughly a level. Ask whether the enforcement mechanism reads the same artifact the executor does; if not, grade it advisory. Control catalog: [[securing-agentic-coding|Securing Agentic Coding]].

Ask the cruder question first. An allowlist an autonomy flag suppresses is not a decision point at any level, and the assessment order is: confirm the guard is consulted under every autonomy mode the deployment permits, then ask what it matches. [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] is the case — Gemini CLI's `--yolo` ignored the fine-grained tool allowlist in `settings.json` outright before 0.39.1, so a workflow's enumerated permissions were evidence of nothing. The L2 capability above assumes a per-agent allowlist that runs; a deployment whose autonomy mode removes it has not met L2. Both checks apply to the CI-runner and unattended-local variants in [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]], where the autonomy modes that suppress these controls are the defaults rather than the exception.

**A managed permission policy the developer's session cannot widen is a decision point, and the harness holding it is where the decision is made.** The policy arrives from a scope outside the session, deployed by device management or delivered to the authenticated session, and the deny rules it sets hold under the autonomy modes the deployment permits ([[securing-agentic-coding|Securing Agentic Coding]]). The harness resolves that policy and returns permit or deny before the tool call runs, and the property the first L3 criterion tests is that an instruction reaching the model's context cannot write the scope the policy came from, which the deployed configuration has to show. Record the circularity in the same assessment. The enforcement point sits outside the model's context and inside the vendor's software, so one code path both runs the model and enforces a policy the customer administers, and the resolved policy, the decision and the record of it all come from that path. Injection resistance survives that, because the administrative scope stays beyond the model's reach. The independence of the evidence does not, and [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]] names the artifacts produced outside the harness that narrow it; [[cmm-known-limitations|CMM Known Limitations]] item 19 records the reading and the residue it leaves. The decision-rights criterion stays not met for this shape, because the approver is whoever holds the session.

## Cost model

Two paths reach the L3 policy decision point at `~0`: a Microsoft E5 incumbent's entitlements, and AWS's consumption-priced route, both platform-native. Google's path runs through IAM Unified Access Policies enforced at Agent Gateway — a feature for which Google states no launch stage and no support for VPC Service Controls[^uap] — so the licensing line stays near zero there too, but the cost surfaces in design instead: a deployment needing both an agent PDP and a service perimeter has to stand up each as its own mechanism. Every stack pays the same for policy-authoring and promotion-rubric labor regardless.

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 | ~0.1–0.25 FTE: allowlists, informal HITL | — |
| L3 | ~0 for an E5 incumbent (the Agent Governance Toolkit is MIT/OSS; Copilot Studio and Entra are in entitlements); AWS path is consumption-priced but small | the dominant cost: writing the decision-rights matrix and tier assignments per agent type, standing up the PDP, recurring policy review | negligible PDP eval cost (sub-millisecond) |
| L4 | ~0 incremental (MS) / small (AWS) | the dominant cost: the progressive-autonomy promotion rubric, HITL-coverage measurement, trifecta-breaker tuning, SoD pipeline separation | trifecta-breaker + JIT-elevation logging into the SIEM (couples to D7 ingest spend) |
| L5 | step-down leans on a preview product | per-release policy-compile / no-drift discipline; crypto-SoD key management | step-up / anomaly-signal logging |
| L5+ | OSS (Cedar Analysis; per-task capability tokens ~0 license) | research-grade integration labor (capability-token integration; formal verification over MCP; CaMeL pilot) | — |

D3 licensing is near-zero through L4 for an incumbent; the dominant cost is policy-authoring and promotion-rubric labor.

The L3 labor line assumes tool access already exists as configuration the PDP can read. Where tool access was historically expressed in prompts alone, capability enforcement has to be reconstructed before it can be graded, and the Exchange records that retrofit as a stated difficulty of the control.[^aix-escape] Budget an inventory pass over prompt-declared tool access ahead of the PDP standup.

## Customer critiques folded in

- *"D3 today is L1–L2; informal HITL on writes."* Addressed: the recalibrated L3 (PDP + action-risk tiers + decision-rights matrix) is the near-term target and is met platform-native — Copilot Studio approvals plus the Agent Governance Toolkit (OSS) for an all-Microsoft shop, with no off-stack purchase.
- *"L5 names just-GA'd products."* Addressed by capability-decoupling: L5 states an approval token bound to the parameters a human approved and a deploying key that cannot approve itself, neither of which names a product. Per-task scoped authorization with cryptographic binding is stated at L5+, where the cadence qualifier puts a capability with no production-hardened path in the tooling map above. The slow procurement cadence is the examiner-approved behavior.
- *"Microsoft is thin on per-task tokens and A2A authorization."* Confirmed and narrowed: the Agent Governance Toolkit's ToolPolicy adds declarative approval/justification guards but is not a holder-bound capability token. That is the genuine residual D3 gap, and it sits at L5+.
- *"Cost was invisible."* Addressed: licensing is near-zero through L4; the spend is decision-rights and promotion-rubric labor.

## Open questions

- Per-task holder-bound tokens sit at L5+ on the cadence qualifier. What would return them to L5 is a platform-native product, or production evidence for the open-source primitive from an organization that did not write it. The vault's reading of that primitive dates to August 2026 and records no general-availability declaration, no version tag and no production deployment; its reading of the company behind it dates to May 2026 and records the public examples as demo-shaped with case studies pending. Re-check quarterly, with [[agentic-ai-security-cmm-d2-identity|D2]] and [[agentic-ai-security-cmm-d5-egress-network|D5]].
- The Microsoft step-down control (Defender Predictive Shielding) is preview; the L5 step-down capability leans on it.
- Google ships a declarative decision-rights engine, IAM Unified Access Policies, evaluated at Agent Gateway by Identity-Aware Proxy, with CEL conditions, an allow effect and a deny effect on each rule, Principal Access Boundary on the agent identity, and a DRY_RUN mode ahead of ENFORCE. Google states no launch stage for it and states that it does not support VPC Service Controls,[^uap] so a deployment needing both an agent PDP and an egress perimeter covers each path separately. Whether this domain grades that composition failure or leaves it to [[agentic-ai-security-cmm-d5-egress-network|D5]] is an open calibration question. No Google approval-gate primitive is documented, so the HITL row above keeps its thin grade.
- No FFIEC/GLBA/NCUA mapping yet for SoD and approval-gate controls (model-risk-management expectations); deferred to the crosswalk.
- Approval tiers are defined over action classes, and the attested agent–agent collusion case ran entirely inside authorized action classes. Whether D3 should grade *who else can observe an action's side effects* (a property of the medium rather than of the action) or leave that to D4, D5, or D8 — none of which grades it today — is an open calibration question raised by the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]].
- A harness holding its own enforcement point produces every L3 artifact from the code path the policy governs, so the evidence carries no separation between the control and the report of it. [[agentic-ai-security-cmm-measurement-protocol|The measurement protocol]] narrows that with artifacts the harness does not write. Whether the assurance classes should carry a fourth class for a control the customer administers and the vendor interprets, or whether the named-artifact rule already covers it, is an open calibration question.

## D3→D4 dependency cap

The active rule set caps D4's effective score at D3's raw score (`effective(D4) ≤ raw(D3)`): runtime guardrails (D4 / PEP) can only enforce decisions a competent PDP (D3) actually makes. A program with strong D4 guardrails but no out-of-context PDP has its D4 effective score capped at the D3 level. For the persona (D4 raw L2–L3 from Prompt Shields and Groundedness, but D3 L1–L2), the cap pulls effective D4 down to roughly L1–L2. The cheapest high-leverage D4 investment is therefore standing up the PDP (D3-L3), which makes the already-owned guardrails load-bearing. See [[agentic-ai-security-cmm-dependency-rules|the dependency rules]].

## Notes

[^semgrep]: Semgrep, [Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses) (July 2026; no day-level date is exposed, and the month is inferred from an embedded screenshot dated 2026-07-20 and a forward reference to a Black Hat announcement in August 2026). The capability matrix and the per-tool detail sections are labelled by Semgrep as LLM-generated summaries of the repositories; the static-versus-dynamic finding is human-written. See [[semgrep-oss-ai-security-harness-comparison|the source summary]].
[^avp]: [AWS — Amazon Verified Permissions now generally available](https://aws.amazon.com/blogs/aws/simplify-how-you-manage-authorization-in-your-applications-with-amazon-verified-permissions-now-generally-available/), 2023. Cedar policy-as-a-service.
[^agentcore]: [AWS — Policy controls for Bedrock AgentCore generally available](https://aws.amazon.com/about-aws/whats-new/2026/03/policy-amazon-bedrock-agentcore-generally-available/), 2026. Cedar-based agent-runtime PDP; GA March 2026.
[^agt]: [Microsoft — Introducing the Agent Governance Toolkit](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/), 2026. Agent OS sub-millisecond PEP/PDP (OPA Rego / Cedar / YAML), MIT; v3.7.0 ToolPolicy guards.
[^copilot]: [Microsoft Learn — Advanced approvals in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/flows-advanced-approvals), 2026. Multistage / AI approvals; human-supervision for computer use in preview.
[^uap]: [Google Cloud — IAM Access policies overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap), fetched 2026-09-16. Unified Access Policies govern communication between agent principals and destination resources; Agent Gateway uses Identity-Aware Proxy to evaluate and enforce them. Each rule carries both an allow effect and a deny effect, conditions are written in Common Expression Language, Principal Access Boundary applies on the agent identity, and Google recommends configuring the gateway in `DRY_RUN` before `ENFORCE`. The page states no launch stage, and its opening note reads "This feature does not support VPC Service Controls"; `Agent Gateway` does not appear on the [VPC Service Controls supported-products list](https://docs.cloud.google.com/vpc-service-controls/docs/supported-products), fetched the same day.
[^cedaranalysis]: [AWS — Introducing Cedar Analysis open-source tools](https://aws.amazon.com/blogs/opensource/introducing-cedar-analysis-open-source-tools-for-verifying-authorization-policies/), 2026. SMT/Lean verification of authorization policies.
[^fides]: [Microsoft Learn — Agent Security with FIDES](https://learn.microsoft.com/en-us/agent-framework/agents/security), page dated 2026-09-16 and fetched 2026-09-19; announced at [Microsoft — FIDES in Agent Framework](https://devblogs.microsoft.com/agent-framework/fides/), 2026-05-20. FIDES implements the design of Costa et al. ([arXiv:2505.23643](https://arxiv.org/abs/2505.23643)) and enforces its labels in function-invocation middleware, checking each tool call against the current context label before the call runs. The same page states the limitations: labels are opt-in per data source, most-restrictive-wins propagation is conservative, approvals are coarse, the quarantined model is single-turn and tool-free, and an MCP server's labels are authoritative only where the operator opts into trusting them. Release upload times are read from the [PyPI release metadata for `agent-framework-core`](https://pypi.org/pypi/agent-framework-core/json), fetched 2026-09-19, which records 1.3.0 uploaded 2026-05-08.
[^bhusa]: Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06); summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
[^aix-escape]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18.
[^aix-leastmodelpriv]: [OWASP AI Exchange — LEAST MODEL PRIVILEGE](https://owaspai.org/go/leastmodelprivilege/), retrieved 2026-08-19.
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. Document 5's agentic-testing methodology, including the direction to test tool-call validation independently of the LLM by sending crafted invocations directly to the access-control or API gateway layer, and its statement that controls existing only in a system prompt are not enforced against injection.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19.
[^taiwan]: Dream Security, "[Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia)," 2026-08-12. See [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]].
[^msdefense]: [Microsoft Learn — Microsoft Copilot prompt defense in depth](https://learn.microsoft.com/en-us/microsoft-365/copilot/copilot-prompt-defense-in-depth), 2026-09-10, covering Microsoft Copilot across a Microsoft 365 tenant. The grounding-stage row reads "Microsoft's analysis includes evaluating tool usage, assessing the risk of a prompt injection attack, validating tool capabilities, and deciding whether to block or allow a request." The same row adds that "Tool chain analysis produces a signal that is used to support task adherence." The article names no policy language, states no coverage quantifier over tool calls, and places the analysis at no stated point in the call path.
[^mscopilotaudit]: [Microsoft Learn — Audit logs for Copilot and AI applications](https://learn.microsoft.com/en-us/purview/audit-copilot), 2026-08-26, covering the `CopilotInteraction` record written for every Microsoft 365 Copilot interaction in the tenant. `DLPEvaluationDeferred` is "an integer bitmask that indicates DLP evaluation of one or more content processing stages couldn't be completed, so it's deferred for later reevaluation", over the prompt, response, grounding and web-grounding stages; `DLPEvaluationDeferredReason` gives "Timeout, Authentication Error, Service Unavailable" as examples.
[^gdlpgemini]: [Google Workspace Admin Help — About DLP for Gemini](https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini), updated 2026-09-16, covering the Gemini app, Gemini in Workspace, Gemini Notebook, Personal Intelligence in Workspace and Workspace Studio for a Workspace tenant. The block action "stops Gemini from using the source to generate its response" and the event is logged; where rules conflict the stricter action prevails.
[^msagentdetails]: [Microsoft Learn — Understand agent details in Microsoft 365 admin center](https://learn.microsoft.com/en-us/microsoft-365/admin/manage/agent-details), 2026-08-05, covering the agent registry for a Microsoft 365 tenant. The **Data & tools** tab is read-only and states that "each tool listed represents an action the agent can invoke at runtime"; the tab carries capabilities, knowledge sources and tools, and no risk tier. Microsoft records that the tab is empty for some agents and that the admin center does not hold this metadata for all agent types.
