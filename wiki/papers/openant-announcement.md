---
type: paper
title: "OpenAnt: LLM-Based Vulnerability Discovery"
address: c-000059
created: 2026-05-15
updated: 2026-08-21
tags:
  - papers
  - vuln-discovery
  - open-source
  - knostic
status: summarized
scope_axis: [ai-in-sec-defense]
year: 2026
authors:
  - "[[nahum-korda|Nahum Korda]]"
venue: "Knostic blog"
source_url: https://www.knostic.ai/blog/openant
archived_copy: ".raw/articles/openant-2026-05-15.md"
key_claim: "Verified vulnerability discovery via frontier LLMs becomes operationally tractable when false-positive control is treated as the architectural primary — implemented here as a six-stage pipeline (static parse → reachability filter → agentic exposure classification → vulnerability detection → adversarial-reflexion verification with constrained attacker personas → docker-sandboxed dynamic test) that drops OpenSSL's 15,232 candidate units to 3 confirmed exploitable findings (99.98% reduction) at ~$443 in Anthropic API tokens."
methodology: "Six-stage pipeline. Stages 1–2 (parse + reachability) are pure static analysis on a call-graph representation called 'units' (code block + caller/callee metadata) and incur no LLM cost. Stage 3 (Agentic Exposure Classification) is a Sonnet agent that iteratively classifies each reachable unit as exposed externally / exposed internally / security control / neutral. Stage 4 (Vulnerability Discovery) is Claude Opus over externally exposed units. Stage 5 (Exploitability Verification) is Claude Opus with agentic tool use under a tightly constrained attacker persona — explicitly NO server access, NO assumed credentials, NO ability to read local files, NO ability to run CLI commands for CLI/library targets — that must trace each exploit step explicitly against the actual codebase. Stage 6 (Dynamic verification) runs Docker-sandboxed exploit testing under a security-researcher persona."
related:
  - "[[openant|OpenAnt product]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[nahum-korda|Nahum Korda]]"
  - "[[knostic|Knostic]]"
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[big-sleep|Big Sleep]]"
  - "[[codemender|CodeMender]]"
  - "[[mdash|MDASH]]"
  - "[[mythos|Mythos]]"
  - "[[xbow|XBOW]]"
  - "[[anthropic-glasswing-announcement|Anthropic Project Glasswing]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[control-efficacy-gate|Control-Efficacy Gate]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[agentshield-announcement|AgentShield]]"
  - "[[codex-security|Codex Security]]"
  - "[[codex-security-announcement|Aardvark / Codex Security Announcement]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[claude-code-security-announcement|Claude Code Security Announcement]]"
sources:
  - "[[.raw/articles/openant-2026-05-15.md]]"
  - https://www.knostic.ai/blog/openant
  - https://openant.knostic.ai/
  - https://github.com/knostic/OpenAnt
aliases:
  - papers/openant-2026-05-15
  - openant-2026-05-15

---

# Introducing OpenAnt — LLM-Based Vulnerability Discovery for Open-Source Software

