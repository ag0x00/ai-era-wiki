---
type: review
title: "EU AI Act Standards Review"
address: c-000228
origin: produced
created: 2026-06-22
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - eu-ai-act
  - regulation
  - compliance
  - high-risk-ai
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[eu-ai-act]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-22"
review_completed: "2026-06-22"
adversarial_pass: "completed 2026-06-22"
related:
  - "[[eu-ai-act]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[iso-iec-42001]]"
  - "[[standards-review-backlog]]"
sources:
  - "https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng"
  - "https://artificialintelligenceact.eu/"
---

# Standards Review — EU AI Act, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to Regulation (EU) 2024/1689 (the EU AI Act), including Annex IV (technical documentation): primary-source citations from the EUR-Lex consolidated text and the article-by-article rendering at artificialintelligenceact.eu, an article- and annex-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains, falsifiable absence claims, and an adversarial second pass. It supersedes the EU AI Act row in [[agentic-cmm-vs-standards-validation|the 2026-04-30 first-pass validation]], which worked from the wiki summary rather than the operative articles.

The review verified every article and annex number the wiki cites for the Act against the primary source, corrected the Annex IV item map in the [[agentic-ai-security-cmm-crosswalk|crosswalk]] (which mislabelled three top-level Annex IV points), and confirmed the Act's structural position relative to the CMM: it is risk-management and conformity-assessment law for high-risk AI, not an agentic technical-control standard.

**The EU AI Act is a risk-management and conformity-assessment regime, not an agentic technical-control standard.** Its high-risk requirements (Art. 9-15) state outcomes — appropriate accuracy, robustness, cybersecurity, oversight, traceability — and delegate the control specifications to harmonized standards (CEN/CENELEC, in development). Article 15 names attack classes (data poisoning, model poisoning, adversarial examples / model evasion, confidentiality attacks, model flaws) as outcomes to "prevent, detect, respond to, resolve and control for," but specifies no control, threshold, or test procedure. The Act has no agent-identity, least-agency, or multi-agent content. Annex IV is a documentation schema — the closest thing in any binding instrument to an AI-BOM mandate, but a disclosure list, not a bill-of-materials format.

## Primary documents reviewed

The Act has a single authoritative consolidated text on EUR-Lex. The article-level pages at artificialintelligenceact.eu render the same enacted text per article and annex, and were used to read individual articles and Annex IV verbatim. Article and annex numbers below were verified against the fetched text, not recited from memory.

