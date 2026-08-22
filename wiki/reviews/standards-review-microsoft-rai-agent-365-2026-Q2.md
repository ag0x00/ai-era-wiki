---
type: review
title: "Microsoft RAI and Agent 365 Standards Review"
address: c-000230
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - microsoft
  - responsible-ai
  - agent-365
  - agentic-ai
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[microsoft-rai]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
related:
  - "[[microsoft-rai]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[standards-review-backlog]]"
sources:
  - "https://www.microsoft.com/en-us/ai/responsible-ai"
  - "https://www.microsoft.com/en-us/ai/principles-and-approach"
  - "https://www.microsoft.com/en-us/microsoft-365/blog/2025/11/18/microsoft-agent-365-the-control-plane-for-ai-agents/"
  - "https://learn.microsoft.com/en-us/microsoft-agent-365/overview"
  - "https://news.microsoft.com/ignite-2025-book-of-news/"
---

# Standards Review — Microsoft RAI and Agent 365, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to two Microsoft instruments the wiki had filed under a single framework page: the **Responsible AI Standard v2** (the published goals-and-requirements standard) and the **Agent 365** agent-management control plane (announced at Ignite 2025, GA 2026-05-01). It produces a goal- and capability-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains, falsifiable absence claims, and an adversarial second pass.

The review draws a hard scope boundary with the completed [[standards-review-microsoft-zt4ai-2026-Q2|ZT4AI review]]. ZT4AI is Microsoft's Zero-Trust control catalogue (Prompt Shields, Purview answer-time entitlement, Defender runtime protection, the Agent Governance Toolkit, and so on). That catalogue is **not re-reviewed here**. This review covers only (1) the RAI Standard as a responsible-AI goals document and (2) Agent 365 as a product control plane (registry, identity, access, observability). Where the two stacks share a control — Entra Agent ID, Purview, Defender — the control's *catalogue* membership stays in the ZT4AI review; this review records only its role in the Agent 365 management plane.

**RAI v2 is a responsible-AI goals standard, not a security-control catalogue.** Its seventeen goals (Accountability A1-A5, Transparency T1-T3, Fairness F1-F3, Reliability & Safety RS1-RS3, Privacy & Security PS1)[^rai] specify *what outcomes a team must reach* — an impact assessment, a named owner, a disclosure to users — not *which technical control enforces them*. It anchors the CMM's governance and accountability domain (D1) and contributes to human oversight (D9), and almost nothing else. Agent 365 is the inverse: a product control plane with real D2/D7 substance, but vendor-specific and product-bounded, not a portable standard.

## Primary documents reviewed

Neither instrument is a single security specification. RAI v2 is a published PDF of general requirements; Agent 365 is documented across an Ignite 2025 announcement and Microsoft Learn product pages. None is paywalled; none has a local archived copy (HTML/PDF sources).

