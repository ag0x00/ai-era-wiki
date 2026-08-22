---
type: comparison
title: "Wiki Novelty and Counter-Arguments"
created: 2026-05-02
updated: 2026-06-22
tags:
  - comparisons
  - peer-review
  - methodology
  - novelty
  - counter-arguments
status: developing
scope_axis:
  - sec-of-ai
question: "What does the wiki contribute that wasn't already in OWASP / NIST / Gartner / CSA — and where would a serious peer reviewer push back hardest on the wiki's load-bearing theses?"
related:
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[source-triangulation-audit-2026-05-02]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
---

# Wiki Novelty and Counter-Arguments

This page has two parts: what the wiki contributes beyond the standards literature, and the strongest counter-arguments a serious peer reviewer would raise against the wiki's load-bearing theses, with a response to each.

**Mature frameworks acknowledge their critics.** CMMI, BSIMM, and NIST CSF all document their trade-offs and counter-positions in their own appendices. This page is that appendix: where a serious skeptic would push back on the wiki's load-bearing theses, and how the wiki answers.

## Part 1 — What's actually new in the wiki

### Genuinely novel contributions

The wiki adds these that are not in OWASP ASI Top 10, NIST AI RMF, Gartner Guardian Agents, ISO 42001 (governance-only; no agentic technical control — per [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]]), MITRE ATLAS, CSA MAESTRO, or AIUC-1 as of mid-2026:

| Contribution | Where defined | What's new |
|---|---|---|
| **6-plane RA with [[xacml\|XACML]] PIP/PDP/PEP/PAP roles colored across deployment shapes** | [[agentic-ai-security-reference-architecture\|RA]] | Most prior work names some of these planes; nothing else maps the full XACML role split across all six and across 7 deployment shapes (chatbot / generative coding / data-science / RAG / MCP server / agent skill / multi-agent mesh) in one artifact |
| **5×9 CMM with L5/L5+ split + dependency-resolved effective scores + ID-tagged evidence at L3+** | [[agentic-ai-security-cmm-2026\|CMM]] + [[agentic-ai-security-cmm-dependency-rules\|Dependency Rules]] | The cumulative-floor rule (CMMI/CMMC import) was *replaced* on 2026-05-04 with dependency-resolved effective scores under a small conservative active rule set (v1 = 3 rules) — a substantive aggregation that captures cross-domain attack paths without punishing strategic trade-offs. Combined with the L5 / L5+ split (achievable-today vs. leading-edge) and mandatory `ASI##` / AIVSS / `AML.T####` tagging at L3+, this is the load-bearing scoring innovation in the wiki |
| **Cognitive File Integrity (CFI) for `SOUL.md` / `IDENTITY.md` / system prompts** | [[supply-chain-security-for-agents\|Supply Chain Security]], [[agent-observability\|Agent Observability]] | Extension of traditional FIM to agentic-specific identity files; not in any standard |
| **Identity-Credential Coupling concept** | [[identity-credential-coupling\|Identity-Credential Coupling]] | Surfaced from [[what-are-non-human-identities\|Oasis]] but operationalized as a CMM D2 L4 evidence requirement (coupled-credential migration plan); not in NIST or ISO |
| **D9 Operations & Human Factors as 9th cross-cutting domain** | [[agentic-ai-security-cmm-2026\|CMM]] D9 | Packages the operational gaps NIST AI 800-4 flagged as biggest blind spot; named no current standard |
| **Four-quadrant red-team coverage requirement at D7 L4** | [[agentic-ai-security-cmm-2026\|CMM]] D7 L4 | "Single-tool coverage is not L4" — orchestration ([[pyrit\|PyRIT]]) × probe library ([[garak\|Garak]]) × CI regression ([[promptfoo\|Promptfoo]]) × continuous CART ([[mindgard-cart\|Mindgard]]). Independent benchmark anchor ([[agentdojo\|AgentDojo]]). Not in any standard |
| **Multi-agent runtime security depth** | [[multi-agent-runtime-security\|Multi-Agent Runtime Security]] | Cascade-detection symptoms + 3 academic primitives + stop-mesh-vs-isolate IR decision tree + maturity ladder honest about academic-prototype state. ASI08 names the threat; no standard designs the response |
| **Five-class threat expansion beyond OWASP ASI** | [[agentic-ai-threat-classes-2026\|Threat Classes 2026]] | Insider with model access; long-running adaptive APT; agent-agent collusion; model-version-degradation; jurisdictional adversaries — none of these are first-class in OWASP ASI / MITRE ATLAS |
| **AI-BOM + always-on customer eval as multi-class absorber** | [[agentic-ai-threat-classes-2026\|Threat Classes 2026]] §Cross-class synthesis | Single highest-leverage control argument absorbs Classes 1, 2, 4. Synthesis is wiki-original |
| **Stop-mesh-vs-isolate containment doctrine** | [[multi-agent-runtime-security\|Multi-Agent Runtime Security]] | First-principles decision tree for multi-agent cascade IR; literature names the threat but not the decision rule |

