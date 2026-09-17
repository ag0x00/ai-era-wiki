---
type: comparison
title: "Wiki Novelty and Counter-Arguments"
created: 2026-05-02
updated: 2026-09-16
tags:
  - comparisons
  - peer-review
  - methodology
  - novelty
  - counter-arguments
status: developing
scope_axis:
  - sec-of-ai
origin: produced
question: "What does the wiki contribute that wasn't already in OWASP / NIST / Gartner / CSA — and where would a serious peer reviewer push back hardest on the wiki's load-bearing theses?"
sources:
  - "[[breaking-the-lethal-trifecta-talk]]"
related:
  - "[[peer-review-readiness-2026-05-02]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[source-triangulation-audit-2026-05-02]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[owasp-genai-crosswalk]]"
verified: 2026-09-16
verified_against:
  - ".raw/articles/owasp-genai-crosswalk-2026-09-16.md"
verified_findings: 0
---

# Wiki Novelty and Counter-Arguments

This page has two parts: what the wiki contributes beyond the standards literature, and the counter-arguments a peer reviewer would raise against its load-bearing theses, each with a response.

CMMI, BSIMM and NIST CSF each document their trade-offs and counter-positions in their own appendices. This page is that appendix.

## Part 1 — The wiki's contributions

### Novel contributions

The wiki adds these that are not in OWASP ASI Top 10, NIST AI RMF, Gartner Guardian Agents, ISO 42001 (governance-only; no agentic technical control — per [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]]), MITRE ATLAS, CSA MAESTRO, or AIUC-1 as of mid-2026:

| Contribution | Where defined | Novelty |
|---|---|---|
| **6-plane RA with [[xacml\|XACML]] PIP/PDP/PEP/PAP roles colored across deployment shapes** | [[agentic-ai-security-reference-architecture\|RA]] | Most prior work names some of these planes; nothing else maps the full XACML role split across all six and across 7 deployment shapes (chatbot / generative coding / data-science / RAG / MCP server / agent skill / multi-agent mesh) in one artifact |
| **5×9 CMM with L5/L5+ split + dependency-resolved effective scores + ID-tagged evidence at L3+** | [[agentic-ai-security-cmm-2026\|CMM]] + [[agentic-ai-security-cmm-dependency-rules\|Dependency Rules]] | The cumulative-floor rule (CMMI/CMMC import) was *replaced* on 2026-05-04 with dependency-resolved effective scores under a small conservative active rule set (v1 = 3 rules) — a substantive aggregation that captures cross-domain attack paths without punishing strategic trade-offs. Combined with the L5 / L5+ split (achievable-today vs. leading-edge) and mandatory `ASI##` / AIVSS / `AML.T####` tagging at L3+, this is the load-bearing scoring innovation in the wiki |
| **Cognitive File Integrity (CFI) for `SOUL.md` / `IDENTITY.md` / system prompts** | [[supply-chain-security-for-agents\|Supply Chain Security]], [[agent-observability\|Agent Observability]] | Extension of traditional FIM to agentic-specific identity files; not in any standard |
| **Identity-Credential Coupling concept** | [[identity-credential-coupling\|Identity-Credential Coupling]] | Surfaced from [[what-are-non-human-identities\|Oasis]] but operationalized as a CMM D2 L4 evidence requirement (coupled-credential migration plan); not in NIST or ISO |
| **D9 Operations & Human Factors as 9th cross-cutting domain** | [[agentic-ai-security-cmm-2026\|CMM]] D9 | Packages the operational gaps NIST AI 800-4 flagged as biggest blind spot; no current standard names it |
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
| **Standards-crosswalk as auditable matrix** | NIST AI RMF, ISO 42001 and AIUC-1 each crosswalk individually; the [[owasp-genai-crosswalk\|GenAI Crosswalk]] maps 51 risk entries to 26 frameworks in one dataset | The wiki's [[agentic-ai-security-cmm-crosswalk\|crosswalk]] trades breadth for provenance: a dated review stands behind every column but AIUC-1's, and the dataset's 3,781 mappings are unreviewed |

### Prior art the wiki documents

