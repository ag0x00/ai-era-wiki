---
type: maturity-model
title: "Agentic SOC CMM D6 Detection and Response Tradecraft"
address: c-000192
created: 2026-06-03
updated: 2026-06-23
tags:
  - maturity-models
  - cmm
  - agentic-soc
  - detection-engineering
  - deception
status: developing
origin: produced
scope_axis:
  - ai-in-sec-defense
  - sec-against-ai
  - sec-of-ai
related:
  - "[[agentic-soc-cmm]]"
  - "[[agentic-soc-reference-architecture]]"
  - "[[d3fend-ai-defense-technique-gap]]"
  - "[[mitre-atlas]]"
  - "[[detection-deception-engineering-orbie-talk]]"
  - "[[syara-semantic-detection-talk]]"
  - "[[binaryshield-ai-fingerprints-talk]]"
  - "[[mythos-ready-security-program]]"
  - "[[cyber-poverty-line]]"
  - "[[nist-ai-800-4]]"
sources:
  - "[[d3fend-ai-defense-technique-gap]]"
  - "[[mitre-atlas]]"
  - "[[detection-deception-engineering-orbie-talk]]"
  - "[[mythos-ready-security-program]]"
---

# Agentic SOC CMM D6 Detection and Response Tradecraft

Companion deep-dive to [[agentic-soc-cmm|the Agentic SOC CMM]]'s D6 domain. D6 measures the quality and coverage of the SOC's actual defensive craft: how completely its detections cover known adversary behavior, how good those detections are once they fire, and how mature its response playbooks and deception are. Coverage is scored against three catalogues: **MITRE ATT&CK** for offensive technique coverage, **MITRE D3FEND** for defensive-technique coverage, and [[mitre-atlas|MITRE ATLAS]] for threats to the in-house AI-application surface. Detection quality (false-positive control and fidelity), deception engineering, and response-playbook maturity round out the domain. Its content lives on the [[agentic-soc-reference-architecture|reference architecture]]'s Data & Knowledge and Policy planes; its application runs through the detection-engineering, triage, hunting, and incident-response agent surfaces.

D6 is an **efficacy gate, not an autonomy gate.** This is the distinction that governs how it scores. The autonomy gates (D1, D3, D4, D5, D7, D8) determine whether an agent can be trusted to act with less supervision; a function's earned autonomy ceiling is set by its least-mature *autonomy* gate. D6 does not cap that ceiling. A SOC can run detection fully autonomously — high D1, D3, and D5, every autonomy gate satisfied — and still detect nothing useful if its detection coverage is thin, its rules are noisy, or its playbooks are stale. D6 therefore caps the *value* of whatever autonomy is exercised, not the autonomy itself. A detection function can be both fully delegated and ineffective at once: D6 measures the second half of that sentence. It is scored like every other domain, but a low score is read as poor defensive output, never as a reason to throttle delegation.

> [!gap] Wiki-internal calibration, and a thin D3FEND axis
> The level criteria, cost model, and right-sizing below synthesize the wiki's own design spec against the grounding sources named in `sources`. They are wiki-internal calibration, not an externally ratified standard, and will firm up as the sibling domains and crosswalks are tested. One axis carries a known defect: D6 scores defensive coverage against MITRE D3FEND, which is thin on AI-era defensive techniques (agent supervision, evaluation-gating, intent attribution, deception against AI-powered attackers). Until that gap closes, **the D3FEND axis reads as a coverage floor, not a measure** of an agentic SOC's AI-era defenses — see [[d3fend-ai-defense-technique-gap|the D3FEND AI-Defense Technique Gap]].

## Control landscape (dated)

Detection and response tradecraft has a mature catalogue, content, and testing layer. The new layer is AI-authored detection content, semantic detection of natural-language threats, and AI-specific response playbooks, which sit on top of it. The AI-specific particulars are dated and swappable, and are kept here rather than in the level definitions.

