---
type: concept
title: "CyberGym-E2E"
address: c-000323
created: 2026-08-31
updated: 2026-08-31
tags:
  - concepts
  - benchmarks
  - vuln-discovery
  - ai-vuln-discovery
  - oss-fuzz
  - public-benchmark
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
aliases:
  - "CyberGym-E2E"
authors:
  - "Tianneng Shi"
  - "Robin Rheem"
  - "Dongwei Jiang"
  - "Mona Wang"
  - "Francisco De La Riega"
  - "Zhun Wang"
  - "Jingzhi Jiang"
  - "Alexander Cheung"
  - "Sean Tai"
  - "Jonah Cha"
  - "Jianhong Tu"
  - "Gabriel Han"
  - "Chenguang Wang"
  - "Jingxuan He"
  - "Wenbo Guo"
  - "Dawn Song"
homepage: "https://www.cybergym.io/cybergym-e2e/"
paper: "https://arxiv.org/abs/2606.04460"
venue: "ICML 2026"
related:
  - "[[cybergym|CyberGym Benchmark]]"
  - "[[exploit-benchmarks|ExploitBench & ExploitGym]]"
  - "[[uc-berkeley-rdi|UC Berkeley RDI]]"
  - "[[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]]"
  - "[[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]]"
  - "[[end-to-end-harness-evaluation|End-to-End Harness Evaluation]]"
  - "[[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
sources:
  - "https://www.cybergym.io/cybergym-e2e/"
  - "https://arxiv.org/abs/2606.04460"
  - ".raw/articles/cybergym-e2e-2026-08-31.md"
---

# CyberGym-E2E

**CyberGym-E2E** is a public benchmark for the whole discover-prove-fix lifecycle: **920 tasks across 139 OSS projects**, each carrying four cumulative validation stages.[^cybergym-e2e] It is published at ICML 2026 as [arXiv:2606.04460](https://arxiv.org/abs/2606.04460), sixteen authors led on the senior side by [[uc-berkeley-rdi|UC Berkeley RDI]]'s Dawn Song. It is the third of [[uc-berkeley-rdi|UC Berkeley RDI]]'s three benchmarks, sibling to [[cybergym|CyberGym]] (reproduction) and [[exploit-benchmarks|ExploitGym]] (exploit generation), and the only one of the three that scores discovery, patching and post-patch functionality together rather than one stage in isolation.

CyberGym-E2E is one instrument built on a general grading method; [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] states that method — cumulative stage gating, behavioral rather than similarity grading, and the patch-only ablation arm — apart from this benchmark's own parameters.

## Environment

Each task places the agent inside the target project's own build environment, with build scripts and test scripts, rather than granting read-only access to a single vulnerable function — the site states this mirrors how a coding agent is actually deployed by an engineer.[^cybergym-e2e] The corpus derives from resolved OSS-Fuzz vulnerabilities and is automatically extended as new OSS-Fuzz findings appear, so it remains a fixed, already-resolved corpus evaluated inside a benchmark-supplied build environment rather than a live target.

## Two settings

- **End-to-end.** All ground-truth data is withheld. The agent must discover the vulnerability, construct an input that triggers a sanitizer crash, and produce a patch — full discovery in a build environment, not blind analysis of an isolated function.
- **Patch-only.** The agent receives the ground-truth proof-of-concept and crash log, isolating the task to root-cause analysis and patching.

## Validation stages

The four stages are cumulative: each requires every prior stage to pass.[^cybergym-e2e]

| Stage | What it checks |
|---|---|
| S1 | The agent's proof-of-concept crashes the unpatched build |
| S2 | The agent's patch fixes that crash |
| S3 | The patched project still passes its developer-written functionality tests |
| S4 | Diagnostic: the patch fixes the specific ground-truth vulnerability |

The method behind this ladder — why grading is behavioral, what the S3–S4 gap measures, and how a harness comparison should treat its cost budget — is stated on [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] rather than restated here.

## Result

With the \$10 evaluation cap lifted, Claude Opus 4.6's patch-only curve plateaus near **86%** almost immediately, while its end-to-end curve keeps rising past roughly \$30 to reach about **63%**.[^cybergym-e2e] The gap between the two figures is why the benchmark grades discovery and repair as separate arms rather than reporting one score: repair saturates long before discovery does. No other CyberGym-E2E score is cited here — the capped public leaderboard renders client-side and its rows did not appear in the fetched page. [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] states what the gap means operationally, and [[autonomous-exploit-generation|Autonomous Exploit Generation]] reads the same pair against a second comparison that starts from a defect somebody has already localized, where building a working exploit is the harder step.

## Worked example: GraphicsMagick

Given only the GraphicsMagick codebase, an agent parses the task, browses the source tree, and uses targeted search to locate `ReadMNGImage()` in `coders/png.c`. It examines the code around `mng_LOOP` chunk handling, inspects a sample MNG file's byte layout, and constructs a minimal malformed MNG input that triggers a heap-buffer overflow. Validation confirms the crash (S1); the agent's first patch attempt fails to fully stop the crash, and it iterates until the patched build eliminates the crash and passes the project's functionality tests (S2 and S3). The site reports the whole discover-prove-fix loop unfolding in a couple dozen execution steps.[^cybergym-e2e]

## See also

- [[cybergym|CyberGym Benchmark]] — sibling benchmark from the same group, scoring reproduction alone.
- [[exploit-benchmarks|ExploitBench & ExploitGym]] — sibling benchmark scoring exploit generation.
- [[uc-berkeley-rdi|UC Berkeley RDI]] — publisher of all three benchmarks.
- [[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]] — where this benchmark sits among the others.
- [[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]] — the design theory this benchmark's stage ladder instantiates.
- [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] — the grading method this benchmark instantiates.
- [[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]] — the sibling method page for the reproduction stage this benchmark subsumes.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the wiki thesis this benchmark's discovery-versus-repair gap feeds.

[^cybergym-e2e]: UC Berkeley RDI, [CyberGym-E2E](https://www.cybergym.io/cybergym-e2e/) (fetched 2026-08-31); [arXiv:2606.04460](https://arxiv.org/abs/2606.04460), ICML 2026. Local copy: `.raw/articles/cybergym-e2e-2026-08-31.md`.

<!-- sources:auto -->
## Sources

- [CyberGym-E2E](https://www.cybergym.io/cybergym-e2e/)
- [arxiv.org](https://arxiv.org/abs/2606.04460)
<!-- /sources -->
