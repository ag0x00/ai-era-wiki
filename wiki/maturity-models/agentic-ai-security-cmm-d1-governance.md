---
type: maturity-model
title: "CMM D1: Governance and Accountability"
address: c-000136
created: 2026-05-24
updated: 2026-08-31
tags:
  - maturity-models
  - cmm
  - governance
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[aiuc-1-critical-evaluation]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[decision-rights]]"
  - "[[shadow-automation]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[owasp-ai-exchange]]"
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-rai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[cyera-agent-guardian-release]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[aiuc-1-critical-evaluation]]"
  - "[[iso-iec-42001]]"
  - "[[nist-ai-rmf]]"
---

# Agentic AI Security CMM — D1 Governance & Accountability (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D1 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D1 fixes who is accountable for agent behavior, with what authority, and on what auditable record. The recalibration makes one material change: **L5 no longer mandates a single certification.** The [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] flagged that pinning L5 to [[aiuc-1|AIUC-1]] both moves the bar on a vendor's cadence and concentrates assurance in one certifier; the [[aiuc-1-critical-evaluation|AIUC-1 critical evaluation]] examined the question and found the mandate indefensible. D1-L5 now states a capability that several schemes satisfy: *current, independent, third-party assurance of the governance program*.

> [!gap] Single-source grounding
> The level criteria and cost model synthesize the wiki's own recalibration method against one representative-customer source (the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]]) plus the [[aiuc-1-critical-evaluation|AIUC-1 evaluation]]. They are wiki-internal calibration, not an externally ratified standard, and will firm up as later domains and crosswalks test them. Part of the L2 criterion set now has an external counterpart. The Exchange's eight-step first iteration for AI governance names a published policy and assigned responsibilities, which L2 grades as the AI-use policy and the signed RACI ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). Three further steps in that iteration sit at no rung of this ladder: identifying the applicable laws and regulations, running an AI-literacy program, and the proposal half of its inventory survey, which covers AI ideas and concerns as well as AI in use. L3's shadow-agent inventory reaches deployed agents and stops there. In the other direction, L2's agent risk-tier scheme has no counterpart in the iteration. The level boundaries stay wiki-internal in every case.

## Threat coverage

D1 is the accountability wrapper, so its threats are cross-cutting rather than a single ASI category; it is the only domain that addresses **Class 5 (jurisdictional adversary)** — vendor abstraction, jurisdiction tagging, and contract resilience against a regulatory cutoff, the one threat no technical plane control mitigates. The full mapping is in the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix; the class detail is on the [[agentic-ai-threat-classes-2026|threat-classes page]].

## Control landscape (dated)

Governance has no enforcement engine. Its controls are documents, committees, and audits, so the landscape comprises assurance schemes and the platforms that hold the evidence.

| Layer | What ships today | Status (May 2026) |
|---|---|---|
| Governance frameworks | [[nist-ai-rmf\|NIST AI RMF]] (Govern), [[iso-iec-42001\|ISO/IEC 42001]], [[eu-ai-act\|EU AI Act]] Art. 9 + Art. 17, [[csa-maestro\|CSA]] ATF, [[cosai\|CoSAI]] WS3, [[google-saif\|SAIF]] Governance | Stable; no agentic Govern profile — see below |
| Governance control catalogues | [[owasp-ai-exchange\|OWASP AI Exchange]] G.U.A.R.D. steps with `AI PROGRAM`, `SEC PROGRAM`, `CHECK COMPLIANCE`, `SEC EDUCATE`, `DISCRETE` | Stable as of Aug 2026; supplies no organizational maturity criteria; grades some named standards' coverage of a control |
| Third-party assurance schemes | ISO/IEC 42001 certification (Schellman, BSI, SGS, A-LIGN); [[aiuc-1\|AIUC-1]] (single ANAB auditor); SOC 2 + AI controls as a partial proxy | ISO audits available now; AIUC-1 population small |
| Evidence and crosswalk platforms | GRC suites (ServiceNow, OneTrust, AuditBoard); Microsoft Purview / Compliance Manager; the [[agentic-ai-security-cmm-crosswalk\|standards-crosswalk matrix]] | GA |
| Decision-rights and inventory | [[decision-rights\|Decision Rights for AI Agents]] matrices; agent / NHI inventory; shadow-agent reaper ([[shadow-automation\|Shadow Automation]]) | Pattern-level, not a product |

