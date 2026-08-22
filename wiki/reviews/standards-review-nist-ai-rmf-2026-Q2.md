---
type: review
title: "NIST AI RMF Standards Review"
address: c-000216
origin: produced
created: 2026-06-21
updated: 2026-06-21
tags:
  - reviews
  - standards-review
  - nist
  - ai-rmf
  - risk-management
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[nist-ai-rmf]]"
reviewer: "Claude (Opus 4.8)"
review_started: "2026-06-21"
review_completed: "2026-06-21"
adversarial_pass: "completed 2026-06-21"
primary_documents:
  - title: "NIST AI 100-1 — Artificial Intelligence Risk Management Framework (AI RMF 1.0)"
    url: "https://doi.org/10.6028/NIST.AI.100-1"
    version: "1.0"
    published: "2023-01-26"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-100-1-rmf-2023-01-26.pdf"
    scope_in_wiki: "Core functions GOVERN/MAP/MEASURE/MANAGE (Tables 1–4), foundational §3 trustworthiness characteristics"
  - title: "NIST AI 600-1 — AI RMF: Generative Artificial Intelligence Profile"
    url: "https://doi.org/10.6028/NIST.AI.600-1"
    version: "1.0"
    published: "2024-07-26"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-600-1-genai-profile-2024-07.pdf"
    scope_in_wiki: "§2 twelve GenAI risk categories; §3 ~200 Suggested Actions (GV/MP/MS/MG IDs)"
  - title: "NIST AI 800-4 — Challenges to the Monitoring of Deployed AI Systems"
    url: "https://doi.org/10.6028/NIST.AI.800-4"
    version: "final"
    published: "2026-03"
    retrieved: "2026-06-21"
    archived_copy: ".raw/papers/nist-ai-800-4-monitoring-deployed-ai-2026-03.pdf"
    scope_in_wiki: "Full report — six monitoring categories (§2); cross-cutting and category-specific challenges (§3); open questions (§3.3)"
related:
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-600-1]]"
  - "[[nist-ai-800-4]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-review-backlog]]"
sources:
  - "[[.raw/papers/nist-ai-100-1-rmf-2023-01-26.pdf]]"
  - "[[.raw/papers/nist-ai-600-1-genai-profile-2024-07.pdf]]"
  - "[[.raw/papers/nist-ai-800-4-monitoring-deployed-ai-2026-03.pdf]]"
---

# Standards Review — NIST AI RMF, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to the NIST AI Risk Management Framework stack: [[nist-ai-rmf|AI 100-1 (AI RMF 1.0)]], its [[nist-ai-600-1|AI 600-1 Generative AI Profile]], and [[nist-ai-800-4|AI 800-4]] ("Challenges to the Monitoring of Deployed AI Systems"). The three are reviewed as one P1 unit because the CMM and crosswalk cite them as a single anchor (`NIST AI RMF + 600-1 / 800-4`). Inputs are the primary PDFs archived under `.raw/papers/`, read clause by clause; the output is a subcategory- and action-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains, a set of falsifiable absence claims, and an adversarial second pass. It supersedes the NIST row in [[agentic-cmm-vs-standards-validation|the 2026-04-30 first-pass validation]], which worked from wiki summaries.

The framework stack is governance- and risk-process-centric. It has strong content for D1, D7, D8, and D9 once 600-1 is included, and is silent on the agentic-control-specific domains: non-human identity (D2), least-agency and tool-invocation control (D3), per-agent egress (D5), and the prompt-injection *defense* half of D4. The review also surfaces systematic risk-category-numbering errors across the AI 600-1 column of the existing crosswalk and one stale version date on the framework page.

**The framework stack names the agentic attack surface but does not yet prescribe agentic controls.** Prompt injection, data poisoning, model extraction, and autonomous agents appear in AI 600-1 and AI 800-4, but each is framed as a *risk to assess or red-team* or a *monitoring challenge to study*, never as a deployed control with acceptance criteria. The most security-direct clause in the base framework, `MEASURE 2.7` (AI system security and resilience), is an "evaluate and document" outcome, not a control specification. The CMM's per-level evidence rubric fills this gap, and the methodology states the gap in bounded form rather than as an unfalsifiable "NIST doesn't cover it."

