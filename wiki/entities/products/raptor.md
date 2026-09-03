---
type: entity
title: "RAPTOR"
address: c-000076
created: 2026-05-15
updated: 2026-09-01
tags:
  - entities
  - product
  - tool
  - vuln-discovery
  - open-source
  - claude-code
status: developing
scope_axis: [ai-in-sec-offense, ai-in-sec-defense, sec-against-ai]
entity_type: product
aliases:
  - "raptor"
  - "Recursive Autonomous Penetration Testing and Observation Robot"
role: "Open-source autonomous offensive/defensive security research framework built on top of Claude Code (v3.0.0; pluggable analysis layer). Chains static analysis, binary analysis, LLM-powered vulnerability validation, exploit generation, and patch writing into a single workflow that runs against a codebase or binary. Recommended in PA1 of the Mythos-ready playbook as an open-source option alongside OpenAnt."
homepage: https://github.com/gadievron/raptor
github: https://github.com/gadievron/raptor
license: "MIT (note: bundled CodeQL has its own license and does not permit commercial use)"
language: "Python (+ Claude Code skills)"
authors:
  - "[[gadi-evron|Gadi Evron]]"
  - "Daniel Cuthbert"
  - "Thomas Dullien (Halvar Flake)"
  - "Michael Bargury"
  - "John Cartwright"
maintainer: "[[gadi-evron|Gadi Evron]]"
first_mentioned: "[[mythos-ready-briefing|Mythos-ready paper]]"
related:
  - "[[gadi-evron|Gadi Evron]]"
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - "[[mythos-ready-security-program|Mythos-ready Security Program]]"
  - "[[openant|OpenAnt]]"
  - "[[codex-security|Codex Security]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
  - "[[semgrep|Semgrep]]"
sources:
  - https://github.com/gadievron/raptor
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# RAPTOR

