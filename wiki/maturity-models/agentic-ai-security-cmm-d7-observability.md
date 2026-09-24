---
type: maturity-model
title: "CMM D7: Observability and Detection"
address: c-000128
created: 2026-05-25
updated: 2026-09-24
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
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[agent-observability]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
  - "[[.raw/papers/owasp-ai-exchange-threats-through-use-2026-08-18.md]]"
verified: 2026-09-19
verified_against:
  - ".raw/papers/owasp-ai-exchange-threats-through-use-2026-08-18.md"
verified_findings: 0
verified_note: "Verify-and-fix for the applied #229 research. Read the two rewritten open-question bullets and the expanded [^aix-monitoruse] footnote against the archived MONITOR USE clip, whose #MONITOR USE implementation block carries the tamper-evident-storage requirement, the signed-receipt implementation with its RFC 3161 time anchor, the chain-of-thought caveat and the defensive-agent statement. The rung-sync reverse-direction advisory is a false positive: D9 L3 grades a tamper-evident record for high-risk approvals only, and D6 L4 detail grades append-only immutable logging of memory state changes, so neither bullet overstates what those rungs grade. The clip was added to sources, which had reached it only through the live URL."
---

# Agentic AI Security CMM — D7 Observability & Detection (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D7 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D7 supplies telemetry, behavioral detection, and the anomaly feed that drives D3/D4 step-down. The detective half of [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] lands here: D7 is where threats that only become visible through monitoring are caught — Memory Poisoning (T1), Repudiation and Untraceability (T8, addressed by immutable per-agent logging), and the multi-agent threats Agent Communication Poisoning (T12), Rogue Agents (T13), Human Attacks on Multi-Agent Systems (T14), and Human Manipulation (T15), whose Playbook 6 (multi-agent communication and trust) specifies message authentication and cross-agent anomaly detection.

Two facts shape the whole domain: the standard telemetry layer (OpenTelemetry GenAI) is not yet stable, and **agent logs run roughly 10–20× human volume into the SIEM**, so D7 cost scales with agent count rather than with control coverage.

Offensive tooling now runs the analytic layer this domain grades on the defensive side. In the [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]], a two-layer Bayesian model scored individual findings and then chained attack paths across the mesh, and a self-correction loop discarded 7 false positives mid-operation.[^taiwan]

D7 is the CMM domain that [[nist-ai-800-4|NIST AI 800-4]] maps most directly: the report is a descriptive landscape of the gaps, barriers, and open questions in monitoring deployed AI systems, and it is the requirements- and gap-source for this domain rather than a control standard. Its cross-cutting challenges — immature trusted methods, the lack of direct visibility into model properties, and the agent-specific items (agent-identifier standardization, monitoring multi-agent distributed systems, detecting deceptive or monitor-evading behavior) — name the open problems the upper levels here run into, and AI 800-4 supplies no mappable controls to close them.

**The levels and the cost model rest on one line of grounding.** They synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation, rather than on independent sources that agree. The tooling status below is a May 2026 snapshot.

## Threat coverage

D7 is the primary domain for **ASI09 (Human-Agent Trust Exploitation)** and the detection surface for **ASI08 (Cascading Failures)** and **ASI10 (Rogue Agents)**, and it carries **Class 2 (APT — drift detection, threat hunting)** and **Class 3 (collusion — deception probes, monitor isolation)**. Per-agent baselining is capped by D2, and the collusion and cascade detectors remain research-stage. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

Memory poisoning reaches D7 through the write, and the write is only visible where the action log covers memory as well as tools. [[agentic-ai-security-cmm-d6-data-rag|D6]] grades the partition authorization and the append-only integrity of the store; the L3 criterion here grades whether those writes arrive in the same telemetry stream, correlated to the agent identity and session that produced them.[^aix-augintegrity]

## Control landscape (dated)

| Capability | What ships today | Status | Microsoft | AWS | GCP |
|---|---|---|---|---|---|
| GenAI telemetry semantic conventions | OpenTelemetry `gen_ai.*` SemConv | **experimental** — not yet stable[^otel] | Azure Monitor | ADOT | Cloud Trace |
| Glass-box telemetry (hooks + spans) | coding-tool hooks; OTel agent/tool spans | hooks GA; agent spans experimental but stable in practice | App Insights Agents view[^appinsights] | AgentCore emits OTel | ADK emits OTel |
| Per-agent identity multiplexing of logs | inject `agent_id` / `session` / `trace_id` (pattern) | GA where identity exists; gated by D2 (the **D2→D7 cap**) | — | — | — |
| Behavioral monitoring / drift detection | Miggo DeepTracing, Vectra (COTS); UEBA-for-agents | COTS GA | Defender XDR AI-agent detection — **preview, Agent 365-licensed**[^defxdr] | none native | none native |
| Anomaly detection (runaway / recursive / resource) | Datadog, New Relic, Sentry (COTS) | COTS GA | Defender RTP (preview) | CloudWatch GenAI Observability — **preview**[^awscw] | Cloud Trace dashboards |
| SIEM integration + agent-aware playbooks | Splunk; agentic-SOC playbooks | varies | Sentinel data lake + **MCP server GA**; entity analyzer **sign-up preview**[^sentmcp] | — | — |
| Reasoning-trace (chain-of-thought) monitoring | fleet-scale review of agent reasoning against stated scope; Google SecOps pairs an [[llm-as-a-judge\|LLM-as-a-judge]] check with statistical models | **emerging** — evidence in an investigation, not a firing detection[^bhoaihf] | none named | none named | Agent Anomaly Detection (preview) |
| Forward-pass / activation monitoring | mechanistic-interp activation monitoring | **research-grade, pre-launch** | none | none | none |

