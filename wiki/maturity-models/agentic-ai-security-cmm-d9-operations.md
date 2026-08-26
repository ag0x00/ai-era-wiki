---
type: maturity-model
title: "CMM D9: Operations and Human Factors"
address: c-000130
created: 2026-05-25
updated: 2026-08-25
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
  - "[[agentic-soc-autonomy-ladders]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Agentic AI Security CMM — D9 Operations & Human Factors (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D9 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. Of the nine domains, D9 carries the most process and labor and the fewest products. About half of it has no product answer on any platform and is pure operating-model work: HITL-fatigue and oversight-quality measurement, IR-runbook authoring, decommission drills, bus-factor continuity. Where products exist, they are platform-native primitives repurposed from adjacent domains (D2 identity lifecycle, D7 observability) rather than built as D9-specific tools.

> [!gap] Single-source grounding
> Levels and cost model synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation. Tooling status is a May 2026 snapshot.

## Threat coverage

D9 is cross-cutting: it appears in every one of the [[agentic-ai-threat-classes-2026|five threat classes]] — decommission and rollback (Class 4), sustained threat hunting (Class 2), incident response and HITL-fatigue measurement against **ASI09**, and the vendor-cutoff playbook (Class 5) — and its bus-factor and continuity prerequisites gate every other domain's L5 claim. The full mapping is in the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix.

## Control landscape (dated)

| Capability | What ships today | Status (May 2026) | Platform-native (MS / AWS / GCP) |
|---|---|---|---|
| AI-specific IR framework / playbooks | CoSAI AI Incident Response Framework (CACAO playbooks for [[prompt-injection\|prompt injection]], memory injection, RAG poisoning) | published 2025-10-30[^cosai] | vendor-neutral; runs on any SOAR |
| Post-deployment monitoring categories | [[nist-ai-800-4\|NIST AI 800-4]] (descriptive, not prescriptive) | published Mar 2026[^nist] | — |
| System-prompt-leakage test cases | [[owasp-llm-top-10\|OWASP LLM07:2025]] System Prompt Leakage (canary-string method; verified by [[standards-review-owasp-llm-top-10-2026-Q2\|the LLM Top 10 review]]) | live | — |
| IR orchestration / SOAR for agents | Sentinel + Security Copilot; CACAO on Step Functions | MS Security Analyst Agent + NL playbook generator shipped 2026[^sentinel] | **MS** strongest; AWS via AgentCore; GCP thin |
| Agent decommission / orphan reaping | NHI lifecycle (Okta NHI, Oasis); SCIM deprovisioning | GA across NHI vendors | **MS:** Entra Agent ID disable/delete with cascade child cleanup; sponsor-based Lifecycle Workflows **preview**[^entra] |
| Canary-token / prompt-leak trip-wire | OSS pattern; embedded in red-team suites | stable pattern | none native — build-it-yourself hook |
| HITL-fatigue / oversight-quality measurement | **no product** — risk-tiered oversight is the documented pattern; rubber-stamp / queue-age metrics are hand-rolled | — | **none on any platform — pure process**[^hitl] |
| Bus-factor / continuity test | **no product** — deputy + runbook-continuity drill is an org practice | — | none — pure process |

The load-bearing center of D9 has no product on any stack: HITL-fatigue measurement, bus-factor continuity, IR-runbook authoring and drilling, decommission cadence. That is a market-wide gap rather than a Microsoft-specific one, so no procurement decision closes it.

What the [[microsoft-zt4ai|Microsoft ZT4AI]] Governance pillar does supply for D9 is the lifecycle-accountability layer — Entra ID Governance sponsors with manager-transfer, and the [[microsoft-entra-agent-id|Entra Agent ID]] disable/delete-with-cascade reaper — crosswalked to D9 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], which confirms the human-factors center has no product on any stack. The [[microsoft-rai|RAI Standard]]'s A5 (Human oversight and control) goal sets the human-oversight outcome at the goal level, and [[microsoft-entra-agent-id|Agent 365]]'s lifecycle management (create / review / decommission via the registry plus the Entra ID Governance sponsor model) supplies the management-plane mechanism behind it — the goal-and-mechanism pairing is set out in [[standards-review-microsoft-rai-agent-365-2026-Q2|the 2026-Q2 RAI / Agent 365 review]]. Neither instrument supplies the human-factors instrumentation (HITL-fatigue measurement, continuity testing) at the center of this domain.

