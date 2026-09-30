---
type: maturity-model
title: "CMM D7: Observability and Detection"
address: c-000128
created: 2026-05-25
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - observability
  - detection
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
  - "[[cmm-known-limitations]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agent-observability]]"
  - "[[opentelemetry-gen-ai]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[nist-ai-800-4]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-escape]]"
  - "[[agent-sandboxing]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[plan-validate-execute]]"
  - "[[tiered-detection-cascade]]"
  - "[[llm-as-a-judge]]"
  - "[[prompt-volume-to-alert-ratio]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[claude-cowork]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[red-teaming-for-ai-synthesis]]"
  - "[[pyrit]]"
  - "[[garak]]"
  - "[[promptfoo]]"
  - "[[mindgard-cart]]"
  - "[[agentic-soc-state-of-the-field]]"
  - "[[wiki-novelty-and-counterarguments-2026]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[agent-observability]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
  - "[[.raw/papers/owasp-ai-exchange-threats-through-use-2026-08-18.md]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Targeted answer logging and L5-rate check against live OWASP monitoring and testing guidance; no archived source opened. Other source claims remain outside this pass."
---

# Agentic AI Security CMM — D7 Observability and Detection

## Domain decision and boundary

D7 grades whether an assessor can reconstruct an agent's actions and whether the deployment detects and routes departures from expected behavior. The assessment covers one deployed agent application or platform, its production agents, and the telemetry stores and security monitoring that serve them. A replica of the same configuration is one agent. A distinct configuration is another. The security monitoring owner supplies the detection rules, triage queue, retention plan, and evidence. Supplier-operated records count toward D7-LOG when the organization can search or export them. D7-SPANS-HELD also accepts a trace backend contractually available to the assessor. A supplier-held control with no inspectable output, viable customer test, or specific attestation is unanswerable. Supplier ownership alone never makes an existing action not applicable. [[agentic-ai-security-cmm-2026|The CMM core]] defines cumulative scoring, and [[agentic-ai-security-cmm-measurement-protocol|the Assessor's Handbook]] records each determination and its confidence.

An action includes a tool call, a write to reusable agent memory, and an answer whether or not the agent holds a tool. An answer is content the agent delivers to a person or a calling workflow as the result of a request or scheduled task. A tool includes a function, connector, command, MCP server, or another agent called through an orchestrator. An agent memory write persists content for a later task, session, or agent. A checkpoint read only when the same session resumes is session state. A record can join to identity and context through a trace, session, or run identifier. “Telemetry the organization holds” means records it can search and export through its own store or contracted vendor access. A vendor console that only shows a chart or individual event does not meet that condition. The D2 identity inventory supplies the population and the human accountable for each run, including the owner of a scheduled run with no human initiator. [[agentic-ai-security-cmm-d2-identity|D2]] grades the identity binding. D7 grades the records and detectors that use it.

Each criterion applies to every instance in its stated population. Sample each agent and every tool type it can call; use configuration and event counts to find unobserved paths. A missing field, excluded production route, or unreviewed alert fails the relevant criterion. A criterion is not applicable only when its governed activity is absent, with that absence documented. The assessor records unanswerable when the activity exists but evidence cannot establish the result. L1 is the lowest described state, not an absence-of-evidence score. The level reached is the highest for which every applicable criterion at that level and below is met.

## Failure paths

