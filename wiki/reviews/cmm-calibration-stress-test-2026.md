---
type: review
title: "CMM Calibration Stress Test: Cumulative-Floor Rule"
address: c-000164
origin: produced
created: 2026-05-02
updated: 2026-05-29
tags:
  - reviews
  - cmm
  - calibration
  - stress-test
  - peer-review
scope_axis:
  - sec-of-ai
status: adopted
target: "[[agentic-ai-security-cmm-2026]]"
related:
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[wiki-novelty-and-counterarguments-2026]]"
  - "[[anti-patterns-and-failure-modes]]"
  - "[[cybersecurity-cmms-exemplars]]"
---

# CMM Calibration Stress Test

Closes [[peer-review-readiness-2026-05-02|peer-review-readiness]] §3: *"L4 → L5 calibration suspect; cumulative-floor rule un-stress-tested. The L4→L5 jump is much bigger than L3→L4. Separately, 'floor across all 9 domains' sounds rigorous but is operationally onerous — most real orgs would self-assess at L1 because of one weak domain. CMMI itself debates this trade-off; ours just declares the rule."*

Two separable problems addressed below: (1) the L4→L5 jump is asymmetric vs L3→L4 and mixes shippable controls with research-stage capabilities; (2) the floor rule is unforgiving in ways that under-report strong-but-asymmetric programs. The stress test runs 5 realistic org archetypes against the current rubric and surfaces the systematic biases.

**Headline conclusion.** The L5 tier as currently defined mixes "achievable maturity" with "leading edge." A clean split into **L5 (Optimizing — stable, achievable)** and **L5+ (Leading Edge — research-stage)** resolves the asymmetric jump without losing aspirational signal. The floor rule should be **kept** (the alternative is cherry-picking, which is the bigger failure per the [[anti-patterns-and-failure-modes|anti-patterns catalog]] B2) but the **per-domain matrix becomes the primary report view**, with the floor as headline. These changes do not lower the bar; they make the bar more navigable.

## Part 1 — The L4→L5 jump is asymmetric

### Current criteria

| Level | What's added vs prior level |
|---|---|
| **L1** | Baseline (ad hoc) |
| **L2** | Written policy + manual inventory + some prompt-level guardrails |
| **L3** | Every agent has its own identity + platform-level hooks + AI-BOM + IR runbook |
| **L4** | Quantitative metrics continuously tracked + behavioral drift detection + ≥quarterly red-team + credential proxy + per-task sandbox |
| **L5** | Platform-level enforcement *everywhere* + Proof-of-Guardrail attestation + real-time AI-BOM + multi-agent behavioral monitoring with cascade detection + contributes to standards (CoSAI, OWASP, AIUC-1) + AIUC-1 cert |

### The asymmetric jump

L3→L4 adds capabilities most orgs at L3 can build in 1–2 quarters:

- *"Behavioral drift detection"* → ship Vectra / Miggo / SecureClaw against the baselines from L3
- *"Red-team eval ≥quarterly"* → schedule Promptfoo / PyRIT / Garak / Mindgard
- *"Credential proxy"* → deploy AgentKeys / Keychains.dev / Aegis
- *"Sandbox per high-risk task"* → containerize agent invocations
- *"Quantitative metrics"* → wire to existing SIEM

L4→L5 mixes three different things:

| L5 criterion | Type | Achievability today |
|---|---|---|
| Platform-level enforcement *everywhere* | Maturity scale-up | Achievable (extend L4 platform-level to all surfaces) |
| AIUC-1 cert against most recent quarterly refresh | Compliance milestone | Gated by Schellman queue (single accredited auditor); quarterly refresh adds maintenance burden |
| Real-time AI-BOM (Miggo DeepTracing) | Vendor-product dependency | Achievable but single-vendor |
| Proof-of-Guardrail TEE attestation | **Research-stage** | No integrated product ships ([[agentic-ai-security-reference-architecture\|RA]] explicitly flags as research-stage) |
| Multi-agent behavioral monitoring + cascade detection | **Research-stage** | Per [[multi-agent-runtime-security\|Multi-Agent Runtime Security]]: SentinelAgent / TraceAegis / Bi-Level GAD are papers, not products |
| Standards contributions (CoSAI / OWASP / AIUC-1) | Org-character | Small orgs cannot contribute to standards regardless of technical maturity |