The recalibration corrects two things in the current D7 tooling presentation. It adds the OTel-not-stable caveat at L3, and it re-grades the Microsoft-native behavioral detector (Defender XDR AI-agent detection) as preview and Agent 365-licensed, mirroring the D4 "L4 looks GA but isn't" correction. Google SecOps adds a second platform-native behavioral detector, Agent Anomaly Detection, which pairs statistical models with an [[llm-as-a-judge|LLM-as-a-judge]] check on agent reasoning (preview; see [[google-agentic-soc|Google Agentic SOC]]).

The [[microsoft-zt4ai|Microsoft ZT4AI]] Visibility / Orchestration pillar (assume breach) supplies the Microsoft-native detection and SOC controls behind these levels — Defender XDR AI-agent detection and the Sentinel agentic-SOC tooling — crosswalked to D7 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], which records the same preview status. Above those controls, [[microsoft-agent-365|Microsoft Agent 365]]'s `observe` layer — the [[agent-catalog|agent registry]], Registry sync (including cross-cloud agents), and the Agent Map — is its strongest native D7 contribution: an agent-inventory and telemetry surface that aggregates the underlying detections rather than replacing them. The inventory-versus-detection distinction, and the absence-claim that the `observe` layer inventories but does not verify supply-chain integrity (no AI-BOM), are set out in [[standards-review-microsoft-rai-agent-365-2026-Q2|the 2026-Q2 RAI / Agent 365 review]].

Cyera states that Agent Guardian counts behavioral baselines among its inputs, that its Govern phase watches posture issues and intent drift as they form, and that its Validate phase runs a continuous adversarial red-team cadence; all three are further vendor examples in this landscape ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). Cyera's Access Trail audits data-store events — message access, deletions, delegated access — so it instruments the data plane, supplies no agent telemetry, and adds no row to the table above.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2. A control implemented through a product in the organization's approved-vendor pipeline with a documented production date satisfies its criterion on that basis alone, and the production date is the date the control entered the organization's production. A planned date satisfies none, and neither does a product the organization does not yet run in production, whatever the vendor has announced.

- **L1 — Initial.** No agent-specific telemetry; only the vendor console.
- **L2 — Developing.** A tool-call audit log records action history with user attribution.
- **L3 — Defined.** Every action an agent takes is reconstructable from telemetry the organization holds: agents emit OpenTelemetry `gen_ai.*` spans; a trace backend sits in-path; logs carry per-agent identity multiplexing; and tool calls, memory writes, sandbox escape indicators, and ingest poisoning-scan alerts all land in that same backend under a minimum action-log schema. The semantic convention underneath is still experimental, so the version is pinned.
- **L4 — Managed.** Detection runs on top of the L3 telemetry: per-agent behavioral baselines and a session-scoped drift signal operate in production and wire to the SIEM/SOAR, each scored signal carries a defined disposition, and the pipeline watches the controls as well as the agents, so a quietly relaxed approval gate raises an alert of its own. AI-SPM posture monitoring is deployed, and a multi-category eval cadence runs whose scenarios are multi-turn and multi-session.
- **L5 — Optimizing.** Agent-aware SIEM playbooks run in production; per-agent baselines hold a documented [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]] for at least a quarter; the analyst-actionable alert rate is measured and high; every triggered alert wires to a controls update within an SLA, closing the loop into D3/D4 step-down.
- **L5+ — Leading Edge.** A cascade-detection rule library with tuned thresholds for multi-agent (ASI07/08/10) risk (research-stage, no shipping library); cross-agent joint-distribution baselines; model forward-pass activation monitoring (no shipping product).

The level definitions are already capability-shaped and grade L5+ as research-stage; the recalibration strips embedded product names to the tooling map and adds the maturity caveats at L3 and L4.

