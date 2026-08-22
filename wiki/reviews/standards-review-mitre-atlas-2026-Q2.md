---
type: review
title: "MITRE ATLAS Standards Review"
address: c-000160
origin: produced
created: 2026-05-27
updated: 2026-05-29
tags:
  - reviews
  - standards-review
  - mitre
  - atlas
  - adversarial-ai
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[mitre-atlas]]"
reviewer: "Claude (Opus 4.7)"
review_started: "2026-05-27"
review_completed: "2026-05-27"
adversarial_pass: "completed 2026-05-27"
related:
  - "[[mitre-atlas]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[standards-review-backlog]]"
sources:
  - "https://atlas.mitre.org/"
  - "https://github.com/mitre-atlas/atlas-data"
---

# Standards Review — MITRE ATLAS, 2026-Q2

This review applies [[standards-validation-methodology-2026-05|the Standards Validation Methodology]] to MITRE ATLAS: primary-source citations from the `mitre-atlas/atlas-data` repository, a technique- and mitigation-level coverage matrix against the nine [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domains, falsifiable absence claims, and an adversarial second pass. It supersedes the ATLAS row in [[agentic-cmm-vs-standards-validation|the 2026-04-30 first-pass validation]] §2, which already flagged a citation error but worked from secondary counts.

The review produced one taxonomy correction, a refresh of stale version and count figures, a case-study name fix, and a technique- and mitigation-level crosswalk across all nine CMM domains and the six [[agentic-ai-security-reference-architecture|RA]] planes. ATLAS anchors the adversarial-technique surface of the CMM (notably D4, D6, and D8); it has no content for governance (D1) and almost none for operations and human factors (D9), by design.

**The CMM cited ATT&CK IDs as ATLAS techniques.** The [[mitre-atlas|ATLAS page]] anchored CMM domains with `T1612 / T0051 / T0054` (D4) and `T1565 / T0080` (D6). `T1612` ("Build Image on Host") and `T1565` ("Data Manipulation") are MITRE **ATT&CK Enterprise** technique IDs — confirmed absent from the ATLAS technique set (Source: [atlas-data techniques.yaml](https://raw.githubusercontent.com/mitre-atlas/atlas-data/main/data/techniques.yaml)). ATLAS techniques use the `AML.T####` namespace. The correct anchors are `AML.T0051` (LLM Prompt Injection), `AML.T0054` (LLM Jailbreak), and `AML.T0053` (AI Agent Tool Invocation) for D4, and `AML.T0080` (AI Agent Context Poisoning) with `AML.T0070` (RAG Poisoning) and `AML.T0020` (Poison Training Data) for D6.

## Primary documents reviewed

ATLAS has no single specification PDF. The authoritative source is the open-data repository; the website renders the same data. Version and counts are read from `data/data.yaml` and the source data files at the `main` branch.

| Document | URL | Scope used in this review |
|---|---|---|
| ATLAS data repository (`atlas-data`) | [GitHub — mitre-atlas/atlas-data](https://github.com/mitre-atlas/atlas-data) | Authoritative IDs, names, counts (v5.6.0) |
| `data/techniques.yaml` | [raw](https://raw.githubusercontent.com/mitre-atlas/atlas-data/main/data/techniques.yaml) | Technique IDs and descriptions |
| `data/mitigations.yaml` | [raw](https://raw.githubusercontent.com/mitre-atlas/atlas-data/main/data/mitigations.yaml) | Mitigation IDs and descriptions |
| `data/tactics.yaml` | [raw](https://raw.githubusercontent.com/mitre-atlas/atlas-data/main/data/tactics.yaml) | The 16 tactics |
| `data/case-studies/AML.CS0050.yaml` | [raw](https://raw.githubusercontent.com/mitre-atlas/atlas-data/main/data/case-studies/AML.CS0050.yaml) | OpenClaw case-study name |
| Live knowledge base | [atlas.mitre.org](https://atlas.mitre.org/) | Per-technique and per-mitigation pages |

> [!contradiction] Version and counts on the ATLAS page are stale and vendor-sourced
> The [[mitre-atlas|ATLAS page]] states "v5.4.0 … 84 techniques, 16 tactics, 56 sub-techniques, 32 mitigations, 42 case studies", attributed to Vectra (a vendor). The current repository is **v5.6.0**, with 16 tactics, 101 parent techniques, 69 subtechniques, 35 mitigations, and 57 case studies.[^counts] Only the tactic count (16) is still correct. The page also names case study `AML.CS0050` as "Exposed OpenClaw Control Interfaces"; the repository name is **"OpenClaw 1-Click Remote Code Execution"** (CVE-2026-25253). "Exposed ClawdBot Control Interfaces" is the separate `AML.CS0048`.

## Structure-to-domain grounding

ATLAS is an attack taxonomy: 16 tactics (adversary goals, `AML.TA####`), techniques and subtechniques (`AML.T####`), descriptive mitigations (`AML.M####`), and case studies (`AML.CS####`). It maps onto the CMM asymmetrically.

- **Adversarial surface (strong): D4, D6, D8.** The prompt-injection, jailbreak, context-poisoning, RAG-poisoning, and supply-chain families are the most developed parts of ATLAS and align closely with the CMM's runtime, data, and supply-chain domains.
- **Identity, control, egress (partial): D2, D3, D5.** ATLAS has credential-access and agent-tool-invocation techniques plus permission-configuration mitigations, but the mitigations are descriptive.
- **Governance and operations (absent or thin): D1, D9.** ATLAS has no governance or accountability content, and only human-in-the-loop and user-training touch the operations and human-factors domain.

## Technique- and mitigation-level coverage matrix (CMM × ATLAS)

Each row cites the ATLAS techniques and mitigations that anchor a CMM domain, with the namespace-correct IDs. Names are from the repository; per-ID detail is at `atlas.mitre.org/techniques/<ID>` and `atlas.mitre.org/mitigations/<ID>`.

| CMM domain | ATLAS techniques | ATLAS mitigations | Coverage |
|---|---|---|---|
| D1 Governance & Accountability | none | none | None — no governance/accountability/risk-framework content |
| D2 Identity & Authorization | `AML.T0012` Valid Accounts; `AML.T0055` Unsecured Credentials; `AML.T0083` Credentials from AI Agent Configuration; `AML.T0098` AI Agent Tool Credential Harvesting; `AML.T0082` RAG Credential Harvesting | `AML.M0026`/`AML.M0027`/`AML.M0028` agent-permission configuration; `AML.M0005`/`AML.M0019` access control | Partial — attack coverage good; mitigations descriptive |
| D3 Control & Least-Agency | `AML.T0053` AI Agent Tool Invocation; `AML.T0081` Modify AI Agent Configuration; `AML.T0105` Escape to Host; `AML.T0108` AI Agent (C2) | `AML.M0029` Human In-the-Loop; `AML.M0032` Segmentation of AI Agent Components; `AML.M0026`–`AML.M0028` | Good techniques; least-privilege mitigations descriptive |
| D4 Runtime & Guardrails | `AML.T0051` LLM Prompt Injection (+ .000/.001/.002); `AML.T0054` LLM Jailbreak; `AML.T0053` AI Agent Tool Invocation; `AML.T0068` Prompt Obfuscation; `AML.T0065` Prompt Crafting | `AML.M0020` Generative AI Guardrails; `AML.M0015` Adversarial Input Detection; `AML.M0033` Input/Output Validation; `AML.M0030` Restrict Tool Invocation on Untrusted Data | Best-covered domain |
| D5 Egress & Network | `AML.T0086` Exfiltration via AI Agent Tool Invocation; `AML.T0024` Exfiltration via AI Inference API; `AML.T0057` LLM Data Leakage; `AML.T0056` Extract LLM System Prompt | `AML.M0032` Segmentation; `AML.M0004` Restrict Number of AI Model Queries | Attack side good; no agent-egress-filtering mitigation |
| D6 Data, Memory & RAG | `AML.T0080` AI Agent Context Poisoning (+ .000 Memory / .001 Thread); `AML.T0070` RAG Poisoning; `AML.T0071` False RAG Entry Injection; `AML.T0020` Poison Training Data; `AML.T0099` AI Agent Tool Data Poisoning | `AML.M0031` Memory Hardening; `AML.M0007` Sanitize Training Data; `AML.M0025` Maintain AI Dataset Provenance | Strong for adversarial poisoning; no non-adversarial drift |
| D7 Observability & Detection | `AML.T0046` Spamming AI System with Chaff Data; `AML.T0034` Cost Harvesting | `AML.M0024` AI Telemetry Logging | Thin — one relevant mitigation; no detection-rule guidance |
| D8 Supply Chain & AI-BOM | `AML.T0010` AI Supply Chain Compromise (+6 subtechniques); `AML.T0019` Publish Poisoned Datasets; `AML.T0058` Publish Poisoned Models; `AML.T0104` Publish Poisoned AI Agent Tool; `AML.T0109` AI Supply Chain Rug Pull; `AML.T0110` AI Agent Tool Poisoning | `AML.M0023` AI Bill of Materials; `AML.M0013` Code Signing; `AML.M0014` Verify AI Artifacts; `AML.M0025` Provenance | Strongest supply-chain taxonomy of any current AI framework |
| D9 Operations & Human Factors | `AML.T0029` Denial of AI Service; `AML.T0034` Cost Harvesting; `AML.T0048` External Harms | `AML.M0029` Human In-the-Loop; `AML.M0018` User Training | Very thin — no runbook, IR, or lifecycle content |

The matrix confirms the CMM's design premise: ATLAS supplies the threat intelligence (what attackers do) but not the graded defensive program. Its mitigations name control categories without implementation specifications, so they ground a CMM domain's *threat model*, not its level criteria.

### RA-plane mapping

The six [[agentic-ai-security-reference-architecture|RA]] planes correspond to CMM D2–D7, so the same IDs apply.

| RA plane | Anchor ATLAS techniques |
|---|---|
| Identity | `AML.T0012`, `AML.T0055`, `AML.T0083`, `AML.T0098` |
| Control | `AML.T0053`, `AML.T0081`, `AML.T0105`, `AML.T0108` |
| Runtime | `AML.T0051`, `AML.T0054`, `AML.T0068`, `AML.T0065` |
| Egress | `AML.T0086`, `AML.T0024`, `AML.T0057`, `AML.T0056` |
| Data | `AML.T0080`, `AML.T0070`, `AML.T0071`, `AML.T0020` |
| Observability | `AML.T0046`, `AML.T0034`; mitigation `AML.M0024` |

## Falsifiable absence claims found

What the CMM scores that ATLAS, as documented in v5.6.0, does not provide. Each is bounded to the searched files and reversible by the stated refuting evidence.

1. **Mitigations are descriptive, not specifications.** ATLAS's 35 mitigations name control categories but carry no implementation specification, pass/fail criterion, or test procedure. *Searched:* `data/mitigations.yaml` (v5.6.0), all descriptions. *Terms:* "measure", "criteria", "evidence", "test procedure", "threshold", "metric", "shall", "minimum". *Verdict:* confirmed. The closest, `AML.M0014` (Verify AI Artifacts), names checksum verification but specifies no algorithm, acceptance condition, or cadence. *Refuting evidence:* a mitigation with measurable criteria or a `controls:` schema field. *Reviewed 2026-05-27.* This is the gap the CMM's per-level evidence rubric fills.

2. **No agent-to-agent trust exploitation or cascade-failure technique.** No technique models a compromised agent corrupting a downstream agent through inherited trust, or cascading failure across an orchestration graph. *Searched:* `data/techniques.yaml` (v5.6.0). *Terms:* "agent-to-agent", "multi-agent", "cascade", "multi-hop", "downstream agent", "orchestrat". *Verdict:* narrowly addressed. **Near-miss disclosed:** `AML.T0061` (LLM Prompt Self-Replication) models a prompt that "replicates … as part of its output" to "propagate to other LLMs" — worm-style propagation through a data channel, not an agent-trust topology or cascade-failure model. `AML.T0080.001` (Thread) notes shared-thread effects across users, not across agents. *Refuting evidence:* a technique modeling orchestrator→subagent trust exploitation or multi-agent cascade. *Reviewed 2026-05-27.* Matches the CMM's `D7 L5+` cascade-detection target and the OWASP ASI08 "○ none" mark.

3. **No model-deprecation / version-pin / end-of-life lifecycle coverage.** No technique or mitigation addresses adversary exploitation of deprecated or end-of-life model versions, or version-pin lifecycle controls. *Searched:* both data files. *Terms:* "deprecat", "end-of-life", "version pin", "retire", "obsolete", "supersede". *Verdict:* confirmed. The one hit, `AML.M0027`, covers decommissioning of an *agent instance's* access, not a *model version's* lifecycle; the `ml-lifecycle:` tag is a pipeline-phase label, not lifecycle content. *Refuting evidence:* a technique or mitigation on model version EOL or pinning. *Reviewed 2026-05-27.* Matches the model-deprecation gap the CMM `D8` carries.

4. **No non-adversarial-failure or governance content.** ATLAS covers only adversarial techniques; it has no entries for non-adversarial AI failure, governance, accountability, or risk-management frameworks. *Searched:* both data files. *Terms:* "governance", "accountability", "risk management", "non-adversarial", "bias", "fairness", "drift". *Verdict:* confirmed — a definitional exclusion, not an omission. `AML.M0022` (Model Alignment) mentions "safety" but is adversarially framed throughout; `AML.T0048` (External Harms) frames harm as adversary impact. *Refuting evidence:* a scope expansion plus entries on non-adversarial failure or governance. *Reviewed 2026-05-27.* The CMM's `D1` and `D9` fill this from other instruments.

## Scope exclusions

- **Per-subtechnique enumeration.** The matrix anchors each domain with its load-bearing techniques, not every subtechnique under them.
- **Case-study mapping.** The 57 case studies were used to confirm names and in-the-wild status, not mapped per CMM rung.
- **Production effectiveness.** This is a document-versus-document review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-05-27`. A second reviewer (separate run) attempted a counter-example for each absence claim against `atlas-data` v5.6.0. **Three of four claims survived; claim 2 was narrowed to its surviving form after a partial refutation.**

1. **Mitigations lack specifications — survives.** Search across all 35 mitigations for measurable criteria, thresholds, evidence, or test procedures returned none; `AML.M0014` names a mechanism (checksum) without acceptance criteria.
2. **No agent-to-agent / cascade — partially refuted, then narrowed.** `AML.T0061` (LLM Prompt Self-Replication) explicitly models a prompt propagating "to other LLMs". This is prompt-worm propagation through a data channel, not agent-trust-chain exploitation or cascade failure, so the narrowed claim holds; the near-miss is disclosed in the claim and on the ATLAS page.
3. **No model-deprecation / EOL — survives.** Only `AML.M0027` (agent-instance decommissioning) surfaced; it does not address model-version lifecycle.
4. **No non-adversarial / governance — survives.** `AML.M0022` (alignment) and `AML.T0048` (external harms) are adversarially framed; neither addresses governance or non-adversarial failure as subject matter.

## Effect on existing wiki pages

- **[[mitre-atlas|ATLAS framework page]]:** the mis-cited `T1612`/`T0051`/`T0054` (D4) and `T1565` (D6) anchors are replaced with namespace-correct `AML.T####` IDs; version and counts updated to v5.6.0 with a primary-source footnote (replacing the Vectra figures); `AML.CS0050` renamed to "OpenClaw 1-Click Remote Code Execution"; the new template's CMM/RA-mapping and known-gaps sections added.
- **[[agentic-ai-security-cmm-2026|Canonical CMM]]:** any `Maps to:` line citing `T1612`/`T1565` as ATLAS is corrected to the `AML.T####` anchors above.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the ATLAS column is updated to the corrected IDs.
- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]] §2 (ATLAS row):** gains a correction marker pointing here; the row had guessed `AML.T0018`/`AML.T0020` as the likely correct IDs — `AML.T0020` (Poison Training Data) is confirmed for D6, but the D4 anchors are the prompt-injection/jailbreak family, not `AML.T0018`.
- **[[standards-review-backlog|Standards Review Backlog]]:** MITRE ATLAS flips to done (2 of 11 reviewed).

## Notes

[^counts]: [MITRE ATLAS — atlas-data repository](https://github.com/mitre-atlas/atlas-data), `data/data.yaml` (version 5.6.0) and source data files, retrieved 2026-05-27. Counts: 16 tactics, 101 parent techniques, 69 subtechniques (170 techniques total), 35 mitigations, 57 case studies.