Regulatory-notification coordination belongs to the compliance function. The Exchange routes it to `CHECK COMPLIANCE` and states the placement outright: a serious incident may trigger parallel obligations such as [[eu-ai-act|EU AI Act]] Art. 73 reporting and NIS2 timelines, and the response is coordinated there rather than inside the monitoring controls ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/secprogram/`](https://owaspai.org/go/secprogram/)). D9's IR content and [[agentic-ai-security-cmm-d7-observability|D7]]'s monitoring content both sit on that line. The runbook names the instrument, the owner and the clock; the telemetry pipeline carries none of the obligation.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2.

- **L1 — Initial.** No guardrail SLAs, no decommission procedure, no HITL-queue monitoring, no system-prompt-confidentiality control, no IR runbook.
- **L2 — Developing.** A runbook documents guardrail fail behavior, decommission and credential rotation on owner departure, HITL-queue monitoring, and basic system-prompt protection. Decommission may be manual.
- **L3 — Defined.** The operating model is written down, exercised, and leaves records: guardrail latency and cost are measured per agent against a tested fail mode, an orphan reaper runs on an SLA, high-risk approvals are pre-categorized and each one writes a tamper-evident record, an IR runbook adapted from CoSAI is exercised at least once and names the regulatory-notification path, and a published disclosure tells users an AI model is involved and covers or records an omission against each property `AI TRANSPARENCY` lists.
- **L4 — Managed.** Measurement reaches the reviewer as well as the queue: alongside rubber-stamp rate and queue p95 the program holds a stated involvement measure, and the oversight path is exercised adversarially, with session-level approval rate limits enforced against each session's own baseline. The surrounding routine runs on a clock — quarterly decommission drills, pinned model versions, a published AI-VEX equivalent, coordinated-disclosure exercises, and a maintained per-credential dependency map.
- **L5 — Optimizing.** A closed loop runs: every guardrail / decommission / HITL incident yields a measurable controls update within a published SLA; attested zero orphaned credentials, zero prompt leaks, and zero undeprecated models for at least two quarters; a quarterly deputy-and-runbook continuity test; HITL-fatigue indicators stay within published thresholds.
- **L5+ — Leading Edge.** Org-level AI risk-observability metrics published externally; named contribution of drift-detection patterns or bypass classes back to standards bodies; cross-org coordinated-disclosure leadership.

**The transparency clause grades which of the five properties are answered; the depth of each answer stays ungraded.** The Exchange pairs this clause with `DISCRETE` and directs that the two be weighed against each other: [[agentic-ai-security-cmm-d1-governance|D1]] L3 grades a review of what technical detail is withheld from publication under `DISCRETE`, bounded by the disclosure this rung requires ([`/go/discrete/`](https://owaspai.org/go/discrete/)).[^aix-discrete] The bound reaches coverage only. A program that leaves a property unanswered and unrecorded fails the rung here, and a program that answers it in one line satisfies it, because the Exchange resolves the trade-off with a direction — minimize what helps an attacker while being transparent — and marks no line where technical detail crosses from safe disclosure into attacker assistance.[^aix-discrete] [[agentic-ai-security-cmm-crosswalk|The standards-crosswalk matrix]] holds the domain split, and [[owasp-ai-exchange|the Exchange page]] holds the trade-off in full.

The disclosure addresses users rather than auditors, so an assessor reads it for coverage of the property list and not for depth. The Exchange states one floor: users are told an AI model is involved at all, which it notes the EU AI Act requires for chatbots.[^aix-aitransparency] Three of the five properties are answered from artifacts other rungs already produce — a deployment consuming a third-party model answers the training-approach and data-source properties from the vendor model cards collected at [[agentic-ai-security-cmm-d8-supply-chain|D8]] L2, and the residual-risk property from the responsibility matrix at D1 L3, which records each unmitigated threat as accepted, self-mitigated, or avoided.

**The Exchange lists four downsides of human oversight, and the indicators above reach three of them.** Cost and slowness surface in queue age, approval fatigue in rubber-stamp rate, and lack of expertise in the qualification and instruction requirements the L3 runbook carries. Lack of involvement, which the Exchange classes as a form of missing expertise, has no counterpart in the set: a reviewer who does not actively perform the task loses the understanding of whether it is correct and what its impact would be, and a badly informed "go ahead" becomes the cheap answer for exactly that reason.[^aix-oversight] The adversarial-testing clause is the paired move: an approval gate nobody has attacked leaves its human failure mode unmeasured, and this domain's instrumentation observes the reviewer only through their output.

> [!gap] No validated involvement indicator
> The out-of-the-loop phenomenon is named by the [[owasp-ai-exchange|OWASP AI Exchange]] and carries no measurement instrument in any source this wiki holds.[^aix-oversight] Rubber-stamp rate, approval rate, and queue p95 are volume measures, and a disengaged reviewer produces unremarkable values on all three — the failure is invisible to the indicator set by construction rather than sitting below its threshold. The L4 criterion above therefore grades that an involvement measure exists and that its method is stated, and grades neither its accuracy nor a threshold against it, because no source supplies either. Candidate constructions are untested as indicators: rotating reviewers through the task they later approve, spot-checks that require reconstructing an action rather than assenting to it, and sampled reversal of approved actions. [[agentic-soc-autonomy-ladders|The agentic-SOC autonomy ladders]] carry the same out-of-the-loop language for a different operator population and are the nearest place on the wiki to test one.

Nothing in D9 L1–L5 depends on just-GA'd tooling. The dependencies are CoSAI v1.0 (out six months), stable OTel/canary patterns, and the GA Entra Agent ID lifecycle path. D9 is therefore cadence-safe for a regulated buyer, unlike D2/D3/D5 where per-task tokens sit on early-stage OSS.

## Assessor detail per level

L1, L2, L5, and L5+ are graded from their statements above. The two rungs below carry criteria an assessor checks item by item, each list stating what its own rung adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable carries that condition beside itself. The lists below hold criteria only; a paragraph after a list carries maturity or market commentary and states no criterion.

### L3 detail

- **Guardrail latency and cost measured per agent** with a tested fail-mode (fail-closed for high-risk tier).
- **An orphan reaper on a scheduled SLA**, met platform-native by Entra Agent ID disable/delete-with-cascade, or by NHI/SCIM deprovisioning.
- **HITL approval-rate and queue-age tracked.**
- **A system-prompt trip-wire deployed** — canary tokens plus LLM07:2025 cases.
- **A published model-deprecation policy.**
- **Participation in at least one disclosure community.**
- **A named AI-security role with bus-factor ≥2.**
- **An IR runbook adapted from CoSAI v1.0 CACAO playbooks and exercised at least once.** It names the regulatory-notification path alongside the technical response — which instrument applies, who owns the notification, and on what clock — held outside the monitoring controls ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/secprogram/`](https://owaspai.org/go/secprogram/)). Its scenario set covers supply-chain model poisoning alongside the prompt-injection, memory-injection, and RAG-poisoning playbooks CoSAI ships. A manipulated third-party model already acquired and in production is an incident class none of those three describes, and the Exchange routes vulnerability response for a supplied model, data pipeline, or dependency into the same incident-response process (§3.0).[^aix-supplychainmanage]
- **High-risk approval categories defined in advance**, along four dimensions — irreversibility, data classification, external parties, and financial thresholds — with each approval writing a tamper-evident audit record carrying the request, the parameters presented to the approver, the approver's identity and authentication method, the decision, and the outcome.[^aix-oversight] The highest-risk categories carry a mandatory minimum delay before execution and segregation of duties across approvers from different roles, assigned by policy and never by the agent.[^aix-oversight] The token that binds an approval to its parameters is graded at [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L5; the record of what was approved, by whom, and on what understanding is graded here.
- **A published user disclosure covering the `AI TRANSPARENCY` property list.** A published disclosure informs users that an AI model is involved and states the system properties a user needs to judge how far to rely on it, what data to send it, and what mitigations to apply independently of it. The Exchange lists five properties such a disclosure can carry — the model's rough working, the training approach, the type and source of the data used, expected accuracy and robustness of the output, and any residual security risk — and the rung grades that each of the five is either covered or recorded as omitted ([`/go/aitransparency/`](https://owaspai.org/go/aitransparency/)).[^aix-aitransparency] The artifacts are the disclosure and that coverage record.

### L4 detail

- **Quantitative HITL-fatigue indicators tracked** (rubber-stamp rate, queue p95) — pure process, no tool.
- **An involvement measure alongside the volume measures.** The indicator set covers involvement as well as volume, since rubber-stamp rate and queue p95 both measure how much a reviewer is asked to absorb and neither detects the out-of-the-loop failure, in which a disengaged reviewer approves at an unremarkable rate having lost the situational awareness to judge what was approved.[^aix-oversight] A program at this rung names its involvement measure and states the method behind it, and is graded on holding one.
- **The oversight path exercised adversarially**, with the testing programme covering urgency-driven bypass, approval-fatigue sequences, confusion injection into the material an approver reads, and multi-step normalisation ahead of a critical action, session-level approval rate limits enforced, and a session flagged where its approval rate significantly exceeds its own historical baseline.[^aix-oversight][^aix-testing]
- **Benign drift separated from adversarial drift.**
- **Quarterly decommission drills.**
- **Model versions pinned in production.**
- **An AI-VEX-equivalent published for own components.**
- **Participation in coordinated-disclosure exercises.**
- **A maintained per-credential dependency map.**

## Right-sizing by deployment shape

Right-sizing matters more in D9 than in any other domain: holding a single low-risk bot to mesh-grade incident response and continuity is the canonical over-scoping error.

| Deployment shape | Realistic D9 target |
|---|---|
| Member-facing RAG bot (no tools) — [[agentic-ai-security-cmm-recalibration-method-2026\|the persona]] | L2 → L3, narrow |
| Coding / copilot | L3 → L4 |
| MCP / skill provider serving others | L4 + selective L5 |
| High-autonomy multi-agent mesh | L4 minimum, L5 where resourced |

**Two controls are load-bearing for a read-only bot: system-prompt confidentiality and an owner-departure decommission runbook.** Canary tokens and `LLM07:2025` test cases cover the first. There is no HITL queue to fatigue, no multi-agent incident response, and no case for a quarterly drill. Holding this bot to L4 KPIs is the over-scoping the recalibration exists to prevent.

**A copilot writes, and writing creates a HITL queue that can be rubber-stamped.** Decommission cadence and prompt leakage matter, and the approval volume makes rubber-stamp measurement worth instrumenting.

**A provider owes its consumers federated CVE disclosure and a published deprecation policy** for every skill and MCP server it ships.

**HITL fatigue at scale is the dominant operational risk in a mesh**, and closed-loop incident response and continuity testing earn their cost only at this shape. The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] runs the same topology from the attacker side: a human set the objective but did not mediate step-by-step sub-agent coordination across a four-day, 12-wave campaign.[^taiwan]

