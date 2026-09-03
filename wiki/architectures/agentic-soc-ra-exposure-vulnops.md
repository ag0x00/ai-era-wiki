---
type: architecture
title: "Agentic SOC Exposure and VulnOps Surface"
address: c-000200
created: 2026-06-03
updated: 2026-09-01
tags:
  - architectures
  - agentic-soc
  - vulnops
  - exposure-management
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
related:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-cmm-d1-telemetry-data-readiness]]"
  - "[[agentic-soc-cmm-d4-identity-action-authority]]"
  - "[[vulnops]]"
  - "[[vulnops-l1-soc-extinction]]"
  - "[[zero-day-clock]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[mythos-ready-security-program]]"
  - "[[continuous-threat-exposure-management]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[plan-validate-execute]]"
  - "[[cyber-poverty-line]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[vulnerability-properties]]"
  - "[[arizona-state-university]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
  - "[[adversarial-reflexion]]"
sources:
  - "[[agentic-soc-reference-architecture]]"
  - "[[agentic-soc-cmm]]"
  - "[[vulnops]]"
  - "[[zero-day-clock]]"
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
---

# Agentic SOC Exposure and VulnOps Surface

The **Exposure & VulnOps** row of the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] runs continuous exposure and vulnerability discovery, plus remediation, across the whole estate — own code, AI-generated and vibe-coded applications, third-party libraries, container images, and the cloud control plane. This page is that row's per-function deep dive. The function holds the SOC's seam with DevSecOps, where build-time security hands off to operate-and-monitor and where production exposure is owned, and it is the operational home of [[vulnops|VulnOps]] — discovery and remediation staffed and automated like DevOps rather than run as a quarterly audit cycle.

The agent surface is a pair of worker patterns under the orchestrator: a read-heavy **discovery/exposure** agent and a state-changing **remediation** agent. The split is load-bearing. Discovery is reversible and can run at high autonomy; remediation applies a patch or isolates an asset, which changes the production estate, so it is gated separately.

The function runs primarily on the RA's **Data & Knowledge plane** (the telemetry, asset, and threat-intel substrate it reads) and the **Policy & Enforcement plane** (the deterministic gate every remediation action crosses). Its autonomy is gated by [[agentic-soc-cmm#the-gating-rule|the CMM gating rule]] on **D1 (Telemetry & Data Readiness)** — the asset and exposure data the agents reason over — and **D4 (Agent Identity & Action-Authority)** — the scoped, blast-radius-bounded authority a remediation agent needs before it may act.

The time-to-exploit collapse drives the function. The [[zero-day-clock|Zero Day Clock]] records the median time from CVE disclosure to first observed exploitation falling from 771 days in 2018 to zero-day in 2025–2026, where the exploit arrives on or before the advisory.[^zdc] A quarterly pen test plus patch-as-CVE-arrives runs at a cadence continuous AI-driven discovery outpaces, and the estate is now too large and too fast-changing for humans to inventory or patch in time.

## The agent surface

The function decomposes into two worker patterns on the supervisor-worker topology, kept distinct because they sit on opposite sides of the consequential-action line.