### Sharpenings — re-framings of existing concepts

These exist in the literature; the wiki sharpens or operationalizes them:

| Sharpening | Existing source | Wiki's sharpening |
|---|---|---|
| **Oversight Layer (PDP+PEP for AI)** | XACML; classical zero-trust; [[gartner\|Gartner]] "Guardian Agent" | Architectural primary term decoupled from procurement-language; cross-walk against Reference Monitor / Supervisory Agent / AI Firewall / Promotion Gate |
| **Sentinels and Operatives split** | [[gartner\|Gartner]] Figure 1 (Market Guide for Guardian Agents) | Mapped explicitly to PIP (Sentinels) and PDP+PEP (Operatives) plus the wiki's 6 planes |
| **Lethal Trifecta as structural test** | [[simon-willison\|Willison]] (Jun 2025) | Kept Willison's framing; added [[breaking-the-lethal-trifecta-talk\|Stripe worked example]] (Bullen) and [[lethal-bifecta\|Bifecta]] write-side analogue |
| **Verified accountable autonomy** | [[gartner\|Gartner]] | Adopted as the description of what the architecture provides; tied to specific control evidence |
| **Standards-crosswalk as auditable matrix** | NIST AI RMF, ISO 42001, AIUC-1 each crosswalk individually | The wiki's [[agentic-ai-security-cmm-crosswalk\|crosswalk]] runs all axes against the CMM domains in one matrix; AIUC-1 publishes the most current single-source crosswalk |

### Honest acknowledgement — what's not new

These the wiki documents but did not originate:

- The "platform-layer over prompt-layer" thesis (consensus across the ingested 2026 papers; not wiki-coined)
- The Lethal Trifecta itself (Willison)
- Credential Proxy Pattern (5-tool OSS convergence, not wiki-discovered)
- OWASP ASI Top 10 / AIVSS / MITRE ATLAS technique IDs (used as evidence anchors)
- Guardian Agent terminology (Gartner; wiki uses as procurement synonym for oversight layer)
- "UEBA for Agents" colloquial branding ([[insight-partners\|Insight Partners]] coining; the wiki softens this to architecturally-neutral *agent behavioral monitoring* in body content)

## Part 2 — Counter-arguments a serious skeptic would push back on

### Thesis 1 — *"Platform-layer enforcement, not prompt-layer"*