## Primary documents reviewed

| Document | ID / version | Published | Type | Scope used |
|---|---|---|---|---|
| [[nist-ai-rmf\|Artificial Intelligence Risk Management Framework]] | AI 100-1, v1.0 | 2023-01-26 | Voluntary framework | GOVERN/MAP/MEASURE/MANAGE Core (Tables 1–4); foundational §3 |
| [[nist-ai-600-1\|AI RMF: Generative AI Profile]] | AI 600-1, v1.0 | 2024-07-26 | Cross-sectoral profile | §2 risk categories; §3 ~200 Suggested Actions |
| [[nist-ai-800-4\|Challenges to the Monitoring of Deployed AI Systems]] | AI 800-4, final | 2026-03 | Descriptive research report (CAISI) | Full report; six monitoring categories, challenge taxonomy |

All three are public, free of charge, and DOI-stable. AI 800-4 is a [[nist|NIST]] Center for AI Standards and Innovation (CAISI) report in the Trustworthy and Responsible AI series; it is **descriptive, not normative**: it catalogs monitoring gaps, barriers, and open questions, and contains no `shall`/`should` statements, no measurable acceptance criteria, and no maturity tiers. For crosswalk purposes it is a requirements- and gap-source, not a control standard; its mapping below is challenge-to-domain, not control-to-control.

## Structure-to-domain grounding

- **AI RMF 1.0** is technology- and lifecycle-neutral by design (p. 2). It supplies the GOVERN/MAP/MEASURE/MANAGE skeleton plus the third-party, lifecycle, human-oversight, and incident-response hooks an agentic CMM hangs controls on, but it carries no GenAI- or agentic-specific control text. Security is one of seven trustworthiness characteristics (§3.3, "Secure and Resilient"), where the document defers to the NIST CSF and the SP 800-53/RMF line.
- **AI 600-1** is content-risk- and governance-centric. Its twelve risk categories and ~200 Suggested Actions map cleanly to D1, D7, D8, D9, the groundedness half of D4, and the data half of D6. The agentic-control domains are where it is silent or names only the threat.
- **AI 800-4** is a monitoring-challenge landscape. Its strongest domain is D7, but as a catalog of what the field cannot yet do, not a control set. It is the only document of the three that treats agentic systems as a recurring lens (agent identifiers, scheming/sandbagging detection, multi-agent logging, the "monitorability tax"), but always as a gap or open question.

## Clause-level coverage matrix (CMM × NIST stack)

Each row cites the subcategory IDs (AI RMF), Suggested-Action IDs (AI 600-1, format `[GV|MP|MS|MG]-[subcategory]-[NNN]`), and section references (AI 800-4) that anchor a CMM domain. IDs are quoted as printed in the source PDFs.