| Document | URL | Date | Scope used in this review |
|---|---|---|---|
| Responsible AI Standard v2 — General Requirements | [Microsoft](https://www.microsoft.com/en-us/ai/responsible-ai) | 2022-06 (v2) | The six principles and the seventeen goals (A/T/F/RS/PS series) |
| Responsible AI principles and approach | [Microsoft AI](https://www.microsoft.com/en-us/ai/principles-and-approach) | current | The six principles, including Inclusiveness |
| Agent 365 — the control plane for AI agents | [Microsoft 365 Blog](https://www.microsoft.com/en-us/microsoft-365/blog/2025/11/18/microsoft-agent-365-the-control-plane-for-ai-agents/) | 2025-11-18 (Ignite) | Five named capabilities: Registry, Access Control, Visualization, Interoperability, Security |
| Agent 365 overview | [Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-agent-365/overview) | ms.date 2026-06-02 | The observe / govern / secure framing; Agent registry, Registry sync, Agent Map; lifecycle, access, compliance |
| Ignite 2025 Book of News | [Microsoft News](https://news.microsoft.com/ignite-2025-book-of-news/) | 2025-11-18 | Agent 365 launch positioning |

> [!contradiction] The framework page conflates RAI, ZT4AI, and Agent 365 under one title
> The [[microsoft-rai|RAI framework page]] frontmatter aliases "Microsoft RAI", "Zero Trust for AI", "ZT4AI", and "Agent 365" to one page and states RAI is "the most control-rich AI security framework available — 700+ specific controls". That description belongs to ZT4AI (and even there the 700-control figure is the whole Zero-Trust Workshop, not the AI pillar — see [[standards-review-microsoft-zt4ai-2026-Q2|the ZT4AI review]]). RAI v2 itself defines seventeen *goals*, not 700 controls, and is a responsible-AI standard, not a security-control catalogue. The page also dates Agent 365 to "March 9, 2026"; the public launch was **Ignite 2025 (2025-11-18)** with GA 2026-05-01.

## Structure-to-domain grounding

The two instruments map onto the CMM in nearly opposite shapes.

- **RAI v2 — governance-heavy (D1), thin elsewhere.** The Accountability goals (A1 Impact Assessment, A2 Oversight of significant adverse impacts, A3 Fit for purpose, A4 Data governance and management, A5 Human oversight and control) and the Transparency goals (T1-T3) are the load-bearing content, and they land almost entirely in D1, with A5 and the human-oversight requirements touching D9. Reliability & Safety (RS1-RS3) and Privacy & Security (PS1) are outcome statements that defer the *how* to Microsoft's internal Security Development Lifecycle and Privacy Standard; they do not specify agent-runtime, identity, egress, or data controls. RAI has no content for D2, D3, D5, D7, or D8 as technical control families.
- **Agent 365 — management-plane shape (D1 inventory, D2 identity, D7 observability).** Its `observe` capability (Agent registry, Registry sync, Agent Map) is an agent-inventory and telemetry layer (D1/D7); `govern` is lifecycle and access control routed through Entra and Purview (D2/D9); `secure` extends Entra risk-based access (D2), Purview information protection / DLP (D6), and Defender runtime protection (D4/D7). The underlying enforcement controls are the same ZT4AI controls already reviewed; Agent 365's own contribution is the *aggregation, registry, and admin surface* over them.

## Goal- and capability-level coverage matrix (CMM x Standard)

Each row cites the verified RAI goal identifiers and the verified Agent 365 capability names. RAI columns cite goals; Agent 365 columns cite named capabilities and the underlying stack component, with the control catalogue itself deferred to the ZT4AI review.

| CMM domain | RAI v2 goals | Agent 365 capability | Coverage |
|---|---|---|---|
| D1 Governance & Accountability | A1 Impact Assessment; A2 Oversight of significant adverse impacts; A3 Fit for purpose; A4 Data governance and management; T1 System intelligibility; T2 Communication to stakeholders | Agent registry (single source of truth / inventory); govern = centralized lifecycle, access, and compliance via the M365 admin center + Purview | Strong (RAI sets the goals; Agent 365 supplies the inventory + admin surface) |
| D2 Identity & Authorization | none (PS1 defers to the Privacy Standard; no identity goal) | Access Control via per-agent [[microsoft-entra-agent-id\|Entra Agent ID]]; Policy Templates; risk-based Conditional Access | Partial (Agent 365 only; identity controls are ZT4AI-catalogued) |
| D3 Control & Least-Agency | A5 Human oversight and control (goal-level only) | Policy Templates; access scoping at the agent identity | Thin — A5 states the outcome; Agent 365 routes to ZT4AI least-action controls |
| D4 Runtime & Guardrails | RS1 Reliability and safety; RS2 Failures and remediations (outcome statements) | `secure` = Defender real-time runtime protection (catalogued in ZT4AI) | RAI sets safety outcomes; runtime control is product/ZT4AI |
| D5 Egress & Network | none | none in Agent 365's named capabilities | None in scope |
| D6 Data, Memory & RAG | A4 Data governance and management; PS1 Privacy Standard compliance (goal-level) | `secure` = Purview information protection, DLP, data-risk safeguards (catalogued in ZT4AI) | RAI sets data-governance goal; Purview enforcement is ZT4AI |
| D7 Observability & Detection | RS3 Ongoing monitoring, feedback, and evaluation | `observe` = Agent registry, Registry sync, Agent Map; Visualization (unified dashboard, agent/user/resource map); telemetry, dashboards, alerts | Moderate — Agent 365's strongest native layer; RS3 sets the monitoring goal |
| D8 Supply Chain & AI-BOM | A4 Data governance and management (training-data provenance, partial) | Interoperability surface (Foundry, open-source frameworks, partner clouds); registry sync inventories cross-cloud agents | Thin — inventory reach, not BOM verification |
| D9 Operations & Human Factors | A5 Human oversight and control; A2 Oversight of significant adverse impacts | Lifecycle management (create / review / decommission) via registry + Entra ID Governance sponsors | Moderate — A5 + sponsor model; no human-factors instrumentation |

The matrix confirms the assessment: RAI supplies the *governance goal set* (D1, with D9 oversight) and Agent 365 supplies the *management plane* (D1 inventory, D2 identity surface, D7 observability). Neither adds a new enforcement control beyond the ZT4AI catalogue; Agent 365's distinct value is the registry and admin aggregation over controls reviewed elsewhere.

## Falsifiable absence claims found

What the CMM scores that RAI v2 or Agent 365, as documented in scope, does not provide. Each is bounded to the searched documents and reversible by the stated refuting evidence.

1. **RAI v2 specifies no technical security controls — only outcome goals.** The seventeen goals state required outcomes (impact assessment completed, AI interaction disclosed, failures remediated) without naming an enforcement mechanism, a configuration, a pass/fail criterion, or a test procedure. *Searched:* the RAI v2 General Requirements goals (A1-A5, T1-T3, F1-F3, RS1-RS3, PS1). *Terms:* "control", "configuration", "enforce", "token", "policy engine", "threshold", "test procedure". *Verdict:* confirmed — RAI is a goals-and-requirements standard, deferring technical enforcement to the SDL, the Privacy Standard, and (in 2026) ZT4AI / Agent 365. *Refuting evidence:* an RAI v2 goal that names a specific technical control with acceptance criteria. *Reviewed 2026-06-22.* This is why the CMM cites RAI only for D1 goals, not for any technical domain.

2. **RAI v2 has no agent-identity, least-agency, or runtime goal.** No goal addresses non-human / agent identity, per-task authorization scoping, deny-by-default action control, or runtime tool-call guardrails. *Searched:* all seventeen goals. *Terms:* "agent identity", "non-human", "least privilege", "tool", "runtime", "authorization scope". *Verdict:* not addressed — A5 (Human oversight and control) is the closest, and it governs human control over the system, not machine-identity or agent-agency controls. *Refuting evidence:* an RAI goal naming agent identity or agent-agency control. *Reviewed 2026-06-22.* The CMM fills D2/D3/D4 from Agent 365 / ZT4AI and other instruments, not from RAI.

3. **Agent 365 is a vendor-bound control plane, not a portable standard.** Its capabilities (Registry, Access Control, Visualization, Interoperability, Security) are product features of a per-user-licensed Microsoft offering that "works best with Microsoft E5", not a specification an organization can implement on another stack. *Searched:* the Agent 365 overview, the Ignite 2025 blog. *Terms:* "specification", "conformance", "implement on", "portable", "standard". *Verdict:* confirmed — Agent 365 is documented as a product, with prerequisites and per-user licensing, not as a published standard with conformance criteria. *Refuting evidence:* a published Agent 365 conformance specification implementable without Microsoft licensing. *Reviewed 2026-06-22.* The CMM may cite Agent 365 as a *shipping implementation* of a capability, never as the standard that defines the level.

4. **Agent 365's `observe` layer inventories and visualizes but does not verify supply-chain integrity (AI-BOM).** The registry, Registry sync, and Agent Map enumerate agents (including cross-cloud via Bedrock / GCP sync) but do not produce a verified AI bill of materials or attest model / tool artifact integrity. *Searched:* the Agent 365 overview and Ignite blog. *Terms:* "AI-BOM", "bill of materials", "artifact integrity", "provenance", "attestation". *Verdict:* not addressed in Agent 365's own capabilities — AI-BOM discovery is a Defender for Cloud AI-SPM control catalogued in [[standards-review-microsoft-zt4ai-2026-Q2|the ZT4AI review]] (D8), distinct from Agent 365 inventory. *Refuting evidence:* an Agent 365 capability that emits or verifies an AI-BOM rather than an agent inventory. *Reviewed 2026-06-22.* The CMM's D8 anchors on Defender AI-SPM, not Agent 365.

## Scope exclusions

- **The ZT4AI control catalogue.** Prompt Shields, Groundedness / Task Adherence, Purview answer-time entitlement, Defender runtime protection, the Agent Governance Toolkit, Entra Conditional Access internals, ID Protection, and AI-SPM are reviewed in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]] and are not re-graded here. Where Agent 365 surfaces these controls, this review records only the management-plane role.
- **The RAI Impact Assessment template and the RAI v2 tooling.** The goals were enumerated; the per-goal requirement text and the Impact Assessment Guide template were not clause-mapped beyond the goal level.
- **Inclusiveness as a lettered goal series.** RAI v2's General Requirements carries goal series for five of the six principles (A/T/F/RS/PS); Inclusiveness is one of the six published principles but is operationalized through accessibility and fairness requirements rather than a distinct lettered goal series. The principle is in scope; an Inclusiveness goal-ID series is not claimed because the source does not publish one.
- **Production effectiveness.** This is a document-versus-product review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second reviewer (separate run) attempted a counter-example for each absence claim against current Microsoft sources through June 2026. **All four claims survived; no counter-example met the bar.**

1. **RAI specifies no technical controls — survives.** The RAI v2 goals are consistently outcome statements; the General Requirements defer technical enforcement to the SDL and the Privacy Standard, and Microsoft's 2026 control substance lives in ZT4AI / Agent 365, not in RAI. No goal names a control with acceptance criteria.
2. **No agent-identity / agency / runtime goal — survives.** A5 (Human oversight and control) governs human control over an AI system; it is not a machine-identity or agent-agency control. The 2022 v2 document predates Microsoft's agent-identity work (Entra Agent ID, 2026), so no agent goal exists in it.
3. **Agent 365 is vendor-bound, not a standard — survives.** The Learn overview states Agent 365 "works best when using Microsoft E5 as a prerequisite" and is "licensed on a per-user basis"; the Ignite framing is a product launch, not a specification.
4. **`observe` inventories but does not verify AI-BOM — survives.** Registry sync extends inventory to cross-cloud agents but the documentation describes discovery and lifecycle action, not artifact attestation or BOM emission. AI-BOM remains a Defender AI-SPM capability (ZT4AI / D8), distinct from the Agent 365 registry.

## Effect on existing wiki pages

- **[[microsoft-rai|RAI framework page]]:** the title and aliases were corrected to separate the RAI Standard (a responsible-AI goals document) from ZT4AI and Agent 365; the "most control-rich … 700+ controls" claim and the "March 9, 2026" Agent 365 date were corrected — RAI v2 defines seventeen goals (A1-A5, T1-T3, F1-F3, RS1-RS3, PS1), and Agent 365 launched at Ignite 2025 (2025-11-18, GA 2026-05-01); the page now points the control-catalogue content to [[standards-review-microsoft-zt4ai-2026-Q2|the ZT4AI review]] and the goals to this review.
- **[[microsoft-entra-agent-id|Agent 365 / Entra Agent ID product page]]:** the five Agent 365 capabilities (Registry, Access Control, Visualization, Interoperability, Security) and the Learn observe/govern/secure framing were reconciled against the verified sources; the Ignite 2025 launch date was added alongside the GA date.
- **[[agentic-ai-security-cmm-d1-governance|CMM D1]]:** the RAI citation was sharpened to name the Accountability and Transparency goals (A1 Impact Assessment, A2 Oversight, A5 Human oversight and control) it grounds, and to cite this review for the goal-level mapping.
- **[[agentic-ai-security-cmm-d2-identity|CMM D2]]:** the Agent 365 anchor was reframed as a *management plane* (registry + admin surface) over ZT4AI-catalogued controls.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** an RAI column was added citing the goal series, separate from the ZT4AI control column, so the goals-standard versus control-catalogue distinction is visible at a glance.
- **[[standards-review-backlog|Standards Review Backlog]]:** the Microsoft RAI / Agent 365 entry flips to done; the ZT4AI entry is unchanged.

## Notes

[^rai]: Microsoft Responsible AI Standard v2 — General Requirements (June 2022), goals enumerated across the six principles: Accountability A1 Impact Assessment, A2 Oversight of significant adverse impacts, A3 Fit for purpose, A4 Data governance and management, A5 Human oversight and control; Transparency T1 System intelligibility for decision making, T2 Communication to stakeholders, T3 Disclosure of AI interaction; Fairness F1 Quality of service, F2 Allocation of resources and opportunities, F3 Minimization of stereotyping, demeaning, and erasing outputs; Reliability & Safety RS1 Reliability and safety guidance, RS2 Failures and remediations, RS3 Ongoing monitoring, feedback, and evaluation; Privacy & Security PS1 Privacy Standard compliance. Inclusiveness is one of the six published principles, operationalized through accessibility/fairness requirements rather than a distinct lettered goal series. Retrieved 2026-06-22 from [Microsoft Responsible AI](https://www.microsoft.com/en-us/ai/responsible-ai) and [Responsible AI principles and approach](https://www.microsoft.com/en-us/ai/principles-and-approach).
