---
type: maturity-model-companion
title: "CMM: Measurement Protocol (Assessor's Handbook)"
address: c-000157
created: 2026-04-30
updated: 2026-09-29
tags:
  - maturity-models
  - measurement
  - assessment
  - audit-protocol
  - agentic-ai
  - 2026-proposal
status: developing
origin: produced
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[cmm-calibration-stress-test-2026]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[adversarial-reflexion]]"
  - "[[agentshield]]"
  - "[[guard-canonicalization-gap]]"
  - "[[guardfall-shell-injection-audit]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[standards-validation-methodology-2026-05]]"
  - "[[threat-modeling-for-ai]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[chain-of-thought-monitorability]]"
  - "[[claude-cowork]]"
  - "[[cmm-vocabulary-and-notation]]"
  - "[[ai-coding-agent-governance]]"
  - "[[securing-agentic-coding]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[iso-iec-42001]]"
  - "[[aiuc-1]]"
  - "[[aiuc-1-critical-evaluation]]"
  - "[[azure-rag-chatbot-security-profile]]"
sources:
  - "https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf"
  - "https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf"
  - "https://www.energy.gov/sites/default/files/2022-06/C2M2%20Version%202.1%20June%202022.pdf"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Targeted assessment-method and 309-criterion packet check against live NIST 800-53A, CSF and DOE C2M2; no archived source opened."
---

# Agentic AI Security CMM — Assessor's Handbook

This handbook assesses one defined agentic deployment against the five cumulative levels of [[agentic-ai-security-cmm-2026|the Agentic AI Security CMM]]. The nine domain pages below define the criteria. This page fixes how an assessor selects evidence, records a verdict, determines a level, and presents a fundable target profile. Its interview and artifact annex is an evidence index. A checklist item is not proof by itself: the assessor tests the pass, failure, and applicability conditions in the domain definition.

The method follows [NIST SP 800-53A Rev. 5](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf#page=22) in selecting assessment objectives, objects, and examine, interview, and test methods with stated depth and coverage. This CMM's criteria and levels are its own; an assessment under this handbook is not a NIST control assessment or a certification. The [NIST CSF 2.0 profile method](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf#page=11) informs the separate current and target profile used for investment decisions.

## Assessment unit and decision

The unit is a deployment: the production configuration of agents, models, tools, data paths, policies, identities, suppliers, and operators serving one purpose and risk decision. Replicas with the same effective configuration may be one agent; a configuration with different grants, tools, or trust paths is another. A shared organizational control can be examined once, then inherited only by deployments inside its documented scope. The report names the decision owner who can fund a target, accept a residual risk, or change the deployment.

The scope record identifies:

- the deployment and assessment date, business purpose, accountable owner, risk tier, and decision to be made;
- each agent configuration, environment, region, production status, autonomy mode, and path to data, tools, external parties, and other agents;
- deployment shape, such as a read-only assistant, tenant-wide productivity assistant, local or CI coding agent, RAG service, or multi-agent workflow;
- customer, internal supplier, and external supplier responsibilities for each material step;
- exclusions, their evidence, and whether an excluded route can still affect the assessed deployment.

An in-suite assistant is not excluded because the provider operates its model or decision point. Customer enablement, configuration, data grants, release decisions, and operating procedures remain assessable. An opaque provider-held step is recorded as unanswerable where the criterion applies. A tool-free agent has no tool-call instance for criteria whose definitions make tools the population. A deployment that has not entered production may be assessed for design and release readiness, but criteria that require production operation receive a recorded applicability or evidence verdict under their own wording. The assessor does not label an untested plan as an operating control.

### No-score intake

Before requesting the full criterion set, the architect and assessor make a short intake record. They identify the deployed configuration and business decision, trace its material data and action paths, and name the controls the customer operates and the supplier holds. They then select target domains and high-consequence paths for evidence planning. Intake assigns no maturity level or provisional verdict; any domain reported later still receives all applicable criteria through its claimed level. This stage helps the decision owner authorize assessment effort without treating a quick inventory as assurance.

## Assessment plan

Before requesting samples, freeze the assessment instrument by recording the core page revision, each domain page revision, this handbook revision, the assessment date, and the period each criterion reads. Record the intended reader of the report and the jurisdictional or assurance crosswalk, if any. Set a target level by domain from the deployment's autonomy, sensitive data, external reach, consequence of error, and the organization's capacity to operate the control. The target is a management decision; it does not change the criteria used for the observed level.

Build a population frame from the agent registry, identity provider, platform inventory, code hosts, supplier list, model and component inventory, data-source map, release history, and action and incident records. Reconcile disagreements before selecting samples. The frame includes:

- every distinct agent configuration and permitted autonomy mode;
- every model, tool, MCP server, execution path, corpus authorization layer, memory store, approval path, external destination, and inter-agent route;
- each supplier-held path and the evidence the customer can request or test;
- changes and events in the lookback periods named by applicable criteria.

For every criterion through the level the report will claim, and through any higher target level, plan the assessment object, method, depth, coverage, owner, safe test conditions, and evidence request. Record a higher level left unexamined as not evaluated; the observed level is the highest one verified, not a finding that higher capability is absent. [NIST SP 800-53A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf#page=21) distinguishes specifications, mechanisms, activities, and people as objects. A policy is a specification. A running gateway is a mechanism. A release or incident response is an activity. An operator is a person. Examine the object that could establish the criterion, interview its owner to clarify how it works, and test the deployed or production-equivalent route when the criterion asks for a refusal, failure mode, or observed effect.

Choose **depth** for the risk and uncertainty: basic checks that the object exists and is coherent, focused checks that its settings and outcomes match the criterion, or comprehensive checks of paths, failure cases, and operating history. Choose **coverage** separately: representative instances, representative instances plus important exceptions, or a sufficiently broad set across the population. Record the rationale. A high-impact write path, an opaque supplier step, and a path with prior failures warrant more scrutiny than a duplicated low-impact configuration. These method attributes describe assessor effort, not CMM levels.

A sample never changes a criterion that says every agent, every route, or each period. Establish the full population from configuration and inventory, then sample behavior across distinct shapes and include likely bypasses, recent changes, emergency paths, and suppliers. One counterexample defeats an all-population claim. If the frame itself is incomplete, record the affected criterion as unanswerable or not met according to what the evidence shows. Name the untested part and its possible effect on the result.

The plan names the assessment team and the competence required for the paths it will examine. At least one assessor must be able to read the deployed harness or platform configuration, identity and policy decisions, network routes, retrieval authorization, traces, and build or release records relevant to this deployment. Bring a specialist for a material path outside the team's competence. The assessor records conflicts of interest and obtains a second review for disputed judgments.

## Evidence method

An assessor uses three methods, selected for the determination rather than applied mechanically to every object:

| Method | Use in this assessment | Example |
|---|---|---|
| Examine | Inspect a specification, deployed configuration, record, mechanism output, or activity history. | Compare a policy revision with the running decision point and a denied call. |
| Interview | Ask the accountable person to explain the route, exception, and evidence location. | Have an operator walk through a guardrail timeout and its runbook. |
| Test | Exercise a mechanism or activity under stated conditions and compare the observed outcome with the required one. | Send a changed tool definition through the actual admission route and observe refusal. |

An interview answer locates evidence and can expose a missing path; it alone does not establish a control as met. A document can establish a documentary criterion. A live control needs its running configuration, decision record, scoped supplier evidence, or a valid test as its domain definition requires. Tests identify their expected result, actual result, tested revision, route, actor, date, and any deviation from production. A staging result counts only where the relevant policy, code, configuration, and route match production. A supplier assertion names the exact product, service, tenant or deployment class, control behavior, revision, and period it covers. A generic statement that the supplier is secure establishes none of those facets.

For each artifact, the evidence index records its issuer or owner, title and version, extraction date, system and tenant, period, immutable identifier or digest where available, and access location. A later assessor must be able to obtain the same object or understand why it expired. Mark evidence as customer observed, supplier inspected, supplier attested, or independently tested. These are provenance descriptions, not alternative scores. Reused common-control evidence must still match the deployment's version and boundary. [NIST SP 800-53A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf#page=37) likewise conditions reuse on credibility and applicability to current operating conditions.

Test safety is planned with the owner. Use a test agent, synthetic data, or a controlled production path where possible. A negative test must reach the real enforcement point. A mock of a gateway, vendor classifier, or approval queue proves only the mock. Record a test the provider prohibits as unavailable and seek scoped supplier evidence. Do not replace the missing result with a product brochure. A criterion that requires a production incident, two quarters of history, or a specific drill is graded on that record. This handbook imposes no universal live-action or continuous-operation gate on the other criteria.

## Criterion record and verdict

Create one record per criterion, per deployment or organization scope as the domain definition states. The record contains the criterion ID and level, applicable population, object and method, planned and actual depth and coverage, evidence IDs, observed pass and failure facets, verdict, reason, owner, and assessor. A shared artifact can appear in several records only when it establishes each distinct facet. A finding is linked to the record it affects, including its reproducing input and rate when variable model behavior matters.

| Verdict | Rule |
|---|---|
| **Met** | Evidence establishes every applicable pass facet across the stated population and period, including the required negative test or operating result. |
| **Not met** | Evidence shows at least one required facet failed, a route bypassed the control, a deadline was missed, or a necessary control is absent. |
| **Not applicable** | The criterion's stated activity, object, or topology is genuinely absent. Record the inventory or path evidence proving absence. Supplier ownership is not absence. |
| **Unanswerable** | The applicable instance exists, but the available customer test, record, and scoped supplier evidence cannot settle it. Name the missing evidence and responsible party. |

A missing requested document does not automatically make the domain L1. It affects the criteria that document could establish. Where a required record is known never to have been made, the relevant criterion is not met. Where a supplier-operated fact cannot be inspected, tested, or attested with scope, it is unanswerable. A verbal claim, an unsigned screenshot detached from a tenant, and a test on a different version do not turn that uncertainty into met.

For example, a tenant assistant uses a supplier-run injection filter. Its general documentation says that filters exist, but neither identifies the product route nor exposes a customer test result. The D4 criterion applies and is unanswerable. If a customer test through its actual route shows an attack delivered without the required screen, the criterion is not met. The same supplier can provide a scoped test report for that route, permitting a met verdict with confidence reflecting the evidence's limits.

## Domain level and confidence

Determine each domain independently. Start with L2 and proceed upward only while all applicable criteria at that level and every lower level are met. A not-applicable criterion is removed from that level's required set with its reason. If every criterion in a domain is not applicable because its governed objects are absent, report the domain **not applicable** outside the scale and show the absence evidence. A not-met or unanswerable criterion stops the cumulative claim at that point. A strong L5 artifact cannot compensate for an unproven L3 path. L1 is the described initial state when observations support it. If even the domain's state cannot be characterized, report the domain **unanswerable** outside the scale and list what is needed to make a determination. Do not turn absence of evidence into a fictitious lower level.

Report one **observed level** from L1 through L5 for each assessable domain, plus its **confidence** as a separate qualitative judgment. Identify higher levels not evaluated where the assessment stopped at its planned range:

| Confidence | Evidence basis |
|---|---|
| High | Population and versions are reconciled, material routes and exceptions were examined, required negative tests reached the deployed path, and records or independent results corroborate operating claims. |
| Moderate | Scope and required facets are established, but some supplier assertions, sampling limits, or short histories leave material uncertainty about generalization. |
| Low | The verdicts have a defensible minimum basis, but supplier opacity, narrow sampling, or weak record independence materially limits assurance. |

Confidence never converts an unanswerable criterion into met. A narrow but valid supplier attestation may support a met verdict and low confidence when its defined scope covers the criterion. A statement that omits a required facet cannot. Explain any low-confidence L4 or L5 claim next to its evidence limits. The target may exceed the observed level; that difference is the investment problem, not an instruction to inflate the observation.

There is no combined maturity number across domains or deployments. The current profile shows nine domain results for each deployment: an observed level and confidence where assessable, or a documented not-applicable or unanswerable result. Organization-wide criteria are inherited where applicable. A domain may reach L5 without unrelated domains first reaching L4. Evidence of sustained operation belongs only to the criteria that expressly require it. Do not produce a second effective level or an arithmetic dependency cap.

## Prerequisites and blockers

A failed criterion can expose a missing prerequisite in another domain. Record the dependency as a causal finding: the affected criterion and target, upstream criterion or artifact, owner, evidence, and remedy. For example, D7 attribution needs a resolvable agent identity from D2; an opaque identity route blocks that D7 determination. D5 task-bound egress needs D2 task identity and D3 task scope. D8 release approval needs D1 risk authority, while its assembled-system test must exercise relevant D4 controls. The missing upstream result is visible in its own criterion record and in the affected target plan. It does not alter an otherwise supported observed level by arithmetic.

A blocker may be an unanswerable supplier step, an absent authorization layer, an uncovered emergency route, a missing lookback period for a time-bound criterion, or an unfunded operating role. The report distinguishes a control defect from an evidence request and from a proposed design change. Record which party can resolve each. Risk-selected cooling-off for an irreversible action belongs to D3's high-risk policy, with a controlled emergency route and a D9 reconstructable approval record; a delay is not a universal D9 level condition.

## Evidence index

The table fixes the principal objects for each domain. The interview script tests material paths; the artifact checklist that follows gives every graded criterion an evidence packet at its assigned level. The domain page remains the authority for the full pass, failure, and not-applicable conditions.

| Domain | Principal assessment objects and selection |
|---|---|
| [[agentic-ai-security-cmm-d1-governance\|D1 governance]] | Approved authority, risk tier, register, gate, supplier allocation, shadow discovery, and board records; sample each deployment decision and the stated periods. |
| [[agentic-ai-security-cmm-d2-identity\|D2 identity]] | Agent principals, credentials, delegated tokens, owner changes, downstream actions, and revocation; sample each identity platform and delegation shape. |
| [[agentic-ai-security-cmm-d3-control-least-agency\|D3 control]] | Callable actions, tier and decision policy, every autonomy mode, approval path, task and delegation state; test direct and bypass routes. |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4 runtime]] | Each prompt, untrusted-content, response, code, tool, and guardrail path; include permissive modes and supplier-held screens. |
| [[agentic-ai-security-cmm-d5-egress-network\|D5 network]] | Allowed destinations and their reach, gateway and resolver routes, local and remote tools, peer traffic, and internal relays; test from the agent position. |
| [[agentic-ai-security-cmm-d6-data-rag\|D6 data]] | Every corpus authorization layer, copy, fine-tuning dataset, memory store, and retrieval route; sample two principals where grants differ. |
| [[agentic-ai-security-cmm-d7-observability\|D7 observability]] | Action traces, collectors, record stores, detection rules, evaluation runs, workflow joins, and triage; cover each agent and action type. |
| [[agentic-ai-security-cmm-d8-supply-chain\|D8 engineering]] | Component and supplier inventory, change, build, test, release, admission, and advisory records; bind each to the deployed version. |
| [[agentic-ai-security-cmm-d9-operations\|D9 operations]] | Guardrail and approval runbooks, queues, owner departures, incidents, drills, notices, and deputy actions; inspect each applicable path and period. |