| CMM domain | AI RMF 100-1 | AI 600-1 | AI 800-4 | Net coverage |
|---|---|---|---|---|
| D1 Governance & Accountability | **Strong** — entire GOVERN function (`GOVERN 1.1`–`1.7`, `2.1`–`2.3`, `3.2`, `4.1`–`4.3`); `MANAGE 1.1` go/no-go | **Strong** — `GV-1.1`–`GV-5.1` block (~50 actions): risk tiers/halt thresholds `GV-1.3-001`…`007`, roles `GV-2.1-001`…`005`, oversight `GV-4.1-001`…`003` | **Partial** — governance *challenges* only: who-monitors §3.3.1, incentives/culture §3.1.4, purpose §3.3.4 | **Strong** |
| D2 Identity & Authorization | **None** — no machine/agent identity, authN/authZ, or credential clause | **None** — only human-personnel credentials `MG-3.1-001`; "access controls" generic | **None–weak** — "agent identifiers" named as an *immature visibility gap* §3.1.2; cites (does not adopt) an OAuth 2.0 extension for agent credentials §3.1.1 | **None** |
| D3 Control & Least-Agency | **Partial** — human oversight `GOVERN 3.2` / `MAP 3.5`; deactivate `MANAGE 2.4`; override `MANAGE 4.1`; knowledge limits `MAP 2.2` | **Partial** — refusal criteria `GV-3.2-003`, malicious-query handling `MS-2.6-006`, override monitoring `MS-4.2-004`, disengage `MG-2.4-001`…`004` | **None** — HITL appears only as a monitoring-scaling tradeoff §3.1.3 | **Partial** — oversight/refusal/deactivate; no least-privilege, PDP, or tool-invocation gating |
| D4 Runtime & Guardrails | **Partial** — fail-safe `MEASURE 2.6`, security/resilience `MEASURE 2.7`, production monitoring `MEASURE 2.4` | **Strong groundedness / thin PI-defense** — groundedness `MS-2.5-005`, source verification `MS-2.5-003`, guardrail review `MS-2.5-006`, content filters `MG-3.2-005`; prompt injection only as red-team target `MS-2.7-007` | **Weak** — groundedness-via-ground-truth §3.2.1, deceptive-behavior detection §3.2.4; no guardrail control | **Partial** — output/groundedness controls present; prompt-injection *defense* absent |
| D5 Egress & Network | **None** — exfiltration named once in foundational §3.3 prose; no Core subcategory | **Weak** — exfil threats to assess `MS-2.7-001`, network-interaction analysis `MP-2.2-002`, PII-in-output detection `MS-2.10-001`; no egress control | **None** — only "air-gapped environment" as a monitoring question §3.1.1 | **None–weak** — threat acknowledged; no per-agent egress filtering or network policy |
| D6 Data, Memory & RAG | **Partial→weak** — data quality `MAP 2.3`; "data poisoning" only in foundational §3.3 prose | **Strong data/RAG, none agent-memory** — data curation `MP-4.1-004`/`005`, lineage `MP-2.1-001`/`002`, poisoning red-team `MS-2.7-007` + value-chain test `MG-3.1-002`, provenance `MG-4.1-006` | **Partial** — distribution shift/drift §3.2.1, ground-truth scarcity §3.2.1; no RAG/memory | **Partial** — training/RAG-data integrity covered; agent-memory poisoning absent across all three |
| D7 Observability & Detection | **Partial** — production/ongoing monitoring `MEASURE 2.4`, `3.1`; pre-trained-model monitoring `MANAGE 3.2`; post-deployment plan `MANAGE 4.1`; no logging/anomaly clause | **Strong** — anomaly handling `MS-2.6-005`, real-time alerting `MG-3.2-006`, post-deployment monitoring `MG-4.1-001`…`006`, incident-DB integration `GV-1.6-003` | **Strong (core topic)** — entire report: drift/degradation detection, fragmented-logging barrier §3.2.2, deceptive-behavior detection §3.2.4, telemetry §3.2.3 | **Strong** — but the strongest content is a *challenge catalog*, not graded criteria |
| D8 Supply Chain & AI-BOM | **Partial** — third-party policy `GOVERN 6.1`/`6.2`, component risk `MAP 4.1`/`4.2`, AI inventory `GOVERN 1.6`, pre-trained-model monitoring `MANAGE 3.2` | **Strong value-chain / no BOM artifact** — `GV-6.1-001`…`010` third-party block, provenance fields `GV-1.6-003`, model/system cards `MG-3.1-005`, approved-provider lists `GV-6.1-007` | **Weak/partial** — value-chain visibility §3.1.2, open-weight downstream tracking §3.2.6; no AI-BOM | **Strong value-chain; AI-BOM artifact absent** |
| D9 Operations & Human Factors | **Strong** — decommissioning `GOVERN 1.7` / `MANAGE 4.1`, IR `GOVERN 4.3` + `MANAGE 2.3`/`4.3`, training `GOVERN 2.2`, feedback `MEASURE 3.3` | **Strong** — Human-AI Configuration (§2.7) category, decommission `GV-1.7-001`/`002`, IR `MG-2.3-001`, operator proficiency `MP-3.4-001`…`006` | **Partial** — human-factors challenges §3.2.3, operational monitoring §3.2.2; IR/lifecycle as open *questions* | **Strong** |

