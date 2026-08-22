---
type: thesis
title: "Secure-SDLC Framework Stack"
address: c-000043
created: 2026-05-13
updated: 2026-08-19
tags:
  - thesis
  - secure-sdlc
  - ssdf
  - samm
  - bsimm
  - slsa
  - secure-by-design
status: developing
origin: produced
scope_axis:
  - sec-of-ai
  - sec-against-ai
question: "For most organizations in 2026, is anchoring the secure-SDLC policy to NIST SSDF and assessing maturity via OWASP SAMM the right approach — or does the 2026 threat surface and AI-component governance reality require an additional layered overlay?"
current_position: "Structurally correct, materially incomplete. The SSDF+SAMM split (policy anchor + maturity assessment) is sound and remains the right baseline for traditional software development. But it is necessary, not sufficient: in 2026 it requires an AI-component governance overlay (NIST AI RMF or ISO 42001 + Agentic AI Security CMM), an AI development-lifecycle layer (ISO/IEC 5338, which 42001 does not reach), a supply-chain layer (SLSA + CycloneDX), and explicit recalibration for AI-augmented attacker pace. Adopting SSDF+SAMM as the foundation and adding the overlay is the right answer; treating SSDF+SAMM alone as the complete answer is the same category error as treating ISO 27001 + SOC 2 as complete in 2018."
last_revised: 2026-08-19
related:
  - "[[cybersecurity-cmms-exemplars]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[sdlc-in-the-ai-attacker-era]]"
  - "[[anthropic-2026-agentic-coding-trends]]"
  - "[[pwc-agentic-sdlc-in-practice]]"
  - "[[pwc-stage-coverage-tiers]]"
  - "[[metr-rct-2025]]"
  - "[[collaboration-paradox]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mdash-defense-at-ai-speed]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[nist-ai-rmf]]"
  - "[[microsoft-sdl]]"
  - "[[microsoft-sdl-evolving-security-practices]]"
  - "[[nist-ssdf]]"
  - "[[nist-sp-800-218a]]"
  - "[[standards-review-nist-sp-800-218a-2026-Q2]]"
  - "[[canadian-bank-secure-sdlc-ai-assessor-scorecard]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[guardfall-shell-injection-audit]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[owasp-ai-exchange]]"
  - "[[owasp-samm]]"
sources:
  - "[[.raw/articles/microsoft-sdl-evolving-security-practices-2026-02-03.md]]"
---

# Secure-SDLC Framework Stack

## On this page

