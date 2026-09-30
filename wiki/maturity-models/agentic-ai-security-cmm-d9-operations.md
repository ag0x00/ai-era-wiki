---
type: maturity-model
title: "CMM D9: Operations and Human Factors"
address: c-000130
created: 2026-05-25
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - operations
  - human-factors
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[anti-patterns-and-failure-modes]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[standards-review-owasp-llm-top-10-2026-Q2]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[claude-cowork]]"
  - "[[cmm-known-limitations]]"
  - "[[canary-tokens-for-llms]]"
  - "[[owasp-agentic-ai-top-10]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
verified: 2026-09-29
verified_against:
  - ".raw/papers/owasp-ai-exchange-general-controls-2026-08-19.md"
  - ".raw/papers/owasp-ai-exchange-testing-2026-08-19.md"
verified_findings: 0
verified_note: "Independent source and criterion read: made CoSAI examples guidance rather than a grading obligation and tied fail-closed treatment to required high-impact decisions; live CoSAI lifecycle checked; no source-fidelity defect remains."
---

# Agentic AI Security CMM — D9 Operations and Human Factors

## Domain decision and boundary

D9 grades how people operate the assessed agentic deployment: approval queues, guardrail failure response, incident handling, owner departure, retirement, and continuity. It uses both deployment evidence and organization-wide incident and duty records. An organization-wide criterion is graded once and inherited only by deployments named in its scope. A deployment criterion covers each production agent and applicable approval path. [[agentic-ai-security-cmm-2026|The CMM core]] defines the cumulative levels; [[agentic-ai-security-cmm-measurement-protocol|the Assessor's Handbook]] records scope, evidence, applicability, and confidence.

[[agentic-ai-security-cmm-d3-control-least-agency|D3]] owns action risk categories, approval policy, and enforcement of a decision before an action runs. D9 measures whether the human queue and operating procedure work. A cooling-off period can be a D3 policy treatment when the risk analysis supports it. The policy also needs an emergency route for urgent legitimate action. A fixed delay on every high-risk action is not a D9 criterion. D9-HIGHRISK-RECORD still tests the quality and integrity of the approval record. [[agentic-ai-security-cmm-d7-observability|D7]] detects behavior and routes alerts. D9 triages drift and responds to incidents. [[agentic-ai-security-cmm-d8-supply-chain|D8]] owns prompt leak tests before release and model version pinning. D9 owns the operating queue and the consumer-facing deprecation promise.

An approval path is a route by which a person permits one action before execution, including a user's decision to send an agent-prepared draft. Queue age runs from request to decision or expiry. Approval rate is the approved share of requests. Rubber-stamp rate is the share approved without a documented sign of review under the organization's method. A reporting period is set in advance and cannot exceed a quarter. A guardrail is a filter, classifier, judge, scan, or groundedness check on the agent's model call. An orphan is an agent without a current owner or a credential held by such an agent, used by no agent, or still usable by a departing person. An incident is an event the organization's incident process records, including a failed guardrail, response stop, or faulty or expired approval. Vendor operation of a relevant control does not make it inapplicable. The assessor tests the customer's exposed route or inspects supplier evidence. A material inaccessible step with no suitable evidence is unanswerable.

## Failure paths

A human approval can turn into a routine click as request volume rises. A delayed or expired request can become an unreviewed action when a path silently changes its failure behavior. Owner departure can leave credentials active after a visible agent is disabled. An incident playbook can miss the actual stop control or notification owner even if it names an AI threat. D9 tests the route and the people who operate it. The [OWASP AI Exchange oversight control](https://owaspai.org/go/oversight/) supplies the approval-path failure modes, and [CoSAI's AI Incident Response Framework](https://www.coalitionforsecureai.org/wp-content/uploads/2026/03/AI-Incident-Response-1.pdf) supplies a response lifecycle and sample AI incident playbooks.

## Level progression

| Level | Observable outcome |
|---|---|
| L1 | Owner departure, guardrail failure, approval review, and incident response have no deployment-specific procedure. |
| L2 | Runbooks assign operator steps for guardrail failures, leavers, credentials, and approval paths. |
| L3 | Guardrail effects, orphan resolution, queue measures, notices, and exercised incident duties are recorded. |
| L4 | Approval quality and coverage, drift triage, adversarial oversight tests, and end-to-end retirement are measured. |
| L5 | Incidents and queue excursions drive dated control decisions within stated limits, and deputies demonstrate continuity. |

## Criterion catalogue

Each criterion fails if one applicable agent or path omits a required facet. “Tracked” means a value computed from the deployment's own records for each period, with enough source data to recalculate it. A tabletop can exercise a playbook. A decommission drill must execute the actual retirement path against an agent on the production platform. A supplier test record can establish a vendor-held fail mode when it names the deployed guardrail and how it behaved under error or timeout. The customer's runbook and action remain required. D9 does not infer that silence means zero incidents, leaks, or orphans.

### L2 detail

- **D9-GUARD-RUNBOOK.** *Deployment.* A runbook states, for each guardrail, the step the operator on duty takes when the guardrail fails, errors or times out, and what happens to the agent's model call meanwhile. For a guardrail the vendor runs, the behaviour is read from the vendor's documentation or a customer test, and the operator's step stays required. Not applicable where no guardrail runs on any agent's model calls. *Evidence:* the runbook's section for each guardrail, set against the deployment's guardrails.
- **D9-OFFBOARD.** *Deployment.* A runbook states what happens to each agent when its owner leaves the organization or moves to a role that no longer owns it: the agent passes to a named successor or is retired, and the runbook names who acts and within what time. The steps can be manual. A leaver procedure that ends the departing person's own access and names no step for the agents the person owns fails the criterion. *Evidence:* the runbook's owner-departure section, with the trigger it names.
- **D9-OFFBOARD-CREDS.** *Deployment.* A runbook states that each of the deployment's credentials that a departing person created, holds or can read is revoked or replaced when that person leaves, and names who does it. Not applicable where the deployment holds no stored credential, or where no person holds or can read one, as the credential store's access policy shows. *Evidence:* the runbook's credential section, set against the deployment's credentials and the access policy of the store that holds them.
- **D9-QUEUE-RUNBOOK.** *Deployment.* A runbook states, for each approval path, the record each approval writes, who reviews the path's requests and decisions and how often, and what the reviewer acts on, such as a request left unanswered past a stated time or a change in how often approvals are given. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the runbook's section for each approval path, with the record it names.

### L3 detail

- **D9-GUARD-COST.** *Deployment.* The cost of each guardrail is tracked for each agent that uses it. Not applicable where D9-GUARD-RUNBOOK is not applicable, or for a guardrail whose cost the vendor includes in the price of the service it runs in, with no charge of its own. *Evidence:* the cost series for each guardrail and agent over the period.
- **D9-GUARD-FAILMODE.** *Deployment.* The organization records whether each guardrail holds, refuses, or permits a model call when the guardrail errors or times out, and confirms that behavior by making the deployed guardrail fail. An action cannot proceed when the unavailable guardrail's decision is required to keep a high-impact path within its authorized boundary. For other guardrails, a fail-open path needs a recorded fallback and risk decision. A unit test with a mocked guardrail, or a behavior read only from code, fails the criterion. Not applicable where D9-GUARD-RUNBOOK is not applicable. *Evidence:* fail-mode and fallback records, with the test that made each guardrail fail and the resulting action outcome.
- **D9-GUARD-LATENCY.** *Deployment.* The latency each guardrail adds to each agent's model calls is tracked for that agent. For a guardrail the vendor runs, the latency is read from the vendor's documentation or a customer test. Not applicable where D9-GUARD-RUNBOOK is not applicable. *Evidence:* the latency series for each guardrail and agent over the period.
- **D9-HIGHRISK-RECORD.** *Deployment.* Each high-risk approval writes a tamper-evident record that carries the request, the parameters presented to the approver, the approver's identity and authentication method, the approver's explicit confirmation of the key parameters, the decision and the outcome. Not applicable where no action any agent can invoke falls in a high-risk category. *Evidence:* the records of a sample of high-risk approvals, read field by field, with the store's write controls set against the approvers, the requesters, the agents and the approving system's administrators.
- **D9-IR.** *Organization.* An approved AI incident playbook covers each agent in scope and gives detection, containment, eradication, and recovery steps for each credible incident class in the deployment's threat model. The classes include prompt injection where an agent reads untrusted content, memory injection where it keeps agent memory, and corpus poisoning where it retrieves from a corpus. *Evidence:* the approved playbook, scope-to-agent and threat-class map, and each class's steps and design record.
- **D9-IR-CONTAIN.** *Deployment.* The AI incident playbook, or a runbook it names, gives the deployment's own containment steps: how to stop each agent, where each agent's records are, and who acts. A playbook whose steps name no control of the deployment fails the criterion. *Evidence:* the steps for the deployment, set against its agents' stop controls and record stores.
- **D9-IR-EXERCISE.** *Organization.* The AI incident playbook was exercised in the twelve months before the assessment starts, and the exercise's after-action report records each action it raised with an owner. *Evidence:* the exercise record with its scenario and participants, and the after-action report.
- **D9-IR-NOTIFY.** *Organization.* The playbook names the regulatory-notification path for an AI incident: each notification instrument that applies to the organization, the person who owns each notification, and the clock each runs on. *Evidence:* the playbook's notification section, set against the instruments that apply to the organization.
- **D9-IR-SCENARIO.** *Organization.* The playbook covers the compromise of a third-party model the organization's agents call: a model manipulated at its supplier and already acquired and in production. Not applicable where no agent in scope calls a model a third party supplies. *Evidence:* the playbook's scenario for a compromised third-party model.
- **D9-MODEL-DEPRECATE.** *Deployment.* For each model, skill, MCP server or agent the deployment publishes for use outside the organization, the organization publishes a deprecation policy stating how long a version stays supported after its successor ships and how consumers are told before a version is retired. Not applicable where the deployment publishes nothing for use outside the organization. *Evidence:* the published policy, set against the components the deployment publishes.
- **D9-NOTICE.** *Deployment.* Each of the deployment's users is told that an AI model is involved, at the start of the interaction or with the first content an agent sends them. A notice a user must look for elsewhere, such as a policy page, fails the criterion. *Evidence:* the notice as each class of user meets it, set against each agent's channels.
- **D9-NOTICE-PROPERTIES.** *Deployment.* Evaluate the user-visible disclosure against each property the [OWASP AI Exchange AI TRANSPARENCY control](https://owaspai.org/go/aitransparency/) names: the model's rough working, training approach, data type and source, expected accuracy and robustness, and residual security risk. Record coverage or a justified omission for each. Correct any published claim that contradicts the deployed system. A checklist with no comparison to the actual disclosure fails. The text can be the organization's own or a supplier document the organization publishes or links for its users. *Evidence:* coverage decisions, user-visible disclosure, and current system facts.
- **D9-QUEUE-AGE.** *Deployment.* The median queue age of each approval path is tracked, with the requests that expired unanswered. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the median queue age and the expired requests of each path for each period.
- **D9-QUEUE-RATE.** *Deployment.* The approval rate of each approval path is tracked. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the approval rate of each path for each period, with the records it is computed from.
- **D9-REAP.** *Deployment.* A process finds the deployment's orphaned agents and orphaned credentials on a schedule the organization sets, and a rule sets the time within which each is passed to a successor, retired or revoked. Each orphan found in the twelve months before the assessment starts was resolved within that time. A process that looks for orphaned agents and not for orphaned credentials, or an orphan resolved past its time, fails the criterion. *Evidence:* the process's schedule and scope, the rule with its time, and the period's findings with the date each was resolved.
- **D9-ROLE.** *Organization.* A document the organization approved, such as a job description or a charter, names a role accountable for the security of the organization's AI systems in operation and lists the role's duties, and a person holds the role on the assessment date. *Evidence:* the document with the duties, and the personnel record of the person who holds the role.
- **D9-ROLE-DEPUTY.** *Organization.* For each duty D9-ROLE's document lists, an approved document names a deputy who performs the duty when the holder cannot, and the deputy holds the access the duty needs. A deputy named for some of the duties fails the criterion. *Evidence:* the deputy's designation for each duty, with the deputy's access to the systems the duty uses.

### L4 detail

- **D9-APPROVE-COVERAGE.** For each action class, measure from the decision path’s own records the share of executions a person approved on a stated cadence. Review each share against that class’s D3 tier and record any confirm-tier class below full coverage as a finding. Aggregate approval counts that conceal an unapproved class fail. Not applicable where no action routes to human approval. *Evidence:* path records, per-class report, cadence, and review.
- **D9-DRIFT-TRIAGE.** *Deployment.* Each drift finding on an agent in the period carries a recorded classification, benign or adversarial, made by a method the organization states that tests for an adversarial cause, such as a check of the period's inputs, configuration changes and detections, and each finding classed adversarial reaches the queue the organization's security monitoring triages. A cause recorded with no test for an adversarial cause fails the criterion. Not applicable where nothing monitors the agents' behaviour or their models' performance against a baseline. *Evidence:* the method, with each drift finding of the period, its classification and, for each adversarial one, its record in the monitoring queue.
- **D9-DRILL.** *Deployment.* A decommission drill ran in each of the last two quarters: the runbook that retires an agent was carried out against an agent on the production platform, such as a test agent registered for the drill, from stopping the agent to revoking its credentials and connections and closing its register entry, and each step's result and time were recorded. A tabletop exercise fails the criterion. *Evidence:* the drill report for each of the two quarters.
- **D9-OVERSIGHT-FLAG.** *Deployment.* A detection compares each approver's approval rate in each session with a historical baseline the organization defines, such as the approver's own earlier sessions, and raises an alert when the rate exceeds the baseline by a threshold the organization states with its method. A comparison with no stated threshold fails the criterion. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the baseline's definition, the threshold with its method, and an alert the detection raised or a test of it.
- **D9-OVERSIGHT-INVOLVE.** *Deployment.* The organization names an involvement measure for each approval path, states the measure's method, and takes it for each period. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the measure's method for each path, with each period's result.
- **D9-OVERSIGHT-LIMIT.** *Deployment.* Each approval path limits the approvals one approver can give within one session, or within a window the policy sets, and a request past the limit waits or goes to another approver. A limit on the requests an agent sends, such as a count of payment calls in a chat session, meets no part of the criterion. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the limit's configuration in the approval path, with a request it held or routed.
- **D9-OVERSIGHT-TEST.** *Deployment.* In the twelve months before the assessment starts, the testing programme exercised each approval path adversarially, against people who hold the approver's role, with four techniques: urgency-driven bypass, approval-fatigue sequences and multi-step normalisation before a critical action, which the Exchange's `OVERSIGHT` names, and confusion injection, which the Exchange's testing guidance names without defining and this domain reads as content in one approval request built to mislead the approver about what the action does. A programme that exercised three of the four techniques fails the criterion. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the test report for each approval path, technique by technique, with its dates and results.
- **D9-QUEUE-P95.** *Deployment.* The 95th percentile of each approval path's queue age is tracked. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the 95th percentile for each path and each period, with the records it is computed from.
- **D9-QUEUE-STAMP.** *Deployment.* The rubber-stamp rate of each approval path is tracked, by a method the organization states for the path. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the method for each path, with the rate for each period.

### L5 detail

- **D9-LOOP.** *Deployment.* Each operational incident on an agent of the deployment in the last two quarters reached, within an SLA the organization publishes to the deployment's owners, a change to a control or a recorded decision to leave the controls unchanged, and the incident's record names the change or the decision. *Evidence:* the SLA, with each incident of the two quarters matched to its change or decision and its date.
- **D9-QUEUE-THRESHOLD.** *Deployment.* The organization publishes to the deployment's approvers and owners a threshold for each approval path's rubber-stamp rate, the 95th percentile of its queue age and its involvement measure, and in each period of the last two quarters each measure stayed within its threshold, or the excursion was recorded with the change to the approval path that followed. Not applicable where no action any agent can invoke waits for a human approval. *Evidence:* the published thresholds, with each measure's values over the two quarters and the record of each excursion.
- **D9-ROLE-CONTINUITY.** *Organization.* In each of the last two quarters a continuity test had the deputies perform the duties of D9-ROLE's role without its holder for a period the test states, including one run of the AI incident playbook from start to finish, and the test report records what each deputy did and each step that failed. *Evidence:* the continuity-test report for each of the two quarters.

## Prerequisites and blockers

D9-HIGHRISK-RECORD reads categories chosen in [[agentic-ai-security-cmm-d3-control-least-agency|D3]] and the approval route that enforces them. D9-APPROVE-COVERAGE compares executed actions with that D3 policy. A missing D3 decision record prevents an assessor from establishing queue coverage, so the D9 result records the affected criterion and evidence gap. The approval criteria are not applicable only when no agent action waits for a person's approval, including a send or commit decision. [[agentic-ai-security-cmm-d2-identity|D2]] supplies current agent owners and credential revocation; D9 tests the leaver procedure, orphan sweep, and retirement drill. [[agentic-ai-security-cmm-d7-observability|D7]] provides drift events and incident records for D9 triage and response. [[agentic-ai-security-cmm-d8-supply-chain|D8]] supplies the deployment threat model whose credible incident classes D9-IR must cover.

A D3 policy can impose a cooling-off delay on a hazard-selected action. It must also define who can use the emergency path, what authorizes the exception, and what D9 record permits reconstruction. Delay is not a blanket condition of D9 L3. An urgent legitimate containment or customer action can be harmed by an unconditional wait.

## Deployment-shape differences

| Shape | Material D9 difference |
|---|---|
| Tool-free or read-only assistant | Approval-path criteria apply only if a person still approves an agent-prepared action, such as sending a draft. |
| Hosted productivity assistant | Supplier-held fail modes and controls require specific supplier evidence or a customer test; customer runbooks, notices, queue measures, and incident steps remain local responsibilities. |
| Coding agent | Permission prompts, unattended jobs, generated changes, and owner departures create queue and decommission evidence even when individual tasks are short. |
| Multi-agent workflow | Distributed ownership and shared credentials enlarge the orphan and containment inventory; incident playbooks must name each stopping point. |

The target level follows exposure and operating capacity, not a universal timetable. A deployment with limited autonomy can legitimately have fewer applicable approval checks. The assessor records the reason for each absence and still grades incident response and owner continuity that apply to the agents in scope.

## Implementation and effort drivers

Runbooks need named operators, triggering events, specific controls, and time limits. An organization can reuse an existing incident process, but its AI playbook must identify the agent stop control, record store, supplier contact, and notification route for the assessed deployment. Approval metrics need event joins between request, decision, action, and expiry. The labor lies in reviewing those measures, testing approval fatigue, and correcting a weak path. Automated dashboards cannot replace a person capable of rejecting a harmful action.

The recurring work includes orphan sweeps, incident exercises, decommission drills, deputy continuity tests, and queue threshold reviews. The D9 L5 criteria inspect the specified incident, queue, and continuity records from the last two quarters. A zero count at quarter end, public metric publication, or membership in an external group is not a proxy for these controls. Publishing a component's deprecation promise creates a service obligation to its consumers. [[agentic-ai-security-cmm-d8-supply-chain|D8]] separately handles that component's vulnerability and version evidence.

## Sources and material limits

[OWASP AI Exchange oversight](https://owaspai.org/go/oversight/) informs the approval criteria, including fatigue, reviewer involvement, and high-risk records. Its [AI transparency control](https://owaspai.org/go/aitransparency/) gives the disclosure properties. [CoSAI's AI Incident Response Framework](https://www.coalitionforsecureai.org/wp-content/uploads/2026/03/AI-Incident-Response-1.pdf) supplies a response lifecycle and example scenarios, while [NIST SP 800-61 Rev. 3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r3.pdf) provides the wider incident-response basis. D9's thresholds and cadences are this CMM's assessable choices. The organization states its involvement measure and tests whether it changes reviewer behavior; these sources provide no universal numeric fatigue threshold.