Net per-domain verdict: **Strong** — D1, D7, D8 (value-chain), D9. **Partial** — D3, D4, D6. **None / none–weak** — D2, D5.

### RA-plane mapping

The six [[agentic-ai-security-reference-architecture|RA]] planes correspond to CMM D2–D7. The NIST stack anchors the governance band (D1) and operations band (D9) strongly and the observability plane (D7) strongly; it is thinnest exactly on the identity (D2), control (D3), and egress (D5) planes that distinguish an agentic architecture from a general ML system.

## Falsifiable absence claims found

What the CMM scores that the NIST stack, as published, does not provide. Each is bounded to the searched documents and reversible by the stated refuting evidence.

1. **No non-human / agent identity control.** None of the three documents prescribes authentication, authorization, credential, or lifecycle controls for non-human / agent principals. *Searched:* AI 100-1 Core (Tables 1–4); AI 600-1 §3 all Suggested Actions; AI 800-4 full text. *Terms:* "identity", "authentication", "authorization", "credential", "service account", "non-human", "agent identifier", "OAuth". *Verdict:* confirmed. The only hits are human-personnel credentials (`MG-3.1-001`), generic access-attempt metrics (`MS-2.7-004`), and AI 800-4's framing of "agent identifiers" as an unstandardized *visibility gap* (§3.1.2) plus a *cited* OAuth 2.0 extension for agent credentials (§3.1.1, South et al.) that NIST does not adopt. *Refuting evidence:* any subcategory or Suggested Action prescribing authN/authZ or credential lifecycle for a machine/agent principal. *Reviewed 2026-06-21.* This is the [[non-human-identity|NHI]] gap the CMM `D2` fills from other instruments.

2. **No least-agency / least-privilege / tool-invocation control.** The stack governs autonomy through human oversight, content refusal, and deactivation, not through per-action authorization, a policy decision point, or least-privilege scoping of agent tool calls. *Searched:* all three documents. *Terms:* "least privilege", "privilege", "policy decision", "autonomy", "tool", "permission", "scope of action". *Verdict:* confirmed. **Near-miss disclosed:** AI 600-1 covers refusal criteria (`GV-3.2-003`), malicious-query handling (`MS-2.6-006`), and supersede/disengage (`MG-2.4-001`…`004`); AI RMF covers override (`MANAGE 4.1`) and deactivate (`MANAGE 2.4`). These are agency *governance*, not agency-limiting controls. "Autonomous agents" appears in 600-1 once, only as an attack surface (`MS-2.7-001`). *Refuting evidence:* a clause prescribing per-action authorization, a PDP, or least-privilege tool-invocation gating. *Reviewed 2026-06-21.* Matches the CMM `D3` least-agency target.

3. **Prompt injection appears only as a red-team / assessment target, never as a deployed defensive control.** *Searched:* all three documents. *Terms:* "prompt injection", "jailbreak", "input sanitization", "input validation", "guardrail". *Verdict:* confirmed. AI 600-1 names prompt injection once, in the red-teaming action `MS-2.7-007` ("assess resilience against … GAI attacks (e.g., prompt injection)"), and again in the §2.9 risk narrative; no Suggested Action prescribes an input-sanitization or injection-mitigation control. AI 100-1 does not contain the term (post-dates the document). AI 800-4 does not contain "prompt injection" or "guardrail". *Refuting evidence:* a clause prescribing a prompt-injection *mitigation* control rather than a test. *Reviewed 2026-06-21.* The CMM `D4` runtime-guardrail rungs fill this.

4. **No per-agent egress filtering or network-policy control.** Data exfiltration is acknowledged as a threat to assess, never as an enforced egress control. *Searched:* all three. *Terms:* "egress", "exfiltration", "network policy", "allowlist", "outbound", "DLP", "firewall". *Verdict:* confirmed. AI RMF names exfiltration once in foundational §3.3 prose with no Core subcategory; AI 600-1 lists exfil threats for assessment (`MS-2.7-001`) and network-interaction *analysis* (`MP-2.2-002`); AI 800-4 mentions only an "air-gapped environment" as a monitoring question. *Refuting evidence:* a clause prescribing per-agent egress filtering, an egress allowlist, or enforced network segmentation. *Reviewed 2026-06-21.* Matches the CMM `D5` egress target and the `D2→D5` dependency (per-agent egress needs per-agent identity, which the stack also lacks).