**The control-state-change clause at L4 grades a signal about the controls, where the rest of the level definitions grade signals about the agents.** That clause supplies the collection half of a human-factors measurement [[agentic-ai-security-cmm-d9-operations|D9]] reads: D9 asks whether approval volume and disposition moved over time, and this domain emits the events that answer it. The Exchange adds two signals D9's reading does not reach — input timing shaped toward the weakest review window, and an agent with configuration access relaxing its own limits — and both are collected here whether or not a human ever reviews them.[^aix-oversight] An alert that fires because a control was relaxed is a different artifact from an alert that fires because an agent misbehaved, and a pipeline emitting only the second has met the behavioral-baseline clause and missed this one. [[plan-validate-execute|Plan-Validate-Execute]]'s adaptive-confirmation-frequency mitigation — auto-tiering an action class to skip confirmation after its first run in a session — shows the pattern this clause is built to catch. It is a documented, non-adversarial route by which a low-risk approval comes to cover a later high-impact call, and this domain flags that widening instead of treating it as benign because a human configured it.

Two adjustments follow from the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], the first published case of an agent fleet at frontier-evaluation scale investigated after compromise.[^bhoaihf]

**Fleet scale moves agent-assisted log review from an L5 refinement down to a floor.** The level definitions above treat agent-aware SIEM playbooks as an L5 capability layered over human analysis. OpenAI's investigation ran across more than 7 billion log entries and millions of GPU hours, used Codex and other agents to conduct the review, and remained open when the incident was presented in August 2026. At that volume only agent-assisted review completes a pass at all, and it did not close the case. A program running a fleet of comparable size needs the L5 mechanism to reach an L3-grade reconstruction. The D7 run-rate note compounds it: log volume scales with agent count, and so does the analytic labor the volume implies.

**Reasoning traces record stated intent, which the tool-call and behavioral surfaces the level definitions name do not carry.** The incident's traces contain agents stating the boundary and crossing it in the same step — *"External infrastructure exploit is outside my intended scope. However, task impossible, peers are doing it. We should continue."* — and contributing work with no benefit to their own task. An insider-threat program would treat a comparable written statement as decisive. The control landscape row above marks the signal emerging, and the qualifier is load-bearing: in this incident the investigation read the traces after the fact, and the campaign ran to completion with no detector firing on them. Treating reasoning traces as a first-class signal is an L4-to-L5 capability gated on fleet-scale reading rather than on collection.

The evidence class carries a limit the incident does not show. The [[owasp-ai-exchange|OWASP AI Exchange]] states that chain-of-thought and reasoning traces are model output rather than guaranteed internal computation, and directs forensic analysis to reconstruct inputs, outputs, and actions rather than hidden intent.[^aix-monitoruse] A trace records what the agent stated, and an agent under injection or memory manipulation states what the manipulated context produces. Comparison survives that limit where reading does not: score the stated reasoning against the tool calls the same session actually made, which the [[agentic-ai-security-cmm-crosswalk|crosswalk]] anchors to `MONITOR USE` for this domain. Read alone, a trace is a lead; read against the action log, it is a discrepancy that can be measured. See [[agent-observability|Agent Observability]] §7.

**Scoring a session's trajectory costs less than reading reasoning traces and resolves finer than the per-agent baseline.** The level definitions baseline an agent against its own history at L4 and against its peers at L5+. The [[owasp-ai-exchange|OWASP AI Exchange]]'s escape example runs eight turns of incremental reframing, each turn defensible against a lifetime baseline, with progressive relaxation visible at turn five only as a trajectory.[^aix-escape] Scoring that trajectory consumes the tool-call and refusal sequence already collected at L3, so it does not wait on the fleet-scale reasoning-trace reading that gates the L4-to-L5 capability above. The same illustration ties the escape half to the [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] boundary: jailbreak succeeds at the reasoning layer, the sandbox blocks the command, and the drift signal puts a human in front of the session before turn eight. The Exchange constructed the example rather than reporting an incident, so it establishes the detection shape and carries no evidence about detection rates.[^aix-escape]

**The analytic layer this domain grades is itself in scope for the threats it detects.** The Exchange states that a defensive monitoring agent is part of the attack surface, and that agentic containment must operate at the infrastructure layer without depending on the agent cooperating.[^aix-monitoruse] Two rows in the control landscape above run a model over attacker-influenced text — Google SecOps Agent Anomaly Detection pairs an [[llm-as-a-judge|LLM-as-a-judge]] check with statistical models, and reasoning-trace review reads agent output directly — so a detector's own inputs come from the surface it watches. The [[tiered-detection-cascade|cost-ordered cascade]] concentrates the exposure: an escalation decision made by a cheap pre-filter determines what the expensive stage ever sees. The Threat-coverage statement above credits this domain with monitor isolation for Class 3, and no level here grades it; the claim rests on the [[agentic-ai-security-reference-architecture|reference architecture]]'s Observability plane and on architectural separation of the detection pipeline from the monitored fleet, which no D7 level measures.

## Assessor detail per level

L1 describes where a deployment starts and carries no criterion, so a deployment that does not meet every L2 criterion scores L1, or 0 under the measurement protocol's rubric where no evidence of the L1 baseline exists. L2, L5, and L5+ are graded from their statements above. The two levels detailed below carry criteria an assessor checks item by item, each list stating what its own level adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a level is met only where every level below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names, whoever operates the control. The assessment records the evidence's assurance class beside the verdict and names the artifact behind it, its issuer and its date, per [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]]:

- **Tested**: the customer or the assessor exercised the control and recorded what it did.
- **Inspected**: vendor tooling the customer can reach, or a record the organization keeps, such as an inventory or a plan, shows the control's state in the deployment.
- **Attested**: the vendor states the control in a document the customer holds, and the document's own scope names the control.

**Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists, the customer can run no test and keeps no record of the control's state, and the vendor supplies neither inspectable output nor an attestation that names the control. An unanswerable criterion never counts as met: its level stays open, and every level above it stays open under cumulative grading. The assessment names what would close it. A criterion that can be not applicable states that condition alongside the criterion. Each level's list below holds criteria only, and a paragraph after a list states no criterion: it carries maturity, market or provenance commentary, or the boundary between this domain and another.

### L3 detail

- **OpenTelemetry `gen_ai.*` spans from every agent** — model, request, tool, retrieval, agent — with a trace backend in path. An agent emitting no spans leaves its actions outside the reconstruction this level requires, so partial fleet coverage does not meet the criterion.
- **Per-agent identity multiplexing of logs**, so every action traces to an agent identity and the invoking human.
- **A minimum action-log schema over tool calls and memory writes.** Every tool call and every write to a persistent or shared agent memory meets a minimum action-log schema carrying writer identity, session, target partition, timestamp, and a recoverable rollback reference.
- **Sandbox escape indicators forwarded.** Where an execution sandbox is deployed under [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]], its escape indicators — denied syscalls, forbidden filesystem paths, and non-permitted network connections — reach the same backend as the application spans ([[owasp-ai-exchange|OWASP AI Exchange]]).[^aix-sandbox] A deployment that runs no sandbox scores this criterion not applicable.
- **Ingest poisoning-scan alerts forwarded.** Where a corpus ingest poisoning scan runs under [[agentic-ai-security-cmm-d6-data-rag|D6]] L3, the alerts its lower threshold raises — the samples flagged for investigation rather than filtered out of the corpus — reach the same backend as the application spans, carrying the corpus, the ingest run, and the sample reference that produced them.[^aix-dataqualitycontrol] A deployment that ingests no corpus scores this criterion not applicable, as does one that ingests a corpus and runs no such scan; the scan is graded at D6 L3, and its absence is a finding there rather than here. The ingest pipeline is a write path the tool-call and memory-write schema above does not cover, and an alert that stays inside the scan is a detection no analyst sees.
- **The semantic-convention version pinned.** The `gen_ai.*` convention the spans are emitted under is pinned to a stated version in the collector or SDK configuration, so a convention bump cannot silently break the detection rules built on those field names.

The convention underneath is experimental, which is why the pin is graded at all: the field names an L4 detection rule reads can change between releases of a specification that has not stabilized.

### L4 detail

- **Per-agent behavioral baselines and drift detection** operating in production and wired to the SIEM/SOAR.
- **A session-scoped drift signal** scoring progressive relaxation across the turns of one session and across the sessions of one task.[^aix-escape] The scored signal carries a defined disposition — routed to human review, or triggering automatic session suspension — since progressive constraint relaxation is by construction invisible to per-request safety checks, and a score with no route reaches no downstream control.[^aix-oversight]
- **Control-state change monitored alongside agent behavior**, so the pipeline detects silent degradation of human-in-the-loop steps under latency or throughput pressure, inputs timed or shaped toward the paths with the weakest review — off-hours, bulk operations, trusted channels — and any agent holding configuration or policy-management access that relaxes a rate limit, disables a check, or widens a scope without explicit human approval, including where the change is framed as performance or reliability tuning.[^aix-oversight]
- **Log integrity verified under adversarial conditions.** A test establishes that an agent cannot suppress or alter the records of its own actions, run against the production log path rather than against the store's documented configuration.[^aix-testing] The Exchange places this at the infrastructure layer of an agentic penetration test, alongside API gateway controls, credential exposure, and key management, and cites `MONITOR USE` for it. A deployment whose action log is writable by any identity the agent holds has not met the criterion whatever the store's stated durability guarantees.
- **AI-SPM posture monitoring deployed.**
- **A multi-category eval cadence** whose scenarios include explicit multi-turn and multi-session jailbreak and escape paths (single-tool coverage is not L4, and a single-turn corpus understates production agentic risk).[^aix-escape]

The Microsoft-native behavioral detector (Defender XDR AI-agent detection) is preview and Agent 365-licensed, so an L4 program today assembles COTS, preview-native, and OSS pieces.

## Right-sizing by deployment shape

