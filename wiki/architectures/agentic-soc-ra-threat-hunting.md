---
type: architecture
title: "Agentic SOC Threat Hunting Surface"
address: c-000198
created: 2026-06-03
updated: 2026-06-03
tags:
  - architectures
  - agentic-soc
  - threat-hunting
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-cmm-d1-telemetry-data-readiness]]"
  - "[[agentic-soc-cmm-d3-evaluation-ground-truth]]"
  - "[[agentic-soc-cmm-d5-observability-oversight]]"
  - "[[security-data-pipeline-architecture]]"
  - "[[ai-automation-boundary-threat-hunting-talk]]"
  - "[[sift-claude-code-dfir-talk]]"
  - "[[cyber-poverty-line]]"
sources:
  - "[[ai-automation-boundary-threat-hunting-talk]]"
  - "[[sift-claude-code-dfir-talk]]"
  - "[[security-data-pipeline-architecture]]"
  - "[[agentic-soc-reference-architecture]]"
---

# Agentic SOC Threat Hunting Surface

Per-function deep dive on the **threat hunting** surface of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]]. Threat hunting is the proactive, hypothesis-driven search for adversary activity that detection content has not already flagged: a hunter forms a hypothesis ("a credential-theft tool would leave *this* trace in *that* telemetry"), tests it against the data, and either confirms a lead, refines the hypothesis, or discards it. It sits beside detection engineering and triage in the detection-and-analysis group, but it works the opposite direction — outward from a question into the data, rather than inward from an alert toward a verdict.