**Sources:** [GitHub — `gadievron/raptor`](https://github.com/gadievron/raptor) · [[mythos-ready-briefing|Mythos-ready paper]] §Appendix A *Historical Precedent* + PA 1 (*Point Agents at Your Code and Pipelines*) · [Smithery skill listing](https://smithery.ai/skills?ns=gadievron).

## Description

RAPTOR — **Recursive Autonomous Penetration Testing and Observation Robot** — is an open-source autonomous security research framework built on top of Claude Code (v3.0.0; the analysis layer is pluggable, so other coding-agent platforms can be substituted). It chains static analysis (Semgrep + CodeQL), binary analysis + crash analysis (AFL++ fuzzing + `rr` deterministic debugger), LLM-powered vulnerability validation, exploit generation, and patch writing into a single workflow that runs against a codebase or a binary. MIT license (with CodeQL having its own license that does not permit commercial use).

Self-described as *"not polished software. It was built in free time, held together with enthusiasm and duct tape, and it works well enough that we can't stop using it."*

## Authors

- [[gadi-evron|Gadi Evron]] (lead; [@gadievron](https://github.com/gadievron))
- Daniel Cuthbert ([@danielcuthbert](https://github.com/danielcuthbert)) — also a contributing reviewer of the [[mythos-ready-briefing|Mythos-ready briefing]]
- Thomas Dullien (Halvar Flake) ([@thomasdullien](https://github.com/thomasdullien))
- Michael Bargury ([@mbrg](https://github.com/mbrg))
- John Cartwright ([@grokjc](https://github.com/grokjc))

## Capability Surface

RAPTOR exposes a slash-command interface inside Claude Code. Stable commands as of repository fetch (2026-05-15):

| Command | What it does |
| :--- | :--- |
| `/agentic` | Full autonomous workflow: scan, validate, exploit, patch |
| `/scan` | Static analysis with Semgrep and CodeQL |
| `/understand` | Map attack surface, trace data flows, hunt vulnerability variants |
| `/validate` | **Multi-stage exploitability validation pipeline (Stages 0–F)** |
| `/codeql` | CodeQL-only deep analysis with SMT dataflow pre-screening |
| `/fuzz` | Binary fuzzing with AFL++ and crash analysis |
| `/crash-analysis` | Autonomous root-cause analysis for C/C++ crashes |
| `/oss-forensics` | Evidence-backed forensic investigation for GitHub repositories |
| `/project` | Named workspaces to organize runs and track findings over time |

Beta commands: `/exploit` (PoC exploit generation), `/patch` (secure-patch generation). Alpha: `/web` (web app scanning).

The `--privileged` flag is required for the `rr` deterministic debugger inside the dev container; image is ~6 GB and starts from Microsoft's Python 3.12 devcontainer.

Semgrep's July 2026 survey adds detail this page's command table does not carry. Per its LLM-generated repository summary and capability matrix, RAPTOR runs approximately 107,000 lines of Python orchestrating Semgrep, CodeQL, AFL++ fuzzing, Z3-based exploit feasibility, software composition analysis, and multi-model consensus across Claude and GPT-class backends, fronted by Claude Code with loadable "expert personas."[^semgrep] The matrix records 22 vulnerability classes spanning memory safety, web, auth and logic; JSON, SARIF and SBOM output; and a Landlock, seccomp and namespaces isolation stack. That isolation stack describes a different layer than the `--privileged` requirement above: the dev-container flag governs the `rr` debugger's own host access, and the Landlock/seccomp/namespaces set is the sandbox RAPTOR builds around the code it analyzes. Both stand.

## Relevance to This Wiki

RAPTOR is the **author-tracing closure** for the wiki's coverage of the Mythos-era vuln-discovery cluster. The same [[gadi-evron|Gadi Evron]] who:

- Co-introduced [[vulnops|VulnOps]] (with Heather Adkins and Bruce Schneier, October 2025),
- Lead-authored the [[mythos-ready-briefing|CSA / SANS / Unprompted / OWASP Mythos-ready briefing]] (April 2026),
- Runs [[knostic|Knostic]] (which ships [[openant|OpenAnt]] and [[kirin|Kirin]]),

is also the lead author of RAPTOR — *the* open-source autonomous-security-research framework the Mythos-ready playbook recommends in PA 1 alongside OpenAnt for the *Monday-morning* operationalization of *Point Agents at Your Code and Pipelines*.

Structurally, RAPTOR is **the offensive/defensive twin to OpenAnt**: where OpenAnt operates a six-stage vuln-discovery pipeline with [[adversarial-reflexion|constrained-attacker-persona]] FP control, RAPTOR is a Claude-Code-skill-driven multi-stage framework that goes further down the pipeline (exploit generation + patch writing in addition to discovery + validation). Both are auditable open-source instruments; both are recommended by the same briefing.

The **multi-stage exploitability validation pipeline (Stages 0–F)** is structurally adjacent to OpenAnt's six-stage `Parse → Reachability → Classification → Discovery → Verification → Dynamic` pipeline and to [[mdash-defense-at-ai-speed|MDASH]]'s five-stage `Prepare → Scan → Validate → Dedup → Prove` pipeline — the architectural convergence on *multi-stage validation as the FP-control discipline* (see [[adversarial-reflexion|Adversarial Reflexion]]) extends from frontier-vendor harnesses through OSS instruments down to this Claude-Code-skill-level tool.

> [!contradiction] Stage count of the validation pipeline is disputed
> This page records a **Stages 0–F** exploitability-validation pipeline (`/validate`), sourced from a 2026-05-15 fetch of the RAPTOR README at v3.0.0. Semgrep's July 2026 survey, in an LLM-generated repository summary, describes "a six-stage exploitability-validation pipeline."[^semgrep] The two figures are not reconciled here; the README-sourced count above is not overwritten. The comparison to OpenAnt's six-stage and MDASH's five-stage pipelines above rests on whichever count is correct.

## Adjacent / Open

- **No benchmark result is published for RAPTOR.** Semgrep's July 2026 comparison of nine open-source harnesses supplies a placement instead of a measurement: RAPTOR is the maximalist entry, holding the broadest capability surface and the heaviest operational cost, recommended for a vulnerability hunter running one platform across scanning, dataflow, fuzzing, exploit feasibility and SCA.[^semgrep] Placement carries no recall or false-positive figure, so the page still records none — no published benchmark comparable to MDASH 88.45% / Aardvark 92% / OpenAnt filter-ratio metrics. The framework remains practitioner-grade rather than benchmark-validated.
- **Pluggable analysis layer** — the README is explicit that RAPTOR is "not tied to" Claude Code; the abstraction surface for plugging in alternative LLM-coding-agent platforms is not documented in the README excerpt and is a follow-up question. Semgrep's July 2026 survey narrows the model half of this question: RAPTOR runs multi-model consensus across Claude and GPT-class backends, per its LLM-generated capability matrix.[^semgrep] The harness half stays open — Semgrep also describes RAPTOR as "fronted by Claude Code," and no source documents an alternative coding-agent integration.
- **Smithery distribution** — the skills are also distributed via [Smithery](https://smithery.ai/skills?ns=gadievron); the relationship between the Smithery skill package and the GitHub repository (versioning, parity) is not captured here.

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The "vulnerability hunter" recommendation, the "maximalist" framing and the tool inventory (Semgrep, CodeQL, fuzzing, exploit feasibility, SCA, patching) are human-written; the six-stage figure, the LOC count, the Z3 and multi-model-consensus components, and every capability-matrix value (vulnerability classes, output formats, isolation, default models) are from Semgrep's LLM-generated repository summary and Reference 4 capability matrix. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