### Interview script

The questions locate and challenge evidence. Each ends with the criteria it addresses. Ask the organization questions once when the criterion says *Organization*; ask deployment questions for each distinct assessed deployment. Follow an affirmative answer with the artifact row and the criterion's negative test. Criteria declared artifact only still receive a verdict after their row is examined.

**D1 Governance**

- Who currently holds the approved AI governance role, and where do the policy, risk-tier rule, and RACI state their scope and authority? (D1-ACCOUNTABLE, D1-POLICY, D1-SCHEME, D1-RACI)
- Which register entry covers each agent in this deployment, and which scored inputs produced its recorded risk tier? (D1-REGISTER, D1-REGISTER-TIER)
- Who could approve production at that tier, when did approval occur, and how was each condition met or changed before its deadline? (D1-GATE-RULE, D1-GATE, D1-GATE-CONDITION)
- Which source finds agents on endpoints, tenants, cloud accounts, and code hosts, and how did the last comparison and timed resolution handle each shadow agent? (D1-SHADOW, D1-SHADOW-REAP)
- How were the deployment's component threats allocated to each internal or external supplier, and who disposed of each residual risk? (D1-ALLOCATE, D1-ALLOCATE-RESIDUE)
- Which independent readiness and current assurance records name this deployment and the organization's governance program in scope? (D1-READINESS, D1-ASSURE)


**Artifact only:** D1-BODY, D1-BODY-CADENCE, D1-BODY-HISTORY, D1-BOUNDARY, D1-BOUNDARY-DATA, D1-CROSSWALK, D1-CROSSWALK-REFRESH, D1-DETAIL, D1-DETAIL-REVIEW, D1-DETAIL-RULE

**Artifact only:** D1-METRICS, D1-METRICS-ATTEST

**D2 Identity and Authorization**

- Which distinct non-human principal does each agent authenticate as, who can use its credential, and what inventory entry maps the principal back to that agent? (D2-IDENTITY, D2-INVENTORY)
- How does a downstream service verify a provider-issued agent assertion, and what does its own action record reveal about the agent and accountable human? (D2-IDENTITY-VERIFY, D2-TRACE)
- For a human-started task, which approved human workflow could cause each proposed business effect, and where does the downstream decision refuse an effect beyond that human's authority, including through a specialist? (D2-DELEGATE)
- Where does each delegation hop exchange a token naming both parties, and what fields bind a delegated credential to scope, task, and expiry? (D2-DELEGATE-EXCHANGE, D2-DELEGATE-TOKEN)
- Can the agent process read any stored credential from memory, environment, mount, or model context, and which broker or credential-less path replaces it? (D2-NOCRED)
- Show an end-to-end kill of one running agent on the production identity path: what stopped, what was revoked, and what continued for other agents? (D2-KILL)
- Does the registry answer which identity reaches which resource on every platform the deployment uses, and did the pipeline write sampled changes? (D2-REGISTRY)


**Artifact only:** D2-ADMIN, D2-AUDIT, D2-AUTHZ, D2-CONDITIONAL, D2-COUPLING, D2-COUPLING-MIGRATE, D2-COUPLING-ZERO, D2-DELEGATE-LINK, D2-DISCOVER, D2-IDENTITY-ATTEST

**Artifact only:** D2-LIFECYCLE, D2-MUTUAL, D2-OWNER, D2-OWNER-TRANSFER, D2-ROTATE, D2-ROTATE-MAP, D2-TASKBIND

**D3 Control and Least Agency**

- List every callable tool, command, destination, and autonomy mode; what does the external allowlist refuse in the most permissive mode? (D3-ALLOW)
- Trace a call from proposal to decision to execution under each tool route. Is the call held for the decision, and what happens without a matching rule? (D3-MEDIATE, D3-MEDIATE-SYNC, D3-DENY)
- Can a crafted call bypass the model and the policy? For an in-process decision point, show the resolved production policy, its protected source, a denial under the most permissive mode, and the runtime version. (D3-MEDIATE-OUTSIDE)
- What happens when the decision point fails or its policy is malformed, and does the executor receive exactly the action the policy evaluated after expansion or path resolution? (D3-MEDIATE-FAILCLOSED, D3-MEDIATE-EXACT)
- Which destructive and high-risk writes fall in each action tier, who approves a critical action, and what record fixes any risk-selected cooling-off or emergency exception? (D3-TIER-DESTRUCT, D3-HIGHRISK, D3-HIGHRISK-SOD)
- For an external write with an uncertain outcome, who fixes its request identifier, what holds a retry, and how are provider state and compensation resolved? (D3-WRITE-RECONCILE)
- Where is the task scope set outside the model, and which otherwise permitted call was refused for falling outside that scope? (D3-TASKSCOPE)
- What trusted session identity holds write history across a restart, and which individually permitted write was refused after crossing the session limit? (D3-LEDGER)
- For each security-consequential write sequence, show the temporal rule and an out-of-order or out-of-window refusal. (D3-LEDGER-SEQUENCE)
- Before a policy release, which decision tests detect unintended access, and how do owners resolve findings before promotion? (D3-POLICY-SEMANTIC)


**Artifact only:** D3-ADAPT, D3-APPROVE, D3-APPROVE-GATE, D3-APPROVE-TOKEN, D3-CHAIN, D3-CHAIN-DEPTH, D3-CHAIN-SUBSET, D3-ELEVATE, D3-NOREPLAY, D3-ORCHESTRATE

**Artifact only:** D3-POLICY-COMPILE, D3-POLICY-DRIFT, D3-POLICY-LOCK, D3-POLICY-REVIEW, D3-PROMOTE, D3-RIGHTS, D3-SOD, D3-SOD-CRYPTO, D3-TIER, D3-TIER-ENFORCE

