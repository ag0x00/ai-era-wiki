---
type: maturity-model
title: "Agentic SOC CMM D8 People and Governance"
address: c-000194
created: 2026-06-03
updated: 2026-08-21
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - governance
  - people
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-ra-incident-response|Agentic SOC Incident Response Surface]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[mythos-ready-security-program]]"
  - "[[vulnops-l1-soc-extinction]]"
  - "[[decision-rights]]"
  - "[[distributed-kill-switch]]"
  - "[[shadow-automation]]"
  - "[[citizen-coders]]"
  - "[[cyber-poverty-line]]"
  - "[[eu-ai-act]]"
sources:
  - "[[agentic-soc-cmm]]"
  - "[[mythos-ready-security-program]]"
  - "[[vulnops-l1-soc-extinction]]"
  - "[[decision-rights]]"
---

# Agentic SOC CMM D8 People and Governance

Companion deep-dive to the [[agentic-soc-cmm|Agentic SOC CMM]]'s D8 domain. D8 measures whether the organization can run agentic defense safely *at scale*: whether the team has the skills and AI adoption to operate the agents, whether analyst capacity survives the workforce shift rather than burning out under it, whether roles have evolved deliberately, whether response is rehearsed against *simultaneous* incidents, whether the human-authority boundary is a governed commitment rather than a tool setting, and whether Security, Legal, Procurement, and Engineering are aligned closely enough that a function's autonomy can be raised without a process stall. It is the second of the two **L4 autonomy gates** (delegated), paired with D7 (Resilience & Agent Supply Chain): a function may not be delegated to its agents until the organization can govern that delegation at scale. The domain scores the **human-authority boundary** plane of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] and, jointly with D4, the deterministic policy plane where autonomy is enforced.

D8 is a single domain because team readiness and cross-functional alignment are the same axis at two scales. At a solo or small SOC the axis is informal: one person holds the skills, the authority boundary, and the vendor relationship in their head. At enterprise scale the same axis is formal, carried by a documented role taxonomy, a governance body, an autonomy-change procedure, and signed cross-functional agreements. Splitting them into separate domains would imply a small team must build enterprise machinery to be mature, which inverts the org-profile rule: a small team running this informally for its scale is right-sized, not immature.

> **The governing question of D8 is who is allowed to grant a SOC function more autonomy, and on what evidence.**
> Every other criterion in the domain — skills, capacity, rehearsal, cross-functional alignment — exists to make that decision answerable. D8 is where the autonomy ladder's mechanism becomes an organizational commitment: the human-authority boundary is asymptotic by design (no unsupervised L5), and D8 is the domain that holds it there.

The autonomy-raising decision is a [[decision-rights|decision right]] in its own right. Raising a triage agent from L2 (act with approval) to L3 (autonomous in-bounds) is an action class; like any action class it needs a named approver, a justification, and a time bound. The mature form records this as a row in the decision-rights matrix: *who* may raise a function's autonomy, *on what evidence* (the D3 evaluation result, the D5 oversight record, the D7 supply-chain attestation), and *for how long* before the grant is re-reviewed. Without that row, autonomy creeps upward informally: someone flips a setting because the agent "seems reliable," which is the model's defined failure mode of running above earned autonomy.

> [!gap] Wiki-internal calibration
> The level criteria and the L4 gating threshold are wiki-internal calibration synthesized from the CMM design spec, the [[mythos-ready-security-program|Mythos-ready Security Program]] playbook, and the grounding sources below, not an externally ratified standard. The control landscape names real, dated standards and practices; the maturity ladder and the gate are the model's own and will firm up as the SOC pair is stress-tested against practitioner deployments.

## Control landscape (dated)