Untrusted retrieved content can cause an agent to call a tool outside its task, change memory, or persuade a human to approve a harmful action. The records must connect the input, agent, task, approval, tool call, and result so that an investigator can distinguish the attempted crossing from the executed effect. A detector also needs a tested route into triage: a dashboard that nobody reviews does not interrupt a campaign. An orchestrator can hide an unlogged subagent step if the workflow and action logs are never reconciled. The [OWASP AI Exchange monitoring control](https://owaspai.org/go/monitoruse/) describes action and memory audit records, while its [oversight control](https://owaspai.org/go/oversight/) addresses changed approval behavior and anomalous paths.

## Level progression

| Level | Observable outcome |
|---|---|
| L1 | Agent actions cannot be reconstructed from records the organization holds. |
| L2 | Every tool call and answer has a searchable record, time, and accountable human. |
| L3 | Attributed action, memory, and relevant guardrail records join into a retained trace with a controlled schema. |
| L4 | Production detections use that evidence, and control changes, workflow steps, evaluation results, and log integrity have inspectable outcomes. |
| L5 | Alert handling closes within a stated service level; applicable inter-agent baselines and cascade rules operate against tested paths. |

## Criterion catalogue

The definitions below are the assessment conditions. “Alert” means an event delivered to a security monitoring queue with a triage record. A private message or graph is insufficient. An analyst-actionable alert leads to escalation, containment, or a control change beyond closure as benign. A detector “in production” reads production activity as it arrives. A negative test exercises the actual route or control where the criterion requires one, not a mock that cannot fail the deployed path. The organization can choose its own threshold, but must state it before grading outcomes. [OpenTelemetry's GenAI conventions](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/README.md) offer one trace schema. D7 tests reconstruction and compatibility rather than requiring a specific experimental release.

### L2 detail

- **D7-LOG.** Every tool call and every answer each agent makes has a record in telemetry the organization can search and export. The record gives the event time, identifies the tool or answer, and names the human accountable for the run or, for a scheduled run, its accountable owner. An answer record may hold a protected reference instead of answer text. The writer may be the agent or another component. A record visible only in a vendor console, a missing answer from a tool-using agent, or a record with no event identity or accountable human fails. *Evidence:* sampled tool-call and answer records from each agent and output route, with the store and search or export that reaches them.

### L3 detail

- **D7-ATTRIBUTE.** Every record of an agent's action names the agent and the human accountable for the action, in the sense the grading words give "names". *Evidence:* a sample of action records, each set against the agent's identity in the inventory and the human of the session it belongs to.
- **D7-FORWARD-ESCAPE.** Each sandbox that an agent's code or commands run in forwards its escape indicators to the deployment's telemetry, in the three classes the [OWASP AI Exchange sandbox guidance](https://owaspai.org/go/agentsandboxing/) names: unexpected system calls, access to forbidden filesystem paths, and connections its network policy does not permit. A sandbox that forwards nothing, or forwards two classes and not the third, fails the criterion. Not applicable where no agent executes code or commands. *Evidence:* the sandbox's forwarding configuration, with a forwarded event of each class, from the telemetry or from a test.
- **D7-FORWARD-SCAN.** Where an agent retrieves from a corpus, each investigable poisoning finding that the applicable [[agentic-ai-security-cmm-d6-data-rag|D6]] screening process produces reaches telemetry with the corpus, screening run, and affected item identified. A missing D6 control is a failed prerequisite, not a reason to mark this criterion inapplicable. Not applicable where no agent retrieves from a corpus. *Evidence:* the D6 control output and forwarding configuration, with a finding forwarded from production or a test.
- **D7-LOG-MEMORY.** Every write an agent makes to agent memory is recorded in the deployment's telemetry with its time, the writing agent and its session, the store and the entry written, a hash or a summary of the content, and the content's source, given directly or through the trace or session identifier that joins the write to the call or input it came from. Not applicable where no agent writes to agent memory. *Evidence:* the telemetry records of a sample of memory writes, set against the store's own view of the entries.
- **D7-LOG-SCHEMA.** Besides D7-LOG's time and tool, the records of every tool call give together the call's arguments, the target resource it reached, its outcome, and the trace or session identifier that joins them to the rest of the session. The arguments can be reduced to a hash, a summary or a redacted copy, and the target can be a record, a file, an account or a tenant. The records of a call that writes also carry a reference that locates what it changed, such as the value it replaced, the identifier of the record it created or the commit it pushed, and a call that only reads needs none. Not applicable where no agent holds a tool. *Evidence:* the records of a sample of calls to each tool, read field by field.
- **D7-RETAIN.** The organization states, for each kind of record the L3 criteria read, the period its records stay searchable, and every store that holds such records keeps them searchable for at least that period. A kind of record with no stated period, or a store whose retention falls short of the period stated for its records, fails the criterion. *Evidence:* the stated period for each kind of record, with the document that states it, set against each store's retention setting.
- **D7-SPANS.** Each performed inference, tool execution, agent invocation, and retrieval appears in a correlated trace attributed to the agent and session. An operation visible only as an uncorrelated HTTP request fails. *Evidence:* backend traces for each applicable path and a reconstruction of one sampled action; OpenTelemetry GenAI spans are one implementation.
- **D7-SPANS-HELD.** Each required trace reaches a backend the organization can search and export, or one contractually available to the assessor. A pipeline that silently drops required paths fails. *Evidence:* collector/export configuration, pipeline loss controls, and successful lookup of sampled production traces.
- **D7-SPANS-PIN.** The emitting instrumentation and trace schema have recorded versions or a compatibility contract, and a change that removes a field needed for reconstruction or detection is caught before use. A floating experimental schema with no compatibility test fails. *Evidence:* schema contract, instrumentation version record, and a field-compatibility test.
- **D7-PROMPT-CANARY.** Each effective system prompt carries a planted canary, and every agent output and tool argument is checked for it. A match enters the security triage queue. A token that is planted but never checked on tool arguments fails. For a supplier-held prompt, use supplier evidence or a customer test; unavailable material evidence is unanswerable. *Evidence:* token record, check configuration, and alert or test.

### L4 detail

- **D7-IDENTITY-BASELINE.** Each non-human identity used by the deployment has a normal-activity baseline covering resources and origin, and a production detector alerts on a departure. A dashboard baseline with no rule fails. *Evidence:* per-identity baseline, detector, and an alert or test.
- **D7-ESCAPE-RECORD.** Every out-of-task call that D3 task-scope enforcement denies emits an escape event naming the agent, task, tool, and violated scope. A generic denial that cannot identify the task fails. *Evidence:* event schema and a sampled denied call joined to its escape event.
- **D7-BASELINE.** Each agent that can call a tool has a recorded baseline of its tool calls, covering which tools it calls, how often and in what order, built from the deployment's telemetry, and a detection in production compares the agent's calls with the baseline and raises an alert when they depart from it. A baseline read from a dashboard or a periodic report, with no detection behind it, fails the criterion. Not applicable where no agent can call a tool. *Evidence:* the baseline record for each applicable agent and the detection rule, with an alert it raised or a test of it.
- **D7-CONTROL-APPROVE.** A detection raises an alert when an action that requires a human approval, as [[agentic-ai-security-cmm-d3-control-least-agency|D3]] defines one, runs with no approval on record, and when an approval step begins to approve without a person, such as an action class re-tiered to skip confirmation after its first run in a session, or a queue that approves automatically under latency or throughput pressure. Not applicable where no action any agent can invoke requires a human approval. *Evidence:* the detection rule over the decision path's approval records, with an alert it raised or a test of it.
- **D7-CONTROL-RELAX.** A detection raises an alert on each change that relaxes a control of the deployment, whoever makes it, the agent, another agent or a person, such as a policy that permits more, a raised rate limit, a disabled check, a widened scope, a removed approval requirement or a reduced logging setting. A change record that no rule reads, or a notice sent to one person, fails the criterion. *Evidence:* the list of the deployment's controls, each with the change record it writes and the rule that reads that record, and an alert a relaxation raised or a test of it.
- **D7-DRIFT.** A detection in production scores each session for progressive relaxation across its turns, such as a request refused early in the session and fulfilled later, or tool calls widening in scope from turn to turn, and raises an alert when a session's score passes a threshold the organization sets. A check that reads each request with no memory of the session's earlier turns fails the criterion. Not applicable where no agent's session runs past one turn. *Evidence:* the scoring configuration with its threshold, and a scored session from the period.
- **D7-DRIFT-ROUTE.** Each alert D7-DRIFT raises goes to a disposition set before the session runs: review by a person in a named role, or automatic suspension of the session. An alert that reaches no reviewer and suspends nothing fails the criterion. Not applicable where D7-DRIFT is not applicable. *Evidence:* the routing rule, and the disposition log for the period, with each routed or suspended session and its outcome.
- **D7-EVAL.** Each agent is evaluated at least once a quarter, and each quarter's evaluations together cover two or more threat categories, state their coverage criteria before they run, such as the categories, the layers tested and the corpus size, and report each category left untested. A quarter with no evaluation, or evaluations whose reports state no coverage, fails the criterion. *Evidence:* the evaluation schedule with its category list, and the report of each run in the period with its coverage statement.
- **D7-EVAL-SESSIONS.** Each quarter's evaluations include a scenario that carries an attack across two or more sessions, through content one session writes and a later session reads. Not applicable where nothing an agent writes in one session can reach an agent's context in a later session. *Evidence:* the multi-session scenario list, with the results of each scenario.
- **D7-EVAL-TURNS.** Each quarter's evaluations include multi-turn jailbreak and escape scenarios, run and reported apart from the single-turn cases. Not applicable where no agent's session runs past one turn. *Evidence:* the scenario list with each scenario's turn count, and the multi-turn results reported apart.
- **D7-LOG-OUTSIDE.** A record of each tool call and answer, carrying at least what D7-LOG requires, is written by a component outside the agent, such as a gateway, a decision point or an output service, into a store that no identity the agent holds can write to, change or delete. The agent's own spans and log lines count toward the L3 criteria and toward no part of this one, whoever ships them. *Evidence:* the writing component's configuration and identity, output-route coverage, and the store's access policy set against every identity the agent holds.
- **D7-LOG-TAMPER.** A test run from the position of each identity the agent holds, against the log pipeline and the store that production writes to, shows that the agent cannot suppress, alter or delete the records D7-LOG-OUTSIDE requires. A test against the store's documented configuration, or against another store, fails the criterion. *Evidence:* the test record, naming the pipeline, the store, the identities tried and what each attempt did.
- **D7-ORCHESTRATE.** Each orchestrator's workflow, meaning the tasks it delegates, the agent each goes to and the result each returns, is recorded in a workflow log held outside the orchestrator's memory, written by a component outside the orchestrator into a store that no identity the orchestrator holds can write to, change or delete. Not applicable where no agent delegates a task to another agent. *Evidence:* the workflow log's writer and store, with the store's access policy set against the orchestrator's identities, and a sample of its entries.
- **D7-ORCHESTRATE-RECONCILE.** A reconciliation compares the actions the delegated agents and their tools recorded with the steps the workflow log holds, at the end of each workflow or on a schedule the organization sets, and raises an alert on each action no logged step accounts for. Not applicable where no agent delegates a task to another agent. *Evidence:* the reconciliation rule with its schedule, and its results for the period with each alert.
- **D7-POSTURE.** A posture inventory covers each account, subscription, or tenant hosting the deployment and each agent in scope. Current configuration findings reach a security queue or a scheduled review that records dispositions. A discovered inventory with findings nobody handles fails. *Evidence:* coverage comparison and sampled findings with dispositions; no particular posture product is required.
- **D7-REVIEW-EVASION.** A detection raises an alert on inputs timed or shaped toward the paths with the weakest human review, such as requests clustered off-hours, actions split across bulk operations, or content routed through a channel the review trusts. Not applicable where no action any agent can invoke passes a human review. *Evidence:* the detection rule with the review paths it compares, and an alert it raised or a test of it.

### L5 detail

- **D7-A2A-BASELINE.** Where agents exchange Agent2Agent messages, each agent has a baseline for peers, message types, and volume, and a running detector alerts on relevant departures. A baseline with no message records or alert rule fails. Not applicable where the assessed agents exchange no Agent2Agent messages. *Evidence:* message records, baseline, running rule, and alert or test.
- **D7-ALERT-ACTIONABLE.** The share of the L4 detections' alerts that analysts triaged as analyst-actionable is measured each period and compared with a target the organization documents, and a period below the target is recorded with the tuning that followed. *Evidence:* the actionable-rate report for each period, with the documented target.
- **D7-ALERT-LOOP.** Each alert the L4 detections raise reaches, within an SLA the organization documents, a change to a control, a recorded decision to leave the controls unchanged, or, for a false positive, a tuning record for the detection that raised it. *Evidence:* the SLA and the controls-update log for the period, with each alert matched to its change, decision or tuning record.
- **D7-CASCADE.** Where multiple agents can propagate a harmful instruction or action along a real workflow path, production rules detect the cascade paths selected by that deployment’s threat model and alert at tested thresholds. A generic “cascade” feature with no path test fails. Not applicable to a single-agent deployment or to a multi-agent topology whose threat model establishes no cascade path. *Evidence:* path model, running rules, threshold tests, and alerts.
- **D7-JOINT.** Where agents interact or share tools or state, a detector compares their joint behavior with a recorded baseline and alerts on workflow-relevant departures, such as coordinated calls or unusual shared-tool sequences. Separate per-agent baselines alone fail. Not applicable where there is no interacting or shared-resource agent pair. *Evidence:* joint statistics, running rule, and alert or test.
- **D7-PLAYBOOK.** The security monitoring function runs a playbook in production for each class of alert the L4 detections raise, and each playbook carries the agent-specific steps: identifying the agent and its accountable human from the telemetry, and stopping or containing the agent through the control that does so, such as D2-KILL's kill switch or D3-ADAPT's step-down. A generic incident playbook with no agent-specific step fails the criterion. *Evidence:* the playbook for each alert class, with its run history for the period.

## Prerequisites and blockers

D7-ATTRIBUTE and the per-agent baselines require an identity that the organization can resolve to one agent. A weak [[agentic-ai-security-cmm-d2-identity|D2]] identity record makes those D7 criteria fail or unanswerable on their own evidence. D7-FORWARD-ESCAPE depends on a real [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] sandbox when agents execute commands. D7-FORWARD-SCAN depends on the corpus screening process assessed in [[agentic-ai-security-cmm-d6-data-rag|D6]]. D7-CONTROL-APPROVE needs [[agentic-ai-security-cmm-d3-control-least-agency|D3]] approval records so that a bypass can be distinguished from an action with no approval requirement. The assessor names the missing upstream artifact and the D7 criterion it blocks.

A detection that reads model-generated reasoning must treat the trace as attacker-influenced output. Compare a stated intent with the action and side-effect records before concluding that a boundary held. The [OWASP monitoring guidance](https://owaspai.org/go/monitoruse/) makes the same forensic distinction. A model-based detector also needs an isolated rule and evidence store, because an agent that can edit its own detector can hide the action the detector exists to report.

## Deployment-shape differences

| Shape | Material D7 difference |
|---|---|
| Tool-free assistant | D7-LOG covers its answers as it does every agent's; tool-call schema and tool baseline are conditional on actual tools. |
| Hosted productivity assistant | Supplier audit schema and export determine reconstruction; a console-only view cannot establish the records. |
| Coding agent | Shell calls, file writes, prompt canary checks, and sandbox escape indicators broaden the observed paths. |
| Multi-agent workflow | Workflow logging and reconciliation apply where delegation occurs; joint and cascade checks require real interaction or shared-resource paths. |

The deployment’s threat model determines which multi-agent paths need a cascade rule. A single-agent deployment cannot be failed for lacking a cross-agent detector. A multi-agent topology with no harmful propagation path can record D7-CASCADE as not applicable only when its threat model supports that conclusion. A shared tool, mailbox, or repository can create a joint-behavior path even when agents do not directly message one another.

## Implementation and effort drivers

The cost driver is the volume retained, queried, and reviewed, plus the work to join records across providers. Start with the action fields used by a real incident reconstruction, then protect the record and add the detections that can consume it. Hash or redact arguments and memory content when raw text is unnecessary; preserve the target, outcome, content reference, and correlation key. Retention needs a stated forensic window and a disposal rule for sensitive telemetry. Baseline rules need tuning and triage capacity. An alert that consumes analyst time but never leads to a decision has an operating cost without a measured response.

A product may supply collection or anomaly scoring, but its release stage is no maturity criterion. [Google Cloud's Agent Anomaly Detection](https://docs.cloud.google.com/gemini-enterprise-agent-platform/agent-anomalies-overview) documents cascade and rogue-behavior detectors in **allowlisted Preview**, not general availability, as of this review. D7-CASCADE and D7-JOINT therefore grade a deployed, tested capability on the customer’s actual paths; a preview product description by itself is no evidence. Forward-pass activation monitoring has no scored D7 criterion.

The dated [[microsoft-entra-agent-id|Microsoft Entra Agent ID]] profile distinguishes identity issuance from Agent 365 telemetry entitlements. Assess deployed event coverage and export against D7 rather than infer a level from either product.

## Sources and material limits

The [OWASP AI Exchange's monitoring](https://owaspai.org/go/monitoruse/), [oversight](https://owaspai.org/go/oversight/), and [agentic testing](https://owaspai.org/docs/5_testing/#agentic-ai-security-testing) guidance provides signal and test patterns. [NIST AI 800-4](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.800-4.pdf) describes deployed-AI monitoring challenges; it does not prescribe these levels. [OpenTelemetry GenAI semantic conventions](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/README.md) can carry interoperable traces, but schema movement and vendor-held gaps require explicit version and coverage checks. The thresholds, lookback windows, and cumulative placement in this page are AAI-S CMM assessment choices. No published detection rate is inferred from the existence of a detector.

Cross-session series detection remains ungraded; [[cmm-known-limitations|CMM Known Limitations (current state)]] records the open D4/D7 boundary.