**Artifact only:** D3-TRIFECTA

**D4 Runtime and Guardrails**

- Show every prompt and response path, including model content sent to tools. Where do the input filter and output classifier screen and block each? (D4-INPUT, D4-OUTPUT)
- Which routes bring untrusted content into the model, and where did a crafted attack through each route meet the inline classifier? (D4-INJECT-INDIRECT)
- Which obfuscation variants were tested on each applicable injection route, and how did syntactic matchers or semantic classifiers handle both the attack variants and a benign control? (D4-INJECT-CANON)
- What code, command, hook, and local server executes in the sandbox or a narrower enforced service boundary in each autonomy mode, and does confinement start before workspace configuration can execute? (D4-SANDBOX, D4-SANDBOX-FIRST)
- Which before-execution monitor compares a proposed tool call with the task and holds a misaligned call? (D4-ALIGN)
- For a high-impact call, which preview and deterministic impact limit run before execution, where each is applicable? (D4-VALIDATE-DRYRUN, D4-VALIDATE-IMPACT)
- Which bypass class did classifier tests miss, and where are its owner, mitigation or acceptance, and retest? (D4-INJECT-BYPASS, D4-INJECT-REMEDIATE)


**Artifact only:** D4-BUDGET, D4-CONTEXT, D4-GROUND, D4-INJECT-DIRECT, D4-INJECT-LANG, D4-INJECT-REFRESH, D4-OUTPUT-LEAK, D4-OUTPUT-SCOPE, D4-PLATFORM, D4-SANDBOX-CLEAN

**Artifact only:** D4-SANDBOX-CONFINE, D4-SANDBOX-CREDS, D4-SANDBOX-LIMITS, D4-SANDBOX-MAC, D4-SHARED

**D5 Egress and Network**

- From each agent position, which enforced destination list covers every connection and autonomy mode, and what can each listed destination reach? (D5-ALLOW, D5-REACH)
- Which model, tool, and MCP call routes pass an agent-aware gateway, and what rule rejected a call by an agent lacking that tool's grant? (D5-GATEWAY, D5-GATEWAY-AUTHZ)
- Which resolver answers the agent, and what blocks direct DNS, encrypted DNS, and an unlisted name or address? (D5-DNS, D5-DNS-ONLY)
- For remote HTTP MCP, what broker rejects the wrong issuer, resource, or caller? For local stdio MCP, what launcher rejects an unapproved executable or substituted command? Which changed tool definition was refused? (D5-MCP-BROKER, D5-MCP-PIN, D5-MCP-RUGPULL)
- Can agents exchange messages directly, or does the broker authenticate and validate each? Show an unauthenticated or malformed message refused. (D5-A2A-BROKER)
- From the agent workload, which direct, redirected, metadata, rebinding, and internal-relay paths were refused outside the allowlist? (D5-SSRF, D5-RELAY)
- Where each outbound decision binds the agent, current task, and approved destination, show separate refusals for a changed actor, destination, and closed task. (D5-TASK-EGRESS)
- If proxies span clouds, which approved revision is active at each proxy and which injected mismatch did reconciliation find? (D5-FEDERATE, D5-FEDERATE-RECONCILE)


**Artifact only:** D5-A2A-AUTH, D5-A2A-CHAIN, D5-A2A-REPLAY, D5-A2A-SCREEN, D5-A2A-SIGN, D5-A2A-TLS, D5-ALLOW-LOCK, D5-CEILING, D5-CEILING-TOKENS, D5-CEILING-TOOLS

**Artifact only:** D5-CONTRACT, D5-GATEWAY-EXCHANGE, D5-GATEWAY-ONLY, D5-GATEWAY-SCREEN, D5-INSPECT, D5-MCP-CVE, D5-MCP-POISON, D5-MCP-QUARANTINE, D5-ORCHESTRATE

**Artifact only:** D5-SEGMENT

**D6 Data, Memory and RAG**

- Which authorization grain does each corpus carry, and does the classification record cover each unit and copy at that grain? (D6-CLASSIFY)
- For each retrieval layer, do two asking principals with different source grants receive only their own permitted item before the model answers? (D6-ENTITLE)
- Which sources can extend the corpus, who reviewed each before first retrieval, and how does a returned item retain its source identity? (D6-EXTEND, D6-ORIGIN)
- Which fields and content units may reach each model route, and does a sampled outbound request exclude fields outside the recorded task scope? (D6-SCOPE)
- Which threat cases does the real ingest check address, what happens to held and alerted items, and when are existing items rescanned? (D6-SCAN, D6-SCAN-INJECT)
- Which memory partition can each agent read and write, and what prevents an entry with failed integrity from reaching later context? (D6-MEMORY-PARTITION, D6-MEMORY-WRITE, D6-MEMORY-VERIFY)
- Which production detection observes agent writes to memory or corpus, and where does its alert enter human triage? (D6-DETECT)
- What earlier state did each applicable store actually restore in a test, and what time did quarterly drills measure against the target? (D6-ROLLBACK, D6-ROLLBACK-DRILL)
- For an item returned from a corpus, which authenticated provenance and integrity check runs before context entry, and what tampered item was refused? (D6-ATTEST)


**Artifact only:** D6-CONTRADICT, D6-DETECT-CLASSES, D6-DETECT-PROTECT, D6-ENTITLE-INFER, D6-ENTITLE-TASK, D6-HOLDOUT, D6-HOLDOUT-TRANSFER, D6-LABEL-CARRY, D6-LABEL-GATE, D6-MEMORY-LOG

**Artifact only:** D6-MEMORY-PROVENANCE, D6-MEMORY-RESET, D6-OBFUSCATE, D6-OBFUSCATE-RESIDUAL, D6-OBFUSCATE-TABLES, D6-REACH, D6-REACH-CADENCE, D6-REACH-REMEDIATE, D6-SCAN-BOUND, D6-SCAN-DRIFT

**Artifact only:** D6-SCAN-TEST, D6-SCOPE-IDENTIFIERS, D6-SCOPE-MEASURE, D6-SCOPE-PROPAGATE, D6-STORE-ACCESS, D6-STORE-ENCRYPT, D6-STORE-RETAIN, D6-TRUST, D6-TRUST-WEIGHT

**D7 Observability and Detection**

- Can the organization search or export a record of every tool call and every answer, including answers from tool-using agents, with event time and accountable human? (D7-LOG)
- Does each agent action retain agent and human attribution, arguments or their protected representation, target, outcome, and a reference to any write? (D7-ATTRIBUTE, D7-LOG-SCHEMA)
- Which inference, tool, delegation, and retrieval spans reach a searchable backend, and what test catches a schema change that removes a needed field? (D7-SPANS, D7-SPANS-HELD, D7-SPANS-PIN)
- Which detector acts on per-agent tool departures, approval bypass, or relaxed control state, and where did its alert reach triage? (D7-BASELINE, D7-CONTROL-APPROVE, D7-CONTROL-RELAX)
- Which quarterly evaluation covered the selected threat categories, multi-turn attempts, and cross-session carryover on applicable paths? (D7-EVAL, D7-EVAL-TURNS, D7-EVAL-SESSIONS)
- For a real multi-agent propagation path, what deployed cascade rule and tested threshold raised an alert? (D7-CASCADE)
- Where agents share a tool or state, what joint baseline and running rule detected a workflow-relevant departure? (D7-JOINT)


**Artifact only:** D7-A2A-BASELINE, D7-ALERT-ACTIONABLE, D7-ALERT-LOOP, D7-DRIFT, D7-DRIFT-ROUTE, D7-ESCAPE-RECORD, D7-FORWARD-ESCAPE, D7-FORWARD-SCAN, D7-IDENTITY-BASELINE, D7-LOG-MEMORY

**Artifact only:** D7-LOG-OUTSIDE, D7-LOG-TAMPER, D7-ORCHESTRATE, D7-ORCHESTRATE-RECONCILE, D7-PLAYBOOK, D7-POSTURE, D7-PROMPT-CANARY, D7-RETAIN, D7-REVIEW-EVASION

**D8 Engineering and Supply Assurance**

- Which running models, harnesses, abilities, libraries, datasets, and configurations appear in the component inventory, with source and version? (D8-INVENTORY)
- Before this release, which changed trust boundary or untrusted input did design review treat, and where is the linked risk decision? (D8-DESIGN)
- Which first-party changes, agent-generated code included, passed human review and blocking analysis before the shipped revision? (D8-IMPLEMENT)
- Which assembled-system test crossed the material threat paths, with exact versions, repeated model outcomes, findings, and retests? (D8-TEST)
- Which security decision authorized the exact promoted version, and did manual, emergency, and vendor-console routes refuse a missing decision? (D8-RELEASE)
- For a coding fleet, what managed settings actually resolved on each runner, and which configuration-tree changes received review before taking effect? (D8-HARNESS, D8-HARNESS-REVIEW)
- Which supplier assessments cover the deployed component and, where the provider processes task data, its permitted use, retention, deletion, and subcontracted processing terms? (D8-SUPPLIER)
- Which installed component with a bad or missing approved digest was refused by a check outside the agent? (D8-VERIFY)
- Which externally published component has an affecting vulnerability, and where are its VEX statement and updateable consumer feed? (D8-VEX, D8-VEX-FEED)
- Which advisory missed its fix deadline, and what dated compensating control reduced exposure before that deadline? (D8-DISCLOSE, D8-DISCLOSE-CONTAIN)


**Artifact only:** D8-A2A-SIGN-TEST, D8-ABILITY-SCAN, D8-ABILITY-SOURCE, D8-AIBOM, D8-AIBOM-DRIFT, D8-AIBOM-RUNTIME, D8-BUILD, D8-BUILD-HARDENED, D8-DATASET, D8-DEPS-INSTALL

**Artifact only:** D8-DEPS-LOCK, D8-DEPS-SCAN, D8-DOCS, D8-DOCS-ACCESS, D8-HARNESS-EXTEND, D8-HARNESS-LOCK, D8-HARNESS-RESTORE, D8-INSTRUCT-BASELINE, D8-LINEAGE, D8-LOOP

**Artifact only:** D8-LOOP-SLA, D8-MODEL-CARD, D8-MODEL-INSPECT, D8-MODEL-PIN, D8-MODEL-PROBE, D8-MODEL-SCAN, D8-PROMPT-PROBE, D8-PROMPT-SECRETS, D8-REPO-WRITE, D8-RETIRE