**The framework layer is stable, and none of it grades a program.** NIST has not shipped an agentic Govern profile (expected later 2026). The EU AI Act Art. 9 and Art. 17 anchors are duties and process rather than graded criteria, per [[standards-review-eu-ai-act-2026-Q2|the 2026-Q2 EU AI Act review]]. The CoSAI and SAIF anchors are category-level only, per [[standards-review-saif-cosai-2026-Q2|the 2026-Q2 SAIF/CoSAI review]]. ISO 42001's Annex A controls that anchor D1 — A.2 Policies, A.3 Internal organization, A.5 Impact assessment — are organizational rather than technical, and [[standards-review-iso-42001-27090-2026-Q2|the 2026-Q2 ISO/IEC 42001 + 27090 review]] confirms them against the public structure only, citation-only and paywall-bounded. The Exchange states that boundary outside the paywall: 42001 extends the risk management system and stops there, and does not discuss how to train models, how to do data lineage, continuous validation, versioning of AI models, project planning, or when sensitive data is used in engineering ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). The lifecycle half sits in ISO/IEC 5338 instead.

Outcome-based regulation states the result to be achieved, where control-focused regulation states the control to be operated, so an information security management system extended to AI needs assurance processes that demonstrate risks were sufficiently mitigated in place of an inventory showing controls exist ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/organize/`](https://owaspai.org/go/organize/)). That distinction sets what L4 and L5 evidence has to show. The Exchange states the sufficiency bar in a form a governance body can apply: an AI system is sufficiently secure when all identified risks can be treated, meaning transferred, avoided, or accepted, where acceptance sometimes follows directly and sometimes requires controls that bring the risk to an acceptable level ([`/go/riskanalysis/`](https://owaspai.org/go/riskanalysis/)). The four treatment options the same source names are mitigate, transfer, avoid, and accept. Its ISMS control `SEC PROGRAM` carries the same requirement into the security program, which must take the AI-specific assets and their threats onto its own books rather than treat AI as out of scope ([`/go/secprogram/`](https://owaspai.org/go/secprogram/)).

The Exchange supplies no organizational maturity criteria. It does grade standards. Each governance control lists the standards relevant to it, and against some of them records a verdict — "covers this control fully" or "covers this control minimally" — twelve such verdicts across the six controls in the governance group ([`/go/governancecontrols/`](https://owaspai.org/go/governancecontrols/)). Most of the standards each control lists carry no verdict at all. Each verdict judges how well a standard covers a control, and says nothing about how mature a program is, so the claim above stands that no framework in this layer supplies maturity criteria. The nearest thing to a ladder the Exchange offers is a starting order: a three-step bare minimum — inventory current AI use and ideas, run risk analysis to identify threats, controls and who owns them, then continue with GUARD step 2 — sitting inside an eight-step first iteration ([`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). It sequences the work and grades neither its sufficiency nor its coverage.

The AIUC-1 certified population is small; [[aiuc-1-critical-evaluation|the evaluation]] carries the count and its provenance. Purview governance is included in E5 entitlements, so the licensing delta on the evidence layer is near zero for an incumbent.

The platform-native column matters for single-stack buyers. An all-Microsoft enterprise reaches the L4 evidence layer (crosswalk, board metrics, risk register) on Purview and Compliance Manager already in its licensing, without a separate GRC purchase. The [[microsoft-zt4ai|Microsoft ZT4AI]] Governance pillar (verify explicitly) grounds this layer in named controls — the [[microsoft-rai|Responsible AI Standard]] and the Purview Compliance Manager AI templates — crosswalked to D1 in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]]. The RAI Standard states responsible-AI **goals** and leaves the control catalogue to ZT4AI: its Accountability and Transparency goals carry D1 at the goal level — A1 Impact Assessment, A2 Oversight of significant adverse impacts, A5 Human oversight and control, T1 System intelligibility, T2 Communication to stakeholders. The goal-level mapping is set out in [[standards-review-microsoft-rai-agent-365-2026-Q2|the 2026-Q2 RAI / Agent 365 review]], which separates RAI's goals from the ZT4AI control catalogue.