The wiki records these and originated none of them:

- The "platform-layer over prompt-layer" thesis (consensus across the ingested 2026 papers; not wiki-coined)
- The Lethal Trifecta itself (Willison)
- Credential Proxy Pattern (5-tool OSS convergence, not wiki-discovered)
- OWASP ASI Top 10 / AIVSS / MITRE ATLAS technique IDs (used as evidence anchors)
- Guardian Agent terminology (Gartner; wiki uses as procurement synonym for oversight layer)
- "UEBA for Agents" colloquial branding ([[insight-partners\|Insight Partners]] coining; the wiki softens this to architecturally-neutral *agent behavioral monitoring* in body content)

## Part 2 — Counter-arguments and responses

### Thesis 1 — *"Platform-layer enforcement, not prompt-layer"*

**The wiki's position:** "Every control that matters runs in the runtime/platform, below the model. Prompt-level guardrails are bypassable by definition." (RA design principle 1)

**Strongest counter-argument: defense-in-depth requires both layers.** Prompt-layer guardrails reduce attack success rate materially even when bypassable. Meta's [[llamafirewall|LlamaFirewall]] eval on [[agentdojo|AgentDojo]] takes ASR from 17.6% to 7.5% with PromptGuard 2, and to 1.75% with AlignmentCheck added; Anthropic's Constitutional Classifiers take jailbreak success from 86% to 4.4%.[^asr-evals] A *strict* "platform over prompt" doctrine implies an organization can drop them.

**Where the counter-argument holds.** Prompt-layer filtering earns its cost on its own. For low-risk-tier interactions the platform-layer overhead — Constitutional Classifiers report 23.7% inference cost — can exceed the exposure it removes, which leaves a prompt-layer-only configuration the defensible choice.[^asr-evals]

**The wiki's response.** The two layers are ranked and both run. Platform-layer enforcement is primary because injection cannot bypass it; prompt-layer filtering reduces the residual. [[breaking-the-lethal-trifecta-talk|Bullen]] holds that preventing untrusted content "is not really that feasible for most agents." The wiki's [[rag-hardening|RAG hardening]] and [[system-prompt-architecture|system prompt architecture]] pages carry residual-risk callouts. The thesis reads *"platform-layer is primary, prompt-layer is residual."*

### Thesis 2 — *"Independent guardian agents eliminate much of the incumbent AI-protection market by 2029"*

**The wiki's position:** cited from the [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents]] (Feb 2026): independent GAs eliminate need for ~50% of incumbent AI-protection systems in 70%+ of orgs by 2029.[^gartner-ga]

**Strongest counter-argument: Gartner consolidation predictions have a poor historical record.** XDR was forecast to eliminate SIEM, SOAR to eliminate ticketing, and CSPM to eliminate cloud-config tools; all three categories still ship alongside what they were to replace. The recurring pattern is that hyperscaler-embedded controls complement point solutions. The 2029 horizon rests on no evidence base and reads as a vendor-positioning forecast.

**Where the counter-argument holds.** This is one analyst's forecast and the wiki should label it as one. Treating it as a load-bearing assumption is the kind of overclaim that surfaces in peer review.

**The wiki's response.** Cite as Gartner's prediction with low forecast credibility, based on prior consolidation-prediction outcomes. The wiki's load-bearing argument is that an independent oversight layer gives cross-cloud, cross-platform and cross-vendor coverage, and that argument holds without the 50% elimination claim.[^gartner-ga] The structural argument stands; the market prediction carries a Gartner-specific and contested flag.

### Thesis 3 — *"Lethal Trifecta is unconditionally vulnerable"*

**The wiki's position:** "Any deployment combining private-data + untrusted-content + external-comms is **unconditionally vulnerable**." (RA design principle 5; [[lethal-trifecta|Lethal Trifecta]])

**Strongest counter-argument: dataflow mediation keeps all three capabilities in one system and still blocks exfiltration**, so the structural condition does not settle exploitability. The pattern is graded research-stage.

**Where the counter-argument holds.** "Unconditional" is design-time pedagogy, and no published measurement covers a trifecta agent running under production containment.