**Artifact only:** D8-SIGN, D8-VERIFY-BUNDLE, D8-VERIFY-GATE, D8-VERIFY-INSTRUCT, D8-VERSION, D8-WEIGHTS

**D9 Operations and Human Factors**

- For each guardrail error or timeout, what does the on-duty operator do and what happens to the model call meanwhile? (D9-GUARD-RUNBOOK)
- Which test made each guardrail fail, did a required guardrail on the high-impact action path prevent that action on error or timeout, and what fallback was approved for other guardrails? (D9-GUARD-FAILMODE)
- When an owner leaves, who transfers or retires each agent and revokes or replaces the person's accessible credentials? (D9-OFFBOARD, D9-OFFBOARD-CREDS)
- What record does each human approval path write, and how are queue age, expiry, and approval rate reviewed? (D9-QUEUE-RUNBOOK, D9-QUEUE-AGE, D9-QUEUE-RATE)
- For a high-risk approval, do the stored request, shown parameters, authenticated approver, explicit confirmation, decision, and outcome resist alteration? (D9-HIGHRISK-RECORD)
- Which incident class did the AI playbook exercise, and can its operator stop this deployment and locate its records? (D9-IR, D9-IR-CONTAIN, D9-IR-EXERCISE)
- How does the user see an AI notice, and how were each transparency property and any omission checked against the deployed system? (D9-NOTICE, D9-NOTICE-PROPERTIES)
- Which adversarial test put urgency, fatigue, multi-step normalization, and confusion through each actual approval path? (D9-OVERSIGHT-TEST)
- When a deputy performed the operating duties without the role holder, which playbook steps failed and what action followed? (D9-ROLE-CONTINUITY)


**Artifact only:** D9-APPROVE-COVERAGE, D9-DRIFT-TRIAGE, D9-DRILL, D9-GUARD-COST, D9-GUARD-LATENCY, D9-IR-NOTIFY, D9-IR-SCENARIO, D9-LOOP, D9-MODEL-DEPRECATE, D9-OVERSIGHT-FLAG

**Artifact only:** D9-OVERSIGHT-INVOLVE, D9-OVERSIGHT-LIMIT, D9-QUEUE-P95, D9-QUEUE-STAMP, D9-QUEUE-THRESHOLD, D9-REAP, D9-ROLE, D9-ROLE-DEPUTY

### Artifact checklist

Each row below is one criterion's minimum evidence packet at its assigned level. Read it with that criterion's full definition. Link a general policy or supplier document to the deployment and period it covers; a supplier assertion used to establish operation must identify the relevant product route, tenant or deployment class, revision, and period. A required refusal or operating result cannot be inferred from a configuration alone.