| The wiki's position | "Every control that matters runs in the runtime/platform, below the model. Prompt-level guardrails are bypassable by definition." (RA design principle 1) |
|---|---|
| Strongest counter-argument | **Defense-in-depth requires both.** Prompt-layer guardrails reduce attack success rate materially even when bypassable. Meta's [[llamafirewall\|LlamaFirewall]] eval on [[agentdojo\|AgentDojo]]: PromptGuard 2 takes ASR from 17.6% to 7.5%; combined with AlignmentCheck to 1.75%. Anthropic Constitutional Classifiers: jailbreak success 86%→4.4%. These are non-trivial reductions. A *strict* "platform over prompt" doctrine implies you don't need them. |
| Where the skeptic's right | Prompt-layer is not pointless. Cost-benefit analysis sometimes favors prompt-layer-only for low-risk-tier interactions where platform-layer overhead (Constitutional Classifiers report 23.7% inference cost) doesn't justify itself. |
| The wiki's honest response | The framing is **hierarchy, not exclusivity.** Platform-layer is primary because it's not bypassable by injection; prompt-layer is residual-risk reduction. [[breaking-the-lethal-trifecta-talk\|Bullen's]] *"untrusted content can't be removed"* is the clean statement. The wiki's [[rag-hardening\|RAG hardening]] and [[system-prompt-architecture\|system prompt architecture]] pages explicitly carry residual-risk callouts. The thesis should read *"platform-layer is primary, prompt-layer is residual"*, not *"prompt-layer is useless."* |

### Thesis 2 — *"Independent guardian agents eliminate much of the incumbent AI-protection market by 2029"*

| The wiki's position | Cited from [[guardian-agents-market-guide\|Gartner Market Guide for Guardian Agents]] (Feb 2026): Independent GAs eliminate need for ~50% of incumbent AI-protection systems in 70%+ of orgs by 2029. |
|---|---|
| Strongest counter-argument | **Gartner consolidation predictions have a poor historical record.** XDR was supposed to eliminate SIEM (didn't); SOAR was supposed to eliminate ticketing (didn't); CSPM was supposed to eliminate cloud-config tools (didn't). The pattern is hyperscaler-embedded controls *complement* point solutions rather than replace them. The 2029 horizon has no evidence base — it's a vendor-positioning forecast. |
| Where the skeptic's right | The wiki should not cite this as a settled prediction. It is one analyst's forecast; treating it as a load-bearing assumption is the kind of overclaim that surfaces in peer review. |
| The wiki's honest response | Cite as **Gartner's prediction with low forecast credibility** based on prior consolidation-prediction outcomes. The argument the wiki actually relies on — that an independent oversight layer is needed for cross-cloud / cross-platform / cross-vendor coverage — does not require the 50% elimination claim. The structural argument stands; the market prediction should be flagged as Gartner-specific and contested. |

### Thesis 3 — *"Lethal Trifecta is unconditionally vulnerable"*

| The wiki's position | "Any deployment combining private-data + untrusted-content + external-comms is **unconditionally vulnerable**." (RA design principle 5; [[lethal-trifecta\|Lethal Trifecta]]) |
|---|---|
| Strongest counter-argument | **"Unconditional" is too strong.** [[breaking-the-lethal-trifecta-talk\|Stripe (Bullen, March 2026)]] runs trifecta agents in production with platform-level egress containment + sensitive-action HITL, and reports attack success rates of 1.5–6.7% across model generations. That's not "unconditional" — it's *probabilistically exploitable*, with the success rate depending on defense maturity. Bullen explicitly: *"Even 0.1% is too high"* — so the threshold not the unconditional nature is the issue. CaMeL (Google DeepMind), deterministic gating, and multi-LLM separation reduce trifecta exposure further without splitting the trifecta. |
| Where the skeptic's right | The wiki's "unconditional" framing is design-time pedagogy, not empirical fact. The [[breaking-the-lethal-trifecta-talk\|Bullen architecture]] and CaMeL research explicitly demonstrate that containment can drive trifecta-agent ASR to single-digit percentages and below. |
| The wiki's honest response | The Lethal Trifecta is a **necessary condition for natural-language exfil at scale**, and **sufficient given current defense maturity** to require platform-layer containment. The "unconditionally vulnerable" framing is the design-time test (do you split the trifecta or contain it?); in production, containment can drive ASR very low but not zero — and very-low-but-not-zero is unacceptable for high-risk-tier actions. The wiki should reframe from *"unconditional"* to *"sufficient at the design stage; ASR-bounded in production."* |

### Thesis 4 — *"Cumulative floor across all 9 domains"* (revised 2026-05-04 — position changed)

| The wiki's prior position (April 2026 → May 3 2026) | Organization's overall rating is the floor across all 9 domains. (CMM scoring rule, imported from CMMC 2.0) |
|---|---|
| Strongest counter-argument | **Operationally onerous; most real orgs would self-rate L1 because of one weak domain.** The gap doc flagged this; the [[cmm-calibration-stress-test-2026\|stress test]] confirmed it empirically — 3 of 5 realistic archetypes (Stripe-style architectural-containment, Microsoft Agent 365-driven, resource-constrained startup) were misreported by the floor. The L5/L5+ split adopted on 2026-05-04 also broke the floor's "domains are interchangeable units" premise. |
| Where the skeptic's right | The floor rule is unforgiving on archetypes that make calculated cross-domain trade-offs. CMMC's original adoption assumed mandatory regulatory backing + accredited auditors + narrow scope — none of which apply to this advisory CMM. Different regulatory context means cherry-picking discipline must be enforced differently. |
| The wiki's revised position (2026-05-04) | **Thesis 4 was retired. Replaced by dependency-resolved effective scores documented in [[agentic-ai-security-cmm-dependency-rules\|Effective-Score Dependency Rules]].** The new aggregation: a domain's effective score = `min(raw, min over upstream-dependency raw scores)` under a small, conservative active rule set (v1 = 3 rules: D2→D5, D2→D7, D3→D4, anchored to lethal-trifecta + Sondera/AgentCordon evidence). Cross-domain attack paths are captured substantively (D2 weakness genuinely caps D5/D7 because identity gates enforcement and attribution); strategic trade-offs that don't reflect attack paths are not punished (D9 ops lag does not cap D2 identity controls). Cherry-picking is now prevented by **mandatory matrix disclosure** (any rating claim must publish the full per-domain raw + effective matrix and the active rule-set version) rather than by mathematical aggregation. The dependency-rule registry is intentional scaffolding — designed to grow as new attack-path evidence and practitioner architectures land in the wiki, with explicit promotion criteria + revision protocol. |
| New thesis (provisional) | *Aggregation should be substantive and conservative, not blunt.* A small set of evidence-anchored cross-domain caps captures the real weakest-link risk; anything beyond that is punitive. Disclosure discipline handles cherry-picking better than aggregation discipline does. |

### Thesis 5 — *"AI-BOM + always-on customer eval is the single highest-leverage control"*

| The wiki's position | Three of five threat classes (insider, APT, version regression) collapse to the same observable: a delta against a trusted baseline produced by a customer-owned, version-pinned, continuously-executed eval harness with cryptographic provenance. ([[agentic-ai-threat-classes-2026\|Threat Classes 2026]]) |
|---|---|
| Strongest counter-argument | **Cost is non-trivial and not benchmarked.** Continuous re-evaluation over a large eval suite for every model update + every prompt change has no published cost / latency / coverage benchmarks. Eval suites become stale faster than models. Evals can't catch novel attacks they weren't designed for. The wiki itself flags this as an open issue. |
| Where the skeptic's right | The thesis is *theoretically* tight (one observable absorbs three threat classes) but operationally undefined. Without published benchmarks for eval-harness cost as a percentage of inference spend, "always-on" is hand-waved. |
| The wiki's honest response | The wiki's [[agentic-ai-threat-classes-2026\|threat-classes page]] explicitly logs the cost-benchmark gap as open. The thesis is *strategic guidance* (build this primitive first because it absorbs the most threat classes), not *production-ready blueprint*. Pair with the [[agentic-cmm-vs-standards-validation\|validation §3]] gap on guardrail latency / cost budgets — the operationalization is unfinished. |

### Thesis 6 — *"Behavioral monitoring (UEBA-for-agents) for ephemeral agents"*

| The wiki's position | Behavioral monitoring with baselines + drift detection at D7 L3+. (Already softened from the "UEBA for Agents" branding to architecturally-neutral language.) |
|---|---|
| Strongest counter-argument | **Classical UEBA needs stable identities and persistent baselines.** AI agents are often ephemeral and non-deterministic; baselines collapse if the agent population churns. UEBA products had largely merged into SIEM/XDR by 2020 — the metaphor doesn't transfer cleanly. |
| Where the skeptic's right | Already addressed. The wiki softened the language and labels Insight Partners' "UEBA for Agents" coining as informal vendor framing not adopted by NIST/ISO/OWASP. Body uses *agent behavioral monitoring* / *behavioral baselines for agents*. |
| Open question | What does a behavioral baseline look like when 80% of an agent population is short-lived? Aggregate-level invariants (mesh-wide rate caps, pairwise traffic bursts) are partial answers; the problem is not solved. The [[multi-agent-runtime-security\|multi-agent runtime security page]] documents this honestly. |

## Open contests — where the wiki's position is contested and unresolved

These are positions a peer reviewer is right to push on, and the wiki does not yet have a settled answer:

> [!gap] Unresolved contests
> 1. **Floor-rule exemptions.** Whether L4/L5 should be relaxable when D3+D5 are strong, or split consumer-facing vs internal-platform. Documented as open on the [[agentic-ai-security-cmm-2026|CMM]] D7 contradiction callout.
> 2. **Eval-harness cost as % of inference spend.** No published benchmark for the multi-class absorber control. Operationalization unfinished.
> 3. **Cascade-detection numeric thresholds.** Adversa lists categories; rule SQL/YAML is not public; vendor implementations don't surface thresholds. Documented in [[multi-agent-runtime-security|Multi-Agent Runtime Security]].
> 4. **Behavioral baseline definition for ephemeral agents.** Aggregate-level invariants are partial; the full definition is open.
> 5. **MCP CVE percentages** ([[source-triangulation-audit-2026-05-02|Source Triangulation Audit]] §Claim 4 contested). Wiki should re-derive from peer-reviewed denominators.
> 6. **Lab-self-reported scheming rates** awaiting peer-reviewed independent replication ([[source-triangulation-audit-2026-05-02|Audit]] §Claim 8).
> 7. **Bullen-talk-specific ASR figures** (1.5–6.7%) lack independent benchmark replication ([[source-triangulation-audit-2026-05-02|Audit]] §Claim 5).
> 8. **Two-actor AIUC-1 audit model** — issuer (AIUC) ≠ auditor (Schellman). Whether this strengthens or weakens independence is contested ([[aiuc-1|AIUC-1]] caveats §5).
> 9. **A2A v1.0 spec lacks message integrity, replay protection, multi-hop trust chain.** Wiki documents this; the resolution lives in vendor implementations and Issue #1575 — not yet merged.
> 10. **CSA ATF promotion gates not fully specified** by CSA itself; CMM D3 L4 still depends on org-authored rubric ([[agentic-cmm-vs-standards-validation|validation]] §5).

## Peer-review process going forward

This page is the standing pre-peer-review checklist. Every load-bearing thesis added to the RA / CMM should answer:

1. Is this novel, sharpened, or borrowed?
2. What is the strongest counter-argument from a serious skeptic?
3. Where is the skeptic right? (If "nowhere," the thesis is probably overstated.)
4. What is the wiki's honest response?
5. What does the wiki *not* yet have an answer for?

If a thesis cannot survive that exercise, it should not be cited as L3+ evidence.

## See also

- [[peer-review-readiness-2026-05-02|Peer-Review Readiness]] — the sibling readiness audit
- [[agentic-cmm-vs-standards-validation|Validation: Agentic AI CMM vs Widely Adopted Standards]] — sister audit; standards comparison
- [[source-triangulation-audit-2026-05-02|Source Triangulation Audit 2026-05-02]] — sister audit; evidence-source diversification
- [[agentic-ai-security-reference-architecture|RA]] · [[agentic-ai-security-cmm-2026|CMM]] · [[agentic-ai-threat-classes-2026|Threat Classes 2026]] · [[multi-agent-runtime-security|Multi-Agent Runtime Security]] — load-bearing artifacts under review