D8 has no enforcement engine. Its controls are people, roles, procedures, rehearsals, and signed agreements, so the landscape is governance frameworks, workforce practices, and the artifacts that record the human-authority commitment. The AI-specific particulars are dated; the underlying disciplines (governance, capacity planning, table-top exercises) predate AI and outlast it.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Governance frameworks | [[eu-ai-act\|EU AI Act]] Art. 14 human oversight and Art. 50 transparency; NIST CSF 2.0 *Govern* function; the [[mythos-ready-security-program\|Mythos-ready]] Innovation-Acceleration Governance action (PA 4, cross-functional Security/Legal/Engineering body) | Frameworks stable; the EU AI Act's high-risk-system enforcement shifted to Dec 2027 under the Digital Omnibus, with Art. 50 transparency still applying Aug 2026 — verify the deadline that binds your deployment |
| Team-and-process maturity model | [[soc-cmm\|SOC-CMM]]'s People and Process dimensions — the incumbent maturity instrument for SOC staffing, training, and procedure, which as of its 2025 report does not yet model AI or autonomy | GA and widely used; this domain extends it into the autonomy era rather than replacing it |
| Autonomy-safety controls | the [[decision-rights\|decision-rights]] matrix (who may grant which authority, on what justification, time-bound) applied to the autonomy-raising decision; [[distributed-kill-switch\|distributed kill-switch]] / halt-authority as a decision right held by every in-loop human | Pattern-level, not a product; assembled from a governance policy plus the deterministic policy plane (D4) that enforces the granted level |
| Rehearsal practice | table-top exercises extended to **simultaneous-incident** scenarios; AI-incident-response drills; the [[mythos-ready-security-program\|Mythos-ready]] 90-day plan's "update playbooks for simultaneous incidents" and "increase people and capacity / protect against burnout" items | Established discipline; the simultaneous-incident and AI-IR variants are the dated extension |
| Cross-functional alignment | accelerated Security ↔ Legal ↔ Procurement ↔ Engineering onboarding (Mythos-ready PA 4 and the 90-day "accelerate procurement and governance" item); configure/buy/build vendor-selection discipline to cut AI-security vendor noise | Practice-level; the procurement chokepoint and a comply-or-explain posture toward [[shadow-automation\|shadow automation]] are the governing mechanisms |

The skills layer has no purchasable shortcut. D8 is the most labor- and culture-bound domain in the model: its controls are training time, role redesign, rehearsal hours, and the political work of cross-functional alignment, none of which a license buys. AI lowers the automation barrier for the *functions*, but the readiness to govern those functions is built, not bought.

## Capability levels

Levels are cumulative: Level N assumes every Level N−1 criterion. Each is stated as an organizational-readiness capability specific to running an agentic SOC.

- **L1 — Initial.** No defined AI-adoption posture in the SOC; agents are switched on by whoever has access, with no named owner of the autonomy decision. Skills are ad hoc, capacity is unmeasured, and there is no rehearsal of agent-driven response. The human-authority boundary exists only as whatever a tool happens to gate by default.

- **L2 — Developing.** The SOC has a stated AI-adoption posture and at least one named owner accountable for agent behavior in operations. A baseline skills inventory exists and analysts have started using agents on assisted (L1) and approval-gated (L2) work. The human-authority boundary is written down for the functions in use — which actions an agent may take and which require approval — even if enforcement is still partly manual. This readiness, with D1 and D4, is the people-side precondition for running functions at L2.

- **L3 — Defined.** Roles have evolved deliberately rather than by drift: the workforce shift the field calls [[vulnops-l1-soc-extinction|L1-SOC absorption]] is managed, not endured — the team knows which work the agents are absorbing (alert triage, initial investigation, ticket routing) and which work it is elevating analysts into (policy and guardrail design, supervisory tuning, the collaborative analyst-agent "threads" model). A cross-functional governance body meets on a fixed cadence with Security, Legal, and Engineering represented (the [[mythos-ready-security-program|Innovation-Acceleration Governance]] action), and Procurement is in the loop for the agent-and-tooling stack. Table-top exercises run on a schedule and include at least one agent-driven-response scenario. The autonomy-raising decision is documented as a [[decision-rights|decision right]]: a named approver and a written evidence bar for moving a function up the ladder. Capacity is tracked and burnout is monitored rather than assumed away.

- **L4 — Managed.** The organization can govern delegation at scale, and this is the rung that, with D7, unlocks function-autonomy **L4 (delegated)**. The human-authority boundary is a quantitative governance commitment, not a setting: autonomy grants are issued against a documented evidence bar (the relevant D3 evaluation result, D5 oversight record, and D7 attestation), recorded with an approver and a re-review date, and reported upward. Per the gating rule, a function may run delegated only when D8 holds at this level *and* D7 does — the people-and-governance side and the resilience-and-supply-chain side of the L4 gate are both load-bearing. Rehearsal covers **simultaneous incidents** explicitly, evidenced from the [[agentic-soc-ra-incident-response|incident-response surface]] as the only function reaching this rung's full gate set: the team has drilled the case where machine-speed attacks open several incidents at once and the response agents must act in parallel under a coherent authority boundary, with pre-authorized containment and a communications plan that does not assume one incident commander. Capacity is managed against the agent-augmented workload with measured burnout indicators (queue age, override-review load, on-call fatigue), and the workforce plan protects experienced staff as patch-and-incident volume rises. Cross-functional alignment is fast enough that onboarding a defensive capability or raising a function's autonomy does not stall on Legal or Procurement; regulatory and liability exposure is tracked against the binding frameworks. Executive leadership has a working definition of urgency and funds the program accordingly.