5. **No AI-BOM as a named artifact.** The ingredients of an AI bill of materials are present, but never assembled into one. *Searched:* all three. *Terms:* "bill of materials", "BOM", "SBOM", "AI-BOM", "AIBOM", "CycloneDX", "SPDX". *Verdict:* confirmed — zero literal hits. **Near-miss disclosed:** AI 600-1 supplies the components — AI-system inventory with provenance fields ("source, signatures, versioning, watermarks … underlying foundation models, versions … access modes", `GV-1.6-003`), approved-provider lists (`GV-6.1-007`), and model/system cards (`MG-3.1-005`) — but does not formalize them into a BOM artifact. *Refuting evidence:* a clause defining or requiring an AI/ML bill of materials. *Reviewed 2026-06-21.* The CMM `D8` AI-BOM rungs anchor to CycloneDX ML-BOM / SPDX 3.0 and [[nist-sp-800-218a|SP 800-218A]], not to the AI RMF stack.

6. **No agent-memory poisoning coverage.** Data poisoning is scoped to the *training and RAG corpus*; the agent's runtime / conversational / episodic memory is not addressed. *Searched:* all three. *Terms:* "memory", "memory poisoning", "episodic", "conversation history", "context poisoning", "vector store". *Verdict:* confirmed. AI 600-1 covers training-data poisoning (`MS-2.7-007`, `MG-3.1-002`) and "data memorization" as a privacy-leakage risk (§2.4), not memory injection; AI 800-4 does not address agent memory. *Refuting evidence:* a clause on agent runtime-memory or RAG-store poisoning at inference. *Reviewed 2026-06-21.* Matches the CMM `D6` memory-poisoning rung; the [[mitre-atlas|ATLAS]] review found the same gap is filled there by `AML.T0080`.

7. **AI 800-4 supplies no mappable controls.** As a descriptive CAISI report, AI 800-4 contains no normative clauses — no `shall`/`should`, no measurable criteria, no maturity tiers. *Searched:* full text. *Terms:* "shall", "should", "must", "requirement", "control", "criteria", "threshold". *Verdict:* confirmed. Where it touches a practice (e.g. "deviation thresholds" §3.2.1, the "monitorability tax" §3.1.5), it does so by *quoting external sources as unmet needs*, not by recommending. *Refuting evidence:* a normative clause or measurable acceptance criterion authored by NIST in the document. *Reviewed 2026-06-21.* For the crosswalk, AI 800-4 anchors the *why* of D7 (and the agentic-monitoring open items), not graded D7 criteria.

## Scope exclusions

