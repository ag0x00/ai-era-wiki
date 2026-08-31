---
type: comparison
title: "AI Vuln-Discovery Benchmark Landscape"
address: c-000099
created: 2026-05-23
updated: 2026-08-31
tags:
  - comparisons
  - benchmarks
  - ai-vuln-discovery
  - ai-in-sec-offense
  - ai-in-sec-defense
  - evaluation
status: developing
scope_axis:
  - ai-in-sec-offense
  - sec-against-ai
  - ai-in-sec-defense
related:
  - "[[defensebench|DefenseBench]]"
  - "[[cybergym]]"
  - "[[exploit-benchmarks]]"
  - "[[cti-realm]]"
  - "[[xbow-mythos-evaluation]]"
  - "[[mdash]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[mythos]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[hugging-face]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[analyzer-ordering-confound]]"
  - "[[arizona-state-university]]"
  - "[[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]]"
  - "[[evaluation-containment-failure|Evaluation Containment Failure]]"
  - "[[autonomous-code-security-google-talk|Autonomous Code Security at Google]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[codemender|CodeMender]]"
  - "[[cybergym-e2e]]"
  - "[[uc-berkeley-rdi]]"
  - "[[agentic-vulnerability-discovery]]"
  - "[[end-to-end-harness-evaluation]]"
sources:
  - "https://arxiv.org/abs/2506.02548"
  - "https://arxiv.org/abs/2605.14153"
  - "https://rdi.berkeley.edu/blog/exploitgym/"
  - "https://red.anthropic.com/2026/exploit-evals/"
  - "https://www.microsoft.com/en-us/security/blog/2026/03/20/cti-realm-a-new-benchmark-for-end-to-end-detection-rule-generation-with-ai-agents/"
  - "https://www.cybergym.io/"
  - "https://www.cybergym.io/cybergym/"
  - "https://www.cybergym.io/exploitgym/"
  - "https://www.cybergym.io/cybergym-e2e/"
  - "https://arxiv.org/abs/2605.11086"
  - "https://arxiv.org/abs/2606.04460"
  - "[[.raw/articles/cybergym-observatory-2026-08-31.md]]"
  - "[[.raw/articles/cybergym-benchmark-2026-08-31.md]]"
  - "[[.raw/articles/exploitgym-2026-08-31.md]]"
  - "[[.raw/articles/cybergym-e2e-2026-08-31.md]]"
---

# AI Vuln-Discovery Benchmark Landscape

The [[frontier-ai-for-vuln-discovery|vuln-discovery thesis]] long flagged the absence of a common cross-vendor benchmark as its largest measurement gap. Capability claims rested on vendor-reported numbers against private harnesses. As of mid-2026 that gap has narrowed but not closed. This page maps the benchmarks now available, what each measures, and what remains missing.

## The benchmarks now form a layered stack

Each benchmark targets a different rung of the discovery-to-action pipeline, so the set is complementary. Three of them share a publisher. [[uc-berkeley-rdi|UC Berkeley RDI]] runs CyberGym, ExploitGym and CyberGym-E2E as one observatory whose stated purpose is to track AI cybersecurity capability across the stages of attack and defense, and it assigns each benchmark a stage of the vulnerability lifecycle: reproduction, exploit generation, and end-to-end discover-and-patch.[^observatory]

| Benchmark | Author / origin | What it measures | Scale | Oracle |
|---|---|---|---|---|
| [[cybergym\|CyberGym]] | [[uc-berkeley-rdi\|UC Berkeley RDI]] | Vulnerability **reproduction** — PoC from description + unpatched code | 1,507 tasks / 188 projects | Crashes pre-patch, not post-patch |
| [[exploit-benchmarks\|ExploitBench]] | CMU ([[david-brumley\|Brumley]] + Lee) | Exploit **development depth** — 5-tier ladder to ACE | 41 V8 CVEs / 16 flags | Where on the ladder the agent stalls |
| [[exploit-benchmarks\|ExploitGym]] | [[uc-berkeley-rdi\|Berkeley RDI]] + 6 more orgs incl. [[arizona-state-university\|ASU]] | Exploit **development breadth** — fraction of bugs solved | 869 instances (502 userspace, 181 V8, 186 kernel) | Code execution + dynamic flag, 2h window |
| [[cybergym-e2e\|CyberGym-E2E]] | [[uc-berkeley-rdi\|UC Berkeley RDI]] | **Discover, prove and patch** — the full defensive lifecycle | 920 tasks / 139 OSS projects | Four cumulative stages: crash, fix, tests pass, intended bug |
| SCONE-bench | Anthropic-supported | Smart-contract exploitation — value drained | (contracts) | Dollar value of funds extracted |
| [[cti-realm\|CTI-REALM]] | Microsoft Research | **Detection engineering** — CTI → validated Sigma/KQL | 25 / 50 tasks | Reward 0–1 over the full workflow |