> [!check] Approval fatigue is the mechanism that changes the deployment shape
> For agentic coding the D9 human-factors question is not only whether approvals are rubber-stamped. It is whether approval fatigue is converting an interactive deployment into an unattended one. The documented remedy for prompt volume is allowlisting, then autonomous modes, then suppressed prompts — each step defensible on its own and cumulatively a move to a different row of the [[agentic-ai-security-reference-architecture|RA]]'s shape table, where the load-bearing plane is Runtime rather than Control. An assessor should measure approval *volume and disposition* over time, not approval *existence*, and should treat a falling prompt count with a rising action count as a shape change requiring re-assessment rather than as an efficiency gain. See [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]].

The [[lethal-trifecta|lethal-trifecta]] test lowers D9 the same way it lowers D3/D5: a contained, low-autonomy bot legitimately scores lower, and the score records that as an intentional trade-off.

## Cost model

Licensing is near-zero for an E5/Azure or AWS incumbent; the spend is people and run-rate.

| Level | Licensing | Operational labor (the bottleneck) | Run-rate |
|---|---|---|---|
| L2 | ~0 | ~0.1–0.25 FTE: write the first runbook | — |
| L3 | ~0 (Entra / Sentinel / OTel entitled) | the dominant cost: adapt CoSAI CACAO playbooks to your agents; stand up the canary hook + LLM07:2025 cases; staff a named deputy and run one continuity test; participate in a disclosure community | latency/cost-span logging into the SIEM (couples to D7 ingest) |
| L4 | ~0 incremental (MS) / small (AWS) | recurring: quarterly decommission drills; HITL-rotation staffing plus hand-built rubber-stamp / queue dashboards (no product); benign-vs-adversarial drift triage; disclosure-exercise participation | drift + HITL telemetry ingest |
| L5 | ~0 | highest: a closed-loop SLA needs an on-call rotation owning AI-IR; quarterly attestation evidence; a standing continuity test | continuous attestation + incident-replay logging |