- **Sub-action enumeration.** The matrix anchors each domain with its load-bearing Suggested Actions, not every action under a subcategory. AI 600-1 has ~200 actions; only the security-, monitoring-, supply-chain-, and oversight-relevant ones are cited.
- **The full §2 GenAI risk narratives.** The twelve risk categories were read to confirm scope and absence claims, not mapped per CMM rung individually.
- **NIST SP 800-53 / IR 8605A.** The federal control-family bridge is tracked separately in the [[agentic-ai-security-cmm-crosswalk|crosswalk]] §"NIST SP 800-53 control families via IR 8605A COSAiS" and is out of scope here.
- **Production effectiveness.** This is a document-versus-document review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-06-21`. A second pass attempted a counter-example for each absence claim against the three PDFs. **All seven claims survived; claims 2 and 5 were narrowed to their disclosed near-miss forms.**

1. **No agent identity — survives.** Search across all three for machine/agent authN/authZ returned only human credentials and AI 800-4's "agent identifier" *gap* framing.
2. **No least-agency control — survives, narrowed.** AI 600-1 refusal/disengage actions and AI RMF override/deactivate are agency governance, not least-privilege or tool-invocation gating; the near-miss is disclosed in the claim.
3. **Prompt injection as test only — survives.** `MS-2.7-007` is a red-teaming target; no mitigating control found.
4. **No egress control — survives.** Exfiltration is threat-to-assess (`MS-2.7-001`) and network-interaction analysis (`MP-2.2-002`); no enforced egress filtering.
5. **No AI-BOM artifact — survives, narrowed.** `GV-1.6-003` provenance fields, `GV-6.1-007` provider lists, and `MG-3.1-005` model/system cards are BOM ingredients; none is the assembled artifact. Near-miss disclosed.
6. **No agent-memory poisoning — survives.** Poisoning is training/RAG-scoped (`MS-2.7-007`, `MG-3.1-002`); memory injection absent.
7. **AI 800-4 non-normative — survives.** No `shall`/`should`/criteria authored by NIST; all practice-like statements are external quotations.

## Effect on existing wiki pages

- **[[nist-ai-rmf|AI RMF framework page]]:** gains `primary_documents` frontmatter with `archived_copy` paths and `scope_in_wiki` per methodology Step 1. The `current_version` string "1.0 (July 2023)" is corrected — AI RMF 1.0 was published **2023-01-26 (January)**, not July; the July 2024 date belongs to AI 600-1.
- **[[nist-ai-600-1|AI 600-1 framework page]]:** gains `primary_documents` frontmatter with `archived_copy`; publication date set to 2024-07-26.
- **[[nist-ai-800-4|AI 800-4]]:** new framework page created (the wiki previously had none), tagged as a descriptive CAISI report, with the six monitoring categories and the gap/barrier/open-question taxonomy, and an explicit note that it is non-normative.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the entire AI 600-1 column carried systematic risk-category-numbering errors, all corrected against the source §2 numbering (2.1 CBRN, 2.2 Confabulation, 2.4 Data Privacy, 2.7 Human-AI Configuration, 2.8 Information Integrity, 2.9 Information Security, 2.10 Intellectual Property, 2.12 Value Chain):
    - **D2** cited "§2.4" (Data Privacy, not identity) — removed; AI 600-1 has no non-human-identity content, so the cell rests on SP 800-207/800-63 and NIST CAISI.
    - **D3** cited "§2.10 (Excessive Agency)" — §2.10 is Intellectual Property and "Excessive Agency" is an OWASP term, not a 600-1 category; replaced with the refusal/disengage Suggested Actions (`GV-3.2-003`, `MS-2.6-006`, `MG-2.4`).
    - **D4** cited "§2.5 (CBRN)" and "§2.7 (Confabulation)" — §2.5 is Environmental Impacts, §2.7 is Human-AI Configuration; corrected to §2.1 (CBRN), §2.2 (Confabulation), §2.3, with the groundedness action `MS-2.5-005` and the red-team-only note for prompt injection.
    - **D5** cited "§2.11 (Information Security)" — §2.11 is Obscene/Abusive Content; corrected to §2.9, flagged exfil-as-threat-only.
    - **D6** cited "§2.7 (Confabulation)" — §2.7 is Human-AI Configuration; corrected to §2.4/§2.8 plus the poisoning actions, with the agent-memory absence noted.
    - **D7** cited "§2.12 (Value Chain)" — §2.12 is the supply-chain category and belongs in D8; replaced with the monitoring Suggested Actions (`MS-2.6-005`, `MG-3.2-006`, `MG-4.1-001`…`006`).
    - **D8** §2.12 (Value Chain) was already correct; the third-party block `GV-6.1-001`…`010` and the AI-BOM-artifact absence were added.
- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]] (NIST row):** gains a correction marker pointing here; its wiki-summary-level verdict is superseded by this clause-level review.
- **[[standards-review-backlog|Standards Review Backlog]]:** NIST AI RMF flips to done (3 of 11 reviewed).
- **CMM domain pages with NIST absence claims:** D2, D3, D5, and the AI-BOM rung of D8 may cite this review for the bounded-negative form of their NIST absence claims (per methodology §4); the `Maps to:` lines already present in the [[agentic-ai-security-cmm-2026|CMM]] are consistent with this matrix and need no correction beyond the crosswalk fixes above.