L5 mixes **stable maturity** (platform-level enforcement everywhere) with **research-stage capabilities** (TEE attestation, multi-agent cascade detection) with **org-character** (standards contributions). An organization can be the most mature platform-level-enforcement shop in its industry and still fail L5 because it doesn't publish to OWASP. A peer reviewer is right to call this miscalibrated.

### Recommendation — L5 / L5+ split

| Tier | Criteria | What it signals |
|---|---|---|
| **L5 — Optimizing (stable, achievable)** | Platform-level enforcement everywhere; AIUC-1 readiness assessed against most recent quarterly refresh; quantitative drift / red-team / cred-proxy / sandbox running ≥1 year continuously; **per-domain matrix shows L4+ in all 9 domains** (no D9 caveat at L5); deputy + runbook-continuity-test ([[anti-patterns-and-failure-modes\|anti-pattern I3]]) | "This org operates at the upper bound of currently-shippable agentic-AI security." Achievable by ~5–10% of agentic-AI-deploying orgs in 2026 |
| **L5+ — Leading Edge (research-stage)** | All of L5, plus: Proof-of-Guardrail TEE attestation in production; multi-agent behavioral monitoring with cascade-detection rules + thresholds; real-time AI-BOM with cross-vendor reconciliation; **active standards contributions** (named contributor in CoSAI / OWASP / AIUC-1) | "This org is at or beyond the literature." Achievable by <1% in 2026; ambition for 2027–2028 |

This preserves the aspirational signal (L5+ exists, has rigor) without making "stable maturity" gated on research-stage capabilities. The wiki's [[agentic-ai-security-cmm-2026|CMM]] should adopt this split.

### Implications for the existing rubric

If the L5/L5+ split is adopted, the following current-L5 criteria move to L5+:

- Proof-of-Guardrail TEE attestation
- Multi-agent behavioral monitoring with cascade detection
- Real-time AI-BOM with cross-vendor reconciliation
- Active standards contribution

And these stay at L5:

- Platform-level enforcement everywhere
- AIUC-1 cert against current quarterly refresh
- Per-domain matrix L4+ across all 9 domains
- ≥1 year continuous L4 operation
- Bus-factor ≥ 2 with continuity test

The jump from L4→L5 becomes *capability scale-up + duration + bus factor*, which is the "managed → optimizing" transition CMMI defined. The jump from L5→L5+ is *standards contribution + research-stage tooling adoption*, which is honestly different work.

## Part 2 — Cumulative-floor stress test

The floor rule (CMMC import): *"The organization's overall rating is the floor across all 9 domains. An organization rated L4 in D2 and L1 in D9 is rated L1 overall."* Stress-tested against 5 archetypes below.

### Archetype 1 — Stripe-style architectural-containment

