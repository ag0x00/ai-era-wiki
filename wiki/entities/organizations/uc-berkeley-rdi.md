---
type: entity
entity_type: organization
org_type: advisory
title: "UC Berkeley RDI"
address: c-000324
created: 2026-08-31
updated: 2026-08-31
tags:
  - entities
  - organizations
  - academic
  - vuln-discovery
  - benchmarks
status: developing
website: "https://www.cybergym.io/"
focus: "Publishes and operates an open observatory of AI cybersecurity capability across the vulnerability lifecycle"
aliases:
  - "RDI"
  - "Berkeley RDI"
  - "Center for Responsible Decentralized Intelligence"
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
related:
  - "[[agentic-vulnerability-discovery]]"
  - "[[end-to-end-harness-evaluation]]"
  - "[[cybergym|CyberGym Benchmark]]"
  - "[[exploit-benchmarks|ExploitBench & ExploitGym]]"
  - "[[cybergym-e2e|CyberGym-E2E]]"
  - "[[arizona-state-university|Arizona State University]]"
  - "[[yan-shoshitaishvili|Yan Shoshitaishvili]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
sources:
  - "https://www.cybergym.io/"
  - "https://www.cybergym.io/cybergym/"
  - "https://www.cybergym.io/exploitgym/"
  - "https://www.cybergym.io/cybergym-e2e/"
  - ".raw/articles/cybergym-observatory-2026-08-31.md"
  - ".raw/articles/cybergym-benchmark-2026-08-31.md"
  - ".raw/articles/exploitgym-2026-08-31.md"
  - ".raw/articles/cybergym-e2e-2026-08-31.md"
---

# UC Berkeley RDI

**UC Berkeley RDI**, the Center for Responsible Decentralized Intelligence, publishes and operates [cybergym.io](https://www.cybergym.io/) as an **observatory**: a site that self-describes as continuously and openly tracking AI's cybersecurity capabilities across the stages of attack and defense, so developers, researchers and policymakers can stay informed in a timely manner.[^observatory] It runs three benchmarks, one per stage of the vulnerability lifecycle, and takes feedback at `rdi_research@berkeley.edu`.[^observatory] Dawn Song is the senior author common to all three.

## Benchmarks

| Benchmark | Stage | Scale |
|---|---|---|
| [[cybergym\|CyberGym]] | Vulnerability reproduction | 1,507 instances across 188 OSS-Fuzz projects |
| [[exploit-benchmarks\|ExploitGym]] | Exploit generation | 869 instances: 502 userspace, 181 V8, 186 Linux kernel |
| [[cybergym-e2e\|CyberGym-E2E]] | End-to-end discover-and-patch | 920 tasks across 139 OSS projects, four cumulative stages |

CyberGym is published at ICLR 2026; CyberGym-E2E at ICML 2026. ExploitGym is a seven-organization effort: UC Berkeley, the Max Planck Institute for Security and Privacy, UC Santa Barbara, [[arizona-state-university|Arizona State University]], Anthropic, OpenAI and Google — with benchmark design and experimental methodology attributed to the academic authors and Anthropic, OpenAI and Google credited with providing model access and feedback.[^exploitgym] [[yan-shoshitaishvili|Yan Shoshitaishvili]] of Arizona State University is a named co-author of ExploitGym; he does not appear on the CyberGym or CyberGym-E2E author lists.

The site cross-links a separate RDI analysis, "Frontier AI's Impact on the Cybersecurity Landscape", and a live leaderboard site it calls the "Frontier AI Cybersecurity Observatory", at `rdi.berkeley.edu/frontier-ai-impact-on-cybersecurity/benchmarks.html`.[^observatory] The observatory's own leaderboard tables render client-side and carry no score readable from a static fetch, so this page states no leaderboard rank or score.

## See also

- [[cybergym|CyberGym Benchmark]] — the reproduction-stage benchmark.
- [[exploit-benchmarks|ExploitBench & ExploitGym]] — the exploit-generation benchmark, jointly authored with Arizona State University among others.
- [[cybergym-e2e|CyberGym-E2E]] — the end-to-end discover-and-patch benchmark.
- [[arizona-state-university|Arizona State University]] — the wiki's other non-vendor primary source on this axis, overlapping with RDI on ExploitGym's author list.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the wiki thesis this observatory's three benchmarks anchor.
- [[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]] — the method page CyberGym's reproduction and open-ended-discovery results underwrite.
- [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]] — the method page CyberGym-E2E's four-stage validation ladder underwrites.

[^observatory]: UC Berkeley RDI, [CyberGym observatory front page](https://www.cybergym.io/#benchmarks) (fetched 2026-08-31), which carries the observatory self-description, the feedback address and the three-benchmark index, and the [CyberGym benchmark page](https://www.cybergym.io/cybergym/), which carries the cross-links to RDI's own analysis and leaderboard and the ICLR 2026 publication ([OpenReview `2YvbLQEdYt`](https://openreview.net/forum?id=2YvbLQEdYt)). Local copies: `.raw/articles/cybergym-observatory-2026-08-31.md`, `.raw/articles/cybergym-benchmark-2026-08-31.md`.
[^exploitgym]: UC Berkeley RDI, [ExploitGym](https://www.cybergym.io/exploitgym/) (fetched 2026-08-31); [arXiv:2605.11086](https://arxiv.org/abs/2605.11086). Local copy: `.raw/articles/exploitgym-2026-08-31.md`.