Four costs dominate and none of them appears on a licence line. An **on-call rotation** is D9-L5's largest hidden cost. **IR-runbook authoring** runs multi-week per agent class even though CoSAI ships the shape. **Decommission drills** are quarterly facilitation labor. And **HITL-rotation staffing** covers both reviewers and the analyst time to hand-build the fatigue dashboards, because no product measures oversight quality.

## Customer critiques folded in

- *"D9 is L1–L2: no formal decommission or HITL-fatigue tracking."* The recalibrated L3 is the realistic near-term target for the persona's bot and is narrow: canary + LLM07:2025 cases, an owner-departure decommission runbook via Entra Agent ID (already owned), and a named deputy. There is no HITL queue for a read-only RAG bot, so the recalibration right-sizes the fatigue-tracking critique away rather than meeting it with tooling.
- *"L5 assumes a cadence regulated FIs can't follow."* D9's L1–L5 dependencies are all stable or standards-based, so D9 is cadence-safe; the aspirational items sit at L5+.
- *"Cost under-tells the dominant spend."* Licensing is near-zero; the real spend is on-call, drill facilitation, and hand-built HITL dashboards, all labor and run-rate.
- *"Vendor-neutral catalog is an integration tax."* The platform-native column names Entra Agent ID and Sentinel for the all-Microsoft buyer; the off-stack residual (HITL-fatigue measurement, continuity practice) has no product on any stack, so it is a market-wide gap rather than a Microsoft-specific one.