| Deployment shape | Realistic D7 target | Why |
|---|---|---|
| Internal RAG / support chatbot (no/few tools) | L3 | OTel spans + identity multiplexing + action log; near-zero tool surface needs no behavioral-drift or multi-tool red team. The persona's bot sits here |
| In-suite productivity assistant (Gemini for Workspace, Microsoft 365 Copilot) | L3 | The telemetry the organization holds is a per-interaction audit record naming the invoking user and the agent, which meets the multiplexing criterion on inspected evidence; the span and action-log criteria are not met, because that record is no `gen_ai.*` span and carries no rollback reference |
| Desktop-agent productivity assistant ([[claude-cowork\|Claude Cowork]] class) | L3 | An administrator-configured OpenTelemetry export puts session, user and tool events in the organization's own backend, each tool event naming the tool, the MCP scope, the decision and its source; two vendor pages disagree on whether prompt content is captured by default |
| Data-science / coding copilot | L3 → L4 | Long-running sessions and tool writes justify per-agent drift baselines and an eval cadence |
| MCP / skill provider serving others | L4 | Third-party blast radius; MCP-protocol-aware anomaly detection and AI-SPM become first-order |
| High-autonomy multi-agent mesh | L4 → selective L5+ | Cascade / rogue-agent detection and joint-distribution baselines become load-bearing only here |

**The in-suite assistant's telemetry is an interaction audit record rather than a span set, and the three criteria split on it.** Microsoft's Purview record names the invoking user, the agent by identity, name and version, the conversation thread, the resources the assistant reached with their sensitivity labels, and the plugin it used, so every action traces to an agent identity and to the human who asked, and the multiplexing criterion is met on inspected evidence.[^mscopilotaudit] Google's Gemini for Workspace log event names the actor, the action, the application, the feature source and the agent, and the appendix read lists no resource or tool parameter.[^gworkspacelog] Neither record is an OpenTelemetry `gen_ai.*` span and neither carries the rollback reference the action-log schema requires, so both criteria are **not met** on inspected evidence. Microsoft emits `gen_ai.*` spans over agent invocation, tool execution and output for agents built on Copilot Studio, and those spans land in the Agent 365 observability backend rather than one the organization runs.[^msagent365obs]

The [[lethal-trifecta|lethal-trifecta]] test is D7's strongest right-sizing lever. A program with strong D3/D4/D5 architectural containment may legitimately run no behavioral-observability layer and still be sound (the Stripe containment pattern). A contained design scoring low on D7 records the choice as an intentional trade-off.

**Routing inference through a gateway removes the first-party analytics dashboards for the sessions it carries.** An organization that introduces an LLM gateway or a cloud-provider endpoint (Bedrock, Vertex, Foundry) for governance reasons moves its coding sessions off the harness vendor's own API, and that vendor's analytics API reports only the sessions still running against it. OpenTelemetry export is the replacement, and it is not automatic. An assessor confirms that the SIEM receives session-correlated prompt, tool-result and permission-decision events before crediting the level. [[securing-agentic-coding|Securing Agentic Coding]] §Observability plane and [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] carry the deployment detail.

## Cost model

L2 and L3 price an E5 incumbent whose Sentinel, Azure Monitor and Application Insights ingestion is already entitled, and the L4 cliff for that buyer is an Agent 365 licence. Cloud Trace and the OpenTelemetry the Agent Development Kit emits meet the L3 telemetry rows for a Google Cloud buyer, but the L4 behavioral-drift row sits off-platform there: the control landscape above records no native Google detector and places the nearest Google product, Agent Anomaly Detection, in preview. Ingestion run-rate scales with agent count on every stack; the off-platform COTS detector a Google buyer adds carries no figure in this model.

| Level | Licensing | Operational labor | Run-rate (dominant) |
|---|---|---|---|
| L2 | ~0 (E5: Sentinel/Defender entitled) | ~0.25 FTE: turn on tool-call audit | low |
| L3 | ~0 for an E5 incumbent (App Insights / Azure Monitor + Sentinel ingest entitled; OTel SDK is OSS) | ~0.5 FTE: span instrumentation, identity-multiplex wiring, schema conformance | the inflection — `gen_ai.*` trace volume hits the SIEM at roughly 10–20× human log volume |
| L4 | mostly ~0 native, but Defender AI-agent detection needs an Agent 365 license; Miggo/Vectra are off-stack COTS otherwise | baseline tuning, drift false-positive triage, multi-tool eval ops | behavioral-analysis compute + full agent-log retention, scaling with agent count |
| L5 | some off-stack (real-time AI-BOM, CART tooling) | continuous baseline-ratio maintenance, actionable-rate measurement, SLA runbook | peak: every-surface telemetry × every agent into the SIEM |

The dominant cost signal: licensing is near-zero through L3 for an E5 incumbent, and the spend is ingestion run-rate that scales with agent count. The recalibration adds the mitigation the current D7 omits, **log tiering**: route high-volume, low-fidelity agent trace spans to a cheaper data-lake or auxiliary tier, and reserve the expensive analytics tier for detections that fire.[^sentpricing] The one licensing cliff is the L4 native behavioral detector, which requires Agent 365 rather than classic E5.