- **L5 — Optimizing.** Governance adapts under measurement. The autonomy-raising procedure is tuned from operational evidence: grants that were later walked back feed into a stricter evidence bar, and the cross-functional body's decisions are reviewed for whether they accelerated or stalled defensive onboarding. Role evolution is continuous: the team tracks the [[vulnops-l1-soc-extinction|monitor-mode]] trajectory (the SOC supervising rather than executing) and the **CISO-as-router** shift, and adjusts staffing and skills ahead of the curve rather than reacting to it. Simultaneous-incident rehearsal is run regularly with measured outcomes (time-to-coherent-response across parallel incidents) that feed playbook updates. Burnout and capacity indicators stay within published thresholds. The human-authority boundary is held as a standing commitment with periodic re-attestation that no function has drifted above its earned autonomy.

- **L5+ — Leading Edge.** All of L5, plus a named external contribution to the governance-and-workforce layer of agentic defense — for example a published autonomy-change-control procedure or decision-rights schema for SOC functions, a simultaneous-incident table-top scenario library released for others to adopt, or a contribution to NIST CSF *Govern* / SOC-CMM working groups on how to model AI autonomy in SOC maturity.

## Right-sizing by org profile

The realistic D8 target rises with the size of the agent fleet, the blast radius of delegated functions, and the regulatory exposure the organization carries. A small team governing this informally for its scale is right-sized, not immature; the domain is deliberately one axis read at two scales.

| Band | Realistic D8 target | Why |
|---|---|---|
| Solo / small | L2, informally | Near or below the [[cyber-poverty-line\|cyber-poverty line]]. The readiness axis is held by one or two people: the same person owns the AI posture, the authority boundary, and the (often MSSP/MDR) vendor relationship. A named owner, a written authority boundary for the few functions in use, and borrowed capability through ISACs or a managed provider are the right form. Formal governance bodies and simultaneous-incident drills are not warranted; the provider carries much of the burnout-and-capacity load |
| Mid | L3 | An in-house SOC running selective delegation needs deliberate role evolution, a fixed-cadence cross-functional body, scheduled table-tops with at least one agent-response scenario, and a documented autonomy-raising decision right before it lets functions run autonomously in-bounds. This is the band where managing L1-SOC absorption rather than enduring it becomes load-bearing |
| Enterprise | L4, selective L5 | A full agent fleet with broad blast radius and real regulatory exposure needs the quantitative authority-boundary commitment, simultaneous-incident rehearsal, measured capacity-and-burnout governance, and fast Security/Legal/Procurement/Engineering alignment that the L4 gate requires. L5 where the fleet is largest and the workforce shift is most advanced |

A small team at L2 governing the autonomy decision in one person's head is right-sized: with a handful of well-gated agents and capability borrowed from a provider, the marginal value of a standing governance body and a simultaneous-incident drill program is low, and the [[cyber-poverty-line|cyber-poverty-line]] reality makes building them a misallocation. The right move below the line is collective defense — ISACs, CERTs, sector groups — not enterprise governance machinery.

## Cost model

D8 is the model's most labor- and culture-bound domain; the cost is almost entirely people time and organizational change, with licensing near zero. The real spend is the skills build, the rehearsal cadence, and the recurring political work of cross-functional alignment.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 (posture and authority boundary authored in-house) | ~0.25 FTE to set the AI-adoption posture, name an owner, write the authority boundary, and inventory skills | Low; absorbed into existing leadership time |
| L3 | ~0 | ~0.5–1 FTE-equivalent recurring across the team: the cross-functional body's cadence, scheduled table-tops, role-evolution planning, decision-rights upkeep, and capacity/burnout tracking | The dominant cost is meeting and rehearsal time, distributed across Security, Legal, and Engineering, not a budget line |
| L4 | ~0 to low (GRC or exercise-management tooling where used) | the autonomy-change-control procedure and its evidence-gathering; simultaneous-incident drill design and run; quantitative burnout/capacity governance; fast cross-functional onboarding; regulatory-exposure tracking | Peak labor: simultaneous-incident rehearsal and the autonomy-governance procedure are the heaviest recurring commitments, plus the standing cross-functional alignment cost |
| L5 | mostly labor; any contribution tooling is OSS | continuous tuning of the autonomy procedure from outcomes, ahead-of-curve workforce planning, regular measured drills, periodic authority-boundary re-attestation | Sustained governance labor; the spend is keeping the discipline live, not a tool |

