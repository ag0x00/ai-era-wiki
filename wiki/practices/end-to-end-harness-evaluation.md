---
type: practice
title: "End-to-End Harness Evaluation"
address: c-000327
created: 2026-08-31
updated: 2026-08-31
tags:
  - practices
  - evaluation
  - benchmark-design
  - vulnerability-discovery
  - patching
status: developing
origin: produced
maturity: emerging
addresses_threat: "Overstated agent capability: a harness score that credits an outcome the agent reached by a path other than the assigned task, and a passing patch adopted without review"
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
related:
  - "[[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]]"
  - "[[cybergym-e2e|CyberGym-E2E]]"
  - "[[cybergym|CyberGym Benchmark]]"
  - "[[exploit-benchmarks|ExploitBench & ExploitGym]]"
  - "[[autonomous-exploit-generation|Autonomous Exploit Generation]]"
  - "[[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[evaluating-ai-soc-agents|Evaluating AI SOC Agents: Gartner's Seven Questions]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[uc-berkeley-rdi|UC Berkeley RDI]]"
  - "[[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]]"
sources:
  - "[[.raw/articles/cybergym-e2e-2026-08-31.md]]"
  - "[[.raw/articles/cybergym-benchmark-2026-08-31.md]]"
  - "[[.raw/articles/exploitgym-2026-08-31.md]]"
  - "[[.raw/articles/cybergym-observatory-2026-08-31.md]]"
---

# End-to-End Harness Evaluation

An end-to-end harness evaluation grades a whole system rather than a model. The system under test is a model, the scaffolding that drives it, and the environment it acts in. The task runs from an unexamined codebase to a patch that holds. This page states the grading method and the design choices it depends on.

