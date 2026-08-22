---
type: architecture
title: "Agentic SOC Investigation Surface"
address: c-000197
created: 2026-06-03
updated: 2026-06-03
tags:
  - architectures
  - agentic-soc
  - investigation
  - case-management
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-cmm-d5-observability-oversight]]"
  - "[[agentic-soc-cmm-d1-telemetry-data-readiness]]"
  - "[[security-data-pipeline-architecture]]"
  - "[[mate-cd-cr-continuous-detection-response]]"
  - "[[vulnops-l1-soc-extinction]]"
  - "[[sift-claude-code-dfir-talk]]"
  - "[[beyond-the-chatbot-talk]]"
  - "[[llm-as-a-judge]]"
  - "[[cyber-poverty-line]]"
sources:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[mate-cd-cr-continuous-detection-response]]"
  - "[[vulnops-l1-soc-extinction]]"
---

# Agentic SOC Investigation Surface

Per-function deep dive for the **Investigation & case management** function of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]]. Investigation is the step where a SOC turns correlated signals into a reasoned account of what happened: it pulls together alerts, entities, telemetry, and prior context, builds a narrative, and reaches a finding. It produces findings, not actions, which makes it the most reasoning-heavy and the most reversible of the analysis functions. The agent surface is a correlation agent that works a continuously-updated investigation thread alongside the analyst, rather than a queue of static cases an analyst opens and closes by hand.

On the RA's plane model this function runs primarily on two planes. The investigation substrate — the thread, the entities it links, and the knowledge graph it correlates against — lives on the **Data & Knowledge plane**. The agent's reasoning is watched and overridden on the **Observability & Evaluation plane** and at the human-authority boundary. The [[agentic-soc-cmm|Agentic SOC CMM]] gates this function's autonomy on **D1 (Telemetry & Data Readiness)** — whether the agent has the data to correlate — and on **D5 (Observability & Oversight)**, whether a human can see and override the agent's reasoning. Those two gates frame the whole function: data in, reasoning out, both visible.

## The agent surface

On the supervisor-worker topology, investigation is a worker pattern the orchestrator hands a confirmed or suspicious finding to. The worker does not act on the estate; it reads from it and writes a narrative back. Its job is correlation and explanation: gather the entities and events around a trigger, query telemetry across the [[security-data-pipeline-architecture|security data pipeline]] where the data lives, enrich against the threat-intelligence knowledge graph, and assemble a defensible account of scope, root cause, and impact. Triage hands it a verdict to deepen; it hands incident response a finding to act on.

The mechanism mix is **AI-heavy with deterministic scaffolding**. The correlation and narrative-building are reasoning tasks an LLM agent does well — pivoting across entities, hypothesizing a sequence, asking the next question — while the data access underneath is deterministic and typed: bounded queries against the pipeline, read-only tool contracts, fixed enrichment lookups. A useful production shape is the DFIR workstation pattern in [[sift-claude-code-dfir-talk|Protocol SIFT]], where a natural-language prompt drives Claude Code to sequence a workstation's deterministic forensic tools — timeline generation, memory analysis, malware sweeps — over MCP. The reasoning is the agent's; the analytical output stays with the validated tools.