## Open questions

- HITL-fatigue measurement has no product and no metric standard; thresholds (when is approval-without-comment too high?) are unresolved.
- No "AI-VEX" product exists yet; generic VEX is GA, but AI-component exploitability disclosure is emerging only.
- NIST AI 800-4 is descriptive, so the benign-vs-adversarial drift requirement leans on its taxonomy without an implementation spec.
- Entra Agent ID orphan governance is split GA/preview — disable/delete-with-cascade is usable, but sponsor-based Lifecycle Workflows are preview.
- The transparency clause at L3 has no depth floor, and none can be written from the source. The Exchange lists five properties a disclosure can carry, states one floor — that users are informed an AI model is involved — and supplies no measure of sufficiency for any individual property,[^aix-aitransparency] so a one-line answer and a ten-page answer score the same. What a withholding decision at [[agentic-ai-security-cmm-d1-governance|D1]] is bounded by is therefore that floor and a recorded coverage decision, and nothing finer.
- `CONTINUOUS VALIDATION` is anchored to this domain in [[agentic-ai-security-cmm-crosswalk|the standards-crosswalk matrix]] and graded by no rung above. [[agentic-ai-security-cmm-d6-data-rag|D6]] L3 grades protection of the reference corpus and [[agentic-ai-security-cmm-d7-observability|D7]] L4 grades a security eval cadence over jailbreak and escape scenarios; neither grades a model-quality measurement of correctness, robustness, or fairness. The D6 L4 clause requiring a data-removal decision to be justified against measured effect on model performance therefore carries its own measurement, since no rung supplies one.
- The EU-side notification anchor now exists: a serious incident may trigger EU AI Act Art. 73 reporting and NIS2 notification, and the Exchange's regulatory map states the NIS2 clock as 24 hours / 72 hours / one month ([`/go/checkcompliance/`](https://owaspai.org/go/checkcompliance/)). What remains absent is the US FI-sector side — no FFIEC/GLBA/NCUA IR-notification crosswalk, including the GLBA breach-notification timeline; deferred to the crosswalk.

## Cross-cutting note and the gate into L5

D9 is a cross-cutting domain whose capabilities interact indirectly with the per-plane ones: HITL fatigue degrades D3's approval gates, decommission lag orphans D2 identities, and drift remediation feeds on D7 telemetry. Per [[agentic-ai-security-cmm-dependency-rules|the dependency rules]], D9 operational lag leaves upstream domains where they stand and is reported directly in D9's own row.