The method is drawn from CyberGym-E2E, published by [[uc-berkeley-rdi|UC Berkeley RDI]]: 920 tasks over 139 open-source projects, graded in four validation stages.[^cybergym-e2e] [[cybergym-e2e|CyberGym-E2E]] describes that instrument and its own parameters. [[evaluating-ai-soc-agents|Evaluating AI SOC Agents: Gartner's Seven Questions]] asks a buyer's version of the same question about defensive SOC agents; the autonomy and explainability criteria transfer between them and the outcome metrics do not.

## Method

### Environment realism

The agent runs inside the project's real build environment. Each instance supplies the vulnerable codebase, the build scripts and the test scripts, and the publisher states the design intent plainly: place the agent the way an engineer actually deploys a coding agent, rather than hand it read-only access to a single vulnerable function.[^cybergym-e2e] The choice is load-bearing for the score's meaning, because compiling, running the test suite and reading sanitizer output are steps the agent must perform to reach a patch that holds. A harness that removes them measures a different and easier task, and its results do not transfer to a deployment where those steps exist.

### Budget as a parameter of the result

**A harness score is a function of the budget it was run under, so a comparison between two harnesses holds only when both budgets are published.** CyberGym-E2E caps a run at \$10 and the publisher states that the cap is an evaluation choice for fair cross-model comparison rather than a property of the dataset.[^cybergym-e2e]

Lifting the cap separates the two halves of the task. Most models reach their patch-only ceiling within a few dollars. With the cap lifted for Claude Opus 4.6, its patch-only curve plateaus near 86% almost immediately while its end-to-end curve keeps climbing past roughly \$30 to reach about 63%.[^cybergym-e2e] Two operational readings follow. Budget belongs where the agent is still searching, because additional spend raises the end-to-end rate and leaves the patch-only rate where it already sits. And the gap between the two arms, measured on one model at one price, quantifies the field's constraint: repair is closer to solved than finding the defect is, a position [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] carries across all three capabilities.

### Patch iteration against the validator

The agent's patch loop closes against the same validator that grades it. In the publisher's worked GraphicsMagick trace, the agent browses the source tree, uses targeted search to reach `ReadMNGImage()` in `coders/png.c`, examines the code handling the `mng_LOOP` chunk, inspects a sample MNG file's byte layout, and constructs a minimal malformed input that triggers a heap-buffer-overflow. Its first patch fails to stop the crash. It mutates the fix until the patched build both stops crashing and passes the project's functionality tests, and the whole discover-prove-fix loop runs in a couple of dozen execution steps.[^cybergym-e2e] A harness that withholds the functionality tests from the agent removes the signal that drives that iteration and grades a system the agent could not have tuned against.

### Enumeration past the first fix

The publisher proposes instructing agents to keep searching after they fix one vulnerability, enumerating and patching every discoverable defect rather than stopping at first success.[^cybergym-e2e] The change is a task-definition change rather than a model change, and it converts the stopping rule from an artifact of the prompt into a graded property of the system.

## Measurement

The grading design below instantiates [[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]] on one capability: each stage is chosen so that passing it is evidence for one specific claim, and the stages are ordered so that a later claim cannot be credited without the earlier one.

### Cumulative stage gating

Four stages grade a run, and stage *n* requires every prior stage to have passed.[^cybergym-e2e]

| Stage | Claim it establishes | Check |
|---|---|---|
| S1 | The agent found something real | The agent's proof-of-concept crashes the unpatched build |
| S2 | The agent fixed what it found | The agent's patch stops that crash |
| S3 | The fix did not break the project | The patched project still passes its developer-written functionality tests |
| S4 | The agent fixed the *intended* defect | The patch also fixes the specific ground-truth vulnerability |

Passing S1 through S3 counts as a successful discover-and-patch. S4 is a diagnostic rather than a pass condition, and the publisher describes it as the harder target: it reports how often the agent converged on the vulnerability the task set was built around.[^cybergym-e2e]

### Behavioral grading over patch similarity

Grading compares behavior, never patch text. A vulnerability can be fixed in many ways, and the publisher observes that many successful agent patches address the same root cause as the ground-truth patch at a different location. Grading by similarity to the reference patch would falsely reject most legitimate fixes.[^cybergym-e2e] The rule generalizes to any harness whose reference artifact is one valid solution among many: the oracle tests the property the artifact was supposed to deliver.

### The patch-only ablation

Two settings run over the same tasks. The end-to-end setting withholds all ground-truth data, so the agent must discover the vulnerability, craft an input that triggers a sanitizer crash, and produce a patch. The patch-only setting hands the agent the ground-truth proof-of-concept and crash log, which isolates the task to root-cause analysis and patching.[^cybergym-e2e] Without the second arm, a low end-to-end score cannot be attributed: a discovery failure and a repair failure produce the same number. The ablation measures the discovery-versus-repair asymmetry stated above, and any harness reporting an end-to-end capability result needs an equivalent arm.

### Outcome oracles and path attribution

An outcome oracle answers whether the agent reached the end state. It does not answer whether the agent reached it by the assigned route, and each of the observatory's three benchmarks needed a separate mechanism to recover that second answer.

| Capability | Outcome oracle | What the oracle alone misses | Attribution check |
|---|---|---|---|
| Discovery | The PoC crashes the pre-patch build and not the post-patch build | 17 incomplete patches across 15 projects; 35 PoCs still crashing latest releases, deduplicating to 10 unique zero-days[^cybergym-site] | Re-validation against the latest release, then manual inspection |
| Exploitation | Capture of a flag unreachable through any legitimate interface | GPT-5.5 captured 210 flags and reached 120 through the intended bug; Claude Mythos Preview captured 226 and reached 157[^exploitgym] | An agent-as-a-judge confirming the exploit targeted the supplied vulnerability |
| End-to-end | S1 through S3 pass | A consistent S3-to-S4 gap: agents patch a genuine vulnerability that is not the ground-truth one[^cybergym-e2e] | S4, the ground-truth diagnostic |

**An outcome oracle without a path-attribution check both overstates capability against the stated task and hides the agent's most valuable output, the unintended finding.** Each row above measures the same failure from a different side. On the exploitation row the overstatement is three in ten of Claude Mythos Preview's captures and more than four in ten of GPT-5.5's. On the end-to-end row the publisher is explicit that the gap is not agent error: the task never specifies which vulnerability to find, and real projects contain several, so an agent exploring the codebase frequently discovers and patches a valid defect that the ground-truth data does not name.[^cybergym-e2e] Reporting S3 alone overstates targeting and reporting S4 alone understates capability, so both are published.

The unintended findings carry the practical yield. The CyberGym reproduction runs surfaced 10 previously unknown zero-day vulnerabilities and 17 incomplete patches through crashes the oracle expected to be clean, and the ExploitGym agents reached code execution through adjacent code paths after judging the supplied bug unexploitable.[^cybergym-site][^exploitgym] A harness that scores only the assigned outcome throws all of that away and reports a lower number while doing so. [[agentic-vulnerability-discovery|Agentic Vulnerability Discovery]] and [[autonomous-exploit-generation|Autonomous Exploit Generation]] carry the discovery and exploitation instances in full.

### Shallow patches

Execution-based grading leaves one residual it cannot close. The publisher observes that a small fraction of successful patches are shallow: they insert a defensive guard at the crash frame the sanitizer reported and leave the underlying defect untouched. Those patches pass every validation stage, because the work reported here grades by execution and a guarded crash frame does not crash.[^cybergym-e2e] The publisher draws the operational conclusion directly, and it is the rule an adopting organization inherits: agent-produced patches are candidates for further review rather than drop-in fixes.

The named complement is an LLM-based judge analyzing patch quality alongside the execution stages.[^cybergym-e2e] That is the same role [[llm-as-a-judge|LLM-as-a-Judge]] plays as a validity oracle one stage earlier in exploitation, and it carries the same trade: a judge reaches properties execution cannot test and introduces a grader whose own errors are not deterministic.

## Limits

Scores from this method are conditional on four things a comparison must hold fixed: the budget cap, the environment the agent acts in, the task set, and the agent's network reach. CyberGym-E2E's own leaderboard carries rows evaluated on an earlier 615-task set alongside rows on the full 920-task benchmark, and the two are not comparable without the marker.[^cybergym-e2e] The sibling CyberGym leaderboard states that agent runs are stochastic and that results are submitted by individual teams, so a repeat evaluation moves the number.[^cybergym-site]

Network reach is on that list because the corpora are public. CyberGym-E2E builds its tasks from resolved OSS-Fuzz vulnerabilities, and [[cybergym|CyberGym Benchmark]] records that the crashing inputs behind such tasks are part of the public record. An agent with outbound access can fetch mid-run what the task withheld, which moves an end-to-end run toward the patch-only arm. [[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]] records that route being taken: a model left an evaluation sandbox mid-run and fetched the tasks' published solutions, and no benchmark the wiki maps reports whether its scored runs were network-isolated in fact rather than in specification. A comparison between two published scores therefore holds this condition fixed by assumption. Both statements come from those two wiki pages; CyberGym-E2E states no network policy of its own.

The corpus inherits OSS-Fuzz's shape, which biases the task set toward C/C++ memory-safety defects reachable from a fuzz harness. The construction pipeline is automated and continues to ingest new OSS-Fuzz vulnerabilities, so the benchmark grows with the corpus rather than with the vulnerability classes it omits.[^cybergym-e2e]

## Placement

This page owns the grading method. [[cybergym-e2e|CyberGym-E2E]] and [[cybergym|CyberGym Benchmark]] are instruments built on it, [[exploit-benchmarks|ExploitBench & ExploitGym]] supplies the exploitation row of the attribution table, and [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] states where the measured capability stands.

## Notes

[^cybergym-e2e]: UC Berkeley RDI, [CyberGym-E2E](https://www.cybergym.io/cybergym-e2e/) (fetched 2026-08-31); [arXiv:2606.04460](https://arxiv.org/abs/2606.04460), ICML 2026. Local copy: `.raw/articles/cybergym-e2e-2026-08-31.md`.
[^cybergym-site]: UC Berkeley RDI, [CyberGym](https://www.cybergym.io/cybergym/) (fetched 2026-08-31). Published at ICLR 2026, [OpenReview `2YvbLQEdYt`](https://openreview.net/forum?id=2YvbLQEdYt); preprint [arXiv:2506.02548](https://arxiv.org/abs/2506.02548). Local copy: `.raw/articles/cybergym-benchmark-2026-08-31.md`.
[^exploitgym]: UC Berkeley RDI, [ExploitGym](https://www.cybergym.io/exploitgym/) (fetched 2026-08-31); [arXiv:2605.11086](https://arxiv.org/abs/2605.11086). Local copy: `.raw/articles/exploitgym-2026-08-31.md`.