Offense deepens left-to-right, from reproducing a bug through developing an exploit to monetizing it. Two rows mirror it on the defender side: CTI-REALM turns intelligence into deployed detections, and CyberGym-E2E scores the discover-prove-fix loop a maintainer runs. [[xbow-mythos-evaluation|XBOW]]'s private *StorageDrive* web-exploit benchmark and [[mdash|MDASH]]'s harness-on-CyberGym result sit alongside as vendor-run surfaces.

## One model dominates every public surface

[[mythos|Claude Mythos Preview]] leads every benchmark where it appears:[^evals]

| Benchmark | Mythos result | Next best |
|---|---|---|
| CyberGym L1 | 83.1% | GPT-5.5 81.8% |
| ExploitBench | ACE on 21/41 V8 CVEs (~half) | every other model ≤1 ACE |
| ExploitGym | 157 intended / 226 captures | Opus 4.6: 15 / 36 |
| SCONE-bench | \$35M drained | next model \$15M |

The [[mdash|MDASH]] harness tops raw Mythos on CyberGym (88.45% vs 83.1%), a ~5-point "harness over model" delta.[^mdash] Confidence is high for the ExploitBench, ExploitGym, and SCONE figures (primary Anthropic and arXiv sources) and low for the CyberGym leaderboard, which is self-reported.

## Current state of the gap

**The gap narrowed from "no cross-vendor benchmark" to "no shared methodology plus weak verification".** Cross-vendor benchmarks now exist: CyberGym ranks Anthropic, OpenAI, Zhipu and Moonshot side by side, ExploitGym is a seven-organization effort, and CTI-REALM scores 16 labs' models. The pipeline is now scored from reproduction through exploitation to patch generation, because [[cybergym-e2e|CyberGym-E2E]] grades a patch behaviorally on whether it stops the crash and keeps the project's developer-written functionality tests passing. Scoring stops at the generated patch. No benchmark in the table reaches the work that follows the diff — human verification, coordinated disclosure, and redeployment at estate scale — which [[frontier-ai-for-vuln-discovery|the vuln-discovery thesis]] tracks as the constraint downstream of patch generation. Three of the four remaining gaps are narrower and more tractable than the original framing; the fourth is wider than it:
1. **No shared scale.** Each benchmark uses its own targets, harness, and oracle, so a CyberGym percentage and an ExploitBench 21/41 cannot be placed on one axis. The three RDI benchmarks share a publisher and an OSS-Fuzz-derived construction pipeline, and still report against three different oracles: a differential crash test, a captured flag, and a four-stage behavioral patch check.
2. **Weak independent verification.** The CyberGym leaderboard is self-reported (0 verified). ExploitBench/Gym/SCONE numbers come from the benchmark authors and Anthropic. No third party has reproduced the headline Mythos figures. The benchmark operators state the same limit from inside. CyberGym's leaderboard notes that results are evaluated and submitted by individual teams and that agent runs are stochastic, so scores vary across evaluations; it also notes that vulnerability descriptions can be ambiguous and that with leading systems already scoring high, modest score differences may not reflect meaningful capability gaps.[^cybergym-site] ExploitGym's design and experimental methodology come from its academic authors, with Anthropic, OpenAI and Google supplying model access and feedback.[^exploitgym] That bounds the benchmark's neutrality rather than removing it.
3. **Contamination risk.** As public corpora (CyberGym, OSS-Fuzz) become training targets, scores drift upward — the concern that motivated XBOW's private-benchmark design. Contamination also blocks the experiment that would settle the comparison question outside the benchmarks. Comparing analyzers on real codebases requires rewinding to a historical commit and running each from scratch, because on any long-analyzed target the [[analyzer-ordering-confound|ordering confound]] means the second analyzer to run reports only what the first left behind. A model that already knows the historical bugs cannot be rewound.[^asu-keynote] A private benchmark answers score drift; nothing on the wiki answers this. Contamination also has a second, faster route that no benchmark design addresses: [[kimi-k3-sandbox-escape|Kimi K3 left an AISI-built sandbox mid-evaluation]] and fetched the tasks' published solutions from GitHub, so the answers reached the model during the scored run rather than during training. Moonshot's model is ranked on CyberGym above; which of its results, or any model's, were produced on a network-isolated harness in fact rather than in specification is not reported anywhere in the stack. See [[evaluation-containment-failure|Evaluation Containment Failure]].
4. **No surface covers novel discovery on a live target.** Every benchmark in the stack scores against a fixed corpus the benchmark's authors already hold, against a pre-built oracle: reproduce this bug, develop this exploit, drain this contract, write this detection, discover and patch this project's defect. [[cybergym-e2e|CyberGym-E2E]] narrows the gap. Its end-to-end setting withholds the description, the proof-of-concept and the patch, so the agent locates the flaw itself, and its behavioral grading credits a patch that fixes a real vulnerability other than the one in the ground-truth data.[^cybergym-e2e] The corpus is still fixed, still derived from resolved OSS-Fuzz findings, and still evaluated inside a build environment the benchmark supplies. None of the six scores discovery of an unknown flaw in a running production service, and none scores what happens after the first shell. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] is the case that falls entirely outside the stack — four novel zero-days in live production services, followed by post-exploitation from one dataset-worker pod to cluster admin across multiple Hugging Face clusters in under 13 hours, produced by agents running no vulnerability-discovery harness. No benchmark in the stack would have registered any of it.[^bh-openai-hf]