**Source:** [Knostic blog — *Introducing OpenAnt*](https://www.knostic.ai/blog/openant) (fetched 2026-05-15). Project page: [openant.knostic.ai](https://openant.knostic.ai/). Repository: [github.com/knostic/OpenAnt](https://github.com/knostic/OpenAnt). Local copy: `.raw/articles/openant-2026-05-15.md`. Cost benchmarks dated February 2026.

## Key Claim

Verified vulnerability discovery via frontier LLMs becomes operationally tractable when false-positive control is treated as the architectural primary — not a post-hoc filter. OpenAnt's six-stage pipeline ratchets candidate code units down by ~5 orders of magnitude (OpenSSL: 15,232 → 3 confirmed exploitable, **99.98% reduction**) at a token cost of ~\$443 rather than the ~\$329,000 a naive unit-by-unit Opus pass would incur. The architectural primary is the **constrained-attacker-persona** verification step (Stage 5), articulated as *Adversarial Reflexion*, which eliminates the agreeable-judge false-positive class by forbidding the model from assuming capabilities the attacker doesn't have and forcing explicit step-by-step exploit-path tracing against the real codebase.

## Methodology — Six-Stage Pipeline

1. **Code Parsing** *(static, no LLM cost)*. Scan source, extract every function, build the call graph; the function + call graph composes a *unit*. OpenSSL yields **15,232 units** across 1,769 files.
2. **Reachability Analysis** *(static, no LLM cost)*. Identify entry points (CLI handlers, callbacks, `main`) and traverse the call graph forward to find units reachable from external input. OpenSSL: **15,232 → 390 units** (97.4% reduction).
3. **Agentic Exposure Classification** *(Sonnet agent, iterative)*. For each reachable unit, a Sonnet agent classifies exposure (`exposed externally` / `exposed internally` / `security_control` / `neutral`) by iteratively searching callers, reading function definitions, tracing data flow. Token cost grows with iteration depth; median 9 iterations per unit on OpenSSL, capped at 20. Per-unit cost: \$0.13 (1 iteration) to \$10.92 (cap). OpenSSL: **390 → 49 externally exposed units**.
4. **Vulnerability Discovery** *(Claude Opus)*. Opus analyzes each externally exposed unit for vulnerabilities. OpenSSL: **49 → 28 potentially vulnerable units** (\$0.16/unit at this stage).
5. **Exploitability Verification** *(Claude Opus, agentic, constrained attacker persona)*. Opus role-plays the attacker step by step under tight capability constraints — no server access, no credentials, no local-file read, no CLI execution for CLI/library targets. Each exploit step traced explicitly against the real codebase via tool use. OpenSSL: **28 → 3 confirmed exploitable**. Most expensive stage per unit: \$0.14–\$10.54 with agentic verification.
6. **Dynamic Verification** *(Docker-sandboxed)*. Claude generates and executes an exploit test inside a docker-isolated sandbox under a security-researcher persona. \$0.90/finding. OpenSSL: 3 confirmed dynamically verified findings.

## Cross-Project Numbers (Feb 2026)

| Repository | Language | Total units | After reachability | Classification → Discovery → Verification | Total cost (USD) |
| :--- | :--- | --: | --: | :--- | --: |
| OpenSSL | C | 15,232 | 390 (97.4% reduction) | 49 → 28 → 3 | \$442.65 |
| WordPress | PHP | 12,177 | 393 (96.8% reduction) | 93 → 67 → 20 | \$239.45 |
| LangChain | Python | 6,701 | 143 (97.9% reduction) | 37 → 1 → 1 | \$51.48 |
| Rails | Ruby | 89 | 89 (0% reduction) | 19 → 2 → 2 | \$25.18 |
| Grafana | TypeScript + Go | 18,500 | 2,379 (87.1% reduction) | 223 → 143 → 86 | \$1,080.86 |

Two observations from the cross-project table:

- **Reachability filtering does not generalize.** Rails (Ruby) sees 0% reduction at Stage 2 — the static reachability primitive does not collapse Rails's call graph in the way it collapses OpenSSL's. The pipeline still works (89 → 19 → 2 → 2) but Stage 3 carries the full classification load.
- **Per-language verified-rate spread is wide.** Grafana finds 86 verified-exploitable findings on 18,500 units (~0.46%), OpenSSL finds 3 on 15,232 (~0.02%), Rails finds 2 on 89 (~2.2%). The verified-rate is not a function of repo size; it is a function of language ergonomics, project maturity, and attack-surface density.

## Notable Findings

- **The agreeable-judge problem is named explicitly.** *"LLMs are agreeable by default. Ask 'is this code vulnerable?' and the model will find a way to say yes."* This is the [[llm-as-a-judge|LLM-as-a-Judge]] failure mode applied to attacker-role exploit confirmation. OpenAnt's mitigation is the constrained-persona — the model cannot hand-wave through hard steps, cannot assume capabilities, and must show specific input, endpoint, and data flow against the actual codebase. See [[adversarial-reflexion|Adversarial Reflexion]] for the generalization.
- **Architectural primary is FP-control, not pattern coverage.** OpenAnt's design is convergent with [[mdash-defense-at-ai-speed|MDASH]] and [[xbow-mythos-evaluation|XBOW's harness for Mythos]] on the proposition *"the harness does the work, the model is one input"* — but applies a different mechanism. MDASH uses an ensemble of frontier and distilled models with debater agents and a prover stage; OpenAnt uses a single Opus model under tight persona constraints with explicit-trace requirements and tool-use verification. Both reach FP-control by *architectural constraint at the harness layer*, not by post-hoc result tuning. Both are second-vendor confirmations of the same disciplinary observation.
- **Cost discipline as a first-class concern.** The blog publishes both the naive Opus-on-every-unit cost (\$6.5K–\$329K for OpenSSL) and the filtered-pipeline cost (\$442.65). The 3-orders-of-magnitude cost spread is the operational case for the static-then-agentic phasing; running Opus over all 15,232 units would cost more than the engineering effort it took to write the filter.
- **The "unit" abstraction is the foundation.** A unit = function + call graph + caller/callee metadata. Decomposes the codebase into LLM-context-sized chunks with enough surrounding semantics to verify exploitability. The unit abstraction is the load-bearing data structure that makes the pipeline composable; everything downstream operates on units.
- **Six-stage convergence with MDASH's five-stage pipeline.** [[mdash|MDASH]]'s announced pipeline is Prepare → Scan → Validate → Dedup → Prove (5 stages). OpenAnt's is Parse → Reachability → Classification → Discovery → Verification → Dynamic (6 stages). Both decompose vulnerability discovery into static-then-agentic-then-validation phases; both treat validation as the *most* expensive and the *most* load-bearing stage. The convergence across two unrelated vendors is the deeper architectural signal.
- **Peer products explicitly disclaimed as out-of-scope competitors.** The blog cites [Aardvark from OpenAI (now *Codex Security*)](https://openai.com/index/introducing-aardvark/) and [Claude Code Security from Anthropic](https://www.anthropic.com/news/claude-code-security) and states *"we have zero intention of competing with them."* OpenAnt's positioning is OSS-maintainer-side defender tooling, not enterprise managed product. Both Aardvark and Claude Code Security are gaps on this wiki at the time of ingest and warrant their own paper pages.
- **Provenance of the "Reflexion" framing.** *Adversarial Reflexion* is the article's name for the technique; "Reflexion" is borrowed from the Shinn et al. 2023 LLM-self-critique-with-explicit-memory technique. OpenAnt's specialization is applying that pattern with hard capability constraints to attacker-role exploit verification.

## Known Issues (per the article)

- **Dynamic test design quality.** Stage-6 dynamic tests are generated on-the-fly by Opus; the test can technically confirm a vulnerability while the test design is methodologically weak. Pronounced in C codebases (memory management, pointer arithmetic, complex control flow). Mitigation considered but unimplemented: generate multiple test designs and select the strongest — rejected for architectural and orchestration complexity.
- **Context-window constraints.** A single logical unit can exceed model context, especially in C codebases with dense header inclusion chains. Mitigation 1: route to larger-context, more expensive models. Mitigation 2: reduce effective unit size by excluding lower-risk dependencies — but this risks incomplete assessment via excluded paths.
- **Cost-estimate volatility.** Observed cases where actual cost is ~2× the estimate. Driver: error-recovery loops (e.g., invalid JSON triggering automatic correction cycles) consume unplanned tokens.

## Strengths and Weaknesses

**Strengths.** Names the agreeable-judge failure mode explicitly and supplies a load-bearing mitigation (constrained persona + explicit trace + tool-use verification). Publishes concrete cost and unit-reduction numbers across five real OSS projects spanning C / PHP / Python / Ruby / TypeScript+Go — closing the *capability-vs-operational-cost* gap that [[frontier-ai-for-vuln-discovery|the frontier AI thesis]] previously had to argue qualitatively. Treats reachability filtering as a static-time concern with no LLM cost — separates the dominant cost-driver (agentic stages) from the dominant reduction-driver (call-graph traversal). The six-stage decomposition is auditable. Source release is OSS (MIT-license-grade openness; specific license to be confirmed on repo inspection) with a free OSS-scan-by-Knostic program for maintainers who can't host the token cost.

**Weaknesses and open scope.**

- **Reachability does not generalize across language families.** Rails sees 0% Stage-2 reduction; the Stage 3 classifier carries the load and Stage 3 is the per-unit cost driver. The architecture works but the headline cost numbers from OpenSSL do not transfer cleanly to dynamic-typed / convention-over-configuration languages.
- **No false-negative measurement.** The blog reports filter-ratios and verified-exploit counts but does not report what the pipeline *misses*. A peer-reviewed eval against a known-vulnerability corpus (CVE-corpus, [[cybergym|CyberGym]] adjacent, or similar) would let OpenAnt's FN rate be compared against [[mdash|MDASH]] (88.45% recall on CyberGym), [[mythos|Mythos]] raw (83.1%), and [[xbow-mythos-evaluation|XBOW's harness]] (42-55% FN reduction over Opus 4.6).
- **Dynamic-test methodological quality is acknowledged as weak in C.** The article calls this out itself; it is a real bound on Stage-6 utility for the highest-impact memory-safety class.
- **Cost estimates are volatile (~2× observed swing).** Budget planning is unreliable for projects that are sensitive to upfront cost ceilings.

