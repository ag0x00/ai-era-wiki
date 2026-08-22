---
type: maturity-model-companion
title: "CMM: Effective-Score Dependency Rules"
address: c-000158
created: 2026-05-04
updated: 2026-08-20
tags:
  - maturity-models
  - cmm
  - dependency-rules
  - effective-score
  - scaffolding
  - 2026-proposal
status: developing
origin: produced
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
rule_set_version: "v1 (2026-05-04, 3 active rules)"
related:
  - "[[agentic-cmm-regulated-fi-stress-test|Regulated-FI Stress Test]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[cmm-calibration-stress-test-2026]]"
  - "[[lethal-trifecta]]"
  - "[[anti-patterns-and-failure-modes]]"
  - "[[wiki-novelty-and-counterarguments-2026]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[owasp-ai-exchange]]"
sources:
  - "[[cmm-calibration-stress-test-2026]] §Part 2 (cumulative-floor stress test)"
---

# Agentic AI Security CMM — Effective-Score Dependency Rules

This page defines the **dependency-resolved effective-score** mechanism that replaces the single cumulative floor as the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]'s headline aggregation rule. The page is intentionally scaffolded: a **small, conservative active rule set** (v1 = 3 rules) plus a **candidate-rules registry** populated as the wiki gains new attack-path evidence and practitioner architectures.

## On this page