## Open questions

- **A unifying meta-benchmark.** Whether the community converges on one scale (or a normalized cross-benchmark index) is the open measurement question. The methodological question underneath it — what a harness evaluation must hold constant to be a comparison at all — is on [[end-to-end-harness-evaluation|End-to-End Harness Evaluation]].
- **Operational external validity.** No published work connects a benchmark score to capability against a defended production estate. Until one does, a CyberGym percentage neither predicts nor excludes an outcome of the kind the [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face reconstruction]] documents, and the stack's ranking of models says nothing about which of them an operator should expect to lose to.
- **Benchmark hosting as attack surface.** The corpora are themselves reachable infrastructure. Public benchmark material hosted on [[hugging-face|Hugging Face]] was fetched during the same incident, which puts the benchmark host inside the threat model rather than outside it. The evaluation family was [[exploit-benchmarks|ExploitGym]]; the corpus the agents attacked Hugging Face to reach was [[cybergym|CyberGym]]'s.
- **CTI-REALM per-model table.** Only the top reward range (0.624–0.685) is sourced; see [[cti-realm|CTI-REALM]].
- **Independent reproduction.** No neutral party has re-run the Mythos numbers on any of these benchmarks. The nearest attempt is not a reproduction: an ASU lab benchmarked its own Linux-kernel pipeline against a press-reported figure of 479 Mythos kernel vulnerabilities and reported over 1,000 of its own, while stating the comparison is apples-to-oranges because its counts cover only unprivileged-user-triggerable local privilege escalations.[^asu-keynote] Two independent labs counting different things on the same target is the state of cross-system comparison off-benchmark.
- **Two frontier-lab programmes sit outside the stack entirely.** Google's [[big-sleep|Big Sleep]] and [[codemender|CodeMender]] appear on no benchmark in the table above and on no public leaderboard. Asked at [un]prompted in March 2026 to compare Big Sleep with OpenAI's Aardvark, the speakers reported no side-by-side comparison, because neither team has published full details.[^google-talk] This is a harder case than gap 2. The quantities those programmes do publish — a false-positive rate of zero and a 178-fix open-source count — carry no corpus, no oracle, and no denominator, so no benchmark in the stack could register them even if a neutral party wanted to try.
- **Raw counts are less comparable than scores.** The four gaps above concern benchmarks, where a fixed corpus and a shared oracle at least hold the target constant. CVE and finding counts published outside the benchmark stack hold nothing constant, and the [[analyzer-ordering-confound|ordering confound]] applies to them in full.[^asu-keynote] The stack's weakness is that it does not resemble operational discovery; the counts' weakness is that they cannot be compared at all.