| Domain | L2 artifacts | L3 artifacts | L4 artifacts | L5 artifacts |
|---|---|---|---|---|
| D1 | The document naming the role, with the personnel record of the person who holds it (D1-ACCOUNTABLE) |  |  |  |
| D1 | The policy with its approval and its scope statement, and the place it is published (D1-POLICY) |  |  |  |
| D1 | The RACI with its approval record, each responsibility matched to its row (D1-RACI) |  |  |  |
| D1 | The register entry, with the personnel record of the person it names (D1-REGISTER) |  |  |  |
| D1 | The entry's tier, with the record that scored the rule's inputs, such as the deployment's risk assessment (D1-REGISTER-TIER) |  |  |  |
| D1 | The scheme with its rule, and the policy or standard that applies it to AI systems (D1-SCHEME) |  |  |  |
| D1 |  | The matrix and the threat identification, set against the deployment's components and the agreements that make each supplying party one (D1-ALLOCATE) |  |  |
| D1 |  | The disposition of each residue, with the party who decided it and the rule that gives the decision (D1-ALLOCATE-RESIDUE) |  |  |
| D1 |  | The charter's membership, each member matched to a function (D1-BODY) |  |  |
| D1 |  | The charter's cadence, with the dates of the minutes over the period (D1-BODY-CADENCE) |  |  |
| D1 |  | The boundary record for each agent type, set against the deployment's agents (D1-BOUNDARY) |  |  |
| D1 |  | The data-handling statement for each agent type, set against the classes its agents read (D1-BOUNDARY-DATA) |  |  |
| D1 |  | The inventory entries with their classes, set against the deployment's design records and publications (D1-DETAIL) |  |  |
| D1 |  | The review record of each item (D1-DETAIL-REVIEW) |  |  |
| D1 |  | The publication rule, with the review it requires (D1-DETAIL-RULE) |  |  |
| D1 |  | The approval record with its approver and its date, set against the deployment's tier and the date it entered production (D1-GATE) |  |  |
| D1 |  | Each condition of the approval, with the record that met it or the approver's decision (D1-GATE-CONDITION) |  |  |
| D1 |  | The rule, with the approver it names for each tier (D1-GATE-RULE) |  |  |
| D1 |  | The discovery sources with their schedule and coverage, set against the organization's endpoints, tenants, cloud accounts and code hosts, and the latest comparison with the register (D1-SHADOW) |  |  |
| D1 |  | The rule that sets the time, and the findings of the period with the date each was registered or removed (D1-SHADOW-REAP) |  |  |
| D1 |  |  | The crosswalk's latest revision with its review rule, set against the AI policies and standards in force (D1-CROSSWALK) |  |
| D1 |  |  | The quarter's report, traceable finding identifiers, and minutes recording receipt (D1-METRICS) |  |
| D1 |  |  | The readiness report with its scheme, assessor, date, scope and gap list (D1-READINESS) |  |
| D1 |  |  |  | Current independent certificate or assessment of the in-scope governance program, its deployment scope, effective date, and continued validity (D1-ASSURE) |
| D1 |  |  |  | The minutes over the period, with each decision they record (D1-BODY-HISTORY) |
| D1 |  |  |  | The crosswalk's revision history over the four quarters (D1-CROSSWALK-REFRESH) |
| D1 |  |  |  | The attestation for each of the four quarters (D1-METRICS-ATTEST) |
| D2 | The agent's effective access for a sampled human set against that human's own access, and the component that enforces the bound (D2-DELEGATE) |  |  |  |
| D2 | The identity provider's record of each agent identity and, for each credential it holds, the principals able to use it (D2-IDENTITY) |  |  |  |
| D2 | The inventory export, reconciled against the identity provider's list of the deployment's identities (D2-INVENTORY) |  |  |  |
| D2 |  | The inventory's coupling field across the credentials the agents use and those a broker uses for them (D2-COUPLING) |  |  |
| D2 |  | The token-exchange configuration and exchanged tokens naming both parties (D2-DELEGATE-EXCHANGE) |  |  |
| D2 |  | The identity provider's configuration for the agent and signed assertions accepted by sampled services (D2-IDENTITY-VERIFY) |  |  |
| D2 |  | Pipeline definitions and the retirement record of an agent or a production-path test (D2-LIFECYCLE) |  |  |
| D2 |  | The owner field for each agent and identity, checked against the personnel record (D2-OWNER) |  |  |
| D2 |  | Actions re-traced from downstream records to the agent and human, with the code or configuration that supplies the human's identity (D2-TRACE) |  |  |
| D2 |  |  | The policy export with the identity each rule names, and a decision-log sample recording the calling agent (D2-AUTHZ) |  |
| D2 |  |  | The plan, set against the inventory's coupled entries and the decisions that reset a date (D2-COUPLING-MIGRATE) |  |
| D2 |  |  | A second-hop credential showing the field that names its parent (D2-DELEGATE-LINK) |  |
| D2 |  |  | A sample of delegated credentials showing each field (D2-DELEGATE-TOKEN) |  |
| D2 |  |  | The execution record, naming what the operation revoked and stopped and how long it took (D2-KILL) |  |
| D2 |  |  | The deployed channel and endpoint-authentication configuration for sampled services (D2-MUTUAL) |  |
| D2 |  |  | The agent's deployment specification with its environment and mounted secrets, and the broker or vault log or the credential-less identity configuration (D2-NOCRED) |  |
| D2 |  |  | The rotation schedule per class and the rotation log (D2-ROTATE) |  |
| D2 |  |  | The map, and for one recent rotation, the consumers it named and the record that each took the new value (D2-ROTATE-MAP) |  |
| D2 |  |  | Session or token scope, independent mediation policy and coverage where used, and refusals for wrong-task and post-task calls on every reachable credential path (D2-TASKBIND) |  |
| D2 |  |  |  | The role definitions and their assignments (D2-ADMIN) |
| D2 |  |  |  | An audit-log sample covering each event type (D2-AUDIT) |
| D2 |  |  |  | Issuance rule and a sampled agent credential or token-request record showing the changing agent-specific condition, its evaluation, and denial (D2-CONDITIONAL) |
| D2 |  |  |  | The inventory's coupling field and the coupled-credential migration report (D2-COUPLING-ZERO) |
| D2 |  |  |  | The discovery schedule and scope, and recent findings with their outcomes (D2-DISCOVER) |
| D2 |  |  |  | Registry export, pipeline write records, a sampled identity graph, and a platform-to-graph sample for each platform in use (D2-REGISTRY) |
| D2 |  |  |  | The issuer's verification rule and attestation chain behind a sampled credential (D2-IDENTITY-ATTEST) |
| D2 |  |  |  | The personnel trigger and transfer records for sampled owners who left or changed roles (D2-OWNER-TRANSFER) |
| D3 | Each agent's allowlist as the enforcing component reads it at the production version, with the autonomy modes the deployment permits and a call outside the list refused under the most permissive of them (D3-ALLOW) |  |  |  |
| D3 | The approval step or the refusal governing each destructive action each agent can invoke, as configured in production, and one record of each step operating (D3-APPROVE) |  |  |  |
| D3 |  | The gate's configuration with the approval records for a sample of confirm-tier actions, and a gate fire observed live or on the record (D3-APPROVE-GATE) |  |  |
| D3 |  | The policy's default and its handling of an unmatched call, at the running revision (D3-DENY) |  |  |
| D3 |  | The inventory of each agent's tools reconciled against the decision point's configuration and its decision log, showing the calls of each tool reaching it (D3-MEDIATE) |  |  |
| D3 |  | The rules compared with what the executor receives, and a compound or expanded command refused (D3-MEDIATE-EXACT) |  |  |
| D3 |  | The configured failure behaviour with the record of a test that exercised it, or for an in-process decision point the behaviour the vendor documents for the running version when the policy source is absent or malformed (D3-MEDIATE-FAILCLOSED) |  |  |
| D3 |  | The direct-invocation test and decision-log entry, or for an in-process decision point the resolved production policy, protected source scope, runtime version, and denial under the most permissive mode (D3-MEDIATE-OUTSIDE) |  |  |
| D3 |  | The enforcement code or configuration for each tool, showing the call held until the decision returns, and a trace in which the decision precedes the call (D3-MEDIATE-SYNC) |  |  |
| D3 |  | Each scope the running policy takes rules from, with the principals able to change each (D3-POLICY-LOCK) |  |  |
| D3 |  | The matrix with the rule or workflow step that applies each row, and one approval routed to the approver its row names (D3-RIGHTS) |  |  |
| D3 |  | The dated tier record for each agent, covering each action it can invoke (D3-TIER) |  |  |
| D3 |  | The destructive-action classification, which lists each agent's destructive actions with the tier each carries (D3-TIER-DESTRUCT) |  |  |
| D3 |  | The policy rules for each tier compared with the tier record, and a decision-log entry for each tier the record uses (D3-TIER-ENFORCE) |  |  |
| D3 |  | Category and routing rules with sampled decisions and any recorded emergency exception (D3-HIGHRISK) |  |  |
| D3 |  | Routing policy and a sample of critical-action approvals (D3-HIGHRISK-SOD) |  |  |
| D3 |  | Task-and-action-bound request identifier and outcome records, provider-state lookup or reconciliation procedure, named compensation owner, fresh-ID duplicate refusal, and held timeout retry (D3-WRITE-RECONCILE) |  |  |
| D3 |  |  | The chain-validation rule, and a splice test in which a credential from one step was presented at another and refused (D3-CHAIN) |  |
| D3 |  |  | The configured depth, with a delegation refused past it or the grants showing no delegation tool at the last permitted depth (D3-CHAIN-DEPTH) |  |
| D3 |  |  | Approved task authorization fixed outside the model context, each delegation hop's effective scope, and denial of a specialist's wider standing grant outside that task scope (D3-CHAIN-SUBSET) |  |
| D3 |  |  | The elevation mechanism's configuration, and the expiry log for a sample of elevations (D3-ELEVATE) |  |
| D3 |  |  | The per-session limit for each write class, and a call the ledger denied (D3-LEDGER) |  |
| D3 |  |  | The record of a replay test in which another agent and a delegatee each presented the authorization and were refused (D3-NOREPLAY) |  |
| D3 |  |  | The orchestrator's grants compared with the tier record (D3-ORCHESTRATE) |  |
| D3 |  |  | The promotion rubric with each agent's current stage, and the record of each promotion in the period (D3-PROMOTE) |  |
| D3 |  |  | The pipeline's role assignments and protections, and a sample of agent-proposed changes showing three distinct principals (D3-SOD) |  |
| D3 |  |  | The policy input carrying the task scope and role, and a denied out-of-scope call (D3-TASKSCOPE) |  |
| D3 |  |  | The breaker's configuration with its transitive reading of the external leg, and a downgrade it recorded (D3-TRIFECTA) |  |
| D3 |  |  |  | The signal input and policy rule, with test or production records of tier increase, grant withdrawal, and recovery (D3-ADAPT) |
| D3 |  |  |  | An approval-token sample showing the three bound fields, and an execution refused for a deviating parameter (D3-APPROVE-TOKEN) |
| D3 |  |  |  | The path analysis, running temporal rules, trustworthy session-binding configuration, and a denied out-of-order or out-of-window write where applicable (D3-LEDGER-SEQUENCE) |
| D3 |  |  |  | Validation output and release gate records for each policy release in the period (D3-POLICY-COMPILE) |
| D3 |  |  |  | The drift check's configuration and its results over the period, with each alert and its resolution (D3-POLICY-DRIFT) |
| D3 |  |  |  | The decision tests or analysis report, finding dispositions, and protected release-gate records for each release (D3-POLICY-SEMANTIC) |
| D3 |  |  |  | The review record for each release in the period (D3-POLICY-REVIEW) |
| D3 |  |  |  | The key assignment per role, and a deployment refused because the deploying key signed its approval (D3-SOD-CRYPTO) |
| D4 | The filter's configuration at the running version, with the setting or code that applies it to each of the agent's model calls, or for a filter the vendor runs, the vendor document naming it or a record of its result (D4-INPUT) |  |  |  |
| D4 | The classifier's configuration, or the vendor document naming it, showing replies and tool-call content in its scope, or a test in which it blocked each (D4-OUTPUT) |  |  |  |
| D4 |  | The route inventory, matcher normalization or detection configuration where applicable, and injection-variant tests with a refused attack and benign control (D4-INJECT-CANON) |  |  |
| D4 |  | The classifier's configuration and its place on the request path, with a direct attack it blocked, or for a classifier the vendor runs, the vendor document naming it for prompts or a record of a detection (D4-INJECT-DIRECT) |  |  |
| D4 |  | The list of routes that carry untrusted content to each agent's model, and for each route a test record of an attack presented through it or a detection the classifier recorded on content from it (D4-INJECT-INDIRECT) |  |  |
| D4 |  | The dated scope record, set against the classifier's configuration or the vendor's statement (D4-OUTPUT-SCOPE) |  |  |
| D4 |  | Process and tool map against each isolation profile and permitted mode, with a refusal and retry test showing agent-directed effects cannot escape confinement (D4-SANDBOX) |  |  |
| D4 |  | The platform's lifecycle configuration, with its documented or observed teardown on completion and on a forced stop (D4-SANDBOX-CLEAN) |  |  |
| D4 |  | The sandbox's profile, with a staging demonstration of each refusal (D4-SANDBOX-CONFINE) |  |  |
| D4 |  | The environment and the mounts of a sandboxed process, showing no credential, with the store or the proxy that holds each (D4-SANDBOX-CREDS) |  |  |
| D4 |  | The vendor's statement of the order in which the harness reads workspace configuration and starts the sandbox, or the test record (D4-SANDBOX-FIRST) |  |  |
| D4 |  | The limit configuration for each sandbox, with the component that enforces each limit (D4-SANDBOX-LIMITS) |  |  |
| D4 |  | The profile and the capability set the platform reports for the processes of a running sandbox (D4-SANDBOX-MAC) |  |  |
| D4 |  |  | The monitor's configuration and its place before tool execution, with a call it held or blocked, on a production run or in a test (D4-ALIGN) |  |
| D4 |  |  | Risk-state trigger, outside-model task rule, refusal of an injection-origin side effect or restricted-data egress call, and an approved-call control test (D4-CONTEXT) |  |
| D4 |  |  | The check's configuration with its threshold, and the scores for a sample of answers (D4-GROUND) |  |
| D4 |  |  | Dry-run records for a sample of high-impact calls, each with the proposed change and the call it preceded (D4-VALIDATE-DRYRUN) |  |
| D4 |  |  | The rules with their per-call limits, and a call they refused or held (D4-VALIDATE-IMPACT) |  |
| D4 |  |  |  | The budget configuration for each guardrail, with a breach record showing the enforcement (D4-BUDGET) |
| D4 |  |  |  | The evaluation log by bypass class, with the library's revision and date (D4-INJECT-BYPASS) |
| D4 |  |  |  | The evaluation log by language, with each language's test set and date (D4-INJECT-LANG) |
| D4 |  |  |  | The refresh receipts for each classifier over the period, with the stated cadence (D4-INJECT-REFRESH) |
| D4 |  |  |  | Class-level results, owner, treatment and retest records, and closure or authorized residual-risk disposition (D4-INJECT-REMEDIATE) |
| D4 |  |  |  | The scanner's configuration on the output path, with the test results per encoded form (D4-OUTPUT-LEAK) |
| D4 |  |  |  | The coverage report of each L4 control across the agents in scope, with zero opt-outs, and the setting that stops an owner turning each off (D4-PLATFORM) |
| D4 |  |  |  | The shared-service register, set against each agent's configuration, with the decision on each service (D4-SHARED) |
| D5 | Each agent's allowlist as the enforcing component applies it at the production version, set against the parts of the agent's egress, with a connection to an unlisted destination refused (D5-ALLOW) |  |  |  |
| D5 | The reach record, set against each agent's deployed allowlist and the configuration of each destination the organization operates or configures (D5-REACH) |  |  |  |
| D5 |  | The authentication configuration of each endpoint that sends or receives inter-agent traffic, and an unauthenticated message refused (D5-A2A-AUTH) |  |  |
| D5 |  | A delegated message sample showing the reference to the delegating agent and to the accountable human (D5-A2A-CHAIN) |  |  |
| D5 |  | The enforcement profile's replay rule, and a replayed message refused (D5-A2A-REPLAY) |  |  |
| D5 |  | The TLS policy of each receiving endpoint, and a handshake offering an earlier version refused (D5-A2A-TLS) |  |  |
| D5 |  | The source of each agent's allowlist, with the principals able to change it (D5-ALLOW-LOCK) |  |  |
| D5 |  | The ceiling policy with its key and window, and a call it refused or held (D5-CEILING) |  |  |
| D5 |  | The gateway's token policy per agent, and a model call it refused past the ceiling (D5-CEILING-TOKENS) |  |  |
| D5 |  | The invocation ceiling per agent with its key, and an invocation it refused (D5-CEILING-TOOLS) |  |  |
| D5 |  | The resolver's policy for the agent's lookups in allow form, and a lookup outside the list refused (D5-DNS) |  |  |
| D5 |  | The network policy on DNS traffic from each agent's workload, and a query to another nameserver refused (D5-DNS-ONLY) |  |  |
| D5 |  | The route each class of call takes, such as the endpoint, proxy or network setting that directs it, set against the gateway's configuration and its log of each class (D5-GATEWAY) |  |  |
| D5 |  | The gateway's policy for each tool, with the agent each rule names, and a refused call in its log (D5-GATEWAY-AUTHZ) |  |  |
| D5 |  | The gateway's content policy with its classes and directions, and a request or response it blocked (D5-GATEWAY-SCREEN) |  |  |
| D5 |  | Remote HTTP broker policy and refusal for an invalid issuer, audience, or caller, or local stdio launch policy, scoped environment, and denied command substitution (D5-MCP-BROKER) |  |  |
| D5 |  | The fingerprint registry, and the comparison record of a connection (D5-MCP-PIN) |  |  |
| D5 |  |  | The network policy between agents, showing no direct path, with the broker's authentication and validation rules (D5-A2A-BROKER) |  |
| D5 |  |  | The screen's configuration on the inter-agent path, and a message it blocked (D5-A2A-SCREEN) |  |
| D5 |  |  | The pinned contract for each supplied service, with the validation rule and a response it rejected (D5-CONTRACT) |  |
| D5 |  |  | The gateway's token-exchange logs, showing a token issued for each call's tool, and a token refused by a tool it was not issued for (D5-GATEWAY-EXCHANGE) |  |
| D5 |  |  | Effective-origin enforcement design and configuration, with a fronted request to an unapproved origin refused (D5-INSPECT) |  |
| D5 |  |  | MCP version inventory, applicable advisories and findings, disclosure coverage gaps, matching result, and response ticket (D5-MCP-CVE) |  |
| D5 |  |  | The screening rule set, and a definition it flagged or a test of it (D5-MCP-POISON) |  |
| D5 |  |  | The gateway's refusal rule, and a changed definition it refused (D5-MCP-RUGPULL) |  |
| D5 |  |  | Orchestrator route and policy, with a direct sensitive write, protected-data, or external call refused when the delegated path would deny it (D5-ORCHESTRATE) |  |
| D5 |  |  | The segmentation policy, set against the list of sensitive internal services (D5-SEGMENT) |  |
| D5 |  |  |  | Signed Agent Cards, trusted publisher keys, and refusal of an unsigned, altered, or untrusted card (D5-A2A-SIGN) |
| D5 |  |  |  | Approved bundle and distribution configuration, each proxy's active revision, and a cross-proxy decision test (D5-FEDERATE) |
| D5 |  |  |  | Each proxy's decision log and active revision, reconciliation report, and an injected mismatch test (D5-FEDERATE-RECONCILE) |
| D5 |  |  |  | The zero-bypass proof, the routes and network policy around each agent's workload with a direct connection refused (D5-GATEWAY-ONLY) |
| D5 |  |  |  | Urgent-finding classification and admission rules, denied MCP call or launch after a seeded finding, and authorized restoration (D5-MCP-QUARANTINE) |
| D5 |  |  |  | Agent-controllable relay inventory, downstream egress policy, and refusal of an unapproved host through a reachable internal service (D5-RELAY) |
| D5 |  |  |  | The SSRF closure verification, a test from the agent's position of each route, each refused (D5-SSRF) |
| D5 |  |  |  | Bound decision inputs and separate refusal tests for actor, destination, and closed task. Supplier-held paths require scoped supplier evidence or an unanswerable verdict (D5-TASK-EGRESS) |
| D6 | The classification record, set against the list of each corpus's units, the systems the agents' tools read, the stores and the memory stores (D6-CLASSIFY) |  |  |  |
| D6 | The review record of each extension, set against the list of the corpus's sources with the date each first served a retrieval (D6-EXTEND) |  |  |  |
| D6 | A sample of retrievals from a log, a trace or the answers' citations, each item naming its origin (D6-ORIGIN) |  |  |  |
| D6 | The assessment report, with its scope set against the list of each corpus's locations and its findings (D6-REACH) |  |  |  |
| D6 |  | The authorization layer named for each corpus, with a two-principal test record for each layer, or for a layer the vendor operates, the vendor document stating the enforcement (D6-ENTITLE) |  |  |
| D6 |  | The scope the run's identity holds, set against the corpora its task reads, with the setting that binds the scope to the task (D6-ENTITLE-TASK) |  |  |
| D6 |  | The validation corpus's storage and access policy, set against those of the training data, the model artifacts and the agent's repository (D6-HOLDOUT) |  |  |
| D6 |  | The register of the corpus's copies outside the organization's stores, each with its transfer record (D6-HOLDOUT-TRANSFER) |  |  |
| D6 |  | The remediation record, each finding of the assessment matched to its grant change, removal or recorded exception and its date (D6-REACH-REMEDIATE) |  |  |
| D6 |  | Threat model, configured treatment, new-item and scheduled-rescan records, and an exercised hold or alert (D6-SCAN) |  |  |
| D6 |  | The scan's configuration on the ingest path, with a test record of crafted content presented through that path or a detection the scan recorded on ingested content (D6-SCAN-INJECT) |  |  |
| D6 |  | Route-matched planted and benign sets, treatment decisions, poison detection, and false-hold or alert rates (D6-SCAN-TEST) |  |  |
| D6 |  | Scope decisions for corpora, datasets, and model routes, compared with copies and sampled outbound model payloads (D6-SCOPE) |  |  |
| D6 |  | The retained-identifier exception list, set against the fields the training run excludes (D6-SCOPE-IDENTIFIERS) |  |  |
| D6 |  | Each store's access policy, effective reader identities, and each identity's documented operational role and grant scope (D6-STORE-ACCESS) |  |  |
| D6 |  | Each store's encryption configuration (D6-STORE-ENCRYPT) |  |  |
| D6 |  | The stated period and the deletion or rebuild job, with a deleted source item's copy found absent after the period (D6-STORE-RETAIN) |  |  |
| D6 |  | The trust scale, the code or configuration that attaches the level, and a sample of retrieved items carrying it (D6-TRUST) |  |  |
| D6 |  |  | The detection rule over memory and corpus writes, with an alert it raised or a test of it (D6-DETECT) |  |
| D6 |  |  | The coverage record, with each class's planted set and the share flagged (D6-DETECT-CLASSES) |  |
| D6 |  |  | The access policy over each detection's configuration and baselines, set against the identities that write the pipeline and the stores, with the integrity check's record (D6-DETECT-PROTECT) |  |
| D6 |  |  | A sample of answers and created items, each set against the classes of the items it drew on (D6-LABEL-CARRY) |  |
| D6 |  |  | The policy, with an answer it gated or a test of it (D6-LABEL-GATE) |  |
| D6 |  |  | The change log's configuration with its immutability setting, set against the identities able to write it, and a sample of its entries (D6-MEMORY-LOG) |  |
| D6 |  |  | The partition key and the read check for each store, with a read refused across partitions (D6-MEMORY-PARTITION) |  |
| D6 |  |  | A sample of each store's entries, read field by field (D6-MEMORY-PROVENANCE) |  |
| D6 |  |  | Between-task carryover rule and a test showing unauthorized content reset while permitted memory remains (D6-MEMORY-RESET) |  |
| D6 |  |  | The verification step with its integrity record, and an entry it rejected or quarantined, or a test of it (D6-MEMORY-VERIFY) |  |
| D6 |  |  | The write policy for each store's partitions, with a write refused outside them (D6-MEMORY-WRITE) |  |
| D6 |  |  | The obfuscation configuration, set against the dataset's exposure-restricted fields (D6-OBFUSCATE) |  |
| D6 |  |  | The residual record, set against the dataset's fields (D6-OBFUSCATE-RESIDUAL) |  |
| D6 |  |  | The mapping tables' access policy, set against the policy over the source data (D6-OBFUSCATE-TABLES) |  |
| D6 |  |  | The assessment schedule, with the run history for the period and each run's findings with their closure (D6-REACH-CADENCE) |  |
| D6 |  |  | The restore test record, naming the store, the earlier state and what the restore returned (D6-ROLLBACK) |  |
| D6 |  |  | The removal justification with its measurements, set against the dataset's fields (D6-SCOPE-MEASURE) |  |
| D6 |  |  | The source-to-derived link record, with a propagated deletion traced through it (D6-SCOPE-PROPAGATE) |  |
| D6 |  |  | The weighting configuration, with a test in which a lower-trust item planted to match a query ranked below a higher-trust one (D6-TRUST-WEIGHT) |  |
| D6 |  |  |  | Item and verification record, configured pre-context decision point, and a tampered or unauthenticated item refused in a test (D6-ATTEST) |
| D6 |  |  |  | The detection's configuration with its rule, and its flags for a sample of answers or a test (D6-CONTRADICT) |
| D6 |  |  |  | Named restricted inference, role, session-state rule, and prohibited and permitted combination tests (D6-ENTITLE-INFER) |
| D6 |  |  |  | The drill report for each quarter, with the measured restore time and the target (D6-ROLLBACK-DRILL) |
| D6 |  |  |  | Threat-class-specific miss and benign false-hold, reject, or alert tests across applicable stores, configured treatment, and owner residual decision (D6-SCAN-BOUND) |
| D6 |  |  |  | The corpus baseline and the detection rule with its threshold, with an alert it raised or a test of it (D6-SCAN-DRIFT) |
| D7 | Tool-call and answer records from each agent and output route, with event time, accountable human, and searchable or exportable store (D7-LOG) |  |  |  |
| D7 |  | A sample of action records, each set against the agent's identity in the inventory and the human of the session it belongs to (D7-ATTRIBUTE) |  |  |
| D7 |  | The sandbox's forwarding configuration, with a forwarded event of each class, from the telemetry or from a test (D7-FORWARD-ESCAPE) |  |  |
| D7 |  | The D6 control output and forwarding configuration, with a finding forwarded from production or a test (D7-FORWARD-SCAN) |  |  |
| D7 |  | The telemetry records of a sample of memory writes, set against the store's own view of the entries (D7-LOG-MEMORY) |  |  |
| D7 |  | The records of a sample of calls to each tool, read field by field (D7-LOG-SCHEMA) |  |  |
| D7 |  | The stated period for each kind of record, with the document that states it, set against each store's retention setting (D7-RETAIN) |  |  |
| D7 |  | Backend traces for each applicable path and a reconstruction of one sampled action, OpenTelemetry GenAI spans are one implementation (D7-SPANS) |  |  |
| D7 |  | Collector/export configuration, pipeline loss controls, and successful lookup of sampled production traces (D7-SPANS-HELD) |  |  |
| D7 |  | Schema contract, instrumentation version record, and a field-compatibility test (D7-SPANS-PIN) |  |  |
| D7 |  | Token record, check configuration, and alert or test (D7-PROMPT-CANARY) |  |  |
| D7 |  |  | Per-identity baseline, detector, and an alert or test (D7-IDENTITY-BASELINE) |  |
| D7 |  |  | Event schema and a sampled denied call joined to its escape event (D7-ESCAPE-RECORD) |  |
| D7 |  |  | The baseline record for each applicable agent and the detection rule, with an alert it raised or a test of it (D7-BASELINE) |  |
| D7 |  |  | The detection rule over the decision path's approval records, with an alert it raised or a test of it (D7-CONTROL-APPROVE) |  |
| D7 |  |  | The list of the deployment's controls, each with the change record it writes and the rule that reads that record, and an alert a relaxation raised or a test of it (D7-CONTROL-RELAX) |  |
| D7 |  |  | The scoring configuration with its threshold, and a scored session from the period (D7-DRIFT) |  |
| D7 |  |  | The routing rule, and the disposition log for the period, with each routed or suspended session and its outcome (D7-DRIFT-ROUTE) |  |
| D7 |  |  | The evaluation schedule with its category list, and the report of each run in the period with its coverage statement (D7-EVAL) |  |
| D7 |  |  | The multi-session scenario list, with the results of each scenario (D7-EVAL-SESSIONS) |  |
| D7 |  |  | The scenario list with each scenario's turn count, and the multi-turn results reported apart (D7-EVAL-TURNS) |  |
| D7 |  |  | The writing component's configuration and identity, tool and answer route coverage, and the store's access policy set against every identity the agent holds (D7-LOG-OUTSIDE) |  |
| D7 |  |  | The test record, naming the pipeline, the store, the identities tried and what each attempt did (D7-LOG-TAMPER) |  |
| D7 |  |  | The workflow log's writer and store, with the store's access policy set against the orchestrator's identities, and a sample of its entries (D7-ORCHESTRATE) |  |
| D7 |  |  | The reconciliation rule with its schedule, and its results for the period with each alert (D7-ORCHESTRATE-RECONCILE) |  |
| D7 |  |  | Coverage comparison and sampled findings with dispositions, no particular posture product is required (D7-POSTURE) |  |
| D7 |  |  | The detection rule with the review paths it compares, and an alert it raised or a test of it (D7-REVIEW-EVASION) |  |
| D7 |  |  |  | Message records, baseline, running rule, and alert or test (D7-A2A-BASELINE) |
| D7 |  |  |  | The actionable-rate report for each period, with the documented target (D7-ALERT-ACTIONABLE) |
| D7 |  |  |  | The SLA and the controls-update log for the period, with each alert matched to its change, decision or tuning record (D7-ALERT-LOOP) |
| D7 |  |  |  | Path model, running rules, threshold tests, and alerts (D7-CASCADE) |
| D7 |  |  |  | Joint statistics, running rule, and alert or test (D7-JOINT) |
| D7 |  |  |  | The playbook for each alert class, with its run history for the period (D7-PLAYBOOK) |
| D8 | The provenance record of each acquired dataset, set against the inventory's dataset entries, with the entry checksums of a dataset held by reference (D8-DATASET) |  |  |  |
| D8 | The register entries, set against the stores that hold the deployment's design, development, model and experiment documentation (D8-DOCS) |  |  |  |
| D8 | The access list of each store, set against the roles its owner names (D8-DOCS-ACCESS) |  |  |  |
| D8 | The inventory, each entry read field by field and set against the running configuration, the lock files, the image digests or the device inventory (D8-INVENTORY) |  |  |  |
| D8 | The saved card for each model, with its date or version, set against the version in use (D8-MODEL-CARD) |  |  |  |
| D8 | The version history of each component over the period, from change records, run logs, the device inventory or the supplier's change notices, set against the dates the configuration changed (D8-VERSION) |  |  |  |
| D8 | Prompt or scoped supplier record, review, and treatment decisions (D8-PROMPT-SECRETS) |  |  |  |
| D8 |  | Versioned architecture, change-linked threat model, security requirements, review, and treatment record (D8-DESIGN) |  |  |
| D8 |  | The settings as the harness resolved them on an enrolled device or runner, with the fleet record showing the managed source on each device or runner (D8-HARNESS) |  |  |
| D8 |  | The record, set against the harness's documentation of the keys that merge across scopes (D8-HARNESS-EXTEND) |  |  |
| D8 |  | The lock values in the resolved managed settings, set against the locks the harness documents (D8-HARNESS-LOCK) |  |  |
| D8 |  | The restore mechanism's configuration, with its log of a restore or a test of it (D8-HARNESS-RESTORE) |  |  |
| D8 |  | The review records for a sample of changes drawn from the tree's change history (D8-HARNESS-REVIEW) |  |  |
| D8 |  | Change list, scanner and review results, held findings, dispositions, and checked-to-shipped revision link (D8-IMPLEMENT) |  |  |
| D8 |  | The baseline for each instruction file, with the check that verifies it at load, and for a hash baseline its record of a mismatch or a test of it (D8-INSTRUCT-BASELINE) |  |  |
| D8 |  | The scan result for each such ability and version, dated before its install (D8-ABILITY-SCAN) |  |  |
| D8 |  | The approval record or the enforced configuration, set against the abilities each agent uses (D8-ABILITY-SOURCE) |  |  |
| D8 |  | Release AI-BOM compared with configuration, link supplier bills of materials when available (D8-AIBOM) |  |  |
| D8 |  | The registry's policy with its name, publisher and age checks, the package-manager configuration each agent's environment uses, and a refused install or a test (D8-DEPS-INSTALL) |  |  |
| D8 |  | The install command and the lock file of each install, with a run that failed on a mismatch or a test (D8-DEPS-LOCK) |  |  |
| D8 |  | The scan step in each pipeline's configuration, with a recent run's results, set against the packages and images the release installs or runs (D8-DEPS-SCAN) |  |  |
| D8 |  | The inspection record of each such model file, dated before its first load (D8-MODEL-INSPECT) |  |  |
| D8 |  | The probe record of each such model file, with the environment's isolation and monitoring configuration (D8-MODEL-PROBE) |  |  |
| D8 |  | The check record of each model file, dated before its first load, with the enforced load policy and the formats it admits where the policy replaces the scan (D8-MODEL-SCAN) |  |  |
| D8 |  | The signature or attestation of each artifact of the latest release, with the key or identity that verifies it (D8-SIGN) |  |  |
| D8 |  | Assessments, cited records, applicable provider processing terms, and available bill-of-materials links (D8-SUPPLIER) |  |  |
| D8 |  | Probe-to-risk map, versioned runs, outcomes, and disposition (D8-PROMPT-PROBE) |  |  |
| D8 |  |  | Runtime record, release AI-BOM, reconciliation runs, and findings (D8-AIBOM-RUNTIME) |  |
| D8 |  |  | The provenance statement of each artifact of the latest release, with the build platform and its SLSA level (D8-BUILD) |  |
| D8 |  |  | The intake source named for each type of component, the deadlines by severity, and each affecting advisory of the twelve months before the assessment starts with its closure date (D8-DISCLOSE) |  |
| D8 |  |  | Procedure, overdue advisory list, and each dated containment record (D8-DISCLOSE-CONTAIN) |  |
| D8 |  |  | The lineage record of each produced model's current version (D8-LINEAGE) |  |
| D8 |  |  | Identities, endpoint ACLs, and promotion records or a denied write test (D8-REPO-WRITE) |  |
| D8 |  |  | The review's cadence and rule, with its latest record listing the components it read and the action taken on each it found (D8-RETIRE) |  |
| D8 |  |  | Admission settings, approval record, and refused-component test (D8-VERIFY) |  |
| D8 |  |  | The signing manifest or the pinned list, with the verifier's check of each file it names (D8-VERIFY-BUNDLE) |  |
| D8 |  |  | The verification step with the signatures or digests it checks, set against each file an agent loads as instructions, and a refused load or a test (D8-VERIFY-INSTRUCT) |  |
| D8 |  |  | The published statements, set against the vulnerabilities affecting each published component (D8-VEX) |  |
| D8 |  |  | Production configuration, provider version scheme, and change record (D8-MODEL-PIN) |  |
| D8 |  |  | Threat-to-test map, cases, raw results, manifest, and retests (D8-TEST) |  |
| D8 |  |  | Decision, approver, version or digest, linked results, exception, and refusal test (D8-RELEASE) |  |
| D8 |  |  |  | Release-linked audit record, signed Agent Cards, trust anchors, and receiver rejection results (D8-A2A-SIGN-TEST) |
| D8 |  |  |  | Tolerance, reconciliation history, and dated closures or findings (D8-AIBOM-DRIFT) |
| D8 |  |  |  | Signed provenance, builder assurance, and, where feasible, an independent matching rebuild (D8-BUILD-HARDENED) |
| D8 |  |  |  | The reconciliation's configuration and schedule, with its run history and the findings each run recorded (D8-LOOP) |
| D8 |  |  |  | The published SLA, with each finding of the quarter and the control change or decision that closed it, with its date (D8-LOOP-SLA) |
| D8 |  |  |  | Each route’s admission policy and a refused promotion test (D8-VERIFY-GATE) |
| D8 |  |  |  | Feed endpoint, statement match, publication dates, and deadline (D8-VEX-FEED) |
| D8 |  |  |  | The weight store's access policy, set against the identities that need access, with its integrity protection and monitoring configuration (D8-WEIGHTS) |
| D9 | The runbook's section for each guardrail, set against the deployment's guardrails (D9-GUARD-RUNBOOK) |  |  |  |
| D9 | The runbook's owner-departure section, with the trigger it names (D9-OFFBOARD) |  |  |  |
| D9 | The runbook's credential section, set against the deployment's credentials and the access policy of the store that holds them (D9-OFFBOARD-CREDS) |  |  |  |
| D9 | The runbook's section for each approval path, with the record it names (D9-QUEUE-RUNBOOK) |  |  |  |
| D9 |  | The cost series for each guardrail and agent over the period (D9-GUARD-COST) |  |  |
| D9 |  | The recorded fail mode of each guardrail, with the record of the test that made it fail (D9-GUARD-FAILMODE) |  |  |
| D9 |  | The latency series for each guardrail and agent over the period (D9-GUARD-LATENCY) |  |  |
| D9 |  | The records of a sample of high-risk approvals, read field by field, with the store's write controls set against the approvers, the requesters, the agents and the approving system's administrators (D9-HIGHRISK-RECORD) |  |  |
| D9 |  | The approved playbook, scope-to-agent and threat-class map, and each class's response steps and design record (D9-IR) |  |  |
| D9 |  | The steps for the deployment, set against its agents' stop controls and record stores (D9-IR-CONTAIN) |  |  |
| D9 |  | The exercise record with its scenario and participants, and the after-action report (D9-IR-EXERCISE) |  |  |
| D9 |  | The playbook's notification section, set against the instruments that apply to the organization (D9-IR-NOTIFY) |  |  |
| D9 |  | The playbook's scenario for a compromised third-party model (D9-IR-SCENARIO) |  |  |
| D9 |  | The published policy, set against the components the deployment publishes (D9-MODEL-DEPRECATE) |  |  |
| D9 |  | The notice as each class of user meets it, set against each agent's channels (D9-NOTICE) |  |  |
| D9 |  | Coverage decisions, user-visible disclosure, and current system facts (D9-NOTICE-PROPERTIES) |  |  |
| D9 |  | The median queue age and the expired requests of each path for each period (D9-QUEUE-AGE) |  |  |
| D9 |  | The approval rate of each path for each period, with the records it is computed from (D9-QUEUE-RATE) |  |  |
| D9 |  | The process's schedule and scope, the rule with its time, and the period's findings with the date each was resolved (D9-REAP) |  |  |
| D9 |  | The document with the duties, and the personnel record of the person who holds the role (D9-ROLE) |  |  |
| D9 |  | The deputy's designation for each duty, with the deputy's access to the systems the duty uses (D9-ROLE-DEPUTY) |  |  |
| D9 |  |  | Path records, per-class report, cadence, and review (D9-APPROVE-COVERAGE) |  |
| D9 |  |  | The method, with each drift finding of the period, its classification and, for each adversarial one, its record in the monitoring queue (D9-DRIFT-TRIAGE) |  |
| D9 |  |  | The drill report for each of the two quarters (D9-DRILL) |  |
| D9 |  |  | The baseline's definition, the threshold with its method, and an alert the detection raised or a test of it (D9-OVERSIGHT-FLAG) |  |
| D9 |  |  | The measure's method for each path, with each period's result (D9-OVERSIGHT-INVOLVE) |  |
| D9 |  |  | The limit's configuration in the approval path, with a request it held or routed (D9-OVERSIGHT-LIMIT) |  |
| D9 |  |  | The test report for each approval path, technique by technique, with its dates and results (D9-OVERSIGHT-TEST) |  |
| D9 |  |  | The 95th percentile for each path and each period, with the records it is computed from (D9-QUEUE-P95) |  |
| D9 |  |  | The method for each path, with the rate for each period (D9-QUEUE-STAMP) |  |
| D9 |  |  |  | The SLA, with each incident of the two quarters matched to its change or decision and its date (D9-LOOP) |
| D9 |  |  |  | The published thresholds, with each measure's values over the two quarters and the record of each excursion (D9-QUEUE-THRESHOLD) |
| D9 |  |  |  | The continuity-test report for each of the two quarters (D9-ROLE-CONTINUITY) |