| Layer | What ships today | Status (mid-2026) |
|---|---|---|
| Coverage catalogues | MITRE ATT&CK (offensive technique vocabulary); MITRE D3FEND (defensive countermeasures); [[mitre-atlas\|MITRE ATLAS]] for the in-house AI-application surface | ATT&CK and ATLAS are GA and stable; D3FEND is GA but thin on AI-era defenses (see [[d3fend-ai-defense-technique-gap\|the gap]]) |
| Detection content & exchange | Sigma / SigmaHQ as the portable rule format; vendor-native analytics; detection-as-code in version control with CI tests | GA; detection-as-code is established practice, uneven in adoption |
| Coverage & efficacy validation | Atomic Red Team for technique-level tests; breach-and-attack-simulation (BAS) platforms; purple-teaming to confirm a detection fires on a real technique | GA; BAS is commercial, Atomic Red Team is OSS and free |
| AI-authored detection | Agents that surface emergent campaigns and write detection rules from live telemetry ([[detection-deception-engineering-orbie-talk\|GreyNoise Orbie]], over internet-scale honeypot data) | Production at a vendor; a leading-edge pattern, not a packaged product for most SOCs |
| Semantic / intent detection | Rules that match attacker *intent* across paraphrases for natural-language threats, with a cheap pre-filter gating an expensive LLM verdict ([[syara-semantic-detection-talk\|SYARA]], open-source) | OSS and installable; benchmark figures are the project's own, not independently reproduced |
| Cross-org detection sharing | Privacy-preserving fingerprints that let siloed services share prompt-injection intelligence across compliance boundaries ([[binaryshield-ai-fingerprints-talk\|BinaryShield]]) | Research-stage; backed by a published paper, no packaged product |
| Deception & automated response | Canaries and honeytokens keyed to TTPs; pre-authorized containment under deterministic policy gates; machine-speed response playbooks (the [[mythos-ready-security-program\|Mythos-ready]] PA 9 and PA 10 capabilities) | Canary/honeytoken tooling is GA; AI-attacker-keyed deception and machine-speed response are emerging practice |

The load-bearing shift in this landscape is the move from *humans writing rules against vendor libraries* to *agents authoring detection content from live telemetry*, and from *byte-signature matching* to *intent matching* for natural-language threats. The Orbie pattern shows the first: an agent surfacing campaigns and drafting rules from internet-scale honeypot data, where the domain knowledge embedded in the tooling matters more than the model choice. SYARA shows the second: the new payload is a sentence, so detection has to match meaning across surface forms a regex cannot enumerate, with a cheap layer gating the expensive LLM to keep the cost survivable at scale.

## Capability levels

Stated as capabilities specific to detection and response tradecraft; cumulative, so Level N assumes every Level N−1 criterion. The level text is mechanism-agnostic — it describes tradecraft discipline that would survive AI normalizing into ordinary tooling. AI-specific particulars (ATLAS coverage, AI incident-response playbooks, deception against AI attackers) live in the dated control landscape above, not in the level definitions. Because D6 is an efficacy gate, the levels describe rising output quality and coverage, not a rising autonomy ceiling.

- **L1 — Initial.** Detection is ad hoc. Content is whatever the tooling ships by default plus a few hand-written rules; coverage against any catalogue is unknown and unmeasured. False positives are tolerated rather than tuned, and response is improvised per incident with no reusable playbook.
- **L2 — Developing.** A detection-engineering function exists. Detections are inventoried and mapped to MITRE ATT&CK techniques, so coverage and gaps are at least visible. Rules are tuned for fidelity rather than left to flood the queue. Core incident-response playbooks are documented for the common incident types. At this level a delegated detection function's output is at least catalogued and intentional, the minimum for the value of any delegation to track real adversary behavior.
- **L3 — Defined.** Detection content is managed as code: versioned, peer-reviewed, and tested in CI before it reaches production. Coverage is measured against ATT&CK on a standing basis and against D3FEND for the defensive techniques the SOC runs (read as a floor, per the gap caveat). New detections run in shadow before they are enabled, and false-positive rate is a tracked, governed metric rather than an accepted nuisance. Response playbooks are standardized and exercised. At L3 the tradecraft is good enough that a delegated detection or triage agent is acting on measured-quality content, not ungoverned rules.
- **L4 — Managed.** Coverage and efficacy are continuously *validated*, not assumed: technique-level testing (Atomic Red Team, BAS) and purple-teaming confirm that detections fire on real adversary behavior, and the false-positive and detection-fidelity metrics are managed against targets. Coverage extends to the in-house AI-application surface, mapped against ATLAS, with AI-specific incident-response playbooks in place. A deception capability is deployed and feeds detections. At L4 the most consequential delegated functions inherit validated, not hoped-for, coverage and response quality.
- **L5 — Optimizing.** Detection content is authored and refined from live telemetry rather than transcribed from vendor libraries, and coverage is driven toward the AI-era defenses the catalogues under-count — deception keyed to AI-attacker TTPs, semantic/intent detection for natural-language threats, and machine-speed pre-authorized response under deterministic policy gates. Coverage, fidelity, and false-positive rate are continuously optimized against measured drift, and the SOC closes its own coverage gaps faster than the public catalogues do.
- **L5+ — Leading Edge.** All of L5, plus a named contribution to the shared catalogues: published detection content, ATT&CK or ATLAS technique submissions, or — the contribution opportunity this domain names directly — authoring a candidate D3FEND-shaped defensive-technique layer for the AI era (agent-supervision, evaluation-gating, intent-attribution, and AI-attacker-deception techniques), the work tracked in [[d3fend-ai-defense-technique-gap|the D3FEND AI-Defense Technique Gap]].