D8 spend is people and rehearsal time, and the binding constraint below the cyber-poverty line is **capacity, not capability**. As the [[cyber-poverty-line|cyber-poverty-line]] page notes, free and pro-bono tooling raises the capability floor, but the people, time, and triage discipline D8 measures remain the scarce resource. A team can acquire the agents cheaply and still be unready to govern them, the gap this domain exists to surface.

## Open questions

- The L4 gating threshold — that D8 (with D7) must hold before a function may run delegated — is anchored to the CMM's MDPI ↔ SOC-CMM correspondence and is calibratable, not a fixed constant.
- The L1-SOC-absorption and monitor-mode trajectory is partially predictive. The [[vulnops-l1-soc-extinction|VulnOps / L1-SOC-extinction]] source is one strategic-narrative data point, not an empirical anchor; how fast roles actually shift, and in which sectors, is not yet measured, so the L3 role-evolution criterion describes a managed direction rather than a settled end state.
- There is no agreed metric for analyst burnout in an agent-augmented SOC. The model names indicators (override-review load, queue age, on-call fatigue, rubber-stamp rate) but the threshold at which capacity governance is "managed" is wiki-internal, not externally ratified.
- The binding regulatory deadline is jurisdiction- and deployment-specific. The EU AI Act's human-oversight (Art. 14) and transparency (Art. 50) provisions are load-bearing for D8's liability criterion, but the Digital Omnibus moved high-risk-system enforcement to December 2027 while Art. 50 transparency applies from August 2026; an organization must map D8 to the deadline that actually binds it.
- The boundary between D8 (governing the autonomy decision at the organizational level) and D4 (enforcing the granted authority at the policy plane) is clean in principle but blurs in practice: the decision-rights matrix is authored under D8 and enforced under D4, and the two are scored separately though one artifact spans both.

## Relations

- The [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|Agentic SOC CMM]] main page (domain D8) and its [[agentic-soc-cmm#the-gating-rule|gating rule]] — D8 is one of the two L4 autonomy gates, paired with D7 (Resilience & Agent Supply Chain).
- The [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] — D8 scores its human-authority boundary plane and, jointly with D4, the deterministic policy plane where a granted autonomy level is enforced.
- The [[agentic-soc-ra-incident-response|Agentic SOC Incident Response Surface]] is the only SOC function that reaches the full cumulative gate set at L4 — D4 and D5 carried up from the lower rungs, plus D7 and D8 at the top. Response is therefore where this domain's L4 criteria are exercised, because the simultaneous-incident rehearsal and the governed human-authority boundary are written against machine-speed containment.
- Shared layer (partial): the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]'s D9 (Operations & Human Factors — HITL-fatigue, continuity, decommission) and D1 (Governance & Accountability — accountable owner, cross-functional risk body). Most of D8 is SOC-operations-specific — the analyst workforce shift, simultaneous-incident rehearsal, and the autonomy-raising decision — so this page cross-references the application-security pair where the overlap is real rather than restating it.
- Sibling autonomy gates: D1 (Telemetry & Data Readiness) and D4 (Identity & Action-Authority, the L2 gates), D3 (Evaluation & Ground-Truth) and D5 (Observability & Oversight, the L3 gates), and D7 (Resilience & Agent Supply Chain, the L4 partner).
- Program-level companion: the [[mythos-ready-security-program|Mythos-ready Security Program]] — its Innovation-Acceleration Governance action (PA 4), simultaneous-incident playbook update, people-and-capacity / burnout protection, and 90-day plan are the CISO-program-level instruments behind D8's criteria; this domain scores the maturity of the SOC capabilities several of those actions build.
- Domain grounding and concepts: [[vulnops-l1-soc-extinction|the VulnOps / L1-SOC-extinction source]] (L1-SOC absorption, monitor-mode end state, CISO-as-router), [[decision-rights|Decision Rights for AI Agents]] (the autonomy-raising decision and the halt-authority decision right), [[distributed-kill-switch|Distributed Kill Switch]] (halt-authority held by every in-loop human), [[shadow-automation|Shadow Automation]] (the comply-or-explain posture and the procurement chokepoint), [[citizen-coders|Citizen Coders]] (the workforce shift widening the surface the SOC must govern), and the [[cyber-poverty-line|Cyber Poverty Line]] (capacity, not capability, as the binding small-team constraint).