Cyera's Discover phase is a vendor example of the inventory half of the decision-rights and inventory row above: Cyera states it builds a live inventory of agents across cloud, SaaS, endpoint, and Shadow AI. Cyera states its Validate phase produces audit-ready regulatory evidence of the kind the platforms in the evidence and crosswalk row hold ([[cyera-agent-guardian-release|Cyera Agent Guardian Release]]). The release names no reaper and no decommission action, so the decision-rights and inventory row keeps its pattern-level status.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2.

- **L1 — Initial.** No accountable owner; agents deploy without governance review.
- **L2 — Developing.** A named accountable owner holds the role; an AI-use policy is published; an agent risk-tier scheme exists; a signed RACI assigns ownership, covering at least the responsibility types the Exchange gives as examples — model accountability, data accountability, and risk governance ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)).
- **L3 — Defined.** A standing cross-functional risk body gates deployment by risk tier; decision rights and prohibited autonomous actions are documented per agent type; shadow agents are inventoried and reaped on an SLA; each identified threat is allocated between the organization and every party supplying part of the system, internal departments included, with the residue recorded; and technical detail about the system is classified as an asset and reviewed before publication. Every criterion yields a document rather than a product, so the rung is graded off the register and the minutes.
- **L4 — Managed.** Quantitative governance metrics (incidents, drift events, escalations, with `ASI##` / [[owasp-aivss|AIVSS]] rollups) are reported at board level; the [[agentic-ai-security-cmm-crosswalk|standards-crosswalk matrix]] is maintained; a **readiness assessment is completed against a recognized third-party assurance scheme** — ISO/IEC 42001, AIUC-1, or a documented internal equivalent.
- **L5 — Optimizing (amended).** Current, independent, third-party assurance of the agentic-AI governance program, scheme-neutral: an [[iso-iec-42001|ISO/IEC 42001]] certification under active surveillance, an [[aiuc-1|AIUC-1]] certification at the latest quarterly refresh, or an independently reviewed internal equivalent satisfies it, alongside board-attested risk metrics and a decision history the risk body can show for a year.
- **L5+ — Leading Edge.** All of L5, plus an active named contribution to a governance or standards body (PR / RFC / spec authorship, not membership) and an externally published governance or risk-observability artifact.

The amendment removes a defect and keeps the requirement. The security bar is unchanged: independent assurance that the governance program does what it claims. What changes is that meeting it no longer depends on one vendor's product or audit cadence.

