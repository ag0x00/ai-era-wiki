---
type: entity
title: "angr"
address: c-000276
created: 2026-08-16
updated: 2026-08-16
tags:
  - entities
  - products
  - vuln-discovery
  - static-analysis
  - binary-analysis
  - open-source
status: seed
entity_type: product
vendor: "[[arizona-state-university|Arizona State University]]"
website: "https://angr.io/"
license: "BSD-2-Clause (open source; from the project, not the keynote)"
focus: "Multi-modal binary analysis: symbolic execution, static analysis, and the prototype substrate for a long line of academic vulnerability-discovery tools"
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
origin: aggregated
related:
  - "[[arizona-state-university|Arizona State University]]"
  - "[[yan-shoshitaishvili|Yan Shoshitaishvili]]"
  - "[[vulnerability-research-agentic-age-keynote|Vulnerability Research in the Agentic Age]]"
  - "[[analyzer-ordering-confound|Analyzer Ordering Confound]]"
  - "[[vulnerability-properties|Vulnerability Properties]]"
sources:
  - "https://www.youtube.com/watch?v=VNYe3Cnk5Pw"
  - ".raw/talks/2026-08-06_Yan-Shoshitaishvili_Vulnerability-Research-in-the-Agentic-Age_transcript.md"
---

# angr

Open-source multi-modal binary-analysis framework ([angr.io](https://angr.io/)), written by [[yan-shoshitaishvili|Yan Shoshitaishvili]] during his graduate work at [[arizona-state-university|Arizona State University]] as an attempt to automate the reverse-engineering he was doing by hand in CTF competitions. Jeff Moss's introduction to the [[vulnerability-research-agentic-age-keynote|Black Hat USA 2026 keynote]] credits it with team Shellphish's roughly two-year run at the top of worldwide CTF contests, on the basis that it could reverse and exploit in ways the existing tooling could not, and places its subsequent use at over a thousand research, academic, and product tools.[^keynote]

## Relevance to the agentic axis

angr predates agentic vulnerability discovery by more than a decade and matters to this wiki for two reasons that have nothing to do with model capability.

**It is the prototype substrate the keynote's argument is drawn from.** Arbiter, the scale-out platform that analyzed every program shipped in distribution repositories, is built on it. So is the pair of near-identical research prototypes whose asymmetric finding counts produce the [[analyzer-ordering-confound|analyzer ordering confound]]: two cutters built on the same framework, differing slightly, where the second finds only the residue of the first. The confound is stated in terms of angr-based prototypes because that is where it was observed directly.

**Its open-source release is the precedent behind its author's position on capability restriction.** angr and the team's cyber reasoning systems for the DARPA Cyber Grand Challenge and the AI Cyber Challenge were released without capability gating, and the keynote treats fifteen years of that practice as the evidence base for opposing restrictions on capable open-weight models.

Symbolic execution and the static analyses built on it target [[vulnerability-properties|vulnerability properties]] explicitly, which is what makes them the contrast case for language models — where the same targeting happens implicitly and cannot be inspected.

[^keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