## Investment report

The decision report begins with the deployment boundary and a nine-domain **current and target profile**. For each domain, show the observed level and qualitative confidence where assessable, or the documented reason for a not-applicable or unanswerable result. Where the domain applies, show the target level with risk rationale, criterion-level gaps, and any unanswerable supplier step. Keep organization-wide criteria in a separate shared-control appendix with the deployments that inherit them. In D1, identify which result rests on inherited program evidence and which local deployment decision was checked; name a missing inherited prerequisite as a blocker to the target. For supplier-dependent criteria, show the customer-operated capability and the supplier assurance gap separately beside the one domain verdict. A profile is a mapping of decisions, not a combined score.

Where a proposed L5 benefit depends on measured attack or detection performance, report the observed misses and sample size by relevant class, the organization's tolerance, any severe miss it accepted with authority and expiry, and the effect of the miss on this deployment. A rate without its tested route and denominator cannot support an investment or risk decision. The report sets no universal tolerance.

Translate consequential gaps into work packages. Each package names:

- the exposure and failure path it addresses, with the criterion or blocked target;
- the control or evidence change proposed and its expected risk reduction, stated qualitatively unless a measured estimate exists;
- one-time engineering and procurement effort, recurring testing, review, licensing, and incident effort, and the team that bears each;
- dependencies, supplier requests, accountable owner, delivery window, and the evidence that will show completion;
- the fund, defer, or accept decision, decision authority, residual risk, and reassessment trigger.

