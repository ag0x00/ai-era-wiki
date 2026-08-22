---
type: review
title: "ISO/IEC 42001 and 27090 Standards Review"
address: c-000229
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - iso
  - iso-42001
  - iso-27090
  - governance
  - ai-management-system
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[iso-iec-42001]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
review_constraint: "citation-only — primary normative text is paywalled and was not read"
related:
  - "[[iso-iec-42001]]"
  - "[[iso]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-validation-methodology-2026-05]]"
  - "[[standards-review-backlog]]"
  - "[[eu-ai-act]]"
  - "[[nist-ai-rmf]]"
sources:
  - "https://www.iso.org/standard/42001.html"
  - "https://www.iso.org/standard/56581.html"
  - "https://www.isms.online/iso-42001/annex-a-controls/"
  - "https://cyberzoni.com/standards/iso-42001/"
  - "https://www.iso27001security.com/post/ai-security-standard-at-fdis"
---

# ISO/IEC 42001 and 27090 Standards Review

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to ISO/IEC 42001:2023 (AI Management System) and ISO/IEC 27090 (guidance on addressing security threats to AI systems). Both documents are paywalled. Under §8 of the methodology, which permits citation-only degradation for paywalled standards, this review maps the CMM against **public structural metadata only**: the official ISO abstract pages, the published Annex A control objective list, and clearly-attributed reputable secondary summaries. The full normative clause text and the verbatim control statements were **not read**. Every clause and control identifier below is verified against public ISO materials or attributed secondary summaries; identifiers that rest on summary sources rather than primary text are marked as such.

**ISO/IEC 42001 is a management-system standard — process governance on a Plan-Do-Check-Act spine — not a technical-control catalogue.** Its Annex A controls are organizational (policy, roles, impact assessment, lifecycle process, data governance, third-party allocation); none specifies an agentic, least-agency, or non-human-identity technical control, and none carries a testable acceptance criterion. ISO/IEC 27090 is the threat-guidance companion and remained at FDIS stage, not yet published, as of June 2026. The confidence of this review is bounded by the paywall and says so throughout.

## Primary documents reviewed

