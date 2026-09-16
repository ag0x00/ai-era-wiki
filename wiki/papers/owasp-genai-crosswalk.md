---
type: paper
title: "GenAI Crosswalk"
address: c-286239
created: 2026-09-16
updated: 2026-09-16
tags:
  - papers
  - owasp
  - crosswalk
  - standards-mapping
  - agentic-ai
status: summarized
scope_axis:
  - sec-of-ai
origin: aggregated
year: 2026
authors:
  - "Emmanuel Guilherme Junior"
venue: "OWASP GenAI Security Project"
source_url: "https://genai-security-project.github.io/crosswalk/"
archived_copy: ".raw/articles/owasp-genai-crosswalk-2026-09-16.md"
key_claim: "The OWASP GenAI Crosswalk maps 51 GenAI and agentic risk entries to 3,781 control mappings across 26 frameworks, and every one of those mappings is unreviewed."
methodology: "A computed extract from the crosswalk's public data files (data.js, frameworks-registry.js, incidents.js, classifier-predictions.js), combined with the project's site prose and repository README, captured 2026-09-16."
contradicts: []
supports: []
related:
  - "[[owasp-llm-top-10]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-agentic-skills-top-10]]"
  - "[[csa-maestro]]"
  - "[[aiuc-1]]"
  - "[[cosai]]"
  - "[[owasp-aivss]]"
  - "[[owasp]]"
  - "[[ken-huang]]"
  - "[[standards-validation-methodology-2026-05]]"
sources:
  - "[[.raw/articles/owasp-genai-crosswalk-2026-09-16.md]]"
---

# GenAI Crosswalk

