---
type: entity
title: "Arizona State University"
address: c-000275
created: 2026-08-16
updated: 2026-08-31
tags:
  - entities
  - organizations
  - academic
  - vuln-discovery
  - education
status: seed
entity_type: organization
org_type: advisory
website: "https://sefcom.asu.edu/"
focus: "Academic vulnerability analysis, exploitation, and automated repair; open security education"
aliases:
  - "ASU"
  - "SEFCOM"
  - "Center for Cybersecurity and Trusted Foundations"
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
related:
  - "[[yan-shoshitaishvili|Yan Shoshitaishvili]]"
  - "[[angr|angr]]"
  - "[[vulnerability-research-agentic-age-keynote|Vulnerability Research in the Agentic Age]]"
  - "[[vulnerability-properties|Vulnerability Properties]]"
  - "[[analyzer-ordering-confound|Analyzer Ordering Confound]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[cybergym|CyberGym Benchmark]]"
  - "[[exploit-benchmarks|ExploitBench & ExploitGym]]"
  - "[[uc-berkeley-rdi|UC Berkeley RDI]]"
sources:
  - "https://www.youtube.com/watch?v=VNYe3Cnk5Pw"
  - ".raw/talks/2026-08-06_Yan-Shoshitaishvili_Vulnerability-Research-in-the-Agentic-Age_transcript.md"
  - "https://www.cybergym.io/exploitgym/"
---

# Arizona State University

Academic home of the [SEFCOM lab](https://sefcom.asu.edu/) and the Center for Cybersecurity and Trusted Foundations, where [[yan-shoshitaishvili|Yan Shoshitaishvili]]'s group works on vulnerability analysis, exploitation, and automated repair. **The lab is one of two non-vendor sources of primary agentic vulnerability-discovery results on this wiki, and the only one that reports a harness ablation at fixed model capability.** The other is the [[uc-berkeley-rdi|UC Berkeley RDI]] group behind [[cybergym|CyberGym]], whose open-ended runs report zero-day discovery outcomes against real upstream code and no comparison across harness configurations.

The two are separate author sets on the results this wiki cites from each, and they overlap on one benchmark. Shoshitaishvili is a named co-author of [[exploit-benchmarks|ExploitGym]] alongside the Berkeley group, and the benchmark's attribution names Arizona State University among seven contributing organizations rather than naming SEFCOM.[^exploitgym]

Its position on the [[frontier-ai-for-vuln-discovery|frontier-AI-for-vuln-discovery]] axis differs from the vendor sources there in what it is free to publish. Vendor pipelines report benchmark scores and CVE counts that serve a product claim; the ASU results reported in the [[vulnerability-research-agentic-age-keynote|Black Hat USA 2026 keynote]] include a negative result on Rust rewrites, an admission that the lab's disclosure capacity is an order of magnitude behind its discovery rate, and a headline count the keynote itself declines to present as a like-for-like win over the model it benchmarks against.[^keynote]

## Research artifacts on this wiki

- **[[angr|angr]]** — the binary-analysis framework underlying the lab's prototype lineage.
- **Arbiter** — a scale-out analysis platform built on angr, used to analyze every program shipped in distribution repositories rather than a curated sample. Cited in the keynote as the "analyze more" strategy.
- **The Linux-kernel scaling experiment** — over 1,000 triaged local privilege escalations triggerable by an unprivileged user ([Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw)), produced by adding workflow and then [[vulnerability-properties|vulnerability properties]] to a fixed model generation. Funded, unintentionally, by an OpenAI Codex agreement the university had misconfigured to give every employee unmetered access, and drawn on until the university noticed roughly two million dollars of spend and revoked it.
- **The OpenHarmony property-transfer study** — Android vulnerability properties applied to Huawei's OpenHarmony, unpublished at keynote time.
- **The embedded-disclosure-harm paper** (May 2026) — disclosing one embedded-device vulnerability endangers roughly three times as many devices as it secures.
- **Cyber reasoning systems** for the DARPA Cyber Grand Challenge and the AI Cyber Challenge, released open source.

## pwn.college

The lab runs pwn.college, an open security-education platform with about 10,000 people actively learning on it monthly ([Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw)).[^keynote] The keynote uses the platform as the setting for a claim about the field rather than about the platform: the question its learners ask most often is whether security remains worth learning, and Shoshitaishvili's answer is that the human step in his own pipeline, deciding which vulnerability properties to extract and how to present them to the agents, is what produced the final doubling in his lab's Linux-kernel experiment. He states that as a position, not as a measured comparison against any other use of the same effort.

[^keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
[^exploitgym]: UC Berkeley RDI, [ExploitGym](https://www.cybergym.io/exploitgym/) (fetched 2026-08-31); [arXiv:2605.11086](https://arxiv.org/abs/2605.11086). Local copy: `.raw/articles/exploitgym-2026-08-31.md`.
