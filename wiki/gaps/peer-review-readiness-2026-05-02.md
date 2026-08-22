---
type: gap
title: "Peer-Review Readiness: RA and CMM Gaps"
created: 2026-05-02
updated: 2026-05-02
tags:
  - gaps
  - peer-review
  - ra
  - cmm
  - methodology
status: open
scope_axis:
  - sec-of-ai
question: "Before exposing the RA + CMM to industry peer review, what are the gaps that a serious / hostile reviewer would surface, and which should be closed first?"
why_it_matters: "The RA + CMM are conceptually mature but haven't been pressure-tested. Cheap to surface gaps internally before real peers do. Each gap below has been judged by likely reviewer fire and the wiki's current ability to defend its position."
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-cmm-vs-standards-validation]]"
sources: []
---

# Peer-Review Readiness — Gaps in the RA + CMM

Checkpoint review (2026-05-02) by Claude playing devil's advocate, before the RA + CMM are submitted to actual industry peers. Tabled for action — captured here so the gaps don't fall off the radar.

## Top 7 critical gaps (ranked by reviewer fire)

### 1. No empirical validation; no production case studies

Every claim is synthesized from secondary sources (Gartner, vendor blogs, Q1 2026 incidents) plus reasoning. A serious reviewer's first question: *"Name three organizations operating at L3 against this CMM. Show me their evidence."* Without case studies, it reads as a paper architecture. The "[[agentic-cmm-vs-standards-validation|validation page]]" is a Claude-vs-standards self-check, not external peer review.

**Severity: highest.** Everything else flows from this.

### 2. No cost / effort / ROI model

What does it cost to move from L2 to L3? What's the team size? Timeline? Without these numbers, executives can't act, and the CMM is a paper exercise. CMMI, BSIMM, SAMM all have at least rough effort estimates. Ours has none.

### 3. L4 → L5 calibration suspect; cumulative-floor rule un-stress-tested

L4 = "managed, quantitative, continuous"; L5 = "platform-level enforcement everywhere + Proof-of-Guardrail attestation + multi-agent UEBA + standards contributions." The L4→L5 jump is much bigger than L3→L4. Separately, "floor across all 9 domains" sounds rigorous but is operationally onerous — most real orgs would self-assess at L1 because of one weak domain. CMMI itself debates this trade-off; ours just declares the rule.