The substrate is a **thread, not a case**. [[vulnops-l1-soc-extinction|Mallory's framing]] replaces the static case — artifacts in, report out, analyst as the integration layer — with a thread: a continuously-reasoning analyst-agent exchange working the same problem at once, with the agent inside the thread rather than fed from outside. The investigation never fully "closes" into a filing cabinet; it stays a live object that later signals can reopen and that the system can learn from.

> **A detection is an investigation run often enough to automate, and an investigation is a detection that has not been compressed yet.** [[mate-cd-cr-continuous-detection-response|Mate's CD/CR framing]] makes the investigation surface a generator of detections: a closed investigation becomes a compression candidate for a new, context-specific detection shaped by what the organization's own environment showed, on the argument that an investigation observes ground truth where a vendor rule library only guesses. The loop — investigation compresses into detection, detection feeds the next investigation — is where this function feeds detection engineering rather than ending in a report.

The human-authority boundary for investigation is **lighter than for response, because the output is reversible**: a wrong finding can be corrected before anything is done about it. The boundary sits not on whether the agent may act — it cannot — but on whether a human can follow and correct the agent's reasoning before that finding drives a containment decision downstream. That is a D5 oversight surface, not a D4 authority limit.

## Autonomy progression

The autonomy ladder ([[agentic-soc-cmm|CMM]] Axis 1, L0 Manual → L4 Delegated) applies to investigation as a delegation of the reasoning work, and the [[agentic-soc-cmm#the-gating-rule|gating rule]] caps how far it can legitimately go. Because investigation produces findings rather than actions, higher autonomy is safer here than in response — a confident-but-wrong narrative is correctable, a reckless containment is not. The governing gates are **D1 at L2** (does the agent have the data to correlate?) and **D5 at L3** (can a human see and override the reasoning?). D5 carries more weight than authority limits do for this function, because the failure that matters is a plausible wrong story, and the control against it is visibility into how the story was built.

| Level | What it looks like for this function | Gating domains |
|---|---|---|
| **L0 — Manual** | An analyst opens a case, pulls telemetry by hand, pivots across tools mentally, and writes the narrative. The tool is a query box. | — |
| **L1 — Assisted** | The agent enriches and summarizes on request — auto-pulls related entities, drafts a timeline, suggests next pivots — but the analyst drives every step and owns the narrative. Decision support, human-in-the-loop. | — |
| **L2 — Semi-Autonomous** | The agent runs routine correlation sub-tasks end to end inside the thread — gathers entities, builds the timeline, proposes a finding — and the analyst reviews and confirms before it stands. Requires **D1** mature enough that the correlation runs on real, reasonably complete data ([[agentic-soc-cmm-d1-telemetry-data-readiness\|D1]] at L2). | D1 |
| **L3 — Conditional** | The agent owns straightforward investigations within bounds, reaching and recording findings without per-step confirmation, escalating ambiguous or high-impact cases. A human-on-the-loop watches the agent's reasoning on a live oversight surface and can override a finding before it propagates. Adds **D5** ([[agentic-soc-cmm-d5-observability-oversight\|Observability & Oversight]]) at the L3 gate — the agent's reasoning trajectory, not only its output, must be visible and interruptible. | + D5 |
| **L4 — Delegated** | The agent owns the investigation lifecycle and the compression loop — it reasons, files, and feeds closed investigations back as detection candidates — under governed outcomes rather than per-case review. Asymptotic: a human still governs which findings drive consequential action. Adds the L4 gates (resilience and governance) per the CMM. | + D7, D8 |

The defined failure mode is **operating above the earned ceiling**: running investigation at an autonomy level its weakest governing domain does not support. The sharp case for this function is L3 without D5 — letting the agent reach findings on its own when no one can see or override its reasoning. The risk is not a destructive action; it is a **confident-but-wrong narrative** that downstream functions trust and act on. That is why oversight (D5) and human-on-the-loop review matter more here than authority limits, and why D1 underneath them is non-negotiable: a narrative built on partial telemetry is wrong in a way no oversight surface can catch after the fact.

## Control landscape (dated)

Vendors, FOSS, and standards are dated, swappable examples for concreteness, not endorsements. GA versus preview is marked; the durable function and its ladder above do not depend on any of them.

| Capability | What ships today | Status (mid-2026) |
|---|---|---|
| Case / incident substrate | SOAR and SIR case management (Splunk SOAR, Microsoft Sentinel incidents, ServiceNow SIR, TheHive) — the deterministic case object the agent thread overlays | GA and mainstream; the durable substrate the thread model is replacing |
| Investigation-thread model | Continuously-reasoning analyst-agent threads replacing the static case — [[vulnops-l1-soc-extinction\|Mallory's "threads, not cases"]]; [[mate-cd-cr-continuous-detection-response\|Mate's CD/CR loop]] on a Security Context Graph | Emerging, single-vendor-coined; directional rather than independently benchmarked, and Mallory's correlation layer is pre-GA |
| Entity correlation / graph | Entity-behavior and graph correlation across users, hosts, and assets (UEBA in Sentinel / Splunk; graph correlation in [[google-agentic-soc\|Google SecOps]]) feeding the agent's pivots | GA for the underlying correlation; agent-driven pivoting over it is the newer layer |
| Knowledge / context graph | The threat-intelligence and organizational knowledge graph the agent correlates against — Mate's Security Context Graph aggregating data lakes, telemetry, SOPs, and MITRE ATT&CK; vendor context graphs | Concept-level to early product; the "query data where it lives" grounding substrate, not a settled standard |
| Agentic DFIR | Agent sequencing deterministic forensic tools over MCP — [[sift-claude-code-dfir-talk\|SANS Protocol SIFT]] (Claude Code on the SIFT workstation) | Experimental research initiative; explicitly not forensically validated or court-admissible |
| Vendor investigation agents | Investigation/correlation agents in [[microsoft-security-copilot\|Microsoft Security Copilot]], [[google-agentic-soc\|Google SecOps Gemini]], and Salesforce's [[beyond-the-chatbot-talk\|Polyphonic]] production SOC | Mixed GA / preview across vendors; re-verify per product at use |
| Reasoning oversight | Live agent-activity dashboards and override surfaces at the human-authority boundary; [[llm-as-a-judge\|LLM-as-a-judge]] checks flagging a reasoning trajectory that departs from baseline | Oversight surfaces GA as configuration over the orchestration platform; reasoning-level anomaly checks production at scale, preview in some platforms |

The load-bearing rows are the **investigation-thread model** and **reasoning oversight**. The thread model is the structural shift this function turns on, and it is still single-vendor-coined and largely pre-GA, so treat it as an architecture to test, not a measured result. Reasoning oversight is the control that makes L3 legitimate, and it is mostly configuration over existing platforms rather than a shipped product, which means it is easy to skip and expensive to retrofit after a wrong narrative has already propagated.

## Failure modes and what to watch

- **Confident-but-wrong narrative.** The signature failure of an investigation agent: a plausible, well-written account that is incorrect, which downstream triage or response then trusts. The bound is D5 reasoning oversight (a human-on-the-loop who can follow and override the reasoning, not just the verdict) plus D1 data completeness underneath it. Watch the gap between autonomy granted and D5 maturity; running L3 without a working oversight surface is the defined over-ceiling failure.
- **Correlation on partial data.** A narrative built on telemetry with silent gaps is wrong in a way oversight cannot catch downstream, because the missing evidence never appears in the reasoning to be questioned. Bounded by [[agentic-soc-cmm-d1-telemetry-data-readiness|D1]] — measured coverage and a known-blind-spot list, not hoped-for completeness.
- **Anchoring and narrative lock-in.** An agent that commits early to a hypothesis and fits subsequent evidence to it produces a coherent but biased account. Bounded by oversight surfaces that expose the reasoning trajectory and by [[llm-as-a-judge|LLM-as-a-judge]] checks that flag a trajectory diverging from baseline; the human-on-the-loop is the corrective.
- **Compression-loop poisoning.** When closed investigations compress into new detections (the CD/CR loop), a wrong finding does not stay contained — it becomes a bad detection that fires on the wrong things from then on. The loop amplifies investigation errors into detection-engineering errors. Bounded by review of compression candidates before they become live detections, and by the same D5 oversight that should have caught the wrong finding.
- **Over-delegation of judgment.** Letting the agent own ambiguous or high-impact investigations without escalation collapses the human's role to rubber-stamping. Bounded by escalation criteria at the human-authority boundary and by keeping the boundary asymptotic — a human still governs which findings drive consequential action.

> [!gap] The oversight catch-rate against a plausible-but-wrong narrative is unmeasured
> The function's safety rests on D5 oversight — a human-on-the-loop who follows the agent's reasoning and overrides a wrong finding before it propagates. But the signature failure is a coherent, well-written narrative that is incorrect, the failure mode engineered to survive inspection, and the wiki has no measure of how reliably a reviewer catches it or of how that catch-rate degrades under caseload. The risk compounds in the compression loop, where an unspotted wrong finding becomes a standing detection that fires on the wrong things thereafter. The oversight surface is the named control; its actual catch-rate against confident-but-wrong narratives is the open question.

## Right-sizing by org profile

The realistic autonomy target for investigation scales with the data the team can correlate over and the oversight surface it can run. A small team correlating over a provider's telemetry and reviewing the agent's findings is right-sized, not immature.

| Band | Realistic autonomy target | Why |
|---|---|---|
| Solo / small | L1, reaching L2 | Near or below the [[cyber-poverty-line\|cyber-poverty line]]. Investigation depends on broad telemetry the team cannot collect alone, so correlation is borrowed through an MDR/MSSP or ISAC. AI is the barrier-lowering enabler here: an assistive correlation agent gives a one-person team the enrichment, timeline-building, and next-pivot suggestions that previously required a tier-2 analyst, with the human owning the narrative. L2 is reachable once the borrowed telemetry is reasonably complete (D1). |
| Mid | L2, selectively L3 | An in-house SOC can correlate over a commercial SIEM/XDR plus a data-pipeline platform and stand up a human-on-the-loop oversight surface. L3 on straightforward, well-understood investigation types is reachable where D5 ([[agentic-soc-cmm-d5-observability-oversight\|the oversight surface]]) is in production; ambiguous and high-impact cases stay at L2 with confirmation. |
| Enterprise | L3, selectively L4 | A data-pipeline-native, multi-source estate with a knowledge graph and reasoning-level oversight can let the agent own routine investigations within bounds and run the compression loop. L4 earns its cost where investigation volume is high and the closed-investigation-to-detection loop is itself instrumented; the human-authority boundary stays asymptotic regardless. |

Investigation has historically been tier-2 work, out of reach for a team below the cyber-poverty line. An assistive correlation agent over borrowed telemetry puts the reasoning support, not the authority, within reach of a one-person team. That is the barrier-lowering effect the [[agentic-soc-cmm|CMM]] records as the mechanism attribute, and the small-team column is where it shows most.

## Relations

- Hangs off the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] — the investigation thread lives on the **Data & Knowledge plane**; the agent's reasoning is watched and overridden on the **Observability & Evaluation plane** and at the human-authority boundary.
- Governed by the [[agentic-soc-cmm|Agentic SOC CMM]]'s [[agentic-soc-cmm#the-gating-rule|gating rule]]: this function's autonomy is gated by **D1** ([[agentic-soc-cmm-d1-telemetry-data-readiness|Telemetry & Data Readiness]], the L2 data gate) and **D5** ([[agentic-soc-cmm-d5-observability-oversight|Observability & Oversight]], the L3 reasoning-visibility gate).
- The case/thread and knowledge-graph substrate is detailed in [[security-data-pipeline-architecture|Security Data Pipeline Architecture]].
- Practitioner evidence: [[vulnops-l1-soc-extinction|Mallory's "threads, not cases"]] and CISO-as-router framing; [[mate-cd-cr-continuous-detection-response|Mate's CD/CR]] continuous loop and investigation-to-detection compression; [[sift-claude-code-dfir-talk|SANS Protocol SIFT]] agentic DFIR over MCP; [[beyond-the-chatbot-talk|Salesforce's Polyphonic]] supervisor-worker SOC.