**The wiki's response.** The trifecta is **necessary** for natural-language exfil at scale and **sufficient given current defense maturity** to require platform-layer containment.

[[camel-pattern|CaMeL]] (Google DeepMind), deterministic gating, and multi-LLM separation carry this counter-argument. Each keeps the full agentic capability in one logical system and blocks exfiltration by mediating the values that cross between components, so untrusted content never reaches the component holding private data and egress. The [[agentic-ai-security-reference-architecture|reference architecture]] grades CaMeL research-stage, so the strongest form of the counter-argument rests on an unshipped pattern.

Attack-success rates from the public competition on slide 3 of [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta]] do not strengthen that counter-argument. That competition scored 18 undefended frontier models, so its range grades model-layer resistance with no architectural control in the loop, and Bullen shows it to argue that containment is necessary. Nothing published measures a trifecta agent running behind egress containment and sensitive-action HITL, and Bullen states that prevalence in the wild is unknown. The design-time test therefore stands as written, and the wiki's reframe reads *sufficient at the design stage, unmeasured in production*.

### Thesis 4 — *"Cumulative floor across all 9 domains"* (revised 2026-05-04 — position changed)

**The wiki's prior position (April 2026 to 3 May 2026):** an organization's overall rating is the floor across all 9 domains. (CMM scoring rule, imported from CMMC 2.0)

**Strongest counter-argument: operationally onerous, because most real orgs would self-rate L1 on one weak domain.** The gap doc flagged this; the [[cmm-calibration-stress-test-2026|stress test]] confirmed it empirically — 3 of 5 realistic archetypes (Stripe-style architectural-containment, Microsoft Agent 365-driven, resource-constrained startup) were misreported by the floor. The L5/L5+ split adopted on 2026-05-04 also broke the floor's "domains are interchangeable units" premise.

**Where the counter-argument holds.** The floor rule is unforgiving on archetypes that make calculated cross-domain trade-offs. CMMC's original adoption assumed mandatory regulatory backing + accredited auditors + narrow scope — none of which apply to this advisory CMM. An advisory model has to discipline cherry-picking by some other mechanism.

**The wiki's revised position (2026-05-04): Thesis 4 was retired, and dependency-resolved effective scores replaced it, documented in [[agentic-ai-security-cmm-dependency-rules|Effective-Score Dependency Rules]].** The new aggregation: a domain's effective score = `min(raw, min over upstream-dependency raw scores)` under a small, conservative active rule set (v1 = 3 rules: D2→D5, D2→D7, D3→D4, anchored to lethal-trifecta + Sondera/AgentCordon evidence). Cross-domain attack paths are captured substantively (D2 weakness genuinely caps D5/D7 because identity gates enforcement and attribution); strategic trade-offs that follow no attack path are left unpunished (D9 ops lag does not cap D2 identity controls). Cherry-picking is now prevented by **mandatory matrix disclosure** — any rating claim must publish the full per-domain raw + effective matrix and the active rule-set version — in place of mathematical aggregation. The dependency-rule registry is scaffolding by design, built to grow as new attack-path evidence and practitioner architectures land in the wiki, with explicit promotion criteria + revision protocol.

**New thesis (provisional).** *Aggregation should be substantive and conservative.* A small set of evidence-anchored cross-domain caps captures the real weakest-link risk; anything beyond that is punitive. Disclosure discipline handles cherry-picking better than aggregation discipline does.

### Thesis 5 — *"AI-BOM + always-on customer eval is the single highest-leverage control"*

**The wiki's position:** three of five threat classes (insider, APT, version regression) collapse to the same observable, a delta against a trusted baseline produced by a customer-owned, version-pinned, continuously-executed eval harness with cryptographic provenance ([[agentic-ai-threat-classes-2026|Threat Classes 2026]]).

**Strongest counter-argument: cost is material and unbenchmarked.** Continuous re-evaluation over a large eval suite for every model update + every prompt change has no published cost / latency / coverage benchmarks. Eval suites become stale faster than models. Evals miss novel attacks they were not designed for. The wiki itself flags this as an open issue.

