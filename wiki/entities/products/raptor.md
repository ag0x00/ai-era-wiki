---
type: entity
title: "RAPTOR"
address: c-000076
created: 2026-05-15
updated: 2026-08-21
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
sources:
  - https://github.com/gadievron/raptor
  - "[[mythos-ready-briefing|Mythos-ready paper]]"
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

## Relevance to This Wiki

RAPTOR is the **author-tracing closure** for the wiki's coverage of the Mythos-era vuln-discovery cluster. The same [[gadi-evron|Gadi Evron]] who:

- Co-introduced [[vulnops|VulnOps]] (with Heather Adkins and Bruce Schneier, October 2025),
- Lead-authored the [[mythos-ready-briefing|CSA / SANS / Unprompted / OWASP Mythos-ready briefing]] (April 2026),
- Runs [[knostic|Knostic]] (which ships [[openant|OpenAnt]] and [[kirin|Kirin]]),

is also the lead author of RAPTOR — *the* open-source autonomous-security-research framework the Mythos-ready playbook recommends in PA 1 alongside OpenAnt for the *Monday-morning* operationalization of *Point Agents at Your Code and Pipelines*.

Structurally, RAPTOR is **the offensive/defensive twin to OpenAnt**: where OpenAnt operates a six-stage vuln-discovery pipeline with [[adversarial-reflexion|constrained-attacker-persona]] FP control, RAPTOR is a Claude-Code-skill-driven multi-stage framework that goes further down the pipeline (exploit generation + patch writing in addition to discovery + validation). Both are auditable open-source instruments; both are recommended by the same briefing.

The **multi-stage exploitability validation pipeline (Stages 0–F)** is structurally adjacent to OpenAnt's six-stage Parse → Reachability → Classification → Discovery → Verification → Dynamic pipeline and to [[mdash-defense-at-ai-speed|MDASH]]'s five-stage Prepare → Scan → Validate → Dedup → Prove pipeline — the architectural convergence on *multi-stage validation as the FP-control discipline* (see [[adversarial-reflexion|Adversarial Reflexion]]) extends from frontier-vendor harnesses through OSS instruments down to this Claude-Code-skill-level tool.

## Adjacent / Open

- **No published benchmarks** (recall, FP rate) comparable to MDASH 88.45% / Aardvark 92% / OpenAnt filter-ratio metrics. The framework is positioned as practitioner-grade rather than benchmark-validated.
- **Pluggable analysis layer** — the README is explicit that RAPTOR is "not tied to" Claude Code; the abstraction surface for plugging in alternative LLM-coding-agent platforms is not documented in the README excerpt and is a follow-up question.
- **Smithery distribution** — the skills are also distributed via [Smithery](https://smithery.ai/skills?ns=gadievron); the relationship between the Smithery skill package and the GitHub repository (versioning, parity) is not captured here.