## Relations

- **Supports** [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] thesis as a **fourth sourced production path** (alongside [[big-sleep|Big Sleep]] / [[codemender|CodeMender]] / [[mdash|MDASH]]). Strengthens the *"harness does the work, model is one input"* observation with a different mechanism: **constrained persona + explicit trace** instead of *ensemble + debate*. Two distinct mechanisms reaching the same architectural conclusion is structurally stronger than two implementations of the same mechanism.
- **Supports** [[control-efficacy-gate|Control-Efficacy Gate]] indirectly. OpenAnt's filter-ratio reporting (99.98% reduction for OpenSSL) is a *control-throughput* metric rather than a *control-efficacy* metric — it reports how many candidates survive each stage, not how many true positives are missed. A corpus-gate analog for vuln-discovery harnesses (replay a curated CVE corpus; fail if previously-detected CVEs are no longer detected) is the natural efficacy instrument OpenAnt's architecture doesn't yet provide.
- **Supports** [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] obliquely — OpenAnt is *not* a harness-config audit tool (it scans application code) but it is a **second sourced peer instrument for the underlying discipline of *FP-control via architectural constraint*** that [[agentshield-announcement|AgentShield]] surfaced on the config-audit side. See *Litmus-Test Synthesis* below.
- **Bylines.** The announcement's *Credit* section names [[nahum-korda|Nahum Korda]] as research lead and credits productization separately to Alex Raihelgaus and Daniel Geyshis, so the constrained-persona verification design and the shipped tooling carry different bylines.
- **Authored at** [[knostic|Knostic]]. Adds to Knostic's product family alongside [[kirin|Kirin]] (coding-agent runtime security) and the publicly-documented advisory / governance papers ([[ai-coding-agent-governance|AI Coding Agent Governance]] and [[ai-data-security|AI Data Security]]).
- **Peer products named inline.** OpenAnt cites [[codex-security|Codex Security]] — announced as Aardvark and renamed on 2026-03-06 ([[codex-security-announcement|Aardvark / Codex Security Announcement]]) — and [[claude-code-security|Claude Code Security]] ([[claude-code-security-announcement|Claude Code Security Announcement]]) as the two products in the same category, and positions itself away from both.