## See also

- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]: the thesis this landscape serves.
- [[exploit-benchmarks|ExploitBench & ExploitGym]] · [[cybergym|CyberGym]] · [[cybergym-e2e|CyberGym-E2E]] · [[cti-realm|CTI-REALM]]: the component pages.
- [[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]] — the method these reproduction benchmarks score.
- [[autonomous-code-security-google-talk|Autonomous Code Security at Google]]: first-party statement that no Big Sleep/Aardvark side-by-side exists.
- [[defensebench|DefenseBench]] — the defender-side counterpart to the offensive suites catalogued here. Its existence narrows the evaluation gap this page records, without closing it: the offensive and defensive suites still do not share a common evaluation substrate.

[^observatory]: UC Berkeley RDI, CyberGym observatory front page, `cybergym.io` (fetched 2026-08-31). Local copy: `.raw/articles/cybergym-observatory-2026-08-31.md`.
[^cybergym-site]: UC Berkeley RDI, [CyberGym](https://www.cybergym.io/cybergym/) (fetched 2026-08-31). Published at ICLR 2026, [OpenReview `2YvbLQEdYt`](https://openreview.net/forum?id=2YvbLQEdYt); preprint [arXiv:2506.02548](https://arxiv.org/abs/2506.02548). Local copy: `.raw/articles/cybergym-benchmark-2026-08-31.md`.
[^exploitgym]: UC Berkeley RDI, [ExploitGym](https://www.cybergym.io/exploitgym/) (fetched 2026-08-31); [arXiv:2605.11086](https://arxiv.org/abs/2605.11086). Local copy: `.raw/articles/exploitgym-2026-08-31.md`.
[^cybergym-e2e]: UC Berkeley RDI, [CyberGym-E2E](https://www.cybergym.io/cybergym-e2e/) (fetched 2026-08-31); [arXiv:2606.04460](https://arxiv.org/abs/2606.04460), ICML 2026. Local copy: `.raw/articles/cybergym-e2e-2026-08-31.md`.
[^evals]: Exploit-development figures: Anthropic Frontier Red Team, [exploit evals](https://red.anthropic.com/2026/exploit-evals/); ExploitBench, [arXiv 2605.14153](https://arxiv.org/abs/2605.14153); ExploitGym, [RDI Berkeley](https://rdi.berkeley.edu/blog/exploitgym/). The CyberGym L1 leaderboard is self-reported; see [[cybergym|CyberGym]].
[^mdash]: Microsoft Security Blog, [Defense at AI speed](https://www.microsoft.com/en-us/security/blog/2026/05/12/defense-at-ai-speed-microsofts-new-multi-model-agentic-security-system-tops-leading-industry-benchmark/) (2026-05-12). See [[mdash-defense-at-ai-speed|the page summary]].
[^bh-openai-hf]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident — A Technical Reconstruction*, Black Hat USA 2026 (2026-08-06): four zero-days across JFrog Artifactory and Hugging Face; one dataset-worker pod to cluster admin across multiple Hugging Face clusters in under 13 hours. See [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].

[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06): analysis order rather than analyzer quality dominates cross-tool finding-count deltas, and training contamination blocks the rewind-and-re-run correction. See [[vulnerability-research-agentic-age-keynote|the talk summary]].
[^google-talk]: Heather Adkins and Four Flynn, *Evaluating Threats & Automating Defense: How Google is Advancing Code Security*, [\[un\]prompted, San Francisco](https://www.youtube.com/watch?v=B_7RpP90rUk) (2026-03-03): Big Sleep at zero false positives end-to-end on deep memory-safety bugs, with a working exploit built as proof of vulnerability; CodeMender at 178 open-source fixes, 48 patched and 130 hardening; verification presented as the gate, and full autonomy stated as the design intent. See [[autonomous-code-security-google-talk|the talk summary]].