Prioritize by consequence, exposure, and whether the work removes a real blocker. A difference between L3 and L4 is not itself a benefit estimate. A team may accept a lower target where the architecture prevents the relevant hazard or the operating cost exceeds the reduction, provided the decision names the residual exposure. [NIST CSF 2.0](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf#page=11) supports using current and target profiles to communicate and prioritize cybersecurity work; the CMM adds its criterion evidence.

### Worked customer-refund decision packet

This fictional packet assesses the customer-refund agent in [[agentic-ai-security-reference-architecture|the reference architecture]]. The customer-service risk executive decides whether to permit live refunds and fund a higher target. Intake identifies one production agent configuration, analyst-initiated tasks, an external model route, a case corpus, a gateway-held refund credential, and the external refund provider. The assessment frame tests an authorized and an unauthorized analyst, a different customer's case, the model payload, a normal refund, a provider timeout, and the latest release. Evidence IDs below refer to fictional, versioned records in the packet's evidence index. The underlying criterion records establish every claimed level.

| Domain | Observed and confidence | Target | Decision-relevant evidence and blocker |
|---|---|---|---|
| D1 | L3, moderate | L3 | Shared governance E-01 covers this deployment; local tier and production approval E-02 match its refund authority. |
| D2 | L3, high | L3 | E-12 joins the analyst's refund right, agent identity, and gateway decision; an unauthorized analyst's request is denied. |
| D3 | L2, high | L3 | E-31 shows a second refund send after an uncertain provider timeout: D3-WRITE-RECONCILE is not met. |
| D4 | L3, moderate | L4 | Route screens operate in E-40; E-41 finds no pre-execution task-alignment hold on a changed refund proposal. |
| D5 | L3, high | L3 | E-50 denies direct runtime access to the refund API and an unapproved model route. |
| D6 | L2, moderate | L3 | E-61 shows full case notes in a model request beyond the approved fields: D6-SCOPE is not met. |
| D7 | L2, moderate | L3 | E-70 has searchable tool-call and answer records; E-71 lacks a joined provider-call span under D7-SPANS. |
| D8 | L2, low | L3 | Customer release checks E-80 operate; E-81 lacks provider data-use and retention terms and an authorized decision on that gap, so D8-SUPPLIER is not met. |
| D9 | L3, moderate | L3 | E-90 shows staffed approval and incident routes, including a named refund reconciliation operator. |

The D1 result inherits only E-01's organization controls. E-02 proves the deployment's own gate. If E-01 ceased to cover this deployment, the affected D1 criterion would be a target blocker, without a second D1 score. In the supplier view, E-80 establishes customer-operated release checks, while E-81 leaves provider processing behavior unanswerable and the customer's gap decision absent. The assessor requests service-specific terms and records D8-SUPPLIER as not met. An authorized acceptance could complete the supplier-assessment step, but would not prove the provider's practice; the uncertainty would remain in confidence and residual risk.

E-42 tested 40 encoded and 40 retrieved-document prompt-injection attempts through the actual routes. One in each class bypassed the input screen. The retrieved-document miss induced a high-impact refund proposal that the gateway refused. The local tolerance is at most one miss in either 40-case class and zero severe misses. Accepted severe misses: none. The risk executive keeps live refunds disabled while that severe miss and the uncertain-write path are open. These results inform the case for stronger D4 controls. They do not establish L5 or a universal acceptable rate.

| Work package | Expected reduction and completion evidence | One-time and operating effort | Owner and decision |
|---|---|---|---|
| Bind and reconcile refund writes | Gateway-created request IDs and provider-state lookup prevent blind replay; a duplicate and timeout retry are refused in E-31's production-equivalent path. | Illustrative two engineering weeks; daily reconciliation queue review and a named compensation owner. | Application owner funds before live refunds resume. |
| Minimize model requests and settle terms | Field allowlist excludes surplus case notes in a sampled outbound request; service-specific use, retention, deletion, and subcontractor terms settle E-81. | Illustrative one engineering week plus legal and procurement review; review each route or contract change. | Data owner funds; procurement requests provider evidence. |
| Hold misaligned proposals and restore trace joins | A pre-execution task-alignment refusal and joined provider span close E-41 and E-71; E-42's severe miss is retested. | Illustrative two engineering weeks; maintain tests and trace schema each release. | Security engineering funds before a higher D4 or D7 claim. |

The restriction on live refunds takes effect after the evidence cutoff; the profile records the configuration tested before that decision. The risk executive defers any L5 target until the severe miss is treated and later rate tests support an organization-set tolerance. The assessor rechecks D3-WRITE-RECONCILE, D4-ALIGN, D6-SCOPE, D7-SPANS, and D8-SUPPLIER after their respective changes, and reopens the decision on a model-route, refund API, provider contract, or authority change. The unaffected domain results retain their dated evidence cutoff.

## Quality review

A second assessor checks the scope and population frame, every disputed not-applicable or unanswerable decision, a sample of met and not-met records from each domain, each level boundary, and every target blocker. The reviewer follows evidence identifiers back to the version and route that produced them and checks that an artifact cited twice really establishes both facets. A contradiction returns to the first assessor for a recorded resolution; the report keeps the unresolved disagreement visible if the evidence cannot settle it.

Before issue, reconcile the annex against the nine current domain catalogues, check that every surviving criterion has one artifact at its level and either a relevant interview question or an artifact-only declaration, and verify that retired IDs appear in neither. Review the report as a decision instrument: the current and target profile, qualitative confidence, exposure, work, effort, owner, residual risk, and reassessment trigger must agree. The assessor signs the evidence cutoff date and report revision. A later product launch or supplier statement cannot silently revise that historical conclusion.

## Sources and limits

[NIST SP 800-53A Rev. 5](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53Ar5.pdf#page=22) supplies the examine, interview, and test vocabulary and the depth and coverage attributes. [NIST CSF 2.0](https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.29.pdf#page=11) supplies the current and target profile pattern. [DOE C2M2 v2.1](https://www.energy.gov/sites/default/files/2022-06/C2M2%20Version%202.1%20June%202022.pdf#page=25) is a maturity-model precedent for cumulative, independent domain levels. The CMM domain definitions, not these sources, set the AAI-S criteria. Supplier evidence may remain unavailable and probabilistic controls can miss attacks after a passing test; the report preserves those limits in the verdict, confidence, and risk decision.