Because D6 is an efficacy gate, none of these levels appears in the gating table that caps a function's autonomy. A SOC reads its D6 level alongside a function's autonomy: a delegated detection function (high autonomy) governed by weak D6 acts fast on weak tradecraft, and the prescriptive output is "improve coverage and fidelity," not "reduce delegation."

## Right-sizing by org profile

The realistic D6 target is scored against the organization's scale, its threat model, and whether it runs an in-house AI-application surface. A small team consuming a provider's curated detection content is right-sized, not immature.

| Band | Realistic D6 target | Why |
|---|---|---|
| Solo / small | L2 → L3 | Near or below the [[cyber-poverty-line\|cyber-poverty line]], a small team cannot author and validate broad detection coverage. The path is borrowing capability — an MSSP/MDR or a managed stack supplies curated, ATT&CK-mapped content and tuned rules, and the team consumes SigmaHQ community content rather than authoring its own. Coverage validation and a deception capability are rarely warranted near-term. |
| Mid | L3 → L4 | An in-house detection-engineering function can run detection-as-code with CI tests, measure ATT&CK coverage, and stand up Atomic Red Team / purple-teaming validation. Extending coverage to an in-house AI-application surface (ATLAS) and deploying deception are the stretch goals where the org runs such a surface. |
| Enterprise | L4 → selective L5 | A dedicated detection-engineering team and the telemetry volume to justify AI-authored content and intent-detection at scale. L5 deception keyed to AI-attacker TTPs and machine-speed response earn their cost where the org is a likely target of AI-driven attack and runs a meaningful AI-application surface. |

A small SOC at L2 consuming an MSSP's ATT&CK-mapped detection content has right-sized D6: the value of its delegated detection tracks real adversary behavior without the team owning the content pipeline. The model records that as an intentional borrow, not a deficiency.

## Cost model

The dominant cost in D6 is detection-engineering and validation labor, not licensing. The catalogues are free, SigmaHQ and Atomic Red Team are OSS, and detection-as-code runs on existing version control and CI; the spend is the human effort to author, tune, validate, and maintain coverage, plus the LLM budget where intent detection runs at scale.

| Level | Tooling / licensing | Operational labor | Run-rate note |
|---|---|---|---|
| L2 | ~0 (ATT&CK is free; community SigmaHQ content; native analytics) | ~0.25–0.5 FTE to inventory detections, map to ATT&CK, and tune for fidelity | Borrowable via MSSP/MDR at near-zero tooling cost for a small team |
| L3 | ~0 to low (detection-as-code on existing VCS/CI; Atomic Red Team is OSS) | ~0.5–1 FTE recurring: content authoring, peer review, CI test maintenance, shadow rollout, and false-positive governance | Detection-as-code shifts cost from incident firefighting to standing engineering; the labor is recurring, not one-time |
| L4 | BAS platform where bought (commercial); Atomic Red Team is free | Recurring coverage and efficacy validation, purple-teaming, ATLAS-mapped AI-app detection, AI-IR playbook upkeep, and deception operation | Validation is the load-bearing labor — unvalidated coverage is unverified coverage; deception adds standing operational effort |
| L5 | AI-authoring and intent-detection compute (LLM budget); mostly labor otherwise | Heaviest: content authored from live telemetry, intent-detection tuning, AI-attacker-keyed deception, machine-speed response engineering | LLM cost is the new run-rate line — the [[syara-semantic-detection-talk\|pre-filter pattern]] (cheap layer gating the expensive LLM) is the mechanism that keeps intent detection affordable at scale |