- [Question](#question)
- [Current position](#current-position)
- [Supporting evidence: why the foundation is right](#supporting-evidence-why-the-foundation-is-right)
- [Counter-evidence: four structural gaps](#counter-evidence-four-structural-gaps)
- [The recommended 2026 stack](#the-recommended-2026-stack)
- [Conditions where the claim holds](#conditions-where-the-claim-holds)
- [Conditions where the claim breaks down](#conditions-where-the-claim-breaks-down)
- [Position history](#position-history)
- [Open sub-questions](#open-sub-questions)
- [See also](#see-also)
- [Notes](#notes)

## Question

For most organizations in 2026, is the recommendation *"anchor the policy to [[nist-ssdf|NIST SSDF]] and assess current maturity via [[owasp-samm|OWASP SAMM]]"* the right approach, or does the 2026 threat surface (AI-augmented attackers, agentic SDLC adoption, AI-component supply chain) require a layered overlay?

## Current position

The claim is structurally correct but materially incomplete. It is right about the role split (policy anchor versus maturity assessment) and right about the individual framework choices for traditional secure-SDLC programs. It is incomplete on the threat-model recalibration and AI-component governance that 2026 demands.

Treat [[nist-ssdf|SSDF]] plus [[owasp-samm|SAMM]] as the foundation, then add the overlay. For organizations not yet building AI products or facing pressure from AI-augmented attackers, the claim is operationally adequate. For organizations on the leading edge of AI adoption, it is a starting point, not a destination.

## Supporting evidence: why the foundation is right

The role split between policy framework and maturity model is the right pattern; SSDF and SAMM are the right individual choices for each role.

- [[nist-ssdf|NIST SSDF (SP 800-218 v1.1)]] is an outcomes-oriented practice framework: four practice groups (PO/PS/PW/RV), regulatory weight (EO 14028, OMB M-22-18), tool-agnostic. It defines target outcomes and does not address maturity progression. Its Table 1 reference column synthesizes from BSAFSS, BSIMM, EO 14028, IEC 62443, ISO 27034, [[microsoft-sdl|Microsoft SDL]], NIST CSF, OWASP ASVS/MASVS/SAMM, PCI Secure SLC, and NIST SP 800-53/160/161/181, making SSDF a consensus cross-walk for the secure-SDLC field.
- [[owasp-samm|OWASP SAMM v2]] is a prescriptive maturity model: five functions, three practices each, three levels, free, vendor-neutral, with an explicit assessment toolkit. It identifies an organization's current maturity level and prescribes the next improvement steps. See [[cybersecurity-cmms-exemplars|Cybersecurity CMMs Exemplars]] §SAMM for structural analysis.

The OWASP-to-SSDF crosswalk, maintained since 2022, is referenced by both organizations as a recommended pairing pattern. The 2024 SAMM-BSIMM convergence, recognized in both organizations' updates, shows that the AppSec community treats prescriptive maturity (SAMM) and descriptive benchmarking (BSIMM) as complementary: SAMM as the target, BSIMM as the industry mirror. Both frameworks are free, vendor-neutral, and US-regulator-aligned; within the surveyed set, neither has a credible alternative on those three dimensions.

The [[owasp-ai-exchange|OWASP AI Exchange]] endorses the same pairing by name, for exactly this control. `SEC DEV PROGRAM`'s reference list names OWASP SAMM, NIST SSDF and the NIST SSDF AI practices, and its standards section grades the OpenCRE entry linking SSDF and SAMM as covering the control fully, with said particularity ([`/go/secdevprogram/`](https://owaspai.org/go/secdevprogram/)). The particularity is AI: new engineering roles, new assets and threats, a supply chain that includes data and models, a development environment holding sensitive data, and model performance testing before deployment. That reading — a sound foundation with an outstanding AI particularity — is this thesis's position, stated here by a third instrument, independent of this wiki and of both frameworks' own projects.

The Exchange also supplies the normative statement behind the layering recommendation, in both of its development controls. `SEC DEV PROGRAM` writes that there is no need for an isolated secure development framework for AI and that the work is to build on existing secure software development practices and bring AI assets, threats and controls into them ([`/go/secdevprogram/`](https://owaspai.org/go/secdevprogram/)). `DEV PROGRAM` writes that existing practices should be extended to AI development instead of treating AI as something requiring a separate approach, and states the instruction plainly: do not isolate AI engineering ([`/go/devprogram/`](https://owaspai.org/go/devprogram/)). Adopting the baseline and layering on top of it is what this thesis recommends; these two statements are its external warrant.

For organizations that are US federal contractors, supply-chain participants for federal procurement, or building traditional software with minimal AI integration, the SSDF plus SAMM baseline is mature, defensible, and operationally required.

## Counter-evidence: four structural gaps

Four structural gaps in the 2026 evidence base fall outside what SSDF plus SAMM covers.

### Gap 1: AI-augmented attacker pace is in neither framework

Both SSDF and SAMM are calibrated against human-paced adversaries and human-paced development cycles. Three May 2026 sources document the shift: [[xbow-mythos-evaluation|XBOW's Mythos evaluation]], [[mdash-defense-at-ai-speed|Microsoft's MDASH announcement]], and [[anthropic-glasswing-announcement|Anthropic's Glasswing announcement]]. CrowdStrike CTO Elia Zaitsev, speaking as a Glasswing partner: "The window between a vulnerability being discovered and being exploited by an adversary has collapsed — what once took months now happens in minutes with AI."[^glasswing]

SSDF practice group PW (Produce Well-Secured Software) and SAMM's Verification function carry time assumptions calibrated for the pre-AI era. [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] develops the full recalibration argument.

### Gap 2: AI-component governance is at best partial

NIST has extended SSDF for AI via [[nist-sp-800-218a|NIST SP 800-218A — SSDF Community Profile for GenAI]]. Table 1 of the Profile tags six tasks `[Not part of SSDF 1.1]` — `PO.5.3`, `PS.1.2`, `PS.1.3`, and the three tasks of the new practice `PW.3` (Confirm Integrity of Training, Testing, Fine-Tuning, and Aligning Data: `PW.3.1`/`PW.3.2`/`PW.3.3`) — plus AI-specific recommendations across most existing SSDF tasks ([[standards-review-nist-sp-800-218a-2026-Q2|2026-Q2 standards review]]). It is scoped to AI model development only; deployment and operation of AI systems are explicitly out of scope, as is most of the data-governance life cycle beyond training-data security.

That scope makes SP 800-218A a partial AI overlay, not a complete one. As [[standards-review-nist-sp-800-218a-2026-Q2|the standards review]] confirms, it contributes development-time process tasks, not deployment-time controls: it covers model-artifact protection, training-data integrity, AI threat modeling, and AI shutdown, but leaves the runtime, agent-orchestration, and multi-agent surface to other instruments ([[nist-ai-rmf|NIST AI RMF]], [[iso-iec-42001|ISO 42001]], [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]). The [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] (Dimension D8) cites SP 800-218A alongside CycloneDX 1.6 ML-BOM, SPDX 3.0 AI extension, and EU AI Act Annex IV as supply-chain references.

> [!gap] No AI-specific SAMM extension in the surveyed set
> Across the frameworks surveyed for this thesis, OWASP SAMM has no published AI-specific extension as of mid-2026, and no public roadmap for one has been announced.

One measurable form of this gap carries a published figure. The Exchange records industry-average automated test coverage at 43% against an often-cited recommendation of 80%, and reports that automated testing in AI engineering is often neglected because the performance of the AI model is mistakenly regarded as the ground truth of correctness (SIG benchmark report 2023, via [[owasp-ai-exchange|OWASP AI Exchange]], [`/go/secdevprogram/`](https://owaspai.org/go/secdevprogram/)). A model scoring well on its eval set establishes nothing about the correctness of the data-preparation, orchestration, tool-integration and policy-enforcement code around it, and that code is where the security-relevant defects sit. Neither SSDF nor SAMM asks the question in AI-specific terms.

Per [[pwc-agentic-sdlc-in-practice|PwC Middle East 2026]] data, 38% of surveyed regional teams are Pioneer-tier (≥6 of 7 SDLC stages augmented).[^pwc] For these organizations, SSDF plus SAMM does not address AI-BOM and model-artifact integrity, agent identity and [[nhi-governance-for-agents|non-human identity governance]], runtime guardrails for AI components, [[ai-coding-agent-governance|coding-agent governance]], or the [[collaboration-paradox|collaboration paradox]] — where HITL functions as the default mode, not an edge-case guardrail.

### Gap 3: the productivity and pace mismatch

The [[metr-rct-2025|METR 2025 RCT]] (16 experienced developers, 19% slower with AI tools on their own repositories) bounds vendor productivity claims at the experimental level.[^metr] The symmetric implication — that attacker speed claims are also bounded — holds only conditionally: the underlying capability advance is real. The [[anthropic-glasswing-announcement|Glasswing announcement]] discloses a 27-year-old OpenBSD bug, a 16-year-old FFmpeg bug that automated testing tools had hit 5 million times without catching, and an autonomous Linux-kernel privilege-escalation chain.[^glasswing]

The [[anthropic-2026-agentic-coding-trends|Anthropic 2026 Trends Report]] addresses the dual-use risk at the strategic level in Trend 8. Priority 4 is "Embedding security architecture as part of agentic system design from the earliest stages," which the Report treats as a top-four organizational priority.[^anthropic] Neither SSDF nor SAMM was designed for a context in which a frontier-model vendor names this a strategic priority.

PwC's data corroborates this: security is the top adoption barrier for GenAI in SDLC, cited by 37.7% of respondents, and PwC's first recommended enabler is "early compliance guardrails" — treating secure-by-design as the first step.[^pwc]

### Gap 4: the coding agent is an actor in the SDLC, and no layer governs it

Every layer of the stack below governs either the software being produced or the AI product being deployed. None governs the agent doing the producing. That distinction did not matter while AI assistance meant completion suggestions a developer accepted keystroke by keystroke. It matters now that the agent holds repository credentials, executes shell commands, reads attacker-reachable repository content, and in the CI-runner and delegated variants completes work with no human watching a diff.

The instruments in rows 1 through 3 miss this by scope, not by oversight. [[nist-sp-800-218a|SP 800-218A]]'s subject is the generative AI system under development — training data, model weights, model artifacts — which leaves the case of a conventional application built *by* an agent outside its frame, as [[standards-review-nist-sp-800-218a-2026-Q2|the standards review]] records when it characterizes the profile as development-time process tasks for AI systems rather than deployment-time controls. [[microsoft-sdl|Microsoft SDL]]'s 2026 AI extension names threat modeling for AI, AI observability, memory protections, agent identity and RBAC, model publishing, and shutdown — a control set for agents an organization *ships*, not for the agent on its developers' workstations.

The controls that do exist are vendor-side and unstandardized: OS sandboxing, managed permission policy, egress allowlists, harness-configuration audit, and per-agent attribution, catalogued in [[securing-agentic-coding|Securing Agentic Coding]] and graded there by availability. They are real, several are GA, and none is required or even named by a secure-SDLC framework. That is a governance gap, not a tooling gap.

> [!gap] Gap 4 now has one clause-level datapoint; the rest is still scope inference
> The claim that no framework in the stack governs the coding agent as an SDLC actor was drawn from each instrument's stated scope and from the existing reviews of SSDF and SP 800-218A, not from a dedicated clause-level pass. One instrument has since been read clause by clause. The Exchange's `SEC DEV PROGRAM` and `DEV PROGRAM`, the natural home for such a clause, name AI-assisted development nowhere and treat "new types of engineering, together with new types of engineers" as human roles — data scientists, data engineers, AI engineers ([`/go/secdevprogram/`](https://owaspai.org/go/secdevprogram/)). The absence is specific rather than a lack of agentic awareness: the same control treats the capabilities agents interact with dynamically, skills and services reached through MCP, as supply chain — the supply chain of the system being built, not of the agent building it. Issue #171 on the [[standards-review-backlog|standards-review backlog]] stays open for the remaining frameworks, scoped to the coverage matrix and falsifiable absence claims [[standards-validation-methodology-2026-05|the methodology]] requires, and the scope-boundary notes on [[nist-ssdf|SSDF]], [[nist-sp-800-218a|SP 800-218A]], and [[microsoft-sdl|Microsoft SDL]] keep their earlier status.

## The recommended 2026 stack

For most organizations doing or supporting software development:

| Layer | Purpose | Frameworks |
|---|---|---|
| Policy anchor | Outcomes; what good looks like | [[nist-ssdf\|NIST SSDF (SP 800-218 v1.1)]] + [[nist-sp-800-218a\|SP 800-218A (AI Profile)]] |
| Maturity assessment | Where am I, what's next | [[owasp-samm\|OWASP SAMM v2]] for traditional software; CISA Secure-by-Design for the cultural overlay |
| AI governance overlay (if building/deploying AI) | AI-specific governance | [[nist-ai-rmf\|NIST AI RMF]] + [[agentic-ai-security-cmm-2026\|Agentic AI Security CMM 2026]] or [[iso-iec-42001\|ISO/IEC 42001]] |
| AI development lifecycle | Engineering process for AI, as distinct from governance of it | ISO/IEC 5338, which extends ISO/IEC 12207 |
| Supply chain | Provenance and integrity | SLSA v1.0 (target L3 for production); CycloneDX SBOM/ML-BOM |
| Benchmark (optional, large orgs) | What peers actually do | BSIMM for observation-based benchmarking |
| Agentic-coding controls (if developers use coding agents) | Govern the agent doing the building | No framework layer exists; assemble from [[securing-agentic-coding\|Securing Agentic Coding]] against the variant in [[generative-coding-deployment-shape-2026\|Generative Coding Deployment Shapes]] |
| Operational alignment | Org-wide framing | NIST CSF 2.0 (Govern function added in 2024) |

The original claim captures rows 1 and 2. Rows 3 through 8 are what is missing for organizations on the AI adoption curve. Row 7 is the one with no framework to name: it is filled today by vendor configuration and a wiki-produced catalog, which is a weaker foundation than the rest of the stack and should be read as a standing gap rather than a recommendation of equal confidence.

Rows 3 and 4 are separate because governance and lifecycle are separate instruments, and the Exchange states the split. ISO/IEC 42001 extends the risk management system and covers governance; it does not reach the lifecycle processes, and its stated exclusions are how to train models, data lineage, continuous validation, versioning of AI models, project planning, and when sensitive data is used in engineering ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). ISO/IEC 5338 covers what 42001 leaves out — the complete AI development lifecycle, by extending ISO/IEC 12207 with new processes and AI-specific particularities for existing ones, which the Exchange grades as covering its `DEV PROGRAM` control fully ([`/go/devprogram/`](https://owaspai.org/go/devprogram/)). An organization that adopts 42001 alone and reads it as the AI layer is short the engineering half.

## Conditions where the claim holds

The SSDF plus SAMM baseline is the right starting point for organizations that: are US federal contractors or supply-chain participants (SSDF is operationally required via EO 14028 and OMB M-22-18); build traditional software with minimal AI integration; are constrained to free or vendor-neutral tooling; or need a defensible, regulator-recognized baseline. The frameworks are mature, documented, and free, and the 2024 crosswalk lets an organization cite either to auditors. For a median enterprise IT organization in 2026, the pairing is directionally correct and operationally adequate.

## Conditions where the claim breaks down

The baseline is insufficient for organizations that:

- Build AI-powered products: they need a [[nist-ai-rmf|NIST AI RMF]] plus [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] or [[iso-iec-42001|ISO 42001]] overlay.
- Defend against AI-augmented attackers: they need the [[sdlc-in-the-ai-attacker-era|SDLC-versus-AI-attacker]] threat model that neither SSDF nor SAMM addresses.
- Sell into EU markets: they need an EU CRA compliance posture (effective December 2027) with parallel software-security obligations.
- Are heavy supply-chain participants: they need SLSA-level provenance, which SSDF gestures toward but does not operationalize.
- Operate at Pioneer tier per [[pwc-stage-coverage-tiers|PwC's tiers]]: they need the [[anthropic-2026-agentic-coding-trends|Trends Report]] Priority 4 principle — embedding security architecture from the earliest stages — as a first-class design requirement rather than a tier-3 SAMM line item.

## Position history

**2026-08-19 — the AI overlay splits into governance and lifecycle.** The stack's AI layer named a governance standard and no lifecycle standard. ISO/IEC 42001 extends the risk management system and stops short of lifecycle processes, and ISO/IEC 5338 covers them by extending ISO/IEC 12207 — a distinction the [[owasp-ai-exchange|OWASP AI Exchange]] states outright — so the stack gained an eighth row. Gap 4 moved from scope inference alone to one clause-level datapoint, read against the Exchange's two development-programme controls. The position itself did not change.

**2026-07-30 — Gap 4 added.** The three original gaps concern what the frameworks say about the software and the AI product. The July 2026 agentic-coding review surfaced a fourth: the coding agent is now an actor inside the SDLC with credentials, shell access, and in two deployment variants no human reviewer, and no layer of the recommended stack governs it. The stack gained a seventh row that names no framework, which is the honest state of that layer.

- **2026-05-13** — Seeded in response to a direct claim: "For most organizations, the best approach in 2026 is to anchor the policy to NIST SSDF while using OWASP SAMM to assess current maturity and identify gaps." Position: structurally correct, materially incomplete, with a layered-stack recommendation.
- **2026-05-14** — [[microsoft-sdl-evolving-security-practices|Microsoft's SDL-for-AI announcement]] supplied a vendor precedent for the anchor-plus-AI-overlay pattern this thesis prescribes. Microsoft's six SDL-for-AI focus areas (threat modeling for AI, AI system observability, AI memory protections, agent identity and RBAC, AI model publishing, AI shutdown mechanisms) overlap substantially with the AI-overlay layer. The structural recommendation gained the precedent without requiring a position change.
- **2026-05-14** — Direct ingest of [[nist-ssdf|NIST SP 800-218 (SSDF v1.1)]] and [[nist-sp-800-218a|SP 800-218A (AI Profile)]] sharpened two points. SSDF's Table 1 reference column names Microsoft SDL (`MSSDL`) as a source, making the Microsoft-SDL-to-SSDF lineage documentary rather than asserted. 218A's AI scope is narrower than Microsoft SDL's six focus areas suggest: it covers training-data integrity, model weights, AI threat modeling, AI shutdown and rollback, and continuous dev-environment monitoring, but excludes deployment and operation. The recommended stack composition did not change; the precision of its citations did.
- **2026-05-15** — A candidate Layer 4½ between supply chain and benchmark: harness-config audit as a CI-gated instrument over the agent harness configuration tree (see [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] and [[control-efficacy-gate|Control-Efficacy Gate]]). Held as a candidate addition until a second, independent instrument lands; the recommended stack did not change.
- **2026-05-15** — A second candidate layer (Layer 7, AI-driven vulnerability discovery in CI) operating over application source rather than agent configuration, supported by production tools from five vendors converging on a common frame: validation, not rule-based pattern matching, as the architectural primary stage. SSDF practices PW.7 and PW.8 are candidate hosts for a "use AI-driven vuln-discovery" level criterion at a future SSDF v2 or SAMM v3; neither has shipped that language, so the candidate is parked. See [[frontier-ai-for-vuln-discovery|the frontier-AI thesis]]. The recommended six-layer stack stayed unchanged.

## Open sub-questions

- **OWASP SAMM v3** — is there a public roadmap for AI-aware extensions to SAMM? None has been announced as of mid-2026, which is a structural risk for SAMM's long-term relevance.
- **CISA Secure-by-Design maturity** — CISA published Secure-by-Design principles but not a maturity model. Whether an org-level Secure-by-Design rubric materializes, and whether it competes with or complements SAMM, is open.
- **EU CRA compliance crosswalk** — when CRA enforcement begins December 2027, how do SSDF and SAMM map to CRA's essential cybersecurity requirements? Cross-walks are likely; quality and binding interpretation are not yet established.
- **SLSA's relationship to SSDF** — SSDF's PS (Protect Software) gestures at supply-chain integrity and SLSA operationalizes it. Whether a future SSDF v2 absorbs SLSA-grade requirements or the two remain parallel is unresolved.
- **BSIMM versus SAMM in the AI era** — BSIMM's descriptive approach may adapt to AI-era practices faster than SAMM's prescriptive one because it observes what firms actually do. Worth tracking whether enterprise AppSec programs shift their benchmarking center of gravity.
- **The "most organizations" denominator** — per [[pwc-agentic-sdlc-in-practice|PwC's 2026 data]], 38% of regional teams are already Pioneer-tier. PwC forecasts more than half of regional teams running a fully agentic SDLC by 2027 and two-thirds by 2029; as that share grows, the boundary between "SSDF plus SAMM is sufficient" and "needs the overlay" shifts.[^pwc] When does the median enterprise cross over?
- See [[wiki/gaps/_index|Gaps Index]] for related open questions.

## See also

- [[microsoft-sdl|Microsoft Secure Development Lifecycle (SDL)]] — vendor-authored secure-SDLC framework that implements the anchor-plus-AI-overlay pattern this thesis prescribes.
- [[microsoft-sdl-evolving-security-practices|Microsoft SDL]] — the source paper for that announcement.
- [[cybersecurity-cmms-exemplars|Cybersecurity CMM Exemplars and Design Lessons]] — examines CMMI, BSIMM, SAMM, CMMC, and NIST CSF 2.0 with per-exemplar structural analysis.
- [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]] — a CMM built on lessons from those exemplars.
- [[pwc-stage-coverage-tiers|PwC Stage-Coverage Tiers]] — maturity model on the GenAI-in-SDLC adoption-breadth axis.
- [[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]] — thesis on why secure SDLC requires recalibration for AI-augmented adversaries.
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — thesis on the six-layer control landscape for AI systems.
- [[anthropic-2026-agentic-coding-trends|Anthropic 2026 Agentic Coding Trends Report]] — vendor source for Priority 4 on embedding security architecture in agentic system design.
- [[pwc-agentic-sdlc-in-practice|PwC Middle East 2026 Agentic SDLC Report]] — advisory source reporting security as the top GenAI-in-SDLC adoption barrier; the survey figure and its provenance are in Gap 2 above.

## Notes

[^glasswing]: [Anthropic — Project Glasswing](https://www.anthropic.com/glasswing), May 12, 2026. CrowdStrike CTO Elia Zaitsev partner citation; disclosed examples include a 27-year-old OpenBSD remote-crash bug, a 16-year-old FFmpeg bug that automated testing tools had hit roughly 5 million times, and an autonomous Linux-kernel privilege-escalation chain. Summary: [[anthropic-glasswing-announcement|Project Glasswing]].
[^pwc]: [PwC Middle East — The Future of Solutions Development and Delivery in the Rise of GenAI (PDF)](https://www.pwc.com/m1/en/publications/2026/docs/future-of-solutions-dev-and-delivery-in-the-rise-of-gen-ai.pdf), 2026. Survey of 377 respondents across the GCC, Jordan, and Egypt: 38.2% Pioneer-tier (≥6 of 7 SDLC stages augmented); security the top adoption barrier at 37.7%; forecast of more than half of regional teams on a fully agentic SDLC by 2027 and two-thirds by 2029. Summary: [[pwc-agentic-sdlc-in-practice|PwC Middle East 2026 Agentic SDLC Report]].
[^metr]: [METR — Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/), July 2025 ([arXiv 2507.09089](https://arxiv.org/abs/2507.09089)). 16 experienced developers measured 19% slower on familiar codebases when AI tooling was enabled. Summary: [[metr-rct-2025|METR 2025 RCT]].
[^anthropic]: [Anthropic — 2026 Agentic Coding Trends Report (PDF)](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf), 2026. Trend 8 (dual-use) and Priority 4 (embedding security architecture from the earliest stages). Summary: [[anthropic-2026-agentic-coding-trends|2026 Agentic Coding Trends Report]].