**Source:** [OWASP GenAI Security Project — GenAI Crosswalk](https://genai-security-project.github.io/crosswalk/) (fetched 2026-09-16). Local copy: `.raw/articles/owasp-genai-crosswalk-2026-09-16.md`.

## Overview

The GenAI Crosswalk is a dataset and web application published by the OWASP GenAI Security Project, version 4.0.0, licensed CC BY-SA 4.0. Emmanuel Guilherme Junior created and leads the project; he also leads the OWASP GenAI Data Security Initiative. It maps 51 risk entries drawn from four OWASP source lists — the LLM Top 10, the Top 10 for Agentic Applications, the DSGAI 2026 data-security risk list, and the Agentic Skills Top 10 — to 26 industry frameworks, for 3,781 individual control mappings computed as the sum of every entry's mapping list.[^data] It ships three ways: a git repository of per-framework Markdown mapping files, an npm package (`genai-security-crosswalk`), and machine-readable exports in OSCAL 1.1.2 and STIX 2.1.

## Review state

Every one of the 3,781 mappings carries `confidence: "unreviewed"` and an empty `reviewed_by: []`.[^data] No mapping in the dataset is signed by a named reviewer, and the project says so of itself: "Every mapping in this project is unreviewed until a named reviewer signs it."[^noscript] The site adds that two of the 26 frameworks, CoSAI and the EU AI Act Code of Practice, "currently carry candidate rows only": the control ids are accurate to the published framework, but the pairing to a risk is a proposal awaiting subject-matter review rather than an assertion.[^noscript]

The data shows a wider draft surface than the site states. 286 mappings carry the literal string `DRAFT` in their notes field, spread across three frameworks rather than the two the site names: CoSAI (127), the EU AI Act Code of Practice (123), and MAESTRO (36).[^data] The MAESTRO 36 are exactly the AST01–AST10 mappings, each noted "DRAFT — SME review required."[^data] The schema's evidence block sits empty in the data: 18 mappings carry one, none of the 18 confirmed, listing 25 draft incident ids between them, and none records a nonzero evidence count.[^data]

### Classifier pipeline

A machine-learning classifier proposes candidate mappings for the dataset. `classifier-predictions.js` records 615 predictions across 41 of the 51 entries — the Agentic Skills Top 10 is absent — and against 14 of the 26 frameworks, generated 2026-04-09 in reranker mode at top-k 15.[^data] The project's v3.0 changelog describes the pipeline as "BGE + cross-encoder".[^data] A GitHub Action turns the classifier's output into a pull request for a human to accept, reject, or edit. The site describes the flow directly: "Paste your framework controls as JSON below. Our classifier will propose mappings and open a PR for review." And: "The classifier opens a PR with proposed mappings. Reviewers accept, reject, or edit each mapping."[^data]

The proposal step runs in production; the review step it depends on has not run on any of the 3,781 rows. Each prediction carries its own curation flag, and no mapping in the dataset carries a reviewer signature, so nothing in the published data separates a merged classifier proposal from a hand-written row.[^data]

## Coverage denominator

The registry's completeness field reads `unknown` on 15 of the 26 framework records, `partial` on 9, and `complete` on 2.[^data] Only OWASP AISVS 1.0 (191 of 191 controls) and STRIDE (6 of 6) record an authoritative control total; the other 24 record none, and none of the 26 records a source hash.[^data] The AIUC-1 record's own note states the reason: "Every item in this registry is referenced by a mapping, so the registry was derived from the mappings rather than from the framework. It cannot contain controls this crosswalk has not mapped. Authoritative total not yet established."[^data]

Because the registry is derived from its own mappings, it holds no count of a framework independent of what the crosswalk has already mapped to it. A coverage percentage computed against that registry therefore measures the dataset's internal consistency, for 24 of the 26 frameworks, rather than a framework's coverage of GenAI risk.

## Framework registry error

The framework registry records AIUC-1's publisher as the UK AI Safety Institute, under an Open Government Licence v3.0.[^data] The wiki holds AIUC-1's publisher to be the Artificial Intelligence Underwriting Company, a private San Francisco firm — see [[aiuc-1|AIUC-1]]. The registry field is wrong for a framework carrying 147 mappings, consistent with a registry nobody has reviewed.

## LLM Top 10 numbering

Three OWASP-controlled artifacts disagree on the LLM Top 10's numbering as of 2026-09-16. The crosswalk's data layer labels its list `LLM-Top10-2026` and assigns:

| ID | Crosswalk data (2026) | Published list (2025) |
|---|---|---|
| LLM01 | Prompt Injection | Prompt Injection |
| LLM02 | Sensitive Information Disclosure | Sensitive Information Disclosure |
| LLM03 | Excessive Agency | Supply Chain <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |
| LLM04 | Supply Chain | Data and Model Poisoning <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |
| LLM05 | Data and Model Poisoning | Improper Output Handling <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |
| LLM06 | Unbounded Consumption | Excessive Agency <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |
| LLM07 | Misinformation | System Prompt Leakage |
| LLM08 | Hidden Context Exposure | Vector and Embedding Weaknesses <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |
| LLM09 | Vector and Embedding Weaknesses | Misinformation <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |
| LLM10 | Improper Output Handling | Unbounded Consumption <!-- taxonomy-ok: 2026 crosswalk numbering compared to the 2025 published set --> |

`https://genai.owasp.org/llm-top-10/` still carries the 2025 numbering, with System Prompt Leakage at LLM07 and no 2026 edition published.[^llm10] The crosswalk's own `noscript` prose lists the same ten titles in 2025 order but substitutes Hidden Context Exposure for System Prompt Leakage.[^noscript] Neither capture says whether that is a rename or a new category. The substitution takes the seventh slot in an otherwise unchanged 2025 order, and System Prompt Leakage appears nowhere in the 2026 numbering, so this wiki reads it as a rename. The data layer above carries a third, separate assignment. [[owasp-llm-top-10|OWASP LLM Top 10]] records the published set the wiki cites.

## Internal inconsistencies

Hand-written prose in the project's own README disagrees with the numbers generated beside it, and in one case with itself, in three places:

- The README's hand-written sections state 50 documented incidents; the generated `stats:incidents` macro in the same file, and `incidents.js` behind it, state 131.[^readme][^data]
- The README's source-list table records the Agentic Skills Top 10's frameworks-mapped column as "registered; mappings pending,"[^readme] while the data layer already carries 36 AST mappings against MAESTRO.[^data]
- The README's summary table states 25 eval profiles (Garak 13, PyRIT 6, LAAF 6); its repository-structure section states 7 Garak YAML profiles and 3 PyRIT scripts.[^readme]

## Utility

The unreviewed state constrains how a mapping may be cited; it does not make the dataset useless. The incident tracker holds 131 records — real-world 72, research-demonstrated 56, red-team 3, dated 2022 through 2026 — each carrying a MAESTRO layer attribution with a role drawn from Origin, Propagation, Impact, or Blind-spot.[^data] The machine-readable exports and the per-framework mapping files give a security team a starting point for a control lookup, in formats existing GRC and SIEM tooling can ingest directly.

Mapping density varies by framework. OWASP AISVS 1.0 carries 306 mappings, the largest single set in the registry, against 22 for STRIDE, the smallest.[^data] A team checking coverage against a specific standard reads the per-framework file for that standard rather than the aggregate count, because the aggregate hides a fourteen-fold spread. Classifier attention does not explain that spread and runs against it: the classifier produced 173 predictions for MITRE ATLAS, which holds 117 mappings, and none at all for OWASP AISVS 1.0, which holds 306.[^data]

## Relations

- [[owasp-llm-top-10|OWASP LLM Top 10]] — the crosswalk's `LLM-Top10-2026` numbering bears on this page's unreconciled OWASP AI Exchange identifiers.
- [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] — source of the ten ASI entries, the only ones in the dataset carrying an AIVSS score.
- [[owasp-agentic-skills-top-10|OWASP Agentic Skills Top 10]] — AST01–AST10; the only mapped framework is MAESTRO, and every AST mapping is draft.
- [[csa-maestro|CSA MAESTRO]] — target framework for all 36 AST mappings and for the incident tracker's layer attribution.
- [[aiuc-1|AIUC-1]] — 147 mappings in the dataset; the registry's publisher field for this framework is wrong.
- [[cosai|CoSAI]] — largest single block of draft mappings, at 127.
- [[owasp-aivss|OWASP AIVSS]] — scores present on the ten ASI entries and on none of the other 41.
- [[owasp|OWASP]] — publisher of the crosswalk through the GenAI Security Project.
- [[ken-huang|Ken Huang]] — named on the OWASP Agentic Skills Top 10 project page; that list's 36 draft mappings in this dataset are its only current mapping coverage.
- [[standards-validation-methodology-2026-05|Standards Validation Methodology]] — the crosswalk is that methodology's contrast case: it runs Step 2 (control mapping) at public scale and carries none of the other three steps, so most of its rows leave their origin unstated.

## Notes

[^data]: OWASP GenAI Security Project, *GenAI Crosswalk*, v4.0.0, data layer (`data.js`, `frameworks-registry.js`, `incidents.js`, `classifier-predictions.js`), captured 2026-09-16. `.raw/articles/owasp-genai-crosswalk-2026-09-16.md`, Part A.
[^noscript]: OWASP GenAI Security Project, *GenAI Crosswalk*, `noscript` site prose, captured 2026-09-16. `.raw/articles/owasp-genai-crosswalk-2026-09-16.md`, Part B.
[^readme]: OWASP GenAI Security Project, `GenAI-Security-Project/crosswalk`, `README.md`, captured 2026-09-16. `.raw/articles/owasp-genai-crosswalk-2026-09-16.md`, Part C.
[^llm10]: OWASP, [LLM Top 10](https://genai.owasp.org/llm-top-10/), fetched 2026-09-16. 2025 numbering; System Prompt Leakage at LLM07; no 2026 edition published.