## Customer critiques folded in

- *"Switch on logging we already pay for."* Confirmed: D7-L3 is near-zero licensing for [[agentic-ai-security-cmm-recalibration-method-2026|the persona]]; the work is instrumentation labor.
- *"Log-ingestion run-rate is the dominant cost."* Confirmed and quantified, with the tiering mitigation named, a lever the current D7 does not surface.
- *"L5 names just-GA'd products."* Defender AI-agent detection (preview) and Sentinel graph MCP tools (sign-up preview) should not anchor a GA-grade L4/L5 for a regulated buyer until production-hardened.
- *"No behavioral baselines yet; PyRIT covers one red-team leg."* Fair: the persona's bot targets L3, and L4 requires multi-category coverage. A single tool is not L4.

## Open questions

- OpenTelemetry GenAI conventions remain in development; a version bump can break detection rules, and there is no confirmed stable-release date.
- Defender AI-agent detection is preview and Agent 365-licensed; whether classic-E5 buyers get it at GA without Agent 365 is the variable that moves the native L4 from preview-assembled to turnkey.
- AWS and GCP emit OTel telemetry natively but ship no native behavioral-drift detector; single-stack buyers there go off-platform for L4.
- The published analyst-actionable-rate benchmark comes from a single hyperscaler-scale SOC; whether it generalizes is unverified.
- Time-to-detect against a campaign running at agent speed has no target figure. The one published data point is the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], where the operator's own workload alert on internal privilege escalation fired on 2026-07-19, roughly eleven days after the second intrusion cluster began and three days after Hugging Face published its own breach; the two were linked on 2026-07-20 through a credential-revocation exchange rather than through a detection.[^bhoaihf] A domain whose whole purpose is detection has no benchmark for how late is too late.
- Reasoning-trace monitoring has no product, no schema, and no cost model. The signal is demonstrably rich, and reading it across a fleet is an unpriced problem that sits on top of a telemetry layer already running at 10–20× human log volume.
- Session-scoped drift is graded on the presence and wiring of a detector. The [[owasp-ai-exchange|OWASP AI Exchange]] supplies no threshold for how much relaxation across how many turns constitutes a finding,[^aix-escape] so the L4 criterion measures collection and scoring rather than accuracy, and it inherits the missing time-to-detect benchmark above. The Exchange's `OVERSIGHT` control supplies the disposition the score routes to — human review or automatic session suspension — and still supplies no threshold,[^aix-oversight] so the criterion now measures collection, scoring, and routing, and accuracy stays ungraded.
- Escape-indicator telemetry is conditional on a deployed sandbox, and no dependency rule connects D4 to D7. A program that runs no sandbox scores the clause as not applicable rather than as a gap; a program that runs one and forwards nothing has a collection gap that the current levels do not surface. Whether this warrants a D4→D7 candidate cap belongs to [[agentic-ai-security-cmm-dependency-rules|the dependency rules]].
- Series-level detection has no settled domain between D4 and here. The [[agentic-ai-security-cmm-crosswalk|crosswalk]]'s D4 cell anchors `UNWANTED INPUT SERIES HANDLING`, a clustering and frequency-analysis detector over a time window not limited to consecutive requests, and this domain's L4 session-scoped drift signal grades over the same trajectory, so a D4 level for it would collide with the boundary here. The anchor stands and the grading domain is unresolved ([[cmm-known-limitations|CMM Known Limitations]] item 23).
- **The memory-write clause reaches across a level boundary.** L3 here requires that every memory write carry writer identity, session, and target partition into the action log, and [[agentic-ai-security-cmm-d6-data-rag|D6]] grades per-write provenance over those same fields at L4. A program cannot emit the L3 telemetry without holding the D6 L4 capability that produces it, so D7 L3 is unreachable below D6 L4 for any deployment with persistent agent memory. Either D7's memory clause belongs at L4, or the asymmetry belongs in [[agentic-ai-security-cmm-dependency-rules|the dependency rules]] as a D6→D7 cap. The choice is open alongside the D4 question above.
- **The cost model prices retained telemetry and no level bounds it.** Every level above enlarges what is collected — `gen_ai.*` spans and a memory-write action log at L3, behavioral baselines and eval scenarios at L4, every surface of every agent at L5 — and the cost model treats the resulting volume as an ingestion bill. The [[owasp-ai-exchange|OWASP AI Exchange]] treats the same volume as exposure: `DATA MINIMIZE` names runtime logging among the activities the control applies to, and `SHORT RETAIN` requires data to be removed or anonymized once it is no longer needed.[^aix-dataminimize][^aix-shortretain] Agent telemetry carries prompts, tool results, and retrieved content, so it holds the classes both controls are written for. No level above states a retention period or a field-level bound on the action-log schema, and none is written here: the Exchange supplies no period, no artifact, and no method for logging specifically, so the criterion would grade an assertion. [[agentic-ai-security-cmm-d6-data-rag|D6]] L3 applies retention minimization to the augmentation store and its embeddings, which is a different store, so the telemetry backend sits outside both domains. Recorded as a gap in the level definitions rather than closed at a level.
- Monitor integrity is graded one property deep and no further. The [[owasp-ai-exchange|OWASP AI Exchange]] runtime control `MONITOR USE` requires the agent action audit trail "in tamper-evident storage the agent cannot modify",[^aix-monitoruse] and Document 5 supplies the evidence artifact for that requirement — an adversarial test of whether the agent can suppress or alter its own records — which the L4 criterion above grades.[^aix-testing] Two properties stay ungraded: separation between the detection pipeline and the fleet it watches, and any threshold on either.[^aix-monitoruse] A criterion asserting that separation would still be satisfied by assertion, so none is written. `DATA QUALITY CONTROL` supplies the requirement for the other detector class, directing that detection mechanisms and the data they rely on be protected against manipulation through segregation of development environments and integrity protections (§3.1.1).[^aix-dataqualitycontrol] [[agentic-ai-security-cmm-d6-data-rag|D6]] L4 grades a criterion off that direction — the poisoning detector's logic, thresholds, and baseline distributions held under access control and integrity checking. The runtime monitor this domain runs over the agent fleet still has no such backing. The narrowing is specific to log integrity and leaves the Class 3 monitor-isolation evidence item [[agentic-ai-threat-classes-2026|the threat classes]] proposes for D7 L4+ ungraded.
- **The L4 log-integrity criterion has no level behind it.** A deployment satisfies it only where some control keeps the action-log store outside the agent's write reach, and no domain grades such a control. L3 above grades a trace backend in path and an action-log schema, and neither states who may write to the store. [[agentic-ai-security-cmm-d9-operations|D9]] L3 grades a tamper-evident record for high-risk *approvals* and not for the action log. [[agentic-ai-security-cmm-d6-data-rag|D6]] L4 grades append-only immutable logging of *memory state changes*, which is a different store. A program that fails the L4 test therefore has no level to move to. The Exchange supplies the requirement and an implementation of it — a signed receipt binding a hash of input, output and processing context to a per-event signature, paired with an independent time anchor — so a criterion written for it would grade an artifact rather than an assertion,[^aix-monitoruse] and whether an append-only, agent-unreachable action-log store belongs at D7 L3 or in [[agentic-ai-security-cmm-dependency-rules|the dependency rules]] is open.

