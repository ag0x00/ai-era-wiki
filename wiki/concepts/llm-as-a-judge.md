---
type: concept
title: "LLM-as-a-Judge"
created: 2026-04-30
updated: 2026-07-26
tags:
  - concepts
  - evaluation
  - llm-as-a-judge
  - security-agents
status: developing
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
complexity: intermediate
domain: evaluation
aliases:
  - "LLM-as-a-Judge"
  - "model-as-judge"
source_url: "https://arxiv.org/abs/2306.05685"
related:
  - "[[evidence-centered-benchmark-design]]"
  - "[[clasp]]"
  - "[[guardrails-beyond-vibes-talk]]"
  - "[[hitl]]"
  - "[[measuring-agent-effectiveness-talk]]"
  - "[[osint-to-knowledge-graph-talk]]"
  - "[[syara-semantic-detection-talk]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
sources:
  - ".raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_transcript.md"
  - ".raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_slides.pdf"
---

# LLM-as-a-Judge

An evaluation methodology in which a language model scores or rubric-grades the output of another (typically agentic) system. The judge LLM is used in place of deterministic matching (string comparison, keyword patterns, taxonomy labeling) when the target output is subjective, open-ended, or semantically rich — i.e., when the *meaning* of an output matters more than its surface form.

Referenced from [[clasp|CLASP]] as one of two rubric-application approaches for capability scoring.

## The core tradeoff

| Approach | Mechanism | Limitation |
|---|---|---|
| **Deterministic matching** | Compare MITRE categories; pattern-match keywords | "Correct risk, wrong label = failure" — same risk identified, different label across two runs counts as wrong |
| **LLM-as-a-Judge** | Evaluate semantic equivalence between expected and actual output | Introduces a circular dependency: the judge may share the failure modes of the agent under evaluation |

## The circularity problem and its standard resolution

The fundamental tension: LLM-as-a-Judge is used precisely because the output cannot be deterministically verified. But if the LLM judge has the same weaknesses as the agent (hallucination, reasoning gaps), why trust it to evaluate correctly?

The standard resolution (as applied at [[stripe|Stripe]] in their threat modeling agent — see [[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes]]) is a **division of labor**:

1. **Humans write the gold standard.** Domain experts curate past examples of high-quality outputs (e.g., complete, correct threat models from past security reviews). Human judgment defines *what a correct answer looks like*.
2. **The LLM is tasked only with semantic matching.** Given an expected output (gold standard) and an actual output, the judge is asked: *are these semantically equivalent in terms of the risks and mitigations conveyed?* This is a narrower, more tractable task than generating a correct answer.

This isolates the judge's error mode to *semantic similarity assessment* rather than *domain correctness* — a task LLMs are demonstrably better at than humans for high-volume scoring.

## Uses of the eval pipeline

The [[guardrails-beyond-vibes-talk|Stripe threat modeling case]] illustrates three distinct operational uses beyond basic accuracy measurement:

1. **Prompt engineering guidance** — low-scoring test cases highlight where the prompt fails; improvements are guided by the distribution of failures, not individual edge cases. Avoids overfitting.
2. **Model selection** — when choosing between base LLM models, duplicating the golden test set (to average out non-determinism) and running all candidates through the same scorer gives an empirical comparison. At Stripe, this process yielded +10% accuracy improvement.
3. **Regression detection** — the most important use. A prompt change that *looks fine on individual runs* (correctly formatted JSON output) can *reduce overall accuracy by 10%* (agent attends to formatting at the expense of security content). The eval pipeline surfaces this; individual inspection does not.

**Regression detection is the primary value.** The [[guardrails-beyond-vibes-talk|Stripe talk]] is explicit: "This eval pipeline really gives us confidence in the changes we make to our prompt in the sense that they're applying generally speaking, rather than just in minute cases." The pipeline is the ground-truth check against which all prompt modifications are tested — not just a one-time accuracy measurement.

## LLM-as-a-Judge and human-in-the-loop

LLM-as-a-Judge addresses evaluation confidence; it does not replace human review of agent outputs in production. The Stripe team treats these as complementary:

- **LLM-as-a-Judge** scores the agent against a gold standard → produces a confidence/accuracy number used to gate releases and detect regressions.
- **Human-in-the-loop (HITL)** reviews actual agent outputs before they affect real workflows → provides the final quality gate and discovers failure modes not covered by the golden set.

"Eval pipelines validate — humans still discover." See [[hitl|Human-in-the-Loop (HITL) for Agentic AI]].

## As a regression gate in patch generation

A second production pattern uses the judge as a release gate on generated code rather than as an evaluation harness. Google's [[codemender|CodeMender]] applies an LLM judge to check that a security patch preserves the functional behavior of the code it modifies, alongside static analysis, differential testing, and fuzzing. The [[google-cloud-codemender-preview|Google Cloud preview]] carries the same check into the shipped remediate stage, where it screens a patch for functional disruption before the diff reaches a developer.

The circularity problem applies here in a weaker form. The judge is not asked whether the patch is *secure* (the scan and verify stages establish that) but whether two versions of a function are semantically equivalent outside the fixed defect. That is the same narrowed semantic-matching task the [[guardrails-beyond-vibes-talk|Stripe pattern]] isolates, and it has a stronger ground truth than most: the original code is the gold standard, and the test suite is an independent check on the judge's verdict. Google publishes no data on how often the judge is right.

## Contraindications

- **Open-ended routing tasks** with no fixed ground truth (e.g., "which security team should handle this question?"). In this case, a gold standard is hard to define and user feedback in production is a stronger signal. The Stripe security routing agent used a **phased user-feedback rollout** instead of an offline LLM-as-a-Judge pipeline.
- **Tasks with reliable deterministic signals** — if keyword matching or structured schema validation suffices, LLM-as-a-Judge adds cost and latency without benefit.

## See also

- [[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]]
- [[clasp|CLASP]] — uses LLM-as-a-Judge as one of its capability-scoring mechanisms
- [[guardrails-beyond-vibes-talk|Guardrails Beyond Vibes]] — first detailed production case study in this wiki
- [[hitl|Human-in-the-Loop (HITL) for Agentic AI]]
- Three Unprompted talks as production uses: [[measuring-agent-effectiveness-talk|Saxe]] calibrates a judge with ~100 samples and a Bayesian disagreement-weighting model; [[osint-to-knowledge-graph-talk|Sun]] runs a production judge secondary to a curated eval set; [[syara-semantic-detection-talk|SYARA]] uses an LLM matcher as the most expensive layer of a cost-ordered cascade.

<!-- sources:auto -->
## Sources

- [LLM-as-a-Judge](https://arxiv.org/abs/2306.05685)
<!-- /sources -->