D6 is a detection-engineering and validation cost, not a tooling cost. The catalogues and the core content and testing tools are free; the spend is the standing engineering to keep coverage broad, current, and validated, plus the compute to run intent detection at volume. Price the authoring and validation rhythm, not the first rule set.

## Open questions

- D6 scores defensive coverage against D3FEND, which is thin on AI-era defensive techniques. Until that catalogue is extended (or a community counterpart emerges), the D3FEND axis reads as a coverage floor rather than a measure — see [[d3fend-ai-defense-technique-gap|the D3FEND AI-Defense Technique Gap]]. Authoring the candidate layer is the contribution opportunity this domain names.
- Coverage against ATT&CK is *measurable* by mapping detections to techniques, but coverage *quality* — whether a mapped detection actually fires on the technique in production — is only as good as the validation behind it. A coverage map without BAS or purple-team validation overstates real coverage; the model treats validated coverage and mapped coverage as different things, but the calibration of how much validation is enough is open.
- Detection of novel AI-era threats on the in-house AI-application surface ([[prompt-injection|prompt injection]], jailbreaks, agent hijacking) rarely leaves traditional file or network signatures, so coverage there is hard to score against ATLAS in the same way ATT&CK coverage is scored. The L4 criterion names the capability; a rigorous coverage metric for the AI-application surface is an open gap shared with [[mitre-atlas|ATLAS]].
- Detecting an agent that is actively deceptive or evading its own monitoring is named by [[nist-ai-800-4|NIST AI 800-4]] as an unresolved post-deployment monitoring challenge. The L5 deception and intent-detection criteria reach toward it, but no catalogue scores coverage of deceptive-behavior detection the way ATT&CK scores technique coverage, so this axis of D6 is bounded by an open problem rather than a measurable target.
- The intent-detection cost figures from [[syara-semantic-detection-talk|SYARA]] (the pre-filter cost reduction at scale) and the cross-org sharing results from [[binaryshield-ai-fingerprints-talk|BinaryShield]] are reported over the projects' own test sets, not independent benchmarks, and should be re-checked against unrelated corpora.
- The gating model treats D6 as an efficacy gate that does not cap autonomy. Whether some minimum D6 floor should be a *precondition* for delegating a detection function — rather than only a quality signal — is the same calibration question D2 raises, and the sibling autonomy gates do not settle it.

## Relations

- Companion deep-dive to [[agentic-soc-cmm#axis-2--the-eight-maturity-domains|the Agentic SOC CMM]]'s D6 domain, which classifies D6 as an efficacy gate alongside D2 Threat Intelligence & Knowledge. Because D6 is an efficacy gate, it does not appear in the [[agentic-soc-cmm#the-gating-rule|gating rule]] table that caps a function's autonomy.
- Scores detection content and response playbooks on the [[agentic-soc-reference-architecture|reference architecture]]'s Data & Knowledge and Policy planes; its application runs through the detection-engineering, triage, hunting, and incident-response agent surfaces, and it carries the in-house AI-application monitored surface.
- The contrasting efficacy gate is D2 Threat Intelligence & Knowledge — D2 grounds *what to look for*, D6 covers *how well the SOC looks and responds*. The sibling autonomy gates that *do* cap a function's autonomy are D1 Telemetry & Data Readiness, D3 Evaluation & Ground-Truth, D4 Agent Identity & Action-Authority, D5 Observability & Oversight, D7 Resilience & Agent Supply Chain, and D8 People & Governance.
- The named contribution opportunity lives in [[d3fend-ai-defense-technique-gap|the D3FEND AI-Defense Technique Gap]]: D6 scores coverage against D3FEND, which under-counts AI-era defenses, so the D3FEND axis is a coverage floor until the gap closes.
- Domain grounding: [[detection-deception-engineering-orbie-talk|GreyNoise Orbie]] (AI-authored detection content and deception from internet-scale honeypot data), [[syara-semantic-detection-talk|SYARA]] (semantic/intent detection with the pre-filter pattern), [[binaryshield-ai-fingerprints-talk|BinaryShield]] (privacy-preserving cross-org detection sharing), [[mitre-atlas|MITRE ATLAS]] (the in-house AI-application threat surface), and the [[mythos-ready-security-program|Mythos-ready Security Program]]'s PA 9 (deception capability) and PA 10 (automated response capability), which D6 scores the maturity of.