- [The effective-score formula](#the-effective-score-formula)
- [Active rules — v1](#active-rules--v1-2026-05-04-3-rules)
- [Candidate rules registry](#candidate-rules-registry)
- [Promotion criteria](#promotion-criteria)
- [Deprecation criteria](#deprecation-criteria)
- [Revision protocol](#revision-protocol)
- [Reporting impact](#reporting-impact)
- [Worked examples](#worked-examples--re-running-the-stress-test-archetypes)
- [Limitations](#limitations)
- [Open questions & caveats](#open-questions--caveats)
- [Revision history](#revision-history)
- [Relations](#relations)

**The prior single-floor rule misreported most archetypes it was tested against.** That rule (imported from CMMC 2.0) misgraded **3 of 5 realistic archetypes** in the [[cmm-calibration-stress-test-2026|2026-05-02 stress test]] — Stripe-style architectural-containment, Microsoft Agent 365-driven, and resource-constrained startup all received headline ratings that materially under-reported the program. The L5/L5+ split adopted on 2026-05-04 also broke the floor rule's premise that domains are interchangeable units. Dependency-resolved scoring replaces the blunt min() with **substantive cross-domain caps anchored to documented attack paths**, and it records separately which caps rest on documented evidence and which remain candidates.

**Validated by the D1–D9 recalibration (2026-05-25).** The three active caps held up under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration]] and became load-bearing in the per-domain deep dives. **DR-001 (D2→D5)** and **DR-002 (D2→D7)** are the basis for the recalibration's identity-first sequencing finding — per-agent identity (D2-L3) is the highest-leverage rung because it lifts the egress and observability ceilings (see [[agentic-ai-security-cmm-d2-identity|D2]], [[agentic-ai-security-cmm-d5-egress-network|D5]], [[agentic-ai-security-cmm-d7-observability|D7]]). **DR-003 (D3→D4)** is why the [[agentic-ai-security-cmm-d4-runtime-guardrails|D4 deep dive]] reports raw + effective and tells a buyer to firm up the PDP before buying more guardrails. Candidate **DR-C001 (D8→D6)** is noted in the [[agentic-ai-security-cmm-d8-supply-chain|D8 deep dive]] (a poisoned skill/MCP/model can corrupt the RAG corpus); still candidate, still gated on a second incident. No rule changed; the recalibration applied them.

## The effective-score formula

Each domain `D` has two scores:

- **Raw score** — the assessor's per-domain rating against the L1–L5 (and optionally L5+) criteria in the [[agentic-ai-security-cmm-2026|CMM]]
- **Effective score** — `min(raw_score(D), min over deps in dependencies(D) of raw_score(dep))`

In pseudocode:

```python
def effective_score(domain, raw_scores, active_rules):
    deps = [rule.upstream for rule in active_rules if rule.downstream == domain]
    if not deps:
        return raw_scores[domain]
    cap = min(raw_scores[d] for d in deps)
    return min(raw_scores[domain], cap)
```

The **headline** is no longer a single number. It is a three-number summary:

- **Typical** = median of effective scores across all 9 domains
- **Weakest** = min of effective scores (with the domain that set it labeled, plus any cap that fired)
- **Strongest** = max of raw scores (labeled with the domain)

Plus the full per-domain matrix (raw + effective + which caps fired). Plus an optional **strategic rationale** field for any domain whose raw score is intentionally below its peers (Stripe-style architectural-containment trade-offs).

## Active rules — v1 (2026-05-04, 3 rules)

These are the rules currently in force. The set is conservative on purpose: every active rule carries a **cross-domain attack path documented in the wiki** and a **directional rationale** stating why the cap runs from the upstream domain to the downstream one.

| ID | Rule | Direction | Adopted |
|---|---|---|---|
| **DR-001** | D2 caps D5 | `effective(D5) ≤ raw(D2)` | 2026-05-04 |
| **DR-002** | D2 caps D7 | `effective(D7) ≤ raw(D2)` | 2026-05-04 |
| **DR-003** | D3 caps D4 | `effective(D4) ≤ raw(D3)` | 2026-05-04 |

**DR-001 — per-agent egress policy (D5) cannot be enforced below D2, because network gateways have no individual agent visibility.** Per the [[lethal-trifecta|lethal trifecta]], any agent can impersonate any other at the network boundary. Stripe and Salesforce both treat D2 as the precondition for D5 enforcement.

**DR-002 — per-agent identity controls are mandatory for full behavioral anomaly detection.** Below D2, a detector attributes an anomaly to the fleet and not to an agent. The Salesforce Rittinghouse pipeline reduces 1.8M prompts to 30 alerts, and per-agent identity is what makes those 30 actionable. DR-001 governs enforcement in D5; DR-002 governs attribution in D7, which identity gates independently.

**DR-003 — a policy decision is a prerequisite for a runtime guardrail.** D4 is the enforcement point and D3 is the decision point, so D4 is structurally downstream. A lifecycle hook enforces a D3 decision, so a hook firing where no decision exists has no effect. The [[hooking-coding-agents-with-cedar-talk|Sondera Cedar harness]] makes the ordering explicit. The reverse cap holds weakly, so the model adopts only the stronger direction.

**Promotion threshold met for DR-001/002/003**: each has ≥2 wiki-documented practitioner architectures (Stripe + Salesforce + AgentCordon for DR-001/002; Sondera + AgentCordon for DR-003) and a clear lethal-trifecta-class attack path.

## Candidate rules registry

Proposed rules whose evidence is suggestive but not yet sufficient for active promotion. **Add new candidates here freely.** Promotion to active happens at quarterly CMM revisions (or sooner with explicit wiki ingest evidence).

| ID | Proposed rule | Direction | Evidence required for promotion | Status | Notes |
|---|---|---|---|---|---|
| DR-C001 | D8 caps D6 | `effective(D6) ≤ raw(D8)` | ≥2 documented incidents where supply-chain compromise (D8 weak) corrupted data integrity (D6) — e.g. ClawHavoc-class skill swap poisoning a downstream RAG corpus | candidate | Likely promotion in 2026-Q4 once 2+ cross-domain incidents are catalogued; currently 1 ([[clawhavoc\|ClawHavoc]]). The Exchange's §3.1 bears on the cap's shape, not on the count; see below |
| DR-C002 | D5 caps D7 | `effective(D7) ≤ raw(D5)` | Production cases where egress is the only signal source for detection — when D5 is L1, D7 has no telemetry to monitor | candidate | Stripe is the **counter-example**; held pending evidence that the pattern generalizes |
| DR-C003 | D4 caps D5 | `effective(D5) ≤ raw(D4)` | Runtime guardrail bypass enabling egress bypass; or runtime hook gap allowing direct OS-level egress | candidate — weak directionality | Runtime and egress are co-load-bearing in most architectures; directionality is unclear. Park until a clear asymmetric attack path is documented |
| DR-C004 | D6 caps D4 | `effective(D4) ≤ raw(D6)` | Poisoned RAG (PoisonedRAG, ConfusedPilot — see [[memory-poisoning\|memory-poisoning]] concept) corrupting runtime decisions | candidate — needs production evidence | The dependency exists conceptually; production-evidence is still research-stage. Re-check when AgentDojo / equivalent benchmarks publish cross-domain bypass results |
| DR-C005 | D9 caps D2 | `effective(D2) ≤ raw(D9)` | Operational decommission failures leaving identity-bound credentials live after agent retirement | candidate — operational-vs-technical boundary | Likely belongs as a **soft cap** (rate-of-decay rather than hard min), not a hard cap. Defer until soft-cap semantics are designed |
| DR-C006 | D1 caps everything | `effective(D*) ≤ raw(D1)` | Programs with L1 governance that nonetheless ship strong technical controls — does the governance gap actually undermine the technical controls? | candidate — likely **rejected** | Technical controls appear to operate independently of governance maturity at enforcement time; parked as a likely *non-rule* |

DR-C001's evidence count is unchanged by the [[owasp-ai-exchange|OWASP AI Exchange]]'s development-time poisoning section. §3.1 states the mechanism normatively — a supplied dataset or a supplied model can arrive poisoned and reach the deployed model, and §3.1.3 treats the supplied-model case on its own — and it documents no incident in which a weak D8 was observed to cap an achieved D6. What it does supply is the first sourced argument about the cap's *shape*, which is recorded under open question 1 below.

## Promotion criteria

A candidate rule is promoted to active when **at least one** of the following is met, AND the rule is reviewed at the next quarterly CMM revision:

1. **≥2 documented incidents** in the wiki where the dependency manifests as a real attack path (incident pages with cross-domain causation noted)
2. **≥1 peer-reviewed paper or vendor-published threat-model** establishing the dependency as substantive (not theoretical)
3. **≥2 practitioner architectures** documented in the wiki (talks, deployments, vendor whitepapers) where the dependency is treated as load-bearing
4. **Synthetic-incident library coverage** — if the [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]]'s synthetic-incident library (currently a known gap) covers the cross-domain attack path with a documented test case

Any of (1)–(4) is sufficient. The rule's evidence anchor in the active table MUST cite the qualifying source(s).

## Deprecation criteria

An active rule is deprecated when:

1. **Counter-evidence accumulates** — ≥2 documented practitioner architectures where the dependency is *not* load-bearing (e.g. Stripe-style architectural patterns where the upstream domain is structurally bypassed without compromising the downstream domain)
2. **Quarterly revision finds the rule no longer reflects practice** (consensus call, documented in the revision log)
3. **A more precise rule replaces it** (e.g. soft caps, conditional caps, archetype-specific caps)

Deprecated rules stay in the registry with `status: deprecated` and a deprecation rationale. They do not affect new assessments but historical reports can be reproduced.

## Revision protocol

| When | What |
|---|---|
| **Any time** | New candidates can be added to the candidate-rules table by anyone editing this page. Add `id`, proposed rule, direction, evidence required for promotion, status: candidate, notes. |
| **Each wiki ingest of an incident** | Check whether the new incident provides cross-domain evidence relevant to an existing candidate. If so, add the citation to that candidate's notes column. |
| **Quarterly (Q1 / Q2 / Q3 / Q4)** | Review all candidates. Promote, hold, or reject; increment the rule-set version on any promotion or deprecation, and log the revision. |
| **CMM major revision** | Re-validate active rules against the latest evidence; deprecate rules that no longer reflect practice. |

## Reporting impact

The [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]]'s gap report changes shape. Old format:

```
Headline: L1 (floor — D9 set the floor)
Matrix: D1=L3 D2=L4 D3=L4 D4=L3 D5=L4 D6=L3 D7=L2 D8=L3 D9=L1
```

New format (Stripe-style architectural-containment archetype example, under v1 rules):

```
Headline:
  Typical (median effective): L4
  Weakest: D7 effective L2 (raw L2; no upstream cap fired)
  Strongest: D5 raw L4-L5 (effective L4 — capped by DR-001 from D2)
  Strategic rationale: D7 light by deliberate trade-off — D3+D5 architectural containment per Stripe Bullen talk

Per-domain matrix (raw / effective / cap source):
  D1: L3 / L3 / —
  D2: L4 / L4 / —
  D3: L4 / L4 / —
  D4: L3 / L3 / capped by DR-003 to raw(D3)=L4 (no effect — raw already L3)
  D5: L4-L5 / L4 / capped by DR-001 to raw(D2)=L4
  D6: L3 / L3 / —
  D7: L2 / L2 / capped by DR-002 to raw(D2)=L4 (no effect — raw already L2)
  D8: L3 / L3 / —
  D9: L3 / L3 / —

Active rule set: v1 (DR-001, DR-002, DR-003)
```

The headline now shows the program's shape rather than collapsing it to a single number.

## Worked examples — re-running the stress-test archetypes

Comparison of the 5 archetypes from the [[cmm-calibration-stress-test-2026|stress test]] under the old floor vs. v1 effective-score:

| Archetype | Old floor (single number) | v1 effective-score headline (typical / weakest / strongest) | Improvement vs old? |
|---|---|---|---|
| Stripe-style architectural-containment | L2 | L4 typical / L2 D7 (intentional trade-off) / L4 D5 (capped by DR-001 from D2) | **Yes** — typical L4 reflects the program; D7 recorded as weakest with rationale |
| Microsoft Agent 365-driven | L2 | L3 typical / L2 D9 (no upstream cap) / L5 D2 | **Yes** — D9 ops lag does not drag D2 down (no D9→D2 rule in v1; DR-C005 is a candidate rather than an active rule) |
| Startup with bus-factor 1 | L1 | L3 typical / L1 D9 (bus factor) / L3 D2/D3/D4/D5 | **Yes** — technical maturity is not dragged down |
| Regulated FS (balanced L3-L4) | L3 | L3-L4 typical / L3 weakest / L4 strongest | Equivalent — fair under both rules |
| Multi-cloud (balanced L3-L4) | L3 | L3-L4 typical / L3 weakest / L4 strongest | Equivalent — fair under both rules |

**Net effect of v1 rules**: the 3 archetypes the floor misreported are now reported fairly; the 2 archetypes the floor reported fairly are still reported fairly. **Mandatory matrix disclosure** and the **strategic-rationale field** now prevent cherry-picking, in place of mathematical aggregation.

## Limitations

- **Does not eliminate the cross-domain attack-path concern.** DR-001/002/003 capture the strongest known cases. Future incidents and architectures will surface more (the candidates are the parking lot).
- **Does not allow cherry-picking.** Reports MUST publish the full matrix; reports that cite a single domain's score without the matrix are non-compliant with the [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]] (anti-pattern B2 reframed accordingly).
- **Does not replace the prerequisite gate into L5** (≥2 quarters stable L4, [[aiuc-1|AIUC-1]] readiness scheduled, bus-factor ≥2, continuity test). Effective-score is *aggregation*; the prerequisite gate is *eligibility for L5 claims*. Both apply.
- **Does not address weighted scoring.** All 9 domains are still treated as equally important when computing typical/weakest/strongest. Domain weighting (e.g. for high-risk-tier applications) is a separate question parked under the **agent-archetype tailoring** open gap on the CMM page.

## Open questions & caveats

> [!gap] Things this scaffolding doesn't yet handle
> 1. **Soft caps vs hard caps.** DR-C005 (D9 caps D2) is a strong candidate for *soft* capping (operational lag degrades technical controls over time, not in the moment). DR-C001 (D8 caps D6) now has a sourced argument for the same treatment. The [[owasp-ai-exchange|OWASP AI Exchange]] §3.1.3 states that where a supplied model was manipulated at the supplier, parameter protection is outside the receiver's hands, and names what the receiver still holds: the data-poisoning controls, the broad-poisoning controls, and supply chain management. `POISON ROBUST MODEL` applies to an already-acquired model, `TRAIN DATA DISTORTION` is scoped by the Exchange to poisoning that arrived through the supply chain, and `MODEL ENSEMBLE` contains a poisoned member at a stated and falling effectiveness. A receiver with a weak upstream can therefore raise its own integrity by a bounded amount, which is degradation and not a ceiling. The current schema only supports hard caps. Soft-cap semantics are a v2+ design problem.
> 2. **Conditional caps.** Some caps may only apply for specific application archetypes (e.g. D4 caps D5 may apply for consumer-facing chatbots but not for internal agent platforms). The current schema doesn't support conditions.
> 3. **Multi-hop transitive caps.** If D2 caps D5 and D5 caps D7 (DR-C002 candidate), should D2 transitively cap D7 via D5? Currently each rule is independent. Worth re-examining if DR-C002 is promoted.
> 4. **Rule interactions.** Two rules pointing at the same downstream domain currently take min() of their upstream caps. This is the conservative choice but may be wrong in cases where the caps are partially redundant (capture the same attack path). No counter-evidence yet but flag.
> 5. **Negative rules / floor-relaxation.** Should there be rules that *raise* an effective score (e.g. D3+D5 both at L4 raises the effective ceiling on D7 for the Stripe-archetype case, since architectural containment substitutes for behavioral observability)? Currently rules can only cap, not relax. v2+ design problem.
> 6. **Clause-level prerequisites are not caps and have no representation here.** A rung can require an artifact another domain produces without the upstream domain capping the downstream score. Three are on the record. [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L4 validates a delegation chain that [[agentic-ai-security-cmm-d2-identity|D2]] L4 requires the credential to carry. [[agentic-ai-security-cmm-d7-observability|D7]] L3 requires memory writes to reach the action log carrying writer identity, session and partition, which is the per-write provenance [[agentic-ai-security-cmm-d6-data-rag|D6]] grades at L4, so a lower rung in one domain rests on a higher rung in another. [[agentic-ai-security-cmm-d1-governance|D1]] L3 bounds a publication review by the disclosure [[agentic-ai-security-cmm-d9-operations|D9]] L3 requires. Each is recorded on at least one of the two domain pages it joins, and a reader of this page sees none of them. Whether the schema gains a prerequisite edge distinct from a cap, and whether a same-level prerequisite differs from a cross-level one, is a v2 design question.
> 7. **Scoring stability across rule-set versions.** When v1 → v2 promotes a new active rule, prior assessments' headlines may shift. The protocol should specify which rule set a published rating was computed under (annotate as "v1 effective-score" or similar).

## Revision history

| Version | Date | Changes | Active rule count |
|---|---|---|---|
| **v1** | 2026-05-04 | Initial scaffolding. 3 active rules (DR-001 D2→D5, DR-002 D2→D7, DR-003 D3→D4) anchored to lethal-trifecta + Sondera/AgentCordon evidence. 6 candidate rules parked. | 3 |

## Relations

- Replaces: the single cumulative-floor rule in [[agentic-ai-security-cmm-2026|CMM 2026]] (imported from CMMC 2.0)
- Operationalized by: [[agentic-ai-security-cmm-measurement-protocol|Measurement Protocol]] §Floor rule (rewritten 2026-05-04 to point here)
- Resolves: [[cmm-calibration-stress-test-2026|stress test §Change 2]] (matrix-as-primary view) and §Change 4 (D7 contradiction recommendation) — both adopted via the new effective-score headline format
- Reframes: [[anti-patterns-and-failure-modes|Anti-Pattern B1]] (cumulative-floor demoralizes — mostly resolved) and [[anti-patterns-and-failure-modes|Anti-Pattern B2]] (cherry-picking — reframed as disclosure-discipline failure)
- Updates: [[wiki-novelty-and-counterarguments-2026|Counter-Arguments Thesis 4]] — wiki's stated position changes from "keep floor" to "replace floor with dependency-resolved effective scores"
- Anchored to: [[lethal-trifecta|Lethal Trifecta]] (DR-001, DR-002 directional rationale); [[hooking-coding-agents-with-cedar-talk|Sondera Cedar harness]] (DR-003 directional rationale); [[1-8m-prompts-30-alerts-talk|Salesforce Rittinghouse]] (DR-002 production evidence); [[breaking-the-lethal-trifecta-talk|Stripe Bullen]] (Stripe archetype worked example); [[agentcordon|AgentCordon]] (DR-001/003 OSS reference architecture)
- Exercised by: [[agentic-cmm-regulated-fi-stress-test|Regulated-FI stress test]] — runs the D2→D5, D2→D7, and D3→D4 caps against a worked archetype and reports that weak per-agent identity and control pull egress, observability, and runtime down, which the stress test judges a fair reflection of reality rather than an artifact of the rules.
