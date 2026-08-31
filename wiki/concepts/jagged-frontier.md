---
type: concept
title: "Jagged Frontier (AI Cybersecurity Capability)"
address: c-000155
created: 2026-05-26
updated: 2026-08-31
tags:
  - concepts
  - vulnerability-discovery
  - model-capability
  - evaluation
  - ai-vuln-discovery
status: developing
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
related:
  - "[[ai-cybersecurity-after-mythos-jagged-frontier]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[mythos]]"
  - "[[aisle]]"
  - "[[stanislav-fort]]"
  - "[[trajectory-aware-post-training-talk]]"
  - "[[vulnerability-research-agentic-age-keynote]]"
  - "[[exploit-benchmarks]]"
  - "[[uc-berkeley-rdi]]"
  - "[[autonomous-exploit-generation]]"
sources:
  - "[[ai-cybersecurity-after-mythos-jagged-frontier]]"
  - "https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier"
  - "https://www.cybergym.io/exploitgym/"
  - ".raw/articles/exploitgym-2026-08-31.md"
---

# Jagged Frontier (AI Cybersecurity Capability)

AI capability is **jagged**: it does not rise smoothly with model size, model generation, or price. A model strong on one task can fail an easier-looking adjacent task, and the capability ranking across models reshuffles from task to task. The term comes from knowledge-work productivity research (Dell'Acqua et al., "Navigating the Jagged Technological Frontier," 2023); [[stanislav-fort|Stanislav Fort]] ([[aisle|AISLE]]) applies it to AI cybersecurity capability.

## Evidence in cybersecurity

Fort tested the showcase vulnerabilities from Anthropic's [[mythos|Claude Mythos]] announcement on small, cheap, open-weights models once the relevant function was isolated.[^aisle]

- **Detection is commoditized.** A straightforward FreeBSD NFS stack overflow (Mythos's flagship find) was detected by every model tested, including one with ~3.6 billion active parameters costing about \$0.11 per million tokens.[^aisle]
- **Hard reasoning separates models, but not by size.** The 27-year-old OpenBSD TCP SACK bug, which needs reasoning about signed-integer overflow, was recovered in full by a ~5.1B-active open model (GPT-OSS-120b), while a larger 32B dense model declared the same code "robust."[^aisle]
- **Near-inverse scaling appears.** On a trivial OWASP false-positive task (a Java servlet that only looks injectable), small open models outperformed most frontier models from every major lab.[^aisle]

There is no stable "best model for cybersecurity". The capability ranking is task-dependent, and the divergence survives the move from isolated-function probes to end-to-end agentic tasks.

### The divergence holds at end-to-end agentic scale

ExploitGym measures the same task-dependence with whole agents on whole exploitation tasks rather than with single calls on isolated functions. Claude Mythos Preview and GPT-5.5 lead the benchmark, and their success sets diverge: **56 targets are solved only by Mythos Preview, 26 only by GPT-5.5, and 91 by both**. The remaining models contribute another 61 successes, four of them unique to that group.[^exploitgym] The benchmark authors read the split as the models relying on qualitatively different exploitation strategies, and state that an ensemble would substantially expand coverage.

The ranking and the coverage therefore answer different questions, and 82 of the targets the two leading models solve are solved by exactly one of them. The pipeline consequence is on [[autonomous-exploit-generation|Autonomous Exploit Generation]].

## Sensitivity versus specificity

Jaggedness also shows up between finding bugs and recognizing safe code. Across the tested suite, sensitivity was high (models find the bug in vulnerable code) but specificity was uneven (many of the same models false-positive on the *patched* version, fabricating an incorrect signed-integer bypass). Only one model in the suite was reliable in both directions.[^aisle] The gap is the argument for a triage and validation layer around the model rather than trust in raw model output.

## Significance

If capability is jagged and much of the detection work is commoditized, the design consequence is to deploy cheap models *broadly*, scanning everything and compensating for lower per-token intelligence with coverage, rather than betting on one expensive model to look in the right place. This is the capability premise behind the claim that in AI cybersecurity the moat is the system, not the model (see [[ai-cybersecurity-after-mythos-jagged-frontier|the AISLE analysis]]). It also bounds the [[frontier-ai-for-vuln-discovery|frontier-AI-for-vulnerability-discovery]] thesis: the discovery-and-analysis layer is broadly accessible today, while novel constrained-exploit construction is where frontier-scale capability still separates. [[trajectory-aware-post-training-talk|Brown's talk]] illustrates the same jaggedness in offensive security: general models handle atomic tasks (spot an LFI or XSS) but fail multi-stage vulnerability chaining, motivating task-specific post-training.

[[vulnerability-research-agentic-age-keynote|Shoshitaishvili's cookie-cutter model]] frames the same task-dependence from the analyzer side rather than the model side: an analyzer's fixed target shape explains why finding counts reshuffle across bug classes, a complementary account of jaggedness at the harness level.[^asu-keynote]

## Notes

[^aisle]: Stanislav Fort (AISLE), "AI Cybersecurity After Mythos: The Jagged Frontier" (2026). [aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier](https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier). Figures and per-model results are from the post's published test tables and linked transcripts; these are scoped single-call probes on isolated functions, not end-to-end autonomous discovery.
[^asu-keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
[^exploitgym]: UC Berkeley RDI, [ExploitGym](https://www.cybergym.io/exploitgym/) (fetched 2026-08-31); [arXiv:2605.11086](https://arxiv.org/abs/2605.11086). Local copy: `.raw/articles/exploitgym-2026-08-31.md`.

<!-- sources:auto -->
## Sources

- [aisle.com](https://aisle.com/blog/ai-cybersecurity-after-mythos-the-jagged-frontier)
- [cybergym.io](https://www.cybergym.io/exploitgym/)
<!-- /sources -->