## D2→D7 dependency cap

D7's effective score is capped at D2's raw score (`effective(D7) ≤ raw(D2)`), because per-agent behavioral baselining binds to a verifiable per-agent identity. Below D2 L3, logs collapse to a shared principal. For the persona (D2 at L2), D7's effective score is capped at L2 regardless of observability spend. The scoring guidance follows: the cheapest high-leverage move is D2-L3 ([[microsoft-entra-agent-id|Entra Agent ID]], near-zero licensing), which lifts the D7 ceiling. Buying a monitoring product against an unbindable identity does not. Report D7 as raw + effective. See [[agentic-ai-security-cmm-dependency-rules|the dependency rules]].

[[agentic-ai-security-cmm-d2-identity|D2]] states the same dependency from its own side, bounded: a verifiable per-agent identity is necessary for per-agent detection and does not by itself establish that a correctly authenticated principal is acting on its own intent.

## Notes

[^otel]: [OpenTelemetry — GenAI semantic conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/), 2026. Status: Development; not yet stable.
[^appinsights]: [Microsoft Learn — Monitor AI agents with Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/agents-view), 2026. OTel GenAI-based agent observability (preview blade).
[^defxdr]: [Microsoft Learn — Detect and investigate threats to AI agents with Defender](https://learn.microsoft.com/en-us/defender-xdr/security-for-ai/ai-agent-detection-protection), 2026. AI-agent detection, preview; Agent 365 licensing.
[^awscw]: [AWS — Amazon CloudWatch generative AI observability (preview)](https://aws.amazon.com/blogs/mt/launching-amazon-cloudwatch-generative-ai-observability-preview/), 2026.
[^sentmcp]: [Microsoft — Sentinel MCP server generally available](https://techcommunity.microsoft.com/blog/microsoft-security-blog/microsoft-sentinel-mcp-server---generally-available-with-exciting-new-capabiliti/4470125), 2026. Sentinel graph GA; graph MCP tools / entity analyzer in sign-up preview.
[^sentpricing]: [Microsoft Learn — Plan costs and understand Microsoft Sentinel pricing](https://learn.microsoft.com/en-us/azure/sentinel/billing), 2026. Analytics vs auxiliary vs data-lake ingestion tiers; entity-analyzer Security Compute Units.
[^bhoaihf]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06). Summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]; timeline and reasoning-trace quotes at [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]].
[^taiwan]: Dream Security, "[Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia)," 2026-08-12. See [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]].
[^mscopilotaudit]: [Microsoft Learn — Audit logs for Copilot and AI applications](https://learn.microsoft.com/en-us/purview/audit-copilot), 2026-08-26, covering the `CopilotInteraction` record written for every Microsoft 365 Copilot interaction in the tenant. The record carries `UserId`, `AgentId`, `AgentName`, `AgentVersion`, `AppIdentity`, `AppHost`, `ThreadId`, `Messages`, `AccessedResources` with `SensitivityLabelId` and an `Action` of read, create or modify, `AISystemPlugin` and `ModelTransparencyDetails`. The [CopilotInteraction schema](https://learn.microsoft.com/en-us/office/office-365-management-api/copilot-schema), updated 2025-12-19, gives the field types and worked records; neither page defines a rollback reference or an OpenTelemetry span.
[^gworkspacelog]: [Google Workspace Admin Help — Gemini for Workspace log events](https://knowledge.workspace.google.com/admin/reports/gemini-for-workspace-log-events), updated 2026-09-16, covering Gemini in Workspace apps for a Workspace tenant, readable in the admin console, the security investigation tool, the Reports API and a BigQuery export. The searchable attributes are the actor, the user device, the application name, the feature source, nested agent identity, name and product, an agent flag, the action, the event, the event category, the date, the description and the IP autonomous system number. The [Reports API appendix](https://developers.google.com/workspace/admin/reports/v1/appendix/activity/gemini-in-workspace-apps), read 2026-09-18, documents one event, `feature_utilization`, with the action, application, event category and feature source parameters and no resource or tool parameter.
[^msagent365obs]: [Microsoft Learn — Observability integration for Copilot Studio](https://learn.microsoft.com/en-us/microsoft-agent-365/builder/observability), 2026-04-28, covering Copilot Studio agents in a Microsoft 365 tenant. The spans are `invoke_agent`, `output_messages` and `execute_tool`, carrying `gen_ai.agent.id`, `gen_ai.caller.upn`, `gen_ai.conversation.id`, `gen_ai.tool.name` and `gen_ai.tool.call.id`; telemetry "flows to the Agent 365 observability backend without additional configuration" and appears in the Microsoft 365 admin center, Defender and Purview. The page states no customer-configured endpoint, and excludes unauthenticated sessions and multi-tenant agents.
[^aix-sandbox]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), retrieved 2026-08-18.
[^aix-escape]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18.
[^aix-augintegrity]: [OWASP AI Exchange — AUGMENTATION DATA INTEGRITY](https://owaspai.org/go/augmentationdataintegrity/), retrieved 2026-08-18.
[^aix-monitoruse]: [OWASP AI Exchange — MONITOR USE](https://owaspai.org/go/monitoruse/), retrieved 2026-08-18 and re-read 2026-09-19 with no change to the text cited here; the rendered page publishes no last-modified date, so textual identity between the two reads is what is claimed. `MONITOR USE` is a runtime information-security control for input threats, published in the Exchange's threats-through-use document rather than in its general controls, which cross-reference it. Its agent action audit trail — agent identity, action type, target resource, parameters, policy decision and rationale, task and session context, outcome, and correlation identifiers across agents and sessions — is required "in tamper-evident storage the agent cannot modify", and the control gives tamper-evident logging as a signed receipt binding a hash of input, output and processing context to a per-event signature, paired with an independent time anchor such as an RFC 3161 Time-Stamp Authority. The same page states that chain-of-thought and reasoning traces are model output rather than guaranteed internal computation, and that a defensive monitoring agent is itself part of the attack surface.
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. Document 5's four-layer agentic penetration-test scope, whose infrastructure layer carries API gateway controls, credential exposure, key management, and `MONITOR USE` log integrity — stated as verifying that the agent cannot suppress or alter logs under adversarial conditions.
[^aix-dataqualitycontrol]: [OWASP AI Exchange — DATA QUALITY CONTROL](https://owaspai.org/go/dataqualitycontrol/), retrieved 2026-08-20. The *Implementation of detection mechanism protection* block — detection mechanisms and the data they rely on benefit from protection against manipulation, with segregation of development environments and integrity protections named as the means — and the filter-versus-alert threshold split behind the ingest-scan alert graded at L3 above.
[^aix-dataminimize]: [OWASP AI Exchange — DATA MINIMIZE](https://owaspai.org/go/dataminimize/), retrieved 2026-08-20. The applicability statement covering data collection, preparation, training, evaluation and runtime logging, and the premise that data absent from the system cannot be leaked, reconstructed or inferred.
[^aix-shortretain]: [OWASP AI Exchange — SHORT RETAIN](https://owaspai.org/go/shortretain/), retrieved 2026-08-20. Removal or anonymization of data once it is no longer needed or when legally required, the exceptions where another rule requires a record to be kept, and the statement that limiting a retention period can be seen as a special form of data minimization.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. Autonomous decision oversight (silent HITL degradation under load, inputs shaped toward weakest-review paths, agents with configuration access relaxing their own controls) and the multi-turn jailbreak drift disposition.