Per [[breaking-the-lethal-trifecta-talk|Bullen's]] talk: the program leans hard on egress containment + sensitive-action HITL + ToolAnnotations; deliberately *lighter* on behavioral monitoring + AI-SPM (the talk is explicit about this trade-off).

| Domain | Likely level | Notes |
|---|---|---|
| D1 Governance | L3 | Decision rights documented |
| D2 Identity | L4 | Workspace identity + SPIFFE-class workload IDs |
| D3 Control | L4 | ToolAnnotations + HITL on writes |
| D4 Runtime | L3 | Sandbox + Safe Search; not full L4 (no continuous behavioral drift on the model layer) |
| D5 Egress | L4–L5 | Smokescreen + Toolshed + tagged-services CI gate. Arguably L5 architectural maturity |
| D6 Data | L3 | RAG governance + write classification |
| D7 Observability | **L2** | Bullen explicit: they don't run heavy behavioral monitoring; D7 contradiction callout on the CMM page is exactly this case |
| D8 Supply Chain | L3 | Typosquat-aware install gate + ToolAnnotations on install |
| D9 Operations | L3 | Pending Actions UI + LLM-as-2nd-reviewer + named team |
| **Headline floor** | **L2** | Driven by D7 |

The headline rating dramatically under-reports. This is the case the [[agentic-ai-security-cmm-2026|CMM]]'s own D7 contradiction callout was added to flag.

### Archetype 2 — Microsoft Agent 365-driven enterprise

Hyperscaler-platform-driven. Strong on identity (Entra Agent ID), data (Purview), observability (Defender for Cloud Apps); weak on governance discipline + cross-cloud + ops.

| Domain | Likely level | Notes |
|---|---|---|
| D1 Governance | L3 | Standard governance practice |
| D2 Identity | L5 | Microsoft Agent 365 Registry — this is named as L5 evidence in the CMM |
| D3 Control | L3 | Default Agent 365 controls |
| D4 Runtime | L3 | Prompt Shields + content safety |
| D5 Egress | L3 | mTLS via Entra; no cross-cloud broker |
| D6 Data | L3 | Purview |
| D7 Observability | L4 | Defender for Cloud Apps + Sentinel |
| D8 Supply Chain | L3 | Standard MS supply-chain |
| D9 Operations | **L2** | Hyperscaler-platform-buy = coverage-complete anti-pattern ([[anti-patterns-and-failure-modes\|H1]]); ops discipline often lags procurement |
| **Headline floor** | **L2** | Driven by D9 |

Same pattern: strong domains pulled down by one weak domain. The wiki's [[anti-patterns-and-failure-modes|H1 anti-pattern]] (hyperscaler-buy = coverage-complete) names this exact failure shape.

### Archetype 3 — Startup with strong AI security and limited resources

Small team, modern stack, high technical maturity but limited operational resources.

| Domain | Likely level | Notes |
|---|---|---|
| D1 Governance | L2 | Documented but ad-hoc |
| D2 Identity | L3 | Okta for AI Agents |
| D3 Control | L3 | Cedar/OPA + HITL |
| D4 Runtime | L3 | LlamaFirewall stack |
| D5 Egress | L3 | AgentGateway |
| D6 Data | L2 | Basic RAG governance; no attestation |
| D7 Observability | L2 | LangSmith only |
| D8 Supply Chain | L2 | Inventory + manual scanning |
| D9 Operations | **L1** | Single person; no deputy |
| **Headline floor** | **L1** | Bus-factor-1 anti-pattern (I3) |

Under-reports the technical maturity (L3 across most operational domains). The recovery is the bus-factor-2 D9 L3 hard requirement from the [[anti-patterns-and-failure-modes|anti-patterns catalog I3]] — but pre-recovery, the org is L1 by the floor rule despite operating at L3 across most domains.

### Archetype 4 — Regulated financial services

Mature governance + identity programs from compliance lineage; mid-tier on AI-specific runtime / supply-chain.

| Domain | Likely level | Notes |
|---|---|---|
| D1 Governance | L4 | Existing risk-mgmt program extended to AI |
| D2 Identity | L4 | NHI program from existing IAM maturity |
| D3 Control | L4 | Decision rights from compliance lineage |
| D4 Runtime | L3 | LlamaFirewall + sandbox |
| D5 Egress | L4 | mature network security extended |
| D6 Data | L3 | RAG governance |
| D7 Observability | L4 | SOC integration |
| D8 Supply Chain | L3 | AI-BOM exists |
| D9 Operations | L3 | Compliance team baseline |
| **Headline floor** | **L3** | Most balanced archetype |

This is the archetype that maps cleanly to the current rubric. Floor rule fairly reflects the org.

### Archetype 5 — Multi-cloud / multi-vendor program

Cross-vendor coverage from intent; weakest on standards-contribution and AIUC-1 cert (small team).

| Domain | Likely level | Notes |
|---|---|---|
| D1 Governance | L3 | Cross-cloud governance documented |
| D2 Identity | L4 | SPIFFE/SPIRE + per-cloud identity bridges |
| D3 Control | L4 | Vendor-neutral PDP (Cedar/OPA) |
| D4 Runtime | L3 | Mixed runtime stack |
| D5 Egress | L4 | AgentGateway-LF cross-cloud |
| D6 Data | L4 | Cross-cloud data residency + jurisdiction tagging |
| D7 Observability | L4 | OTel `gen_ai.*` cross-cloud |
| D8 Supply Chain | L3 | AI-BOM per cloud |
| D9 Operations | L3 | Strong cross-cloud ops |
| **Headline floor** | **L3** | Could attempt L4; AIUC-1 cert is the next gate |

Healthy program. Floor rule fairly reflects.

### Stress-test verdict

| Archetype | Headline floor | Per-domain matrix range | Floor under-reports? |
|---|---|---|---|
| Stripe-style architectural-containment | L2 | L2–L5 | **Yes — significantly** |
| Microsoft Agent 365-driven | L2 | L2–L5 | **Yes — significantly** |
| Startup with strong AI security | L1 | L1–L3 | **Yes — moderately** |
| Regulated financial services | L3 | L3–L4 | No — fair |
| Multi-cloud program | L3 | L3–L4 | No — fair |

The floor rule is fair when programs are roughly balanced, and unfair-but-honest when programs have one structurally weak domain. The fix is **transparency**, not abandoning the floor.

## Part 3 — Recommended calibration changes

### Change 1 — Adopt L5 / L5+ split

Per Part 1. The wiki's [[agentic-ai-security-cmm-2026|CMM]] L5 row should split into:
- L5 Optimizing (stable maturity)
- L5+ Leading Edge (research-stage capabilities + standards contribution)

Implementation: keep the current L5 description but reframe research-stage items as L5+; add explicit L5/L5+ distinction in the level table.

### Change 2 — Per-domain matrix becomes primary report view; floor is headline

Per Part 2. The [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] already requires the per-domain matrix; this change elevates it from supporting evidence to **primary** report view. The recommendation:

- **Headline rating** = floor across 9 domains (preserves cherry-picking discipline)
- **Primary report view** = per-domain matrix with all 9 levels visible
- **Required L4+ artifact** = gap-closure plan from floor-domain to next level
- **Auditor disclosure** = reports MUST cite floor + matrix together; floor without matrix is non-compliant

This is a documentation / reporting change, not a level-criteria change. It does not relax the floor rule; it makes the rule navigable.

### Change 3 — Document the 5 archetypes

Add to the [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] (or a sister page) the 5 archetype profiles + their typical floor pattern + the dominant gap to close. Helps orgs orient: "we look like Archetype 1 (Stripe-style); the wiki tells us our floor will be D7-driven; the recovery is documented in [[anti-patterns-and-failure-modes|B1]] + [[multi-agent-runtime-security|behavioral baseline maturity ladder]]."

### Change 4 — D7 contradiction callout converges on a recommendation

The [[agentic-ai-security-cmm-2026|CMM]] page already carries a `[!contradiction]` callout (added during the [[breaking-the-lethal-trifecta-talk|Bullen-talk]] follow-up) noting that Stripe-tier architectural-containment can rationally score lower on D7 and still be sound. **Recommendation**: keep D7 L4 criteria as-is (multi-tool red-team + behavioral drift wired to SIEM); add an explicit *Stripe-archetype acknowledgement* — when D3 + D5 are L4+ AND the program documents the architectural-containment rationale, the D7 weak-floor is a **labeled trade-off, not a hidden gap**. The floor rule still applies; the matrix view shows readers what kind of L2-floor they're looking at.

### Change 5 — L4→L5 prerequisite gate

Before claiming L5 attempt, the org must show:
- ≥2 quarters of stable L4 operation across all 9 domains
- AIUC-1 readiness assessment scheduled with Schellman (or accredited equivalent)
- Named individual responsible for standards contribution work (even if work hasn't started)
- Bus-factor ≥ 2 with documented continuity test

This converts L4→L5 from a step to a campaign — closer to how CMMI's "Maturity Level 4 → 5" is treated in practice.

## Open issues

> [!gap] What this analysis doesn't yet cover
> 1. **L5+ adoption signal.** No org publicly claims L5+ today. The wiki should track which orgs first claim it (probably Anthropic / Google / Microsoft on their own platforms; possibly UiPath via AIUC-1 cert + standards contribution).
> 2. **Quantitative threshold for "L4 stable for ≥2 quarters."** What does "stable" mean — no regressions in the matrix? No P1 incidents? No drift > X%? Currently undefined.
> 3. **Archetype-vs-floor mapping accuracy.** The 5 archetypes here are first-principles. Real audit data would show how often each profile appears + how often the floor matches reality.
> 4. **Multi-cloud archetype's L5 path.** The multi-cloud archetype is structurally well-positioned but has no documented path to L5 because cross-cloud AIUC-1 cert is procedurally complex. Worth a separate analysis.
> 5. **L5+ vs aspirational drift.** If L5+ becomes the new aspirational target, does the same calibration debate happen later? CMMI faced this; the wiki should pre-empt with explicit "L5+ is intentionally bleeding-edge and may be unachievable without category-creation."

## Consequences for the framework

This page does NOT unilaterally rewrite the [[agentic-ai-security-cmm-2026|CMM]]. It surfaces the calibration analysis the gap doc demanded and recommends 5 changes. Adoption is a separate decision — the wiki should hold the recommendations as candidate changes pending peer review of the framework as a whole.

> [!check] Adoption status — 2026-05-04 (updated same day)
> **Changes 1 (L5/L5+ split) and 5 (L4→L5 prerequisite gate)** adopted into the [[agentic-ai-security-cmm-2026|CMM]] and [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] in the 2026-05-04 revision. Every L5 row in the CMM was rewritten to point only to currently-shippable products / OSS / specs; research-stage and standards-contribution items moved to a new L5+ Leading Edge tier. The [[agentic-ai-security-cmm-crosswalk|crosswalk matrix]] is now explicitly L5-only with L5+ deferred until standards bodies publish leading-edge guidance.
>
> **Changes 2 (matrix-as-primary-view) and 4 (D7 contradiction resolution)** adopted later the same day via the new [[agentic-ai-security-cmm-dependency-rules|Effective-Score Dependency Rules]] page. The cumulative-floor rule was replaced with dependency-resolved effective scores under a small conservative active rule set (v1 = 3 rules: D2→D5, D2→D7, D3→D4). Headline format becomes typical/weakest/strongest plus the per-domain matrix. Cherry-picking is now prevented by mandatory matrix disclosure rather than by mathematical aggregation. The dependency-rule registry is intentional scaffolding with explicit promotion criteria and quarterly revision protocol — designed to grow as new attack-path evidence and practitioner architectures land in the wiki. The Stripe-archetype D7 contradiction is resolved: D7 raw L2 reports honestly with a strategic-rationale field rather than collapsing the whole rating.
>
> **Change 3 (5-archetype documentation in measurement protocol)** remains candidate. The 5 archetypes are documented in [[cmm-calibration-stress-test-2026|this stress test]] and the worked-examples section of the [[agentic-ai-security-cmm-dependency-rules|dependency-rules page]]; copying them into the measurement protocol as standalone templates is a documentation-clarity follow-up that doesn't gate any other change.

If adopted, the changes are minimally invasive:

| Change | Page edits | Behavior change |
|---|---|---|
| L5 / L5+ split | CMM L5 row + level table; new sub-row for L5+ | Reframes existing L5 criteria; introduces L5+ as new tier |
| Matrix as primary view | Measurement protocol report-format guidance | No level-criteria change; reporting-format change |
| Document 5 archetypes | New sister page or measurement protocol annex | New documentation; no rubric change |
| D7 contradiction recommendation | CMM D7 contradiction callout finalized | Documents trade-off explicitly; no level-criteria change |
| L4→L5 prerequisite gate | CMM L5 row + measurement protocol stage-2 | Adds gate; doesn't change L5 criteria |

## See Also

- [[peer-review-readiness-2026-05-02|Peer-Review Readiness]] — origin (§3 closed by this page)
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — the rubric being calibrated
- [[agentic-ai-security-cmm-measurement-protocol|Measurement Protocol]] — where the matrix-as-primary-view change lives
- [[anti-patterns-and-failure-modes|Anti-Patterns and Failure Modes]] — B1 (floor demoralizes) + B2 (cherry-picking) + I3 (bus factor 1) all anchor here
- [[wiki-novelty-and-counterarguments-2026|Wiki Novelty and Counter-Arguments]] §Thesis 4 (cumulative floor) is the pre-stress-test framing
- [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars]] — CMMI's L4→L5 precedent
- [[breaking-the-lethal-trifecta-talk|Bullen-talk]] — Archetype 1 evidence