**Where the counter-argument holds.** The thesis is *theoretically* tight — one observable absorbs three threat classes — and operationally undefined. With no published benchmark for eval-harness cost as a percentage of inference spend, "always-on" names an intention that no budget yet bounds.

**The wiki's response.** The wiki's [[agentic-ai-threat-classes-2026|threat-classes page]] logs the cost-benchmark gap as open. The thesis is *strategic guidance*: build this primitive first because it absorbs the most threat classes. Pair with the [[agentic-cmm-vs-standards-validation|validation §3]] gap on guardrail latency / cost budgets — the operationalization is unfinished.

### Thesis 6 — *"Behavioral monitoring (UEBA-for-agents) for ephemeral agents"*

**The wiki's position:** behavioral monitoring with baselines + drift detection at D7 L3+, already softened from the "UEBA for Agents" branding to architecturally-neutral language.

**Strongest counter-argument: classical UEBA needs stable identities and persistent baselines.** AI agents are often ephemeral and non-deterministic; baselines collapse if the agent population churns. UEBA products had largely merged into SIEM/XDR by 2020, so the metaphor transfers poorly.

**Where the counter-argument holds.** Already addressed. The wiki softened the language and labels Insight Partners' "UEBA for Agents" coining as informal vendor framing not adopted by NIST/ISO/OWASP. Body uses *agent behavioral monitoring* / *behavioral baselines for agents*.

**Open question.** What does a behavioral baseline look like when most of an agent population is short-lived? Aggregate-level invariants (mesh-wide rate caps, pairwise traffic bursts) are partial answers; the problem is open. The [[multi-agent-runtime-security|multi-agent runtime security page]] records that state.

### Thesis 7 — *"A hand-built standards crosswalk earns its place against a published one"*

**The wiki's position:** the wiki maintains the [[agentic-ai-security-cmm-crosswalk|CMM Standards Crosswalk]] by hand so that a maturity rating resolves to a clause somebody read.

**Strongest counter-argument: a published crosswalk covers more ground for free.** The [[owasp-genai-crosswalk|GenAI Crosswalk]] maps 51 risk entries to 26 frameworks with 3,781 mappings.[^crosswalk-data]

**Where the counter-argument holds.** On coverage and on distribution, entirely. The wiki has no machine-readable export and no submission flow, and reaches far fewer frameworks.

**The wiki's response.** All 3,781 mappings carry `confidence: "unreviewed"` with an empty `reviewed_by`.[^crosswalk-data] A rating shown to an auditor cannot rest on a row nobody has signed.

**The two artifacts trade coverage against provenance.** The dataset wins the distribution half of the skeptic's case: 51 OWASP risk entries against 26 frameworks under CC BY-SA 4.0, shipped as JSON, an npm package, OSCAL 1.1.2 and STIX 2.1. A classifier proposes its rows and a reviewer accepts, rejects or edits each one through a pull request; [[owasp-genai-crosswalk|the dataset page]] holds the pipeline detail. The wiki's matrix takes the opposite trade. It covers fewer standards, it names a dated standards review behind every column but AIUC-1's, and it marks the one column nobody read — ISO 42001 Annex A, bounded by a paywall — as summary-sourced on the page itself. The wiki should import the dataset's machine-readable identifiers and export path, and keep what the dataset does not carry: a person who read the clause and said so.

## Open contests

These are positions a peer reviewer is right to push on, and the wiki does not yet have a settled answer:

> [!gap] Unresolved contests
> 1. **Floor-rule exemptions.** Whether L4/L5 should be relaxable when D3+D5 are strong, or split consumer-facing vs internal-platform. Documented as open on the [[agentic-ai-security-cmm-2026|CMM]] D7 contradiction callout.
> 2. **Eval-harness cost as % of inference spend.** No published benchmark for the multi-class absorber control. Operationalization unfinished.
> 3. **Cascade-detection numeric thresholds.** Adversa lists categories; rule SQL/YAML is not public; vendor implementations don't surface thresholds. Documented in [[multi-agent-runtime-security|Multi-Agent Runtime Security]].
> 4. **Behavioral baseline definition for ephemeral agents.** Aggregate-level invariants are partial; the full definition is open.
> 5. **MCP CVE percentages** ([[source-triangulation-audit-2026-05-02|Source Triangulation Audit]] §Claim 4 contested). Wiki should re-derive from peer-reviewed denominators.
> 6. **Lab-self-reported scheming rates** awaiting peer-reviewed independent replication ([[source-triangulation-audit-2026-05-02|Audit]] §Claim 8).
> 7. **Exploitation rate of a contained trifecta agent.** [[source-triangulation-audit-2026-05-02|Audit]] §Claim 5 recorded the 1.5–6.7% figures as talk-specific and awaiting replication; slide 3 of [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta]] cites them to a published public competition over 18 undefended frontier models, which settles their provenance. What stays open is the rate under containment: no published measurement covers a trifecta agent behind egress containment and sensitive-action HITL, and Bullen states that prevalence in the wild is unknown.
> 8. **Two-actor AIUC-1 audit model** — issuer (AIUC) ≠ auditor (Schellman). Whether this strengthens or weakens independence is contested ([[aiuc-1|AIUC-1]] caveats §5).
> 9. **A2A v1.0 spec lacks message integrity, replay protection, multi-hop trust chain.** Wiki documents this; the resolution lives in vendor implementations and Issue #1575 — not yet merged.
> 10. **CSA ATF promotion gates not fully specified** by CSA itself; CMM D3 L4 still depends on org-authored rubric ([[agentic-cmm-vs-standards-validation|validation]] §5).

## Peer-review process going forward

This page is the standing pre-peer-review checklist. Every load-bearing thesis added to the RA / CMM should answer:

1. Is this novel, sharpened, or borrowed?
2. Which counter-argument from a serious skeptic is strongest?
3. Where does that counter-argument hold? (If nowhere, the thesis is probably overstated.)
4. What is the wiki's response?
5. What does the wiki *not* yet have an answer for?

If a thesis cannot survive that exercise, it should not be cited as L3+ evidence.

## See also

- [[peer-review-readiness-2026-05-02|Peer-Review Readiness]] — the sibling readiness audit
- [[agentic-cmm-vs-standards-validation|Validation: Agentic AI CMM vs Widely Adopted Standards]] — sister audit; standards comparison
- [[source-triangulation-audit-2026-05-02|Source Triangulation Audit 2026-05-02]] — sister audit; evidence-source diversification
- [[agentic-ai-security-reference-architecture|RA]] · [[agentic-ai-security-cmm-2026|CMM]] · [[agentic-ai-threat-classes-2026|Threat Classes 2026]] · [[multi-agent-runtime-security|Multi-Agent Runtime Security]] — load-bearing artifacts under review

## Notes

[^asr-evals]: LlamaFirewall PromptGuard 2 / AlignmentCheck attack-success-rate reductions are Meta's own reported results, evaluated on the [[agentdojo|AgentDojo]] benchmark: [LlamaFirewall — An open source guardrail system for building secure AI agents](https://arxiv.org/abs/2505.03574) (arXiv:2505.03574). The Constitutional Classifiers jailbreak-success and inference-cost figures are Anthropic's reported results (2025), for which this wiki holds no primary link. The same figures and the same caveat are carried at [[agentic-ai-security-reference-architecture|RA]] Principle 1.

[^gartner-ga]: [Gartner — Market Guide for Guardian Agents (G00836300 reprint)](https://www.gartner.com/doc/reprints?id=1-2N2436IJ&ct=260324&st=sb), 2026-02-24. The reprint URL is session-tokened; the canonical research-note ID is `G00836300`. Summarized at [[guardian-agents-market-guide|Gartner Market Guide for Guardian Agents]].

[^crosswalk-data]: OWASP GenAI Security Project, [GenAI Crosswalk](https://genai-security-project.github.io/crosswalk/) v4.0.0, data layer captured 2026-09-16: 3,781 control mappings from 51 risk entries to 26 frameworks, every mapping carrying an unreviewed confidence marker and an empty reviewer list. Summarized at [[owasp-genai-crosswalk|GenAI Crosswalk]].