The agent surface here is a **hypothesis-and-hunt worker pattern**: an agent (or, as the work scales, an orchestrator coordinating subagents) generates candidate queries, runs them against telemetry search-in-place, reads the results, and narrows toward pivotal evidence. It runs on the [[agentic-soc-reference-architecture#4-data--knowledge-plane|Data & Knowledge plane]] (the telemetry it hunts over and the threat-intel that grounds the hypotheses) and the [[agentic-soc-reference-architecture#1-orchestration-plane|Orchestration plane]] (the supervisor-worker decomposition that lets a hunt fan out across subagents). Its autonomy is gated by [[agentic-soc-cmm|the CMM]]'s **D1 (Telemetry & Data Readiness)**, **D3 (Evaluation & Ground-Truth)**, and **D5 (Observability & Oversight)** — there must be data to hunt over (D1), a way to observe the agent's reasoning as it hunts (D5), and a way to judge hypothesis quality before any move toward hunt-and-act (D3).

The feature that distinguishes hunting from every other function on the RA is an **explicit automation boundary**: a named line between where AI accelerates the hunt and where it adds risk. Hunting is exploratory by nature, so the usual failure mode of over-delegation takes a sharper form here — past a point, more automation does not produce more findings, it produces more *plausible-but-wrong* hypotheses that consume analyst time and crowd out the novel threats a hunt exists to find. The boundary is the design artifact that decides where human judgment stays load-bearing. It is drawn from Datadog's practitioner account of migrating a hunt system from a single agent to an orchestrator-subagent architecture ([[ai-automation-boundary-threat-hunting-talk|Exploring the AI Automation Boundary for Threat Hunting]]), the load-bearing source for this page.

## The agent surface

On the supervisor-worker topology, hunting runs as a worker pattern that scales from one agent to many. The [[ai-automation-boundary-threat-hunting-talk|Datadog account]] describes the migration directly: a single agent that handled the whole hunt evolved into an **orchestrator-subagent** system, where an orchestrator decomposes a hunt and routes sub-questions to subagents that each work a slice of telemetry. This matches the RA's [[agentic-soc-reference-architecture#design-principles|supervisor-worker topology with deterministic orchestration]]: control flow between hunt stages is sequenced deterministically, not left to an open-ended autonomous chain, and the orchestrator carries max-iteration limits so a hunt cannot loop indefinitely.

The agent automates three parts of the hunt loop, per the same source: **hypothesis-driven query generation** (turning a hunting question into queries across schema-diverse telemetry), **iterative refinement** (reading results and rewriting the query to sharpen the search), and **narrowing toward pivotal evidence** (driving from a broad question down to the few records that confirm or kill the hypothesis). The bottleneck it relieves is the human limit on navigating overwhelming telemetry volume, not a shortage of telemetry.

The mechanism mix is **hybrid, weighted toward AI**. Query generation and result interpretation are AI work, drawing on the model's ability to reason over heterogeneous data and natural-language intent. The orchestration around them is deterministic: step sequencing, iteration caps, and the tool contracts that bound what a subagent may query. Unlike detection engineering, hunting has little deterministic-playbook content to fall back on. A hunt is by definition a search for what no rule already catches, so the AI carries more of the load and the deterministic layer is mostly the harness around it.

The tools and data the hunt agent calls:

- **Telemetry, search-in-place.** Hunting queries data where it lives rather than requiring everything pre-ingested into one store. The [[security-data-pipeline-architecture|security data pipeline architecture]]'s search-in-place and on-demand correlation layer is the concrete substrate: federated queries over object storage, data lakes, and live sources, with schema applied at read time. This is what makes hunting over schema-diverse telemetry tractable without a centralized SIEM holding all of it.
- **Threat-intelligence grounding.** Threat-intel-led hunts begin from an external signal — a campaign report, a new technique, an IOC set — that the agent turns into hypotheses. The RA grounds this from the Data & Knowledge plane's threat-intel knowledge graph.
- **A forensic / investigation toolset.** Where a hunt crosses into deep host or disk analysis, the agent orchestrates forensic tooling rather than raw queries. [[sift-claude-code-dfir-talk|Protocol SIFT]] is the dated instance: Claude Code wired over MCP to the SANS SIFT digital-forensics workstation, sequencing timeline generation, memory analysis, and malware sweeps from a natural-language "find evil" prompt — an agent orchestrating deterministic forensic utilities on a real workstation. (SANS frames it as experimental, not forensically validated; the deterministic utilities remain the sole source of analytical output.)

The **human-authority boundary** for hunting is the automation boundary itself, and it sits earlier than it does for triage. The agent runs the hunt — generates, refines, narrows — but the human hunter owns hypothesis selection at the start (which questions are worth asking, especially the novel ones an agent would not propose) and judgment on the findings at the end (whether a confirmed lead is a real threat or a plausible artifact). The agent accelerates the middle; the human keeps the ends. Any step from "the agent found this" to "the agent acted on this" crosses the boundary explicitly and is gated separately.

## Autonomy progression

The [[agentic-soc-cmm#axis-1--the-autonomy-ladder-per-function|autonomy ladder]] applies to hunting, but the ladder alone understates the constraint. For most functions, autonomy is bounded by the [[agentic-soc-cmm#the-gating-rule|gating rule]] — a function reaches L_k only when its governing domains are mature enough. Hunting carries that gate *and* a second bound the gate does not capture: the automation boundary. The two are different. The gate asks whether the SOC has earned the autonomy (can it measure, observe, and contain the agent?); the boundary asks whether the autonomy *helps at all* past a point, regardless of maturity.

**Beyond the automation boundary, more autonomy degrades the hunt rather than scaling it: a fully autonomous hunter generates more plausible-but-wrong hypotheses, not more real findings, and the analyst time spent disproving them is time not spent on the novel threats the hunt exists to surface.** This is why hunting, by design, rarely reaches L4 the way triage does — even a SOC whose domains are mature enough to gate L4 should usually hold hunting below it, because the value of the function depends on human hypothesis-framing and human judgment on ambiguous findings, neither of which delegates cleanly.

| Level | What it looks like for threat hunting | Gating domains |
|---|---|---|
| **L0 — Manual** | A hunter forms hypotheses and writes every query by hand against whatever telemetry is reachable. The labor ceiling is the human's ability to navigate volume. | — |
| **L1 — Assisted** | The agent drafts queries and summarizes results; the hunter directs the hunt, selects hypotheses, and verifies every lead. Decision support, human-in-the-loop. | D1 (data to hunt over) |
| **L2 — Semi-autonomous** | The agent runs routine sub-hunts — query generation, iterative refinement, narrowing — and proposes pivotal evidence; the hunter approves which hypotheses to pursue and confirms findings. Every consequential read or conclusion is reviewed. | D1, D4 |
| **L3 — Conditional** | The orchestrator runs whole hunts within bounded scope (defined telemetry, defined hypothesis classes, iteration caps), escalating out-of-bounds or low-confidence results to a hunter who monitors and intervenes (human-on-the-loop). This is the practical ceiling for most SOCs. | + D3, D5 |
| **L4 — Delegated** | The hunt fleet owns proactive hunting end to end, including hypothesis generation, with humans governing outcomes. Reachable for narrow, well-characterized hunt classes only; broad hunting held here is the over-delegation failure mode by design, not just by gate. | + D7, D8 |

The gating rule reads the governing domains at each step. Reaching **L2** needs D1 (the data to hunt over) and D4 (the agent's identity and scoped read authority). Reaching **L3** adds the two domains that make autonomous in-bounds hunting legitimate. **D3** must be mature enough to judge whether the agent's hypotheses and findings are good rather than merely fluent; this is [[agentic-soc-cmm-d3-evaluation-ground-truth|the primary autonomy gate]], and you cannot delegate what you cannot measure. **D5** must let a hunter watch the agent's reasoning as it hunts and stop it before a bad branch consumes the run ([[agentic-soc-cmm-d5-observability-oversight|the oversight half]]). D3 also gates any move toward **hunt-and-act**, where an agent that finds a lead begins containing it, because acting on a wrong hypothesis is far costlier than reporting one. **L4** would add D7 and D8, but hunting's automation boundary usually keeps it below L4 even when those are satisfied.

The defined failure mode is the CMM's general one — **operating a function above its earned autonomy ceiling** — sharpened by hunting's exploratory character. Running hunting above the ceiling set by D3 means the SOC is trusting hypotheses it cannot measure; running it above the automation boundary means the SOC is automating the part of the hunt where human judgment was the point.

## Control landscape (dated)

Real tools and patterns for hunting, deterministic and AI, dated and swappable. GA versus preview is marked; the named products are examples, not endorsements.

| Capability | What ships today | Status (mid-2026) |
|---|---|---|
| Search-in-place hunt substrate | Federated query and search-in-place over object storage and data lakes — Cribl Search, Query.ai, Anvilogic, Panther — so a hunt reaches schema-diverse telemetry without one centralized store (see [[security-data-pipeline-architecture\|the pipeline architecture]]) | GA in the named products; an emerging category, with scale figures vendor-reported |
| Hunt framing / methodology | Hypothesis-driven hunting, ATT&CK-driven hunts (hunting per technique/tactic), and threat-intel-led hunting as the three durable methodologies; the PEAK and TaHiTI frameworks formalize the loop | Established practitioner methodology, mechanism-agnostic and pre-dating AI |
| Hypothesis-and-hunt agent | Orchestrator-subagent hunt systems automating query generation, iterative refinement, and narrowing ([[ai-automation-boundary-threat-hunting-talk\|Datadog]]); single-agent → orchestrator-subagent migration is the reference pattern | Practitioner-reported from internal deployment; not a shipped commercial product |
| Agentic DFIR / forensic hunt | [[sift-claude-code-dfir-talk\|Protocol SIFT]] — Claude Code over MCP orchestrating the SANS SIFT forensic toolset from a natural-language prompt | Experimental research initiative; explicitly not forensically validated or court-admissible |
| Vendor hunt assists | Hunt-oriented copilots and agents in the major stacks — Microsoft Security Copilot, Google SecOps (Gemini), CrowdStrike — generating and running hunt queries from natural language | Mixed GA/preview across vendors; re-verify current name and status at use |
| ATT&CK as hunt scaffold | MITRE ATT&CK technique and tactic catalogue as the structured basis for coverage-driven hunts and for tagging hunt findings | GA reference; plain-text catalogue, no wiki page |

The load-bearing row is the **hypothesis-and-hunt agent**. The search substrate and ATT&CK scaffold are mature and mechanism-agnostic; what is new and unsettled is the agent that generates and refines hypotheses at scale, and the orchestrator-subagent topology that lets it fan out. That layer is practitioner-reported rather than independently benchmarked, which is why the automation boundary is drawn around it rather than assumed away.

## Failure modes and what to watch

- **Plausible-but-wrong hypotheses (the defining risk).** A fluent agent generates hypotheses that read as credible and waste the hunter's time on disproof. Bounded by **D3** — a rubric and ground-truth check on hypothesis and finding quality, not just on output fluency — and by the automation boundary that keeps human hypothesis-selection in the loop. This is the failure that scales *with* autonomy, which is why it caps the ladder.
- **Missed novel threats.** Over-automation narrows hunting toward what the agent already knows to look for, eroding the function's reason to exist — finding what detection content does not. Bounded by keeping human hypothesis-framing load-bearing (the automation boundary) and by not pushing hunting to L4 across broad scope.
- **Hunt fatigue / lead flooding.** An agent that surfaces every weak signal floods the hunter much as a noisy detection floods triage. Bounded by **D3** quality gating and the orchestrator's narrowing step, which is supposed to drive *toward* pivotal evidence rather than enumerate everything.
- **Reckless hunt-and-act.** Letting a hunt agent act on a lead — contain, isolate, block — before a human confirms the hypothesis is the costliest over-delegation, because hunting's hypotheses are unproven by construction. Gated hard by **D3** before any move toward autonomy on the action, and by the incident-response function's own D4/D8 authority gates once an action is in scope; hunting that finds a lead should hand off across the human-authority boundary, not act.
- **Hunting blind in unobserved telemetry.** A hunt is only as good as the data it reaches; D1 gaps mean the agent confidently concludes "no evidence" from a source that was never collected. Bounded by **D1** coverage measured against the threat model, so absence-of-evidence is distinguished from absence-of-telemetry.
- **Unobservable agent reasoning.** If a hunter cannot see *why* the agent pursued a branch, a wrong hunt cannot be corrected in flight and a right one cannot be trusted. Bounded by **D5** — the reasoning trajectory must be observable and the run interruptible.

> [!gap] Hunting autonomy is bounded by two different limits
> The gating rule and the automation boundary are not the same constraint and can disagree. A SOC can be mature enough (D1/D3/D5) to *earn* high hunting autonomy and still be right to hold it lower, because past the automation boundary more autonomy produces more plausible-but-wrong hypotheses rather than more findings. Where exactly the boundary sits is a per-team judgment from the [[ai-automation-boundary-threat-hunting-talk|Datadog account]], not a calibrated threshold; it depends on the hunt class, the telemetry, and the cost of a wasted hunter-hour. This is the open calibration question for the function.

## Right-sizing by org profile

The realistic hunting autonomy target is scored against the team's data reach, evaluation maturity, and the value of a hunter-hour at its scale. A small team that hunts rarely and borrows capability is right-sized, not immature.

| Band | Realistic hunting target | Why |
|---|---|---|
| Solo / small | L0 → L1, mostly borrowed | Near or below the [[cyber-poverty-line\|cyber poverty line]], proactive hunting is a luxury function — the team is consumed by triage and response. Hunting is borrowed through an MDR/MSSP or an ISAC's threat-intel-led hunt packages, where the *provider's* hunt maturity governs. Where the team hunts at all, an AI assist that drafts queries (L1) lowers the barrier that historically kept hunting out of reach, but hypothesis selection stays human. |
| Mid | L1 → L3 (bounded) | An in-house SOC can run hypothesis-and-hunt agents within bounded scope — defined telemetry, defined hypothesis classes — once D1 reaches usable coverage and D3/D5 can judge and observe the agent. L3 on a narrow, well-characterized hunt class (for example an ATT&CK-driven hunt for a specific technique) is realistic; broad open-ended hunting stays human-directed. |
| Enterprise | L3, selective and narrow | A full hunt fleet with a search-in-place data plane and a mature evaluation harness can run orchestrator-subagent hunts autonomously in-bounds across more hunt classes. Even here, L4 is deliberately rare: hunting's value rests on human hypothesis-framing and judgment on ambiguous findings, so the automation boundary holds the function below L4 by design rather than for lack of maturity. |

AI is a barrier-lowering enabler at the small-team floor in a specific way for hunting: it does not make proactive hunting affordable outright — that still costs analyst attention the smallest teams do not have — but where a team can spare any hunting time, an agent that drafts and refines queries removes the deep-query-language expertise that historically gated the work, and search-in-place removes the centralized-store cost that gated the data. Hunting is the function a small team borrows first, not builds first.

## Relations

- **Reference architecture:** the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] and its [[agentic-soc-reference-architecture#4-data--knowledge-plane|Data & Knowledge plane]] (telemetry and threat-intel grounding) and [[agentic-soc-reference-architecture#1-orchestration-plane|Orchestration plane]] (supervisor-worker hunt decomposition); the per-function table places hunting on these planes, gated by D1/D3/D5.
- **Maturity model:** the [[agentic-soc-cmm|Agentic SOC CMM]] and its [[agentic-soc-cmm#the-gating-rule|gating rule]]. Hunting's governing domains deep-dive at [[agentic-soc-cmm-d1-telemetry-data-readiness|D1 Telemetry & Data Readiness]] (the data to hunt over), [[agentic-soc-cmm-d3-evaluation-ground-truth|D3 Evaluation & Ground-Truth]] (judging hypothesis quality; the gate on hunt-and-act), and [[agentic-soc-cmm-d5-observability-oversight|D5 Observability & Oversight]] (seeing and interrupting the agent's reasoning).
- **Data substrate:** [[security-data-pipeline-architecture|Security Data Pipeline Architecture]] — search-in-place and on-demand correlation are what make hunting over schema-diverse telemetry tractable without a centralized SIEM.
- **Practitioner evidence:** [[ai-automation-boundary-threat-hunting-talk|Exploring the AI Automation Boundary for Threat Hunting]] (Datadog — the single-agent → orchestrator-subagent migration and the explicit automation boundary; the load-bearing source) and [[sift-claude-code-dfir-talk|Protocol SIFT]] (Claude Code over MCP on a DFIR/hunt workstation).