## Litmus-Test Synthesis (Cross-Reference to AgentShield)

The user posed OpenAnt as a *"litmus test for our opinionated approach to vulnerability testing with AI."* The opinionated approach in question came out of the 2026-05-15 [[agentshield-announcement|AgentShield ingest]] synthesis: **FP-control is the architectural primary; provenance-aware confidence-weighting (active-runtime vs. project-local-optional vs. template-example vs. docs-example vs. plugin-manifest vs. hook-code) is the generalizable evidence-collection primitive; control-existence checks are insufficient without control-efficacy checks**.

OpenAnt is the second sourced instrument applying the same underlying discipline — *FP-control by architectural constraint* — but on the **vulnerability-discovery side rather than the config-audit side**. The mechanism is different (constrained-attacker-persona with explicit trace versus provenance-labeled finding weights), the domain is different (application code rather than agent harness config), but the load-bearing design choice is the same: **treat the false-positive class as something to eliminate by architecture, not by post-hoc tuning**.

What the litmus test confirms:

- The discipline generalizes beyond AgentShield's domain. *FP-control as architectural primary* is now sourced on both the config-audit side (AgentShield) and the vuln-discovery side (OpenAnt + [[xbow-mythos-evaluation|XBOW]] + [[mdash-defense-at-ai-speed|MDASH]]). The disciplinary observation is robust to domain.
- The agreeable-judge / sycophancy failure mode of plain "is this vulnerable?" prompting is a structural feature of agentic verification stages, not a tuning-and-prompting concern. Both OpenAnt and MDASH treat it as an architectural problem.
- The wiki's CMM revision-pass candidates (filed during the AgentShield synthesis) are *not directly promoted* by OpenAnt, because OpenAnt operates on a different domain (application code, not harness config) and a different artifact (exploit-path traces, not policy / config findings). The harness-config-as-supply-chain-artifact candidate (D8) still needs a non-Claude-Code instrument before promotion. The provenance-aware-evidence primitive (Measurement Protocol) still needs a non-AgentShield instrument before promotion.
- The [[control-efficacy-gate|control-efficacy-gate]] candidate could be strengthened by an OpenAnt-style fixture: a curated CVE corpus replayed against the pipeline as a regression gate. OpenAnt's architecture does not include this today; this is the natural next-step instrument the wiki should look for.