[[owasp-state-of-agentic-ai-security-governance|OWASP's State of Agentic AI Security and Governance]] supplies a parallel governance-maturity ladder (Levels 0–4, from unaware/ad-hoc through experimentation, policy-defined HITL, integrated continuous oversight, to adaptive self-regulation) that this domain's levels track closely. Its central finding is the load-bearing context for D1: organizations are deploying agents faster than they can govern them, and additional budget for existing programs does not close that gap. The report pairs the governance ladder with an Adoption Tier (AT0–AT8) so required governance scales with what is deployed rather than against a flat checklist — the same right-sizing logic the deployment-shape table below applies.

## Assessor detail per level

L1, L2, L4, and L5+ are graded from their statements above. The two rungs below carry criteria an assessor checks item by item, each list stating what its own rung adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable carries that condition beside itself. The lists below hold criteria only; a paragraph after a list carries maturity or market commentary and states no criterion.

### L3 detail

- **A standing risk body with a deployment gate.** A cross-functional AI risk body (security, legal, privacy, engineering) meets on a fixed cadence; risk tiers gate deployment.
- **Decision rights and operational boundaries documented per agent type.** A [[decision-rights|decision-rights]] matrix per agent type, covering every agent type in the inventory, alongside a documented set of operational boundaries — oversight tiers, prohibited autonomous actions, and data-handling rules — which the Exchange requires to be documented in governance and reflected in technical policy ([[owasp-ai-exchange|OWASP AI Exchange]], [`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)).
- **A shadow-agent inventory and reaper SLA in operation.**
- **A provider responsibility matrix.** A documented responsibility matrix allocates each identified threat between the organization and every party that supplies part of the system, with the unmitigated residue recorded as accepted, self-mitigated, or avoided ([`/go/riskanalysis/`](https://owaspai.org/go/riskanalysis/)). Those parties are the hosting, model, extension, and infrastructure providers, and the internal departments and teams that supply data, models, or fine-tuning artifacts. The Exchange places the internal half inside the same control: the supply chain can include the organization's own departments, since data and models come from different departments and sources, which puts data provenance inside supply-chain management (§3.0).[^aix-supplychainmanage] A corpus or model supplied by an internal team is therefore allocated on the same matrix as one acquired outside, and leaves the same residue record.
- **Technical detail carried as a classified asset, and publication reviewed.** Technical details of the AI system — its model type, its model implementation, and the technical content of material published about it — are carried as classified assets in the information-security asset inventory, and technical publication about the system passes a documented review setting what is withheld against the disclosure `AI TRANSPARENCY` asks for ([`/go/discrete/`](https://owaspai.org/go/discrete/)). The artifacts are the classification entry and the review record. The Exchange supplies a direction for that trade-off and no threshold, so the rung grades that the decision was taken and recorded rather than where the line was drawn.

### L5 detail

The assurance capability is scheme-neutral, and any one of three schemes satisfies it.

- **(a) [[iso-iec-42001|ISO/IEC 42001]]** — a current certification under active surveillance. *Preferred*, because it is standards-body-governed, multi-auditor, and regulator-recognized.
- **(b) [[aiuc-1|AIUC-1]]** — a current certification at the latest quarterly refresh. Accepted, with the concentration and freshness caveats the [[aiuc-1-critical-evaluation|evaluation]] documents.
- **(c) Internal equivalent** — a documented internal-equivalent governance attestation, independently reviewed by a qualified third party and crosswalk-mapped to a recognized framework.
- **Standing evidence alongside the scheme.** Board-attested risk metrics, an AI risk body decision history of at least one year, and a crosswalk refreshed each quarter.

## Right-sizing by deployment shape

The realistic target per [[agentic-ai-security-cmm-recalibration-method-2026|rule 4]]:

| Deployment shape | Realistic D1 target | Why |
|---|---|---|
| Internal RAG / support chatbot (no tools) | L2 → L3 | Owner, policy, risk body, decision-rights. Certification is not warranted near-term |
| Data-science / coding copilot | L3 → L4 | Adds board metrics and crosswalk once the agent touches the SDLC |
| MCP / skill provider serving others | L4 | Third-party exposure raises the accountability bar; readiness assessment expected |
| High-autonomy multi-agent mesh | L4 → selective L5 | Certification earns its cost where autonomy and blast radius are highest |

> [!check] The coding row now has a governance object the criteria do not name
> D1 asks who is accountable for agent behavior on what auditable record. For agentic coding the record includes the **harness configuration tree** — hooks, MCP manifests, subagents, skills, and instruction files — which composes runtime behavior from third-party parts and is usually held at per-developer discretion rather than under organizational policy. An L3 claim for this shape should evidence managed policy that local configuration cannot override, plus configuration held under review. Adoption outruns the inventory that governance assumes: peer proximity predicts uptake at odds ratios no enrollment process matches ([[microsoft-cli-coding-agent-adoption-study|Microsoft, 2026]]), which makes [[shadow-automation|shadow automation]] the expected default state rather than a failure. See [[securing-agentic-coding|Securing Agentic Coding]].

Most enterprises land at **L4 with selective L5** in the domains tied to their exposure. A contained, low-agency design that legitimately needs less governance records the choice as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field.

## Cost model

Governance is labor-heavy, and for an incumbent the licensing line is the smallest of the three cost lines below.

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 (policy authored in-house) | ~0.25–0.5 FTE to write policy, stand up the RACI and risk-tier scheme | — |
| L3 | ~0 | ~0.5–1 FTE recurring: risk-body cadence, decision-rights upkeep, shadow-agent reaping | — |
| L4 | GRC platform where used; ~0 on Purview for an E5 tenant | board reporting + crosswalk upkeep; a one-time readiness assessment | — |
| L5 (ISO 42001 path) | certification audit + optional consulting | the dominant cost: internal hours to build and sustain the management system, plus annual surveillance | — |
| L5 (AIUC-1 path) | certification fee (not publicly listed) | heavier recurring labor than ISO's annual surveillance, because the certificate is re-tested on a quarterly cadence | — |

**The spend is governance labor and the certification-evidence treadmill.** Tools are the small line. A buyer choosing the AIUC-1 path inherits a quarterly re-test cycle; the ISO 42001 path runs on annual surveillance. Both are recurring labor. Price the recurring rhythm as well as the first audit.

## Customer critiques folded in

- *"L5 is unreachable because it names a just-GA'd certification we can't procure in time."* Addressed: L5 is scheme-neutral and the production-maturity qualifier applies. Assurance "via a scheme in your approved-vendor pipeline with a documented production date" satisfies it.
- *"A single-certifier mandate is a concentration risk and reads as a vendor play."* Addressed: AIUC-1 is demoted to one option among several, with ISO 42001 preferred. Full reasoning in [[aiuc-1-critical-evaluation|the evaluation]].
- *"Cost is under-told."* Addressed: the cost model names labor and the evidence treadmill as the real spend, and marks licensing as near-zero for incumbents.
- *"Authority to certify is itself contested."* Acknowledged but out of scope here. The wiki records the assurance-scheme landscape; an organization preparing for a regulator should map D1 to its examiner's expectations via the forthcoming FFIEC/GLBA crosswalk.

## Open questions

- No official AIUC-1 certified-organization count is published; the figure in [[aiuc-1-critical-evaluation|the evaluation]] is reconstructed from press releases and should be re-checked each quarter.
- AIUC-1 pricing is not public, so the L5 AIUC-1 cost line is directional.
- The exact 2026 ISO/IEC 42001 certified population is not cleanly published.
- NIST has not shipped an agentic-specific Govern profile; the EU AI Act's agentic provisions remain preliminary, so D1's regulatory anchors will shift as both land.
- The inventory the L3 disclosure clause builds on is graded only in part. [[agentic-ai-security-cmm-d8-supply-chain|D8]] L2 inventories model and development documentation, experiments included, and access-controls it as an asset class in its own right, so the documentation half of the Exchange's twelve-item AI asset list — recorded in full on [[security-controls-for-ai-stacks|the controls thesis]] — is graded there. Two objects stay outside every rung: material the organization publishes about the system, which is created for disclosure rather than acquired or produced as a runtime artifact, and the model-type and model-implementation choice `DISCRETE` names, which is a selection decision taken before an artifact exists. The clause carries those two entries itself.
- The disclosure bound is stated per property. [[agentic-ai-security-cmm-d9-operations|D9]] L3 requires a published disclosure that informs users an AI model is involved and covers or records an omission against each of the five properties `AI TRANSPARENCY` lists, so a review here may not silently drop a property. How much detail each property carries is unbounded: the Exchange supplies a direction and no threshold, and no rung in this CMM states where the line falls.

## Notes

<!-- Footnote definitions: deep links to the page carrying each figure. -->

[^aix-supplychainmanage]: [OWASP AI Exchange — SUPPLY CHAIN MANAGE](https://owaspai.org/go/supplychainmanage/), retrieved 2026-08-20. The statement that the supply chain may include the own organization instead of just third parties, with data and models coming from different departments and sources, which makes data provenance part of the control.