| Document | Identifier / stage | URL | Public or paywalled | Scope used in this review |
|---|---|---|---|---|
| ISO/IEC 42001:2023 — AI management system | Published 2023-12 (1st ed.) | [iso.org/standard/42001](https://www.iso.org/standard/42001.html) | **Paywalled** — abstract + ToC metadata public; normative text not read | Management-system clauses 4-10 (titles only); Annex A control-objective list A.2-A.10; presence of Annexes B/C/D |
| ISO/IEC 27090 — Addressing security threats to AI systems | **FDIS** (registered 2026-03-12); not yet published as of 2026-06 | [iso.org/standard/56581](https://www.iso.org/standard/56581.html) | **Paywalled** — status + title public; draft text not read | Title, stage, and threat-category scope from official metadata and secondary summaries only |
| Annex A control list (secondary) | A.2-A.10, ~38 controls | [isms.online](https://www.isms.online/iso-42001/annex-a-controls/), [cyberzoni.com](https://cyberzoni.com/standards/iso-42001/) | Public summaries | Control IDs and titles below are **summary-sourced**, not read from the primary Annex |
| ISO/IEC 27090 FDIS status (secondary) | FDIS, mid-2026 publication expected | [iso27001security.com](https://www.iso27001security.com/post/ai-security-standard-at-fdis) | Public summary | Threat-category enumeration is **summary-sourced** |

> [!gap] Paywall constraint — bounded confidence
> No `.raw/` archive exists for either document because both are paywalled; the methodology's Step 1 (`archived_copy`) cannot be satisfied. The `scope_in_wiki` for ISO 42001 is therefore restricted to **public structural metadata** (clause titles, Annex A control-objective IDs/titles, annex inventory). Absence claims in this review are bounded to *what the public structure can show* — the *categories of control the standard contains* — and explicitly cannot rest on the unread normative text. Where a control's title plausibly subsumes a CMM concern, that is recorded as "plausible coverage, text not verified," never as confirmed coverage.

## Structure-to-domain grounding

ISO/IEC 42001 uses the ISO Harmonized Structure, identical in shape to ISO 27001 and ISO 9001. This is what makes it a *management-system* standard rather than a control catalogue.

- **Management clauses 4-10 (the conformance spine):** 4 Context, 5 Leadership, 6 Planning, 7 Support, 8 Operation, 9 Performance evaluation, 10 Improvement. These are PDCA process requirements (clause titles verified from public ToC summaries). They define *how* an organization runs an AI Management System (AIMS), not *which* technical defenses it deploys.
- **Annex A (normative reference controls, A.2-A.10):** ~38 controls organized under nine control objectives. Annex A is a *selectable* reference set; an organization documents inclusions/exclusions in a Statement of Applicability against its AI risk assessment. The controls are organizational and process-oriented.
- **Annex B:** implementation guidance for the Annex A controls. **Annex C:** potential AI-related organizational objectives and risk sources. **Annex D:** guidance on applying the AIMS across domains/sectors. (Annex inventory from public ToC summaries; contents not read.)

The mapping onto the CMM is asymmetric in the opposite direction from MITRE ATLAS: ATLAS supplies the *threat surface* with no graded program; ISO 42001 supplies the *governance program* with no technical-control specificity. It anchors **D1 (Governance & Accountability)** strongly and contributes process scaffolding to D6, D8, and D9; it has effectively no agentic-technical content for D2-D5 or D7.

> [!contradiction] The existing crosswalk uses a non-public Annex A numbering
> [[agentic-ai-security-cmm-crosswalk|The CMM crosswalk]] maps controls labeled `A.5.* Leadership and policies`, `A.6.* Planning`, `A.7.1 Resources … A.7.9 Third-party`. That numbering does **not** match the published Annex A objective scheme (A.2 Policies, A.3 Internal organization, A.4 Resources, A.5 Impact assessment, A.6 AI system life cycle, A.7 Data, A.8 Information for interested parties, A.9 Use, A.10 Third-party relationships). The crosswalk appears to have mapped the *management-system clauses 5-10* under an `A.` prefix, conflating clauses with Annex A controls. This is corrected in "Effect on existing wiki pages" below. All control IDs in the matrix below use the public Annex A objective scheme.

## Coverage matrix (CMM x ISO/IEC 42001 + 27090)

Each row cites the public Annex A control objectives/controls that bear on a CMM domain. Control IDs and titles are **summary-sourced** (isms.online, cyberzoni.com) and **not verified against primary normative text** — treat them as the public skeleton, not as read clauses. "Plausible" means a control title appears to subsume the concern but the normative text was not read to confirm scope. ISO 27090 cells are marked FDIS/unpublished.

| CMM domain | ISO 42001 Annex A (public IDs, summary-sourced) | ISO 42001 clauses 4-10 | ISO 27090 (FDIS, unpublished) | Coverage |
|---|---|---|---|---|
| D1 Governance & Accountability | A.2 Policies; A.3 Internal organization (roles & responsibilities); A.5 AI-system impact assessment | Cl. 5 Leadership; Cl. 6 Planning (risk + impact assessment); Cl. 9 Performance evaluation; Cl. 10 Improvement | n/a | **Strong (organizational).** Best-anchored domain. PDCA governance is the standard's core. No agentic-specific governance; testable criteria absent. |
| D2 Identity & Authorization | none specific (A.3 allocates roles to *people*, not non-human/agent identity) | Cl. 7.2 Competence (human) | identity/access threats (FDIS, text not read) | **Near-absent.** No NHI, agent-identity, credential, or authorization control. |
| D3 Control & Least-Agency | A.9 Responsible/intended use (process framing) | Cl. 8 Operation | n/a confirmed | **Absent (technical).** No least-agency tier, no human-in-the-loop gate specification, no autonomy-bounding control. |
| D4 Runtime & Guardrails | A.6 AI-system life cycle (operation/monitoring, plausible, generic) | Cl. 8 Operation | evasion / prompt injection (FDIS, text not read) | **Absent (technical).** Plausibly subsumes operational monitoring but specifies no guardrail, no prompt-injection defense. |
| D5 Egress & Network | none specific | none specific | inter-system threats (FDIS, text not read) | **Absent.** No egress-filtering, network-segmentation, or data-exfiltration control in public Annex A. |
| D6 Data, Memory & RAG | A.7 Data (data for development, acquisition, quality, provenance, preparation) | Cl. 8 Operation | data poisoning (FDIS, text not read) | **Partial (data governance).** Provenance/quality controls anchor RAG data hygiene at a *process* level; no memory-poisoning, no RAG-injection control. |
| D7 Observability & Detection | A.6 (event-log recording, plausible); A.8 Information for interested parties (incident communication) | Cl. 9.1 Monitoring, measurement, analysis & evaluation; Cl. 9.2 Internal audit | n/a confirmed | **Partial (process).** Event-logging and incident-communication exist as organizational obligations; no detection rule, no telemetry schema. |
| D8 Supply Chain & AI-BOM | A.4 Resources (data/tooling resources); A.10 Third-party relationships (suppliers, customers) | Cl. 6 Planning (scope of AIMS) | model extraction / supply threats (FDIS, text not read) | **Partial (third-party governance).** Supplier/customer responsibility allocation exists; **no AI-BOM requirement**, no artifact-signing. |
| D9 Operations & Human Factors | A.4 Resources (human resources); A.6 AI-system life cycle (deployment, documentation); A.8 Information for interested parties | Cl. 7.3 Awareness; Cl. 10 Improvement (corrective action) | n/a confirmed | **Partial (process).** Deployment, documentation, awareness exist as obligations; no decommission/retirement control, no rotation cadence. |

The matrix confirms the framework page's standing assessment: ISO 42001 is a governance instrument that delegates technical security to ISO 27001 (which has no AI-specific controls), so it grounds the CMM's **D1** and contributes process scaffolding to **D6/D8/D9**, but supplies no graded technical defense for the agentic-specific domains (D2-D5, D7). ISO 27090, once published, is the intended technical-threat companion — but it is guidance-only (no "shall" conformance, no certification) and remained unpublished at review time.

## Falsifiable absence claims found

Each claim is bounded to the **public structure** that was actually readable. Because the normative text is paywalled, every claim's refuting evidence is "a control or clause *in the public Annex A objective list or public ToC* — or, once published, in ISO 27090 — that names the missing item." A reviewer with paid access to the full normative text could refute a claim by quoting clause body text; this review cannot, and the claims are scoped accordingly.

1. **No agentic / non-human-identity (NHI) technical control.** The public Annex A objective list contains no control naming agent identity, non-human identity, credential management for agents, or machine-to-machine authorization. *Searched:* public Annex A objective list A.2-A.10 (isms.online, cyberzoni.com); public clause titles 4-10. *Terms:* "agent", "non-human identity", "machine identity", "credential", "authorization", "least privilege". *Verdict:* not addressed in public structure. A.3 allocates roles to **people**. *Refuting evidence:* a control in the full Annex A normative text, or in ISO 27090 once published, naming agent/NHI identity controls. *Confidence bounded by paywall — normative text not read. Reviewed 2026-06-22.*

2. **No testable / measurable technical acceptance criterion.** Annex A controls are organizational obligations selected via a Statement of Applicability; the public structure shows no control with a pass/fail threshold, metric, or test procedure for a technical defense. *Searched:* public Annex A objective list; public clause titles. *Terms:* "measure", "threshold", "criteria", "test", "shall", "metric". *Verdict:* not addressed in public structure — consistent with management-system standards generally, where Annex A is a reference control set, not a conformance test catalogue. *Refuting evidence:* a verbatim Annex A control (paid text) carrying a measurable technical acceptance criterion. *Confidence bounded by paywall. Reviewed 2026-06-22.*

3. **No AI-BOM / artifact-provenance-verification control.** The public Annex A objective list addresses data provenance (A.7) and supplier responsibility (A.10) but names no AI Bill of Materials, model/artifact signing, or pre-deployment integrity-verification control. *Searched:* public Annex A objective list A.4, A.7, A.10. *Terms:* "bill of materials", "AI-BOM", "signing", "checksum", "artifact integrity", "model provenance". *Verdict:* not addressed in public structure. A.7 covers *data* provenance, not model/tool-artifact provenance. *Refuting evidence:* an Annex A control (paid text) requiring an AI-BOM or model-artifact signing/verification. *Confidence bounded by paywall. Reviewed 2026-06-22.*

4. **No decommission / model-version-lifecycle control.** The public A.6 AI-system life-cycle objective enumerates design, development, verification, deployment, operation, documentation but no retirement, decommission, model-version end-of-life, or version-pin control. *Searched:* public A.6 life-cycle control list. *Terms:* "decommission", "retire", "end-of-life", "version pin", "deprecat", "rotation". *Verdict:* not addressed in public structure. *Refuting evidence:* an A.6.x control (paid text) on decommissioning or model-version lifecycle. *Confidence bounded by paywall. Consistent with the same gap recorded in [[agentic-cmm-vs-standards-validation|the first-pass validation]]. Reviewed 2026-06-22.*

5. **ISO 27090 provides no certifiable conformance and was unpublished at review time.** ISO/IEC 27090 is guidance ("addressing security threats … to AI systems"), generating no presumption of conformity and no certification; as of 2026-06 it stood at **FDIS** (registered 2026-03-12), not published. *Searched:* official ISO catalogue metadata ([iso.org/standard/56581](https://www.iso.org/standard/56581.html)); secondary FDIS-status summaries. *Terms:* "published", "FDIS", "certifiable", "shall", "guidance". *Verdict:* confirmed guidance-only and unpublished. *Refuting evidence:* an ISO catalogue status of "Published" (stage 60.60) for 27090, or normative "shall" conformance language in the released text. *Reviewed 2026-06-22.*

## Scope exclusions

- **The full normative clause text of ISO/IEC 42001:2023.** Paywalled; not purchased, not read. All clause and control references are from public ToC metadata and attributed secondary summaries. Any claim about what a control *requires in detail* is out of scope — only what its public *title and objective* indicate is in scope.
- **The verbatim Annex A control statements and Annex B implementation guidance.** Control IDs and titles are summary-sourced; Annex B was not read.
- **ISO/IEC 27090 draft text.** At FDIS and paywalled; only its title, stage, and summary-attributed threat-category scope are used. Clause-level mapping for 27090 is deferred until publication and acquisition.
- **ISO/IEC 42006:2025 (certification-body requirements)** and **ISO/IEC 27091 (AI privacy, DIS).** Noted as adjacent companions; not reviewed here.
- **Production effectiveness / certification audits.** Document-vs-(public-structure) review per the methodology, not a deployment or audit-quality assessment.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second reading attempted to refute each absence claim using only publicly available material (the constraint the original review operated under). **All five claims survived within their paywall-bounded scope; two were narrowed to record near-misses.**

1. **No agentic/NHI control — survives.** No public Annex A objective names agent or non-human identity. Narrowed: A.3 (roles & responsibilities) is the nearest control but is human-role governance, disclosed in the claim.
2. **No testable technical criterion — survives.** This is structural to ISO management-system standards; the public list shows objectives, not test procedures. The claim is explicitly hedged as paywall-bounded.
3. **No AI-BOM — survives.** A.7 (data provenance) is the near-miss; it governs *data* lineage, not a model/tool bill of materials. Disclosed in the claim.
4. **No decommission/version-lifecycle — survives.** The public A.6 life-cycle list shows no retirement control; corroborates the independently-recorded gap in the first-pass validation.
5. **27090 unpublished + guidance-only — survives.** Official catalogue stage and multiple secondary summaries agree it is FDIS and guidance; no source shows a published or certifiable status as of 2026-06.

**Residual constraint, not refutation:** every surviving claim carries the same caveat — a reviewer with the paid normative text could, in principle, refute claims 1-4 by quoting clause body language the public structure hides. This review cannot close that gap and does not pretend to.

## Effect on existing wiki pages

- **[[iso-iec-42001|ISO/IEC 42001 framework page]]:** the Annex A description was reconciled to the public objective scheme (A.2-A.10, ~38 controls under nine objectives); the citation-only/paywall constraint note and a back-link to this review were added; the ISO 27090 FDIS-unpublished status was confirmed; the Annex B/C/D inventory was added.
- **[[agentic-ai-security-cmm-crosswalk|CMM standards crosswalk]]:** the ISO 42001 Annex A row labels, which used a non-public numbering (clauses 5-10 mislabeled under `A.` and `A.7.1-A.7.9`), were re-anchored to the public objective scheme (A.2 policy → D1; A.3 roles → D1; A.5 impact assessment → D1; A.6 life cycle → D6/D9; A.7 data → D6; A.8 information → D9; A.9 use → D3 process-only; A.10 third-party → D8); a note records that the full 38-control map remains summary-sourced and paywall-bounded.
- **[[agentic-ai-security-cmm-d1-governance|CMM D1 deep dive]]:** the A.5/A.6 governance and ISO 42001-certification references were confirmed against the public structure; the citation-only caveat was added to the ISO 42001 anchor.
- **[[agentic-cmm-vs-standards-validation|First-pass validation]]:** a correction marker pointing here was added; the section's claims are confirmed within paywall bounds.
- **[[standards-review-backlog|Standards Review Backlog]]:** ISO/IEC 42001 + 27090 flips to **done (citation-only)** with the paywall constraint recorded.

## Methodology-DoD assessment (paywall / blocked issue)

The GitHub issue for this review is labeled **blocked (paywall)**. Methodology §8 explicitly permits graceful degradation to a citation-only review for paywalled standards, and §5.1 (Definition of Done) is satisfiable in its citation-only form:

- **DoD #1 (review page filed):** met — this page, with a public-structure coverage matrix, falsifiable (paywall-bounded) absence claims, and an adversarial-pass log.
- **DoD #2 (primary sources archived):** **cannot be met** — both documents are paywalled; no `.raw/` archive exists. Documented as a constraint, per §8.
- **DoD #3-#5 (taxonomy reconciliation, authoritative-structure pass, substantive propagation):** met within public scope — the crosswalk's non-public Annex A numbering is corrected; the D1 anchor and validation page are reconciled.
- **DoD #6-#7 (hygiene + hedge-free Effect section):** met — Effect section is past-tense; caches/index/backlog updated on propagation.

**Conclusion:** a citation-only review satisfies the methodology's DoD *to the degree §8 allows* — the deliverable, propagation, and falsifiable-absence requirements are met; only the primary-source-archive requirement (DoD #2) is unmet, and the methodology itself flags that as the unavoidable paywall limit. **The blocked issue can be closed as "done (citation-only, paywall-bounded)"**, with a residual note that a future pass with purchased normative text (and the published ISO 27090) could harden absence claims 1-4 and supply a true clause-level 27090 mapping.
