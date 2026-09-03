---
type: concept
title: "Analyzer Ordering Confound"
address: c-000273
created: 2026-08-16
updated: 2026-09-01
tags:
  - concepts
  - benchmarks
  - vuln-discovery
  - ai-vuln-discovery
  - fuzzing
  - measurement
  - evaluation
status: developing
origin: aggregated
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
related:
  - "[[vulnerability-research-agentic-age-keynote|Vulnerability Research in the Agentic Age]]"
  - "[[vulnerability-properties|Vulnerability Properties]]"
  - "[[yan-shoshitaishvili|Yan Shoshitaishvili]]"
  - "[[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]]"
  - "[[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]]"
  - "[[cybergym|CyberGym Benchmark]]"
  - "[[exploit-benchmarks|ExploitBench & ExploitGym]]"
  - "[[jagged-frontier|Jagged Frontier (AI Cybersecurity Capability)]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
sources:
  - "https://www.youtube.com/watch?v=VNYe3Cnk5Pw"
  - ".raw/talks/2026-08-06_Yan-Shoshitaishvili_Vulnerability-Research-in-the-Agentic-Age_transcript.md"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# Analyzer Ordering Confound

## Definition

The finding-count difference between two vulnerability analyzers run against the same codebase is dominated by which one ran first. The apparent difference in strength is an ordering artifact: the first analyzer harvests every vulnerability both would have found, and the second reports only the residue plus whatever its own coverage adds. Reverse the order and the advantage reverses with it.

The term is the wiki's; the argument is [[yan-shoshitaishvili|Yan Shoshitaishvili]]'s, stated in his [[vulnerability-research-agentic-age-keynote|Black Hat USA 2026 keynote]] through a cookie-cutter analogy: the vulnerabilities in a program are rolled-out dough, and each analysis stamps out one shape and leaves the rest.[^keynote]

## Mechanism

An analysis targets a bounded set of [[vulnerability-properties|vulnerability properties]] — data flow, injection of attacker-controlled content, multi-threaded behavior, and so on. That set defines the shape it can cut. Two consequences follow directly.

**Exhaustion is not cleanliness.** Run a static analyzer, fix everything it reports, run it again, and it reports nothing. The code is not free of vulnerabilities. That analyzer's shape is spent, and its silence carries no information about the vulnerabilities it was never able to target.

**Marginal gains are ordering artifacts.** Prototype B, built slightly differently from prototype A, finds a small additional set. The increment is not evidence that B is the better analyzer; it is evidence that B ran second. The same inversion holds when one of the two passes is a human audit rather than a tool: whichever pass runs first takes the overlap.

Fuzzing is the partial exception, and the exception locates the mechanism. Because fuzzing is stochastic, re-running the same fuzzer on the same target does keep producing findings around corner cases the earlier run missed. Deterministic static analysis does not.

## Two waves of the same error

| Wave | Comparative claim | What ordering explains |
|---|---|---|
| Fuzzing renaissance, roughly 2013–2015 onward | Each new fuzzer beats its predecessors on already-fuzzed code | Later fuzzers are evaluated on corpora their predecessors have already thinned |
| Agentic-era LLM discovery, 2025–2026 | LLMs beat fuzzers, because an LLM finds bugs on code where a re-run fuzzer finds none | The LLM is the second pass over a decade of fuzzing output |

The decade of fuzzer-versus-fuzzer claims following the DARPA Cyber Grand Challenge is the reference case, and the keynote's point is that the field is now repeating it with a new class of analyzer rather than having learned from it.[^keynote]

Both waves and the exhaustion mechanism behind them come from this one keynote by one academic. The static-analyzer-exhaustion observation is uncontroversial in the vulnerability-research literature; the claim that it explains the fuzzing-renaissance and the LLM-versus-fuzzer comparisons is the speaker's, and the corrective study remains unpublished.

## Effect on the wiki's evidence base

The confound applies to every finding count on this wiki produced against long-lived, heavily analyzed software. It reaches the [[frontier-ai-for-vuln-discovery|frontier-AI-for-vuln-discovery]] axis directly: Project Glasswing's 27-year-old OpenBSD vulnerability, the AISLE OpenSSL cohort, and the Anthropic Frontier Red Team's 500-plus figure are all measured on codebases with deep prior analysis history, so their magnitudes carry an unmeasured ordering component. This does not make the counts wrong. It makes them uninterpretable as a *comparison* against the tools that ran before.

Ordering is one reason a cross-tool count does not compare, and a second reason is independently sourced. Semgrep's July 2026 survey of nine open-source harnesses reports that the definition of a finding varies across them, from a triaged static match through a verified pipeline candidate to a reproducible AddressSanitizer crash.[^semgrep] Where two tools count different objects, no ordering correction makes their totals commensurable, and the two reasons compound rather than substitute.

Benchmark-based comparisons are affected differently and less severely. [[cybergym|CyberGym]] and [[exploit-benchmarks|ExploitBench and ExploitGym]] score reproduction and exploit development against a fixed oracle rather than counting novel discoveries, so ordering between competing tools does not enter. They inherit the adjacent problem instead — the corpus is public, which is the contamination channel below.

## Training contamination blocks the correction

The correcting experiment is to rewind the timeline: take a codebase at a historical commit and run every analyzer against it from scratch, so no analyzer inherits another's harvest. The lab is attempting this and reports it is very difficult for a reason specific to language models. **The model already knows the bugs the fuzzer found.** Historical vulnerabilities in widely studied software are in the training corpus, so a model evaluated at a rewound commit is not naive about that commit.

This is a sharper statement of the contamination risk that [[ai-vuln-discovery-benchmark-landscape|the benchmark landscape]] already tracks. There, contamination degrades a benchmark score's validity. Here, it forecloses the experiment that would establish whether any of the field's comparative claims hold.

> [!gap] No confound-free comparison exists
> No source on the wiki reports a vulnerability-discovery comparison controlled for analysis order. The keynote's own Linux-kernel experiment sidesteps the problem rather than solving it: by holding the model generation fixed and varying only the harness, it measures the increment from workflow and from [[vulnerability-properties|vulnerability properties]] without needing a cross-tool baseline. That design is reusable and, on current evidence, is the only one available.

[^keynote]: Yan Shoshitaishvili, *Keynote: Vulnerability Research in the Agentic Age*, [Black Hat USA 2026](https://www.youtube.com/watch?v=VNYe3Cnk5Pw) (2026-08-06). See [[vulnerability-research-agentic-age-keynote|the talk summary]].
[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The finding-definition table is human-written. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