| Document | URL | Scope used in this review |
|---|---|---|
| Regulation (EU) 2024/1689 — consolidated text | [EUR-Lex 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng) | Authoritative enacted text |
| Article 9 — Risk management system | [Art. 9](https://artificialintelligenceact.eu/article/9/) | Continuous lifecycle risk process |
| Article 10 — Data and data governance | [Art. 10](https://artificialintelligenceact.eu/article/10/) | Training/validation/test data quality and governance |
| Article 11 — Technical documentation | [Art. 11](https://artificialintelligenceact.eu/article/11/) | Documentation duty; references Annex IV |
| Article 12 — Record-keeping | [Art. 12](https://artificialintelligenceact.eu/article/12/) | Automatic event logging over system lifetime |
| Article 13 — Transparency and provision of information to deployers | [Art. 13](https://artificialintelligenceact.eu/article/13/) | Instructions for use; output interpretability |
| Article 14 — Human oversight | [Art. 14](https://artificialintelligenceact.eu/article/14/) | Effective oversight; interrupt to safe state |
| Article 15 — Accuracy, robustness and cybersecurity | [Art. 15](https://artificialintelligenceact.eu/article/15/) | Outcome-stated cybersecurity; named attack classes |
| Article 25 — Responsibilities along the AI value chain | [Art. 25](https://artificialintelligenceact.eu/article/25/) | When a third party becomes a provider |
| Article 26 — Obligations of deployers | [Art. 26](https://artificialintelligenceact.eu/article/26/) | Deployer oversight, monitoring, log retention |
| Article 53 — GPAI provider obligations | [Art. 53](https://artificialintelligenceact.eu/article/53/) | Technical documentation; training-data summary |
| Article 55 — GPAI with systemic risk | [Art. 55](https://artificialintelligenceact.eu/article/55/) | Model evaluation, adversarial testing, cybersecurity, incident reporting |
| Article 72 — Post-market monitoring | [Art. 72](https://artificialintelligenceact.eu/article/72/) | Documented post-market monitoring plan |
| Article 73 — Reporting of serious incidents | [Art. 73](https://artificialintelligenceact.eu/article/73/) | Tiered incident-reporting clocks |
| Annex IV — Technical documentation | [Annex IV](https://artificialintelligenceact.eu/annex/4/) | Nine top-level points; point 2 sub-points (a)-(h) |

> [!check] Annex IV verified structure (primary source)
> Annex IV has **nine numbered top-level points**. Point 2 ("detailed description of the elements … and of the process for its development") carries lettered sub-points (a)-(h): (a) development methods / pre-trained components, (b) design specifications and algorithmic logic, (c) system architecture and computational resources, (d) data requirements / datasheets, (e) human oversight measures, (f) pre-determined changes, (g) validation and testing procedures, (h) cybersecurity measures. Point 5 is the **risk management system** (per Art. 9); point 6 is **changes through the lifecycle**; point 9 is the **post-market monitoring** plan. Source: [Annex IV](https://artificialintelligenceact.eu/annex/4/), retrieved 2026-06-22.

## Structure-to-domain grounding

The Act is binding law structured around risk tiers (prohibited, high-risk, limited-risk, minimal-risk) and a conformity-assessment pathway. The agentic-relevant substance sits in three places:

- **High-risk system requirements (Art. 9-15)** — the core obligations, all outcome-stated. These map onto CMM D1, D3, D4, D5, D6, D7, D9.
- **Annex IV technical documentation** — the disclosure schema operationalizing Art. 11. This is the Act's strongest single anchor for **D8** (Supply Chain & AI-BOM) and reaches D1, D3, D6, D7.
- **GPAI obligations (Art. 53, 55)** — documentation for all GPAI providers; for models with systemic risk, model evaluation, adversarial testing, cybersecurity, and incident tracking.

Two domains have weak or no anchor:
- **D2 Identity & Authorization** — the Act has no non-human-identity, agent-identity, credentialing, or authorization content. Human oversight (Art. 14) governs a human-to-system delegation chain, not agent-to-agent or machine identity.
- **D3 Control & Least-Agency** — the Act mandates human oversight and an interrupt capability (Art. 14) but no least-privilege, tool-scoping, or agency-tiering control.

The Act's defining limit for a technical-control CMM: Art. 15 cybersecurity is an *outcome obligation* ("achieve an appropriate level of … cybersecurity") with named threat classes but no specified control, no acceptance threshold, and no test procedure. The control specifications are deferred to harmonized standards not yet published.

## Article- and annex-level coverage matrix (CMM x EU AI Act)

Each row cites the verified articles and Annex IV points that anchor a CMM domain. Article numbers and Annex IV point/sub-point labels are from the primary source (retrieved 2026-06-22).

| CMM domain | EU AI Act articles | Annex IV points | Coverage |
|---|---|---|---|
| D1 Governance & Accountability | Art. 9 (risk management system); Art. 17 (quality management system); Art. 16 (provider obligations); Art. 25 (value-chain responsibilities) | Point 5 (risk management description); point 8 (EU declaration of conformity) | Strong — governance and accountability are the Act's center of gravity, but as duties and process, not graded technical maturity |
| D2 Identity & Authorization | none | none | None — no non-human-identity, agent-identity, or authorization content |
| D3 Control & Least-Agency | Art. 14 (human oversight; interrupt to safe state); Art. 13 (transparency to deployers) | Point 2(e) (human oversight measures) | Partial — human oversight and an interrupt capability are required; no least-privilege or agency-tiering control |
| D4 Runtime & Guardrails | Art. 15 (accuracy, robustness, cybersecurity — names data poisoning, model poisoning, adversarial examples / model evasion, confidentiality attacks, model flaws) | Point 2(h) (cybersecurity measures); point 2(g) (validation and testing) | Outcome-stated only — threat classes named; no control, threshold, or test procedure specified |
| D5 Egress & Network | Art. 15 (resilience against unauthorised third parties altering use, outputs or performance) | Point 2(h) (cybersecurity measures) | Outcome-stated; no egress-filtering or network-segmentation control |
| D6 Data, Memory & RAG | Art. 10 (data and data governance: relevance, representativeness, error-management, bias examination, provenance); Art. 15 (data-poisoning resilience) | Point 2(d) (data requirements, datasheets, provenance); point 2(a) (pre-trained components) | Strong for training/validation/test data governance; no agent-memory or RAG-runtime content |
| D7 Observability & Detection | Art. 12 (automatic event logging over lifetime); Art. 72 (post-market monitoring plan) | Point 3 (monitoring, functioning and control); point 9 (post-market monitoring system) | Logging and post-market monitoring required as capability; no detection-rule or telemetry-schema specification |
| D8 Supply Chain & AI-BOM | Art. 11 (technical documentation); Art. 25 (value-chain provider transitions); Art. 53 (GPAI documentation, training-data summary) | **All nine points** — Annex IV is the disclosure schema; closest binding instrument to an AI-BOM mandate | Strongest single anchor — but a documentation list, not a machine-readable AI-BOM format |
| D9 Operations & Human Factors | Art. 14 (human oversight); Art. 26 (deployer obligations: monitoring, log retention); Art. 72 (post-market monitoring); Art. 73 (serious-incident reporting) | Point 6 (changes through lifecycle); point 9 (post-market monitoring) | Operations and incident-reporting duties are explicit; human-factors content limited to oversight competence and automation-bias awareness (Art. 14) |

The matrix confirms the Act's design: it imposes *duties and disclosures* across most CMM domains but specifies *controls* in none. It grounds a CMM domain's compliance obligation and its documentation evidence, not its level criteria. For agentic systems specifically, GPAI systemic-risk obligations (Art. 55) supply the only adversarial-testing and model-evaluation mandate in the Act — and that applies to model providers, not to enterprise agent deployers.

## Falsifiable absence claims found

What the CMM scores that the EU AI Act, as enacted in Regulation (EU) 2024/1689, does not provide. Each is bounded to the searched articles/annexes and reversible by the stated refuting evidence.

1. **No agent-identity or non-human-identity content.** The Act has no provision for agent identity, machine/workload identity, credential management, or authorization of non-human actors. *Searched:* Art. 9-15, Art. 16-17, Art. 25-26, Art. 53-55, Annex IV. *Terms:* "identity", "credential", "authentication", "non-human", "machine identity", "service account", "authorization". *Verdict:* not addressed. The only delegation construct is human oversight (Art. 14), a human-to-system relationship. *Refuting evidence:* any article or annex point requiring identity or authorization controls for AI components or agents. *Reviewed 2026-06-22.* Matches the CMM D2 "no anchor" mark.

2. **No least-agency / least-privilege control.** The Act requires human oversight and an interrupt capability (Art. 14) but specifies no least-privilege scoping, tool-permission profiling, or graded-autonomy control. *Searched:* Art. 13-15, Annex IV point 2(e). *Terms:* "least privilege", "least agency", "tool scope", "permission", "autonomy tier", "blast radius". *Verdict:* not addressed as a control; human oversight is the only autonomy-bounding mechanism. *Refuting evidence:* any provision specifying privilege-scoping or agency limitation beyond human override. *Reviewed 2026-06-22.* Matches CMM D3.

3. **Article 15 cybersecurity is outcome-stated, not control-specified.** Art. 15 requires "an appropriate level of … cybersecurity" and names attack classes ("data poisoning … model poisoning … adversarial examples or model evasion, confidentiality attacks or model flaws") but specifies no control, configuration, threshold, acceptance criterion, or test procedure. *Searched:* Art. 15 full text; Annex IV point 2(h). *Terms:* "shall implement", "control", "threshold", "minimum", "test procedure", "metric", "configuration". *Verdict:* confirmed — obligations are stated as outcomes; control specifications are deferred to harmonized standards (in development). *Refuting evidence:* any clause specifying a named control, measurable threshold, or test procedure for cybersecurity. *Reviewed 2026-06-22.* This is the gap the CMM's per-level evidence rubric and [[mitre-atlas|ATLAS]] mitigation anchors fill.

4. **No multi-agent or agent-to-agent content.** The Act has no provision addressing multi-agent orchestration, inter-agent communication, agent-to-agent trust, or cascade failure across an agent graph. *Searched:* Art. 9-15, Art. 53-55, Annex IV. *Terms:* "multi-agent", "agent-to-agent", "orchestrat", "inter-agent", "cascade", "downstream agent". *Verdict:* not addressed. The Act's unit of regulation is the "AI system" or "GPAI model", not an agent topology. *Refuting evidence:* any article or annex point addressing interaction between autonomous AI components. *Reviewed 2026-06-22.* Matches CMM D7 cascade-detection.

5. **Annex IV is a documentation schema, not an AI-BOM format.** Annex IV mandates disclosure of architecture, data requirements/provenance (point 2(d)), pre-trained components (point 2(a)), and cybersecurity measures (point 2(h)), but defines no machine-readable bill-of-materials format, no component-dependency graph, and no signing/attestation requirement. *Searched:* Annex IV points 1-9; Art. 11; Art. 53. *Terms:* "bill of materials", "AI-BOM", "SBOM", "machine-readable", "dependency", "attestation", "signature", "format". *Verdict:* confirmed — the closest binding instrument to an AI-BOM mandate, but a prose disclosure list, not a BOM artifact. *Refuting evidence:* any clause specifying a structured/machine-readable component inventory or attestation format. *Reviewed 2026-06-22.* Matches CMM D8.

6. **No graded maturity model.** The Act is binary at the conformity boundary (compliant / non-compliant for a given risk tier); it defines no maturity tiers, capability levels, or progressive-assurance scale. *Searched:* Art. 8-17 (high-risk requirements and conformity), Art. 43 (conformity assessment). *Terms:* "maturity", "level", "tier", "capability", "graded", "progressive". *Verdict:* confirmed — conformity is pass/fail against fixed requirements, not a maturity ramp. *Refuting evidence:* any provision defining graded capability levels. *Reviewed 2026-06-22.* This is the structural reason the Act anchors CMM *obligations* but not CMM *levels*.

## Scope exclusions

- **Prohibited-practice and limited-risk tiers.** Art. 5 (prohibited practices) and Art. 50 (limited-risk transparency) are outside the high-risk technical-control scope this CMM review targets; Art. 50 is noted on the framework page for GPAI/transparency timing only.
- **Conformity-assessment procedure mechanics.** Art. 43, Annexes VI-VII (conformity-assessment routes), notified bodies, and CE marking were not mapped per CMM rung.
- **GPAI Annex XI/XII content detail.** Art. 53/55 were read for agentic-relevant obligations; the line-item content of Annex XI and Annex XII was not enumerated per CMM domain.
- **Enforcement-timeline volatility.** The CEN/CENELEC harmonized-standard schedule is tracked on the [[eu-ai-act|framework page]]; this review fixes article/annex *substance*, which the timeline does not change.
- **Production conformity audits.** Document-versus-document review per the methodology, not a deployment or notified-body assessment.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-22`. A second pass attempted a counter-example for each absence claim against the enacted text. **All six claims survived; claim 3 was sharpened with the verbatim Art. 15 attack-class quote, and claim 5 was sharpened against the Art. 53 training-data summary.**

1. **No agent-identity content — survives.** No identity, credential, or authorization provision for AI components in any searched article or Annex IV point. Art. 14 oversight is human-to-system only.
2. **No least-agency control — survives.** Art. 14 supplies oversight and interrupt; no privilege-scoping or agency-tiering clause surfaced.
3. **Art. 15 outcome-stated — survives, sharpened.** Art. 15 names attack classes as outcomes to "prevent, detect, respond to, resolve and control for," but pairs them with no control, threshold, or test procedure.
4. **No multi-agent content — survives.** The regulated unit is the AI system or GPAI model; no inter-agent or cascade provision exists.
5. **Annex IV not an AI-BOM — survives, sharpened.** Art. 53's public "training-data summary" is the nearest counter-example but is a prose summary on an AI Office template, not a machine-readable BOM; Annex IV remains a disclosure list.
6. **No graded maturity — survives.** Conformity is pass/fail against fixed Art. 8-15 requirements; no capability scale exists.

## Effect on existing wiki pages

- **[[eu-ai-act|EU AI Act framework page]]:** every article and annex number it cites was verified against the primary source and confirmed correct (Art. 9 risk management, Art. 10 data governance, Art. 11 + Annex IV technical documentation, Art. 12 logging, Art. 14 human oversight, Art. 50 transparency, Art. 72 post-market monitoring). The page's "Article 11 and Annex IV effectively mandate AI-BOM-like documentation … not a formal standard" framing was confirmed accurate against Annex IV points 1-9. A `primary_documents:` block and this review's back-link were added.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]] — Annex IV crosswalk section corrected (taxonomy drift):** the Annex IV item map mislabelled three top-level points. "Cybersecurity measures" was corrected to **point 2(h)** (point 5 is the *risk management system*); "risk management" was corrected to **point 5 / Art. 9** (2(g) is *validation and testing procedures*); point 6 is *changes through the lifecycle*, and the risk register maps to point 5. The per-domain article anchors were verified and held.
- **[[agentic-ai-security-cmm-2026|Canonical CMM]] / [[agentic-ai-security-cmm-d1-governance|D1 deep dive]]:** D1 governance anchors citing Art. 9 (risk management) and Art. 17 (quality management) were verified; the absence of an EU AI Act anchor for D2 was confirmed and retained.
- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]] (EU AI Act row):** gains a correction marker pointing here.
- **[[standards-review-backlog|Standards Review Backlog]]:** EU AI Act flips to done.

## Notes

Article and annex numbers verified against the EUR-Lex consolidated text and the per-article rendering at artificialintelligenceact.eu, retrieved 2026-06-22. Annex IV: nine top-level points; point 2 carries sub-points (a)-(h); cybersecurity is 2(h), risk management is point 5, lifecycle changes is point 6.