> [!check] Addressed 2026-05-02 — see [[cmm-calibration-stress-test-2026|CMM Calibration Stress Test]]
> Two-part analysis.
**Part 1**: L5 currently mixes stable maturity (platform-level enforcement everywhere, AIUC-1 cert) with research-stage capabilities (Proof-of-Guardrail TEE attestation, multi-agent cascade detection) with org-character (standards contributions). Recommended fix: split L5 into **L5 Optimizing** (stable, achievable) and **L5+ Leading Edge** (research-stage + standards contribution). The L4→L5 transition becomes capability-scale-up + duration + bus-factor (matching CMMI's "managed → optimizing"); L5→L5+ becomes the genuinely-research-stage step.
**Part 2**: Floor rule stress-tested against 5 realistic archetypes — Stripe-style architectural-containment (floor L2 driven by D7), Microsoft Agent 365-driven enterprise (floor L2 driven by D9), startup with strong AI security (floor L1 driven by D9 bus-factor-1), regulated financial services (floor L3, fair), multi-cloud program (floor L3, fair). Floor rule is fair when balanced, unfair-but-honest when programs have one structurally-weak domain.
**5 calibration changes recommended**: (1) L5/L5+ split; (2) per-domain matrix as primary report view, floor as headline; (3) document the 5 archetypes; (4) D7 contradiction callout converges on labeled-trade-off framing; (5) L4→L5 prerequisite gate (≥2 quarters stable L4, AIUC-1 readiness scheduled, named standards-contributor, bus-factor ≥ 2). Adoption pending peer review of framework as a whole.

### 4. No anti-patterns / failure-modes catalog

Every mature framework documents how it goes wrong: BSIMM has activities-not-undertaken; CMMC has appeals; SAMM has scoring caveats. Common failure modes (PDP becomes a bottleneck; Sentinel signals exceed Operative bandwidth; cumulative-floor demoralizes teams; metagovernance regresses) aren't named.

> [!check] Addressed 2026-05-02 — see [[anti-patterns-and-failure-modes|Anti-Patterns and Failure Modes]]
> 25 catalogued anti-patterns across 9 categories: Architecture (PDP bottleneck, Sentinel flood, egress chokepoint, deep-agent bypass), CMM scoring (floor demoralizes, cherry-picking, evidence theatre, stub-as-evidence), Operations (metagovernance regresses, HITL fatigue, baseline staleness, Goodhart eval, ceremonial red-team), Threat-model (trifecta-split that isn't, cascade-no-thresholds, single-tool red-team L4, behavioral monitoring no SOC integration), Standards/compliance (AIUC-1 frozen, crosswalk-as-decoration, standards shopping), Identity/credential (rotation gap, identity-credential coupling unaddressed), Multi-agent (all-to-all comms, restart-everything recovery), Procurement (platform-buy = coverage-complete, vendor-promise-as-evidence, decision-rights skipped), Talent/org (data scientists with no security training, security team with no AI training, bus factor 1). Each entry: pattern + why it happens + failure mode + recovery + wiki anchor. Maps to BSIMM activities-not-undertaken / CMMC appeals / SAMM scoring caveats / NIST CSF Implementation Tier gaps. Standing process: every L3+ self-assessment walks this list before claiming evidence; orgs that name 3+ anti-patterns affecting them are operating in good faith.

### 5. Adversarial threat model too narrow

Mostly external attackers via prompt injection, supply chain, MCP compromise. Missing: insider threat with model access; long-running adaptive adversarial campaigns; collusion between agents and humans; model-version-degradation attacks; jurisdictional adversaries (state actors with regulatory leverage). [[lethal-trifecta|Lethal Trifecta]] is a structural test for one specific exfil pattern, not a comprehensive threat model.

> [!check] Addressed 2026-05-02 — see [[agentic-ai-threat-classes-2026|Agentic AI Threat Classes — 2026 Expansion]]
> All five missing classes documented with authoritative sources (NIST AI 100-2e2025, RAND RR-A2849-1, CrowdStrike 2026, Anthropic GTG-1002, UK AISI Frontier Trends, Apollo Research steganographic collusion, Anthropic Sleeper Agents, CSET Georgetown export-controls research) and mapped to RA planes + CMM domains. New incident anchor page [[gtg-1002-ai-orchestrated-espionage|GTG-1002]] added. Cross-class synthesis identifies the AI-BOM + always-on customer eval harness as the single highest-leverage control (absorbs Classes 1, 2, 4). Class 5 (jurisdictional) flagged as governance-led, not artifact-controlled.

### 6. Multi-agent specifics under-architected

[[a2a-protocol|A2A protocol]] is a stub. Cascade detection (ASI08) is named but not designed. Multi-agent UEBA is mentioned but not specified. Inter-agent IR is absent. The "multi-agent mesh" deployment shape lacks the depth the single-agent shapes have.

> [!check] Addressed 2026-05-02 — see [[a2a-protocol|A2A Protocol]] + [[multi-agent-runtime-security|Multi-Agent Runtime Security]]
> - **A2A protocol stub filled.** Spec corrected to **v1.0.0 (Mar 2026), Linux Foundation-governed** since June 2025. Documented transport (§7), Agent Card signing framework (§8.4, algorithm-agnostic), opacity principle, what the spec does NOT specify (no replay protection, no mandatory crypto algorithm, no multi-hop trust chain), CSA MAESTRO threat model, Issue #1575 (Agent Passport System), Red Hat hardening guide, IETF AIP draft. Vendor-side enforcement (Oktsec v0.15.2 with **268 detection rules** — wiki's prior "175" was stale) clearly distinguished from spec-mandated controls.
> - **Cascade detection (ASI08) designed.** Six observable symptoms (rapid fan-out, cross-domain spread, oscillating retries, queue storms, repeated identical intents, cross-agent feedback loops) cataloged from OWASP/Adversa. Three academic detection primitives (SentinelAgent graph-walk, TraceAegis provenance, Bi-Level GAD theme-based). Vendor primitives (Oktsec, Aguara, LangSmith) honestly mapped. Wiki is explicit that 2026 is the academic-prototype era — no integrated cascade-detection product ships with documented thresholds.
> - **Multi-agent behavioral baselines specified.** Three shapes additional to single-agent: aggregate-level invariants (capacity, rate-pair, compartmentalization), pairwise/triadic traffic baselines (graph-edge anomaly), cross-agent drift correlation (lockstep drift = shared upstream contamination signal).
> - **Inter-agent IR documented.** First-principles stop-mesh-vs-isolate decision tree (default fail-mode is stop-mesh). Cross-agent forensics primitives (causal correlation across agent boundaries, tamper-evident audit chain, message-graph reconstruction). Three recovery shapes (selective rollback / rolling restart / mesh-wide quarantine).
> - **RA Multi-agent mesh row deepened** to match single-agent shape depth — Identity (Ed25519 + SPIFFE), Control (default-deny ACL + opacity + stop-mesh doctrine), Runtime (per-agent sandbox + AgentGateway broker), Egress (A2A v1.0 + signed Cards + 268 rules + replay protection), Data (cross-agent OTel propagation), Observability (pairwise/triadic baselines + graph-walk anomaly + cascade detection).
> - **Stale references corrected vault-wide.** A2A "v0.3" → v1.0.0 (5 pages); Oktsec "175 rules" → 268 (3 pages); Apollo Research multi-agent collusion attribution clarified (single-model scheming work; multi-agent *detection* counterpart is by other academic groups — NARCBench, Colosseum, Bagdasarian et al.).
>
> Honest read: 2026 is the academic-prototype era for cascade detection at scale. Wiki maturity ladder reflects this — L1/L2 shippable today, L3 prototype-grade, L4 research, L5 aspirational.

### 7. Novelty claims and competing-view callouts absent

What's actually new in the RA + CMM vs. existing literature? What's specifically the wiki's contribution that wasn't already in OWASP / NIST / Gartner / CSA? And where would a serious skeptic push back — what are the strongest counter-arguments to "platform-layer over prompt-layer," to "guardian agents will eliminate 50% of security systems," to "Lethal Trifecta is unconditionally vulnerable"? Mature frameworks acknowledge their critics; ours doesn't yet.

> [!check] Addressed 2026-05-02 — see [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]]
> Two-part appendix: (1) genuinely novel contributions (10 items: 6-plane RA with XACML colors across 7 deployment shapes; 5×9 cumulative-floor CMM with ID-tagged evidence; Cognitive File Integrity; Identity-Credential Coupling operationalized as D2 L4 evidence; D9 Operations & Human Factors; four-quadrant red-team coverage at D7 L4; multi-agent runtime security depth; five-class threat expansion; AI-BOM + always-on eval as multi-class absorber; stop-mesh-vs-isolate doctrine) plus 5 sharpenings of existing concepts. (2) Per-thesis competing-view callouts for the 6 load-bearing positions (platform-vs-prompt; Gartner 50% elimination; Lethal Trifecta unconditional; floor rule; eval-harness as multi-class absorber; UEBA-for-agents). Each thesis answers "where the skeptic's right" and what the wiki's honest response is. RA design principles 1+5 now carry inline competing-view callouts; lethal-trifecta page softens "unconditional"; guardian-agent page flags Gartner 2029 prediction as low-credibility forecast. 10 unresolved contests explicitly logged.

## Honorable-mention gaps (real, but less urgent)

- **Statistics drawn from a narrow source set** — Gartner, Insight Partners, Knostic, OWASP — same handful. No academic literature or industry surveys beyond these. **Addressed 2026-05-02** — see [[source-triangulation-audit-2026-05-02|Source Triangulation Audit]]. 8 load-bearing claims triangulated against academic + government-survey sources; 5 corroborated, 2 partially corroborated, 3 remain genuinely single-sourced (MCP CVE percentages, lab-self-reported scheming rates, Bullen-talk-specific ASR figures) and now labeled as such. Five new sources added to the citation rotation: [[stanford-hai|Stanford HAI AI Index]], Verizon DBIR 2025, [[metr|METR]], [[wef|WEF Global Cybersecurity Outlook]], [[agentdojo|AgentDojo]] (NeurIPS); 5 sources flagged as vendor-conflicted (CyberArk, SailPoint, Knostic, Salesforce, Promptfoo).
- **No formal-methods / proof story** — Reference Monitor named but not used to establish properties.
- **No skills / talent / team-composition story** — who runs this? What roles? What career paths?
- **No tooling-lock-in or vendor-risk analysis** — RA name-drops vendors without analyzing switching costs.
- **No EU AI Act Annex IV per-item evidence walkthrough** — crosswalk maps it but doesn't generate the artifact.
- **HITL named as primitive** but no taxonomy of when/how/why human-in-the-loop is appropriate.
- **L3+ ID-tagging requirement is a high bar** — has anyone actually done it?
- **No model-version-update re-evaluation playbook** — when foundation model updates, what re-evaluation is required?
- **IR specificity thin** — every page has §Defensive Lessons; none has a full incident-response runbook.
- **["UEBA for Agents" attribution is single-source]** — see [[#UEBA-for-Agents single-source attribution]] below.

> [!check] Stub-filling pass 2026-05-02 (peer-review readiness #3)
> High-leverage stubs filled with source-anchored pages:
> - **[[aiuc-1|AIUC-1]]** — six pillars, accreditation status (Schellman first, LRQA pilot), quarterly cadence, certified-org list, two-actor audit caveat. Closes the largest peer-review gap — D1 L4/L5 evidence claims now have a defensible reference.
> - **[[pyrit|PyRIT]]** + **[[garak|Garak]]** + **[[promptfoo|Promptfoo]]** + **[[mindgard-cart|Mindgard CART]]** — the four-quadrant red-team coverage the CMM D7 L4 demands now has individual pages with version pins, scope/non-scope, and explicit seam analysis. CMM D7 L4 cell wikilinks updated.
> - **Promptfoo's "Your model upgrade just broke your agent's safety"** post is the empirical anchor for both [[agentic-ai-threat-classes-2026|Threat Class 4]] and the still-open *model-version-update re-evaluation playbook* honorable-mention above. Headline numbers (94% → 71% prompt-injection resistance on GPT-4o → GPT-4.1) now in the wiki.
>
> Remaining stubs (lower leverage): SLSA, AgentDojo, FIDES, BSIMM, CMMI, A2A protocol, multiple incident pages.

## UEBA-for-Agents single-source attribution

Identified during this checkpoint. The phrase "UEBA for Agents" is amplified across ~10 wiki pages. Source trace:

- **Origin**: `wiki/papers/securing-the-autonomous-future.md` (Insight Partners, Oct 2025), which states: "Termed 'UEBA for Agents' by enterprise CISOs."
- **Attribution chain**: One vendor paper (Insight Partners — VC firm with portfolio investments in named security vendors), citing anonymous "enterprise CISOs."
- **Conceptual issue**: UEBA originated for stable user/host identities with persistent behavioral baselines. Most UEBA products merged into SIEM/XDR by 2020. AI agents are often ephemeral, non-deterministic, and lack stable baselines. The metaphor may not transfer cleanly.

**Recommendation**: soften wiki references to "agent behavioral monitoring" or "behavioral baselines for agents"; reserve "UEBA-for-Agents" as an Insight-Partners coining cited at first occurrence with a callout. Don't drop the underlying concept — behavioral monitoring of agent activity is real and useful — drop the colloquial branding.

## Sources for "what reviewers would actually say"

Persona simulation (one-shot exercise — deferred for now):

| Persona | Likely sharpest critique |
|---|---|
| Skeptical CISO | "I have agents with all three Lethal Trifecta properties and they aren't compromised — what's the empirical base rate?" / "Show me an audit finding from real use." / "What's the median cost of L2→L3?" |
| Formal-methods researcher | "PDP/PEP framing is good but you don't formalize what the PDP is *guaranteeing*." / "Reference Monitor properties (always-invoked, tamper-proof, verifiable) — which controls actually meet these?" |
| Startup CTO (5 engineers, shipping next week) | "Your L1–L5 ladder assumes Fortune 500. What's the minimum viable RA for me?" |
| Compliance officer | "Does any of this satisfy any actual regulation in a defensible way?" / "EU AI Act Annex IV walkthrough?" / "What's the audit trail I hand to a regulator?" |
| Safety / alignment researcher | "Where's the model-internal monitoring? You have behavioral observability but not interpretability." / "Goal manipulation is named but not defended against in any deep way." / "Multi-agent emergent behavior gets one bullet." |

## Status

**Tabled.** Captured here so the gaps don't fall off the radar. Triage and execution to be sequenced separately.

The full cynical-reviewer exercise (4-persona structured critique pass) is **not** in flight; this page is the human-pass note-taking version.

## See Also

- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — the artifact under review
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — the artifact under review
- [[agentic-cmm-vs-standards-validation|Validation: Agentic AI Security CMM vs Widely Adopted Standards]] — earlier validation pass (Claude vs standards), not external peer review
