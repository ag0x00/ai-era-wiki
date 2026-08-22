---
type: entity
entity_type: person
title: "David Brumley"
address: c-000100
created: 2026-05-23
updated: 2026-05-29
tags:
  - entities
  - people
  - ai-vuln-discovery
  - ai-in-sec-offense
status: stub
scope_axis:
  - ai-in-sec-offense
role: "CMU professor; ForAllSecure / Mayhem founder; DARPA Cyber Grand Challenge winner; co-author of ExploitBench"
related:
  - "[[exploit-benchmarks]]"
  - "[[ai-vuln-discovery-benchmark-landscape]]"
sources:
  - "https://arxiv.org/abs/2605.14153"
---

# David Brumley

**Sources:** [ExploitBench (arXiv 2605.14153)](https://arxiv.org/abs/2605.14153)

## Identity and role

Carnegie Mellon University professor and founder of ForAllSecure, maker of the **Mayhem** autonomous analysis system. Brumley's team won DARPA's 2016 **Cyber Grand Challenge** — the first machine-vs-machine autonomous capture-the-flag, where systems found and patched binary vulnerabilities without human intervention. With Seunghyun Lee he co-authored [[exploit-benchmarks|ExploitBench]] (2026), the capability-ladder benchmark for LLM exploit development.

## Relevance to This Wiki
Brumley connects the **symbolic-execution / fuzzing** era of autonomous exploitation (Mayhem, DARPA CGC) to the **LLM-agent** era ([[exploit-benchmarks|ExploitBench]]). ExploitBench's framing — exploitation as a measurable ladder from coverage to arbitrary code execution — carries the rigor of the CGC scoring model into frontier-model evaluation. His work grounds the [[frontier-ai-for-vuln-discovery|frontier-AI-for-vuln-discovery thesis]] in a decade of prior autonomous-exploitation research.