**FP-control as architectural primary — second-domain confirmation.** Across [[agentshield|AgentShield]] (config audit), OpenAnt (vuln discovery), [[mdash-defense-at-ai-speed|MDASH]] (vuln discovery), and [[xbow-mythos-evaluation|XBOW × Mythos]] (vuln discovery), the load-bearing design choice is the same: false-positive control is implemented at the harness layer by *architectural constraint*, not by *post-hoc result tuning*. The mechanisms vary — provenance-aware source-kind weighting, constrained-attacker-persona with explicit trace, ensemble + debate, ensemble + prover stage — but the discipline is convergent. The agreeable-judge failure mode of plain "is this vulnerable?" / "is this exploitable?" prompting is a structural feature, not a prompting concern, and the production-grade response is to remove the cheap-yes path at the architecture level.

**Both peer products OpenAnt names implement false-positive control as a dedicated verification stage.** Each is now summarized on the wiki. [[codex-security|Codex Security]] attempts every candidate finding in an isolated sandbox to confirm exploitability, against a stated goal of *"high-quality, low false-positive insights"* ([OpenAI, Introducing Aardvark](https://openai.com/index/introducing-aardvark/)). [[claude-code-security|Claude Code Security]] runs a multi-stage self-critique in which the model *"attempts to prove or disprove its own findings"* ([Anthropic, Claude Code Security](https://www.anthropic.com/news/claude-code-security)). The observation holds at vendor scale: four mechanisms — constrained-attacker persona, sandboxed exploit trigger, self-critique, ensemble-and-prover — carry the same architectural commitment.

Two things the cross-check leaves open. Claude Code Security discloses no sandboxed dynamic execution, so the *execute-the-exploit* half of the discipline is sourced on OpenAnt and Codex Security only. None of the three announcements states how many true positives its verification stage discards as false negatives. Codex Security's 92% recall figure covers the whole pipeline; it leaves the validation stage's own discard rate unmeasured. The [[control-efficacy-gate|Control-Efficacy Gate]] question stays open on all three instruments.