**A read-heavy discovery agent enumerates the estate and maps exposure onto it.** It covers asset and attack-surface inventory, SBOM and dependency-chain resolution, configuration and cloud-control-plane posture, and code scanning across own and AI-generated repositories. It ingests external intelligence — CTI feeds, ISAC data, vendor advisories, GitHub disclosures, government feeds — and maps it automatically onto organization-specific assets, the un-silo discipline [[vulnops-l1-soc-extinction|Mallory's VulnOps framing]] centers.[^mallory] The mechanism is hybrid, because deterministic scanners and SBOM tooling supply coverage while AI supplies the context, the reachability reasoning, and the exploitability triage that separates reachable flaws from noise. Frontier-model code audit — the [[frontier-ai-for-vuln-discovery|discovery-capability thesis]] — sits here, where the harness around the model does the validation work.[^frontier]

**A state-changing remediation agent proposes or applies the fix**: a patch, a configuration change, a virtual patch or compensating control, or asset isolation. It calls the change-management and deployment tooling, and every action crosses the Policy & Enforcement plane's deterministic gate. The [[plan-validate-execute|plan-validate-execute]] pattern bounds what the agent may do there, and D4's auto / propose / approve / block tiers and blast-radius limits are enforced at the same gate.

Both agents read the Data & Knowledge plane's asset and threat-intel substrate and write findings into the case/thread investigation substrate — [[vulnops-l1-soc-extinction|threads, not cases]], each a collaborative analyst-agent exchange.[^mallory] The **human-authority boundary** sits asymmetrically: light on discovery, because read-only enumeration is reversible, and firm on remediation, because an applied patch or an isolation crosses the approval tier its blast radius warrants.

## Autonomy progression

**Discovery autonomy and remediation autonomy are scored and gated separately**, because discovery is read-heavy and reversible while remediation changes production state. A SOC can legitimately run discovery at high autonomy and hold remediation at a lower rung, and that split is the common and correct posture.

The gating rule applies per the [[agentic-soc-cmm#the-gating-rule|CMM]]: a function reaches autonomy L_k only when its governing domains are mature enough to support it. For this function the L2 gates are **D1** (the asset and exposure data must be real and reasonably complete) and **D4** (a remediation agent must hold scoped, revocable authority before a consequential action it proposes can be approved and bounded). L3 adds D3 (Evaluation & Ground Truth) and D5 (Observability & Oversight); L4 adds D7 (Resilience & Agent Supply Chain) and D8 (People & Governance).

| Level | What it looks like for this function | Gating domains |
|---|---|---|
| **L0 — Manual** | Quarterly pen test; vulnerability scan reviewed by hand; patches scheduled in a maintenance window | — |
| **L1 — Assisted** | Continuous scanning with AI-assisted triage; the agent ranks and explains findings, a human decides and acts | D1 (data to rank against) |
| **L2 — Semi-autonomous** | Discovery runs continuously and proposes findings with exploitability and confidence scores; remediation executes routine sub-tasks, and every applied patch or isolation needs explicit approval | D1 + D4 (scoped, revocable remediation authority; coarse auto/approve split) |
| **L3 — Conditional** | Discovery autonomous in-bounds; remediation auto-applies low-blast-radius, high-confidence fixes within blast-radius limits and escalates out-of-bounds; humans monitor | D1, D4 (auto/propose/approve/block tiers + blast-radius limits) + D3, D5 |
| **L4 — Delegated** | The function owns the discover-triage-remediate lifecycle within governed bounds; humans govern outcomes and the autonomy-raising decision | + D7, D8 |

L4 is asymptotic. High-blast-radius remediation terminates at the human boundary at every level, so L4 describes delegated lifecycle ownership under governance rather than unsupervised patching.

Two changes to the function's shape follow from the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]].[^bh] First, the discovery side acquires a standing red-team component. Autonomous agents found and chained two zero-days in a production enterprise repository manager and two more in a production dataset platform, with no prior knowledge of any of them. Continuous agentic red teaming therefore becomes a permanent function of this row rather than a periodic engagement on an audit calendar. It belongs to the discovery agent's remit — read-heavy, reversible against a staging surface, gated on the same D1 data — and adversary capability sets its cadence. Second, the remediation leg the source names as the actual gap is the leg the L3 and L4 rows above describe, and this page treats that gap as a failure mode below.

The defined failure mode is **operating above the earned ceiling**: granting remediation autonomy the governing domains do not support. Auto-applying patches (L3 remediation) when D4 cannot bound blast radius, or when D3 cannot measure whether the agent's exploitability triage is correct, is reckless autonomy, because a wrong containment or a bad patch is itself an availability incident. The weakest governing domain sets the ceiling, and the common split leaves discovery several rungs above remediation.

**The false-positive flood bounds this function before any autonomy rung does.** AI-generated findings arrive faster than human triage capacity, and most are not reachable in practice. [[anthropic-glasswing-initial-update|Anthropic's Glasswing one-month update]] reported the bottleneck inverting from discovery to verification: of roughly 6,202 estimated high/critical findings, 1,752 were assessed and 75 patched, and maintainers asked Anthropic to slow disclosures.[^glasswing] JFrog's 2026 analysis found 66% of analyzed CVEs had a low applicability rate (0–20%) and only 12% were highly exploitable in real environments.[^jfrog] Exploitability triage bounds the flood — severity and confidence scoring, deduplication, and reachability analysis as first-class queue stages — which [[mythos-ready-security-program|Mythos-ready PA 11]] names as designing VulnOps around triage discipline from the start.

## Control landscape (dated)

Vendors and patterns below are swappable examples carrying a date, and none is an endorsement. The function's spine — continuous discovery, triage, remediation — holds independently of mechanism; the AI-specific particulars and the named products carry the mid-2026 timestamp.

| Capability | What ships today | Status (mid-2026) |
|---|---|---|
| Exposure / attack-surface management | Continuous attack-surface and exposure management platforms; cloud security posture management for the control plane | GA; an established category |
| Continuous exposure program model | Gartner [[continuous-threat-exposure-management\|CTEM]] as the program spine for continuous discovery, prioritization, validation, and mobilization | GA as a framework; adoption maturity varies |
| Asset / dependency inventory | SBOM generation and dependency-chain resolution; [[ai-bom\|AI-BOM]] for the AI-component supply chain | SBOM GA; AI-BOM emerging |
| AI-assisted code audit, commercial | [[codex-security\|Codex Security]], [[claude-code-security\|Claude Code Security]], [[codemender\|CodeMender]]; vendor-internal [[big-sleep\|Big Sleep]] and [[mythos\|Mythos]]-class models | Preview-gated; CodeMender in [[google-cloud-codemender-preview\|managed preview]] since July 2026; Mythos preview-only, no GA planned |
| AI-assisted code audit, open source | [[openant\|OpenAnt]] and the [[oss-ai-vuln-discovery-harness-landscape\|nine harnesses]] Semgrep surveyed, under Apache 2.0, MIT and CC-BY-SA | Installs today; one entry runs a fully local model; no reference implementation has emerged and Semgrep expects none soon |
| Offensive testing at scale | [[wiz\|Wiz]] Red Agent, [[palo-alto-networks\|Palo Alto]] Unit 42 AI pentesting, [[crowdstrike\|CrowdStrike]] Frontier AI Readiness | Productized; vendor-reported coverage figures |
| Exploitability triage | Severity and confidence scoring, deduplication, reachability and applicability analysis as queue stages; [[adversarial-reflexion\|adversarial-reflexion]] control; sandboxed PoC as a pre-patch gate | Pattern-level; the load-bearing scarce-resource discipline. Now packaged in commercial preview, without published false-positive data |
| Continuous / autonomous patching | Automated patch pipelines; virtual patching and compensating controls; autonomous patch deployment where the change is low-blast-radius | Emerging; a growing share of patches now ship without a human in the loop, though most complex applications still patch slowly[^zdc] |
| Remediation authority enforcement | [[plan-validate-execute\|Plan-validate-execute]], policy-as-code, and SOAR/response-platform approval tiers with blast-radius limits (scored by D4) | GA as primitives; per-action authority tiering over remediation agents is configuration |

Two rows carry the function. Exploitability triage bounds the false-positive flood on the discovery side, and remediation-authority enforcement lets remediation autonomy rise while control of production change stays with the organization. AI lowers the barrier on the discovery side, because continuous code audit and CTI-to-asset mapping become reachable without the bespoke engineering they once required. The triage row's sandboxed proof-of-concept gate has [[codemender|CodeMender]]'s verify stage as its sourced instance, and its false-positive control follows the cross-vendor [[frontier-ai-for-vuln-discovery#The validation discipline|validation discipline]].

The discovery row's open-source half changes what arrives in the triage queue. Semgrep's July 2026 survey of nine open-source harnesses compares five of them and records that they do not share a definition of a finding: a triaged static match, a verified candidate from an agentic pipeline, a reproducible AddressSanitizer crash, a re-validated static match.[^semgrep] A discovery agent drawing on more than one instrument feeds the severity, confidence and dedup stages in units that do not compare, so the exploitability-triage row runs on a mixed denominator and the queue length stops being one quantity. The same survey strengthens the row's false-positive control. Adversarial validation, in which a second independent agent attempts to falsify each finding, is widely adopted across the open-source field and works best when a different model attempts the disproof, so the mechanism behind the [[adversarial-reflexion|adversarial-reflexion]] reference in that row is now sourced across a field rather than one vendor.[^semgrep]

## Failure modes and what to watch

- **False-positive collapse (discovery side).** AI-generated findings exceed human triage capacity, and most are not reachable. Unbounded, the queue buries the exploitable flaws among the noise and the team stops trusting it. Bounded by exploitability triage as a first-class discipline (severity/confidence/dedup/reachability) and by **D3 Evaluation**, which measures whether the triage is correct. Triage is the named scarce resource, ahead of patching capacity.[^glasswing][^jfrog]
- **Reckless auto-remediation (remediation side).** Auto-applying a patch or auto-isolating an asset with too large a blast radius is itself an availability incident. Bounded by **D4** — auto/propose/approve/block tiers, blast-radius limits, a documented rollback and human-override path — and by the deterministic Policy & Enforcement plane gate. Remediation autonomy must never exceed what D4 supports, regardless of how good discovery is.
- **Coverage blind spots.** A patch or isolation acts on a defensible picture only if the asset and exposure inventory is real. An un-inventoried asset, a silent telemetry source, or an unresolved dependency is exposure the function cannot see. Bounded by **D1**: measured coverage against the threat model, in place of assumed completeness. [[mythos-ready-security-program|Mythos-ready PA 7]] makes the point that an organization can patch, segment, or defend only what it knows exists.
- **AI-generated and vibe-coded app sprawl.** Coding agents in non-developer hands fragment central visibility, and AI-generated code carries its own flaw profile. These apps are in scope for discovery the same as any other estate, regardless of who shipped them. Bounded by full-estate inventory coverage (D1) and the dependency-chain scope of the discovery agent.
- **The un-automated remediation leg.** The absence of an automated patch bounds this function more often than a bad automated patch does. Where discovery runs at L3 and rollout still crosses a human change-approval queue, every gain in finding rate lands in a backlog, and the function's measured output is a longer queue at an unchanged exposure window. The loop closes only through identify, propose patch, roll out, and **roll back** on an availability regression, with the reversal engineered into the pipeline; automated rollout without automated rollback turns a wrong fix into an outage and forces the human gate back in one stage later.[^bh] Bounded by treating rollback capability as a prerequisite for raising remediation autonomy rather than as a recovery afterthought, and by measuring the finding-to-deployed-fix interval in place of the finding count.
- **Upstream disclosure as an uncosted action.** The discovery agent's scope covers third-party libraries and container images, so a share of its findings are flaws in software the organization does not own and cannot patch. The function's only lever there is upstream disclosure, and one academic measurement puts that lever's sign in question: on embedded devices, at human pace and with no agents involved, disclosing a vulnerability endangered roughly three times as many devices as it secured, because a disclosed flaw carries [[vulnerability-properties|properties]] that transfer to targets outside the advisory's scope.[^asu-keynote] The finding-to-deployed-fix interval this function measures stops at the organization's own estate and never registers that cost. Bounded by treating an upstream disclosure decision as a governed action with a stated exposure window, scored under **D8 People & Governance** alongside the remediation-authority tiers. No source on the wiki offers a disclosure model calibrated for machine-scale discovery, so this stays a named gap rather than a control.
- **Triage / hunt fatigue.** The function absorbs a volume of work no human team alone can, and the team itself can burn out under the flood. Bounded by treating headcount and reserve capacity as a design parameter, scored under **D8 People & Governance**.

## Right-sizing by org profile

Targets split into a discovery rung and a remediation rung, because the two move independently. The realistic remediation target trails discovery in every band.

| Band | Discovery target | Remediation target |
|---|---|---|
| **Solo / small** | L2–L3 | L1–L2 |
| **Mid** | L3 | L2–L3 |
| **Enterprise** | L3–L4 | L3, selective L4 |

**A small team sits near or below the [[cyber-poverty-line|cyber poverty line]] and borrows most of its coverage.** An MSSP, MDR or ISAC supplies exposure coverage and triage, and AI lowers the floor far enough that continuous scanning and AI-assisted triage are reachable without a built-out program. Remediation stays human-approved, because a small team's blast-radius controls are thin. The path runs through borrowed exposure capability and tightly human-gated remediation, and not through standing up a fleet. A second path runs alongside it. A team already working inside a coding agent installs an audit skill that needs no new infrastructure and costs only tokens on the agent it already runs, per Semgrep's LLM-generated deployment comparison, and one surveyed pipeline runs a fully local security-tuned model where source cannot leave the estate.[^semgrep] Both reach the discovery side, and the band's remediation target is unchanged.

**A mid-size team runs a CTEM-shaped program in house.** It owns an exposure platform and a ground-truth store, and remediation can auto-apply low-blast-radius, high-confidence fixes once D4 carries auto/propose/approve/block tiers and blast-radius limits. Higher-impact patches stay gated.

**An enterprise runs the full VulnOps function**, with frontier-model code audit, measured coverage, and a governed remediation pipeline. High-blast-radius remediation still terminates at the human boundary.

## Relations

- Per-function deep-dive hanging off the [[agentic-soc-reference-architecture|Agentic SOC Reference Architecture]] (the **Exposure & VulnOps** row); runs on its **Data & Knowledge** and **Policy & Enforcement** planes.
- Gated by the [[agentic-soc-cmm#the-gating-rule|Agentic SOC CMM gating rule]] on **D1** ([[agentic-soc-cmm-d1-telemetry-data-readiness|Telemetry & Data Readiness]] — asset and exposure data) and **D4** ([[agentic-soc-cmm-d4-identity-action-authority|Agent Identity & Action-Authority]] — remediation authority and blast-radius limits).
- The operational home of [[vulnops|VulnOps]]; the discovery-and-remediation and CTI-fusion framings are sourced in [[vulnops-l1-soc-extinction|From Threat Intel to VulnOps]] and the [[mythos-ready-security-program|Mythos-ready program]] (PA 5 continuous patching, PA 7 inventory/attack-surface reduction, PA 11 stand up VulnOps).
- Driven by the time-to-exploit collapse documented in the [[zero-day-clock|Zero Day Clock]]; the discovery-capability thesis is [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]].
- Build-time counterpart: [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] owns security as code is written; this function is the operate-and-monitor seam where production exposure is owned. The two meet at the DevSecOps handoff.
- Real patterns as dated examples: [[continuous-threat-exposure-management|CTEM]], SBOM, [[ai-bom|AI-BOM]], continuous patching, and exposure/attack-surface management.

## Notes

[^zdc]: [[zero-day-clock|Zero Day Clock]], from zerodayclock.com (Sysdig and collaborators, 2026). Median time-to-exploit by year: 771 days (2018), 84 days (2021), 6.36 days (2023), 4 hours (2024), zero-day (2025–2026). The Qualys 2026 benchmark cited there puts mean time-to-remediation for the most-delayed complex applications at 5 months 10 days even as roughly 40 million of about 150 million deployed patches now ship autonomously.

[^mallory]: [[vulnops-l1-soc-extinction|From Threat Intel to VulnOps]], CYBR.SEC.Media (2026-05-15), featuring Jonathan Cran (Mallory). Continuous ingestion of about 3,000 intelligence sources mapped automatically onto organization-specific assets, cloud, code, and IaC; "threads, not cases" investigation model.

[^frontier]: [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]. The harness around the model does the validation work; the gap between a candidate finding and a validated one is load-bearing.

[^glasswing]: [Anthropic — Project Glasswing: An initial update](https://www.anthropic.com/research/glasswing-initial-update), 2026, via [[vulnops|VulnOps]]. Open-source scanning funnel: 6,202 estimated high/critical found, 1,752 assessed, 75 patched, ~2-week mean patch time; the constraint named as verification, disclosure, and patching, not discovery.

[^bh]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, 2026-08-06. Four zero-days found and chained by autonomous agents against production infrastructure; defender recommendations of continuous agentic red teaming and a fully automated identify → propose patch → roll out → roll back loop, on the reasoning that automating discovery alone relocates the bottleneck to patching. Summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].

[^jfrog]: JFrog 2026 Software Supply Chain Security State of the Union, via [[vulnops|VulnOps]]: 66% of analyzed CVEs had a low applicability rate (0–20%); only 12% were highly exploitable in real enterprise environments.
[^semgrep]: Semgrep, [Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses) (July 2026; no day-level date is exposed, and the month is inferred from an embedded screenshot dated 2026-07-20 and a forward reference to a Black Hat announcement in August 2026): nine open-source harnesses under Apache 2.0, MIT and CC-BY-SA; five compared pipelines with five different definitions of a finding; adversarial validation widely adopted; one pipeline able to run a fully local security-tuned 8B model. The per-tool detail and the isolation column are labelled LLM-generated. See [[semgrep-oss-ai-security-harness-comparison|the source summary]].
[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