The multi-quarter gate a program clears to reach L5 sits substantially inside D9. Two of its four conditions are D9 controls: bus-factor ≥2 with a documented continuity test, and the two-quarter stable-L4 history that D9's closed-loop attestations evidence. D9 therefore carries more than its own climb to L5 — its continuity-test and clean-state attestations supply the org-wide gate evidence every other domain's L5 claim rests on, so a program clears D9's continuity bar before it reaches L5 anywhere.

## Notes

[^cosai]: [CoSAI — AI Incident Response Framework](https://www.coalitionforsecureai.org/coalition-for-secure-ai-releases-two-actionable-frameworks-for-ai-model-signing-and-incident-response/), dated 2025-10-30 on the CoSAI resources listing per [[standards-review-saif-cosai-2026-Q2|the 2026-Q2 SAIF/CoSAI review]]. NIST-IR lifecycle adapted to AI; CACAO playbooks.
[^nist]: [NIST — Challenges to the Monitoring of Deployed AI Systems (AI 800-4)](https://www.nist.gov/news-events/news/2026/03/new-report-challenges-monitoring-deployed-ai-systems), 2026. Six monitoring categories; descriptive, not prescriptive.
[^sentinel]: [Microsoft — What's new in Microsoft Sentinel, March 2026](https://techcommunity.microsoft.com/blog/microsoftsentinelblog/what%E2%80%99s-new-in-microsoft-sentinel-march-2026/4499508), 2026. Security Analyst Agent; natural-language SOAR playbook generator.
[^entra]: [Microsoft Learn — How agent identity deletion works](https://learn.microsoft.com/en-us/entra/agent-id/concept-agent-identity-deletion), 2026. Disable/delete with cascade child cleanup; sponsor-based Lifecycle Workflows in preview.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. The high-risk approval workflow (pre-defined categories, tamper-evident audit trail, minimum pre-execution delay, segregation of duties), the adversarial testing requirement for the oversight path, session-level approval rate limits, and the four stated downsides of human oversight including the out-of-the-loop phenomenon.
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. The agentic red-teaming path that treats human oversight as a social surface, testing whether urgency framing, confusion injection, or approval fatigue can bypass `OVERSIGHT` gates that hold under normal review. The Exchange gives confusion injection without a definition.
[^aix-aitransparency]: [OWASP AI Exchange — AI TRANSPARENCY](https://owaspai.org/go/aitransparency/), retrieved 2026-08-20. The control's purpose — informing users of the AI system's properties so they can adjust how they rely on it, what data they send it, and what additional mitigations they apply — and the five properties the entry states such information *can include*: the rough working of the model, the training approach, the type of data used and its source, expected accuracy and robustness of the output, and any residual security risk. The entry states that the simplest form of the control is informing users that an AI model is involved, which it notes the EU AI Act requires for chatbots; that transparency here is abstract information about the system and not explainability of individual decisions; and that ISO/IEC 42001 B.7.2 covers the control minimally, reaching only the data-management part.
[^aix-discrete]: [OWASP AI Exchange — DISCRETE](https://owaspai.org/go/discrete/), retrieved 2026-08-20. The statement that the control is weighed against `AI TRANSPARENCY`, and the resolution "The key is to minimize information that can help attackers while being transparent," which supplies a direction and no threshold.
[^aix-supplychainmanage]: [OWASP AI Exchange — SUPPLY CHAIN MANAGE](https://owaspai.org/go/supplychainmanage/), retrieved 2026-08-20. The monitoring statement that review of security advisories for supplied models, data pipelines and dependencies lets teams respond through updates, containment, or compensating controls, and that these activities can be integrated into broader vulnerability management and incident response processes.
[^hitl]: [Ou — Confirmation fatigue and the protocol gap in agentic AI oversight](https://changkun.de/blog/ideas/human-in-the-loop-agents/), 2026. No dedicated HITL spec or oversight-quality measurement product as of early 2026.
[^taiwan]: Dream Security, "[Inside a Multi-Agent AI Framework Used to Compromise Government Entities in Asia](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia)," 2026-08-12. See [[taiwan-ai-agent-government-intrusion|Taiwan AI-Agent Government Intrusion]].
