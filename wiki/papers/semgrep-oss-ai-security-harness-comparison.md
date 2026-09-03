---
type: paper
title: "OSS AI Security Harness Comparison"
address: c-000340
created: 2026-08-31
updated: 2026-09-01
tags:
  - papers
  - vuln-discovery
  - sast
  - exploitgen
  - agentic-ai
  - ai-in-sec-defense
status: summarized
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
  - sec-against-ai
origin: aggregated
year: 2026
authors: []
venue: "Semgrep Blog"
publication_date: "2026-07"
publication_date_note: "Not exposed in page metadata. Inferred from an embedded screenshot dated 2026-07-20 and a forward reference to a Black Hat announcement in August 2026. No author is named on the source page."
source_url: "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
archived_copy: ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
no_public_url: ""
key_claim: "Semgrep sorts nine open-source LLM-driven vulnerability-discovery projects into three categories (LLM-led exploitgen, LLM-skill-boosting vulnerability research, SAST+LLM hybrids) and finds that almost all of them separate discovery from validation, that the static/dynamic analysis boundary is blurring, that adversarial validation is widely adopted, and that no reference open-source harness is likely to lead the field before individual companies finish building their own."
methodology: "Comparative survey. The human-written body (categorization, cross-cutting findings, market-structure argument) is Semgrep's own review of nine public repositories. Per-tool detail sections and two of the four comparison tables are explicitly labelled by Semgrep as LLM-generated summaries of the same repositories."
contradicts: []
supports: []
related:
  - "[[ai-deep-sast]]"
  - "[[vvah]]"
  - "[[defending-code-harness]]"
  - "[[deepsec]]"
  - "[[security-audit-skill]]"
  - "[[trail-of-bits-skills]]"
  - "[[raptor]]"
  - "[[semgrep]]"
  - "[[anthropic]]"
  - "[[google]]"
  - "[[cisco]]"
  - "[[visa]]"
  - "[[vercel]]"
  - "[[trail-of-bits]]"
sources:
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
  - "https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses"
---

# OSS AI Security Harness Comparison

**Source:** [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses) (Semgrep Blog, July 2026[^date]). Local copy: `.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md`.

## Key Claim

[[semgrep|Semgrep]] sorts nine open-source projects that point a large language model at a codebase to find vulnerabilities into three categories, then draws six cross-cutting findings from the comparison and argues that no single open-source harness is likely to lead the field before individual companies finish building their own.

## Methodology

The survey's body — the taxonomy, the cross-cutting findings, and the market-structure argument — is human-written and names no author. A delimiter partway through the source marks a second part: per-tool detail sections and two comparison tables that Semgrep states are LLM-generated summaries of the same repositories.[^delimiter] Every claim below drawn from that second part carries the same qualifier.

## Three categories

Semgrep sorts the projects into three categories, a framing introduced for this survey.

- **LLM-led exploitgen.** The harness drives to a crashing end-state as an oracle for vulnerabilities; Semgrep calls this "a new form of fuzzing." Model guardrails that block exploit generation make the category hardest to use outside a trusted-access or cyber-verification exemption, and scope tends to stay narrow: a few vulnerability types with a reliable exploit condition.
- **LLM-skill-boosting vulnerability research.** Skills added inside an LLM push it to reason about code the way a human vulnerability researcher would.
- **SAST+LLM hybrids.** Deterministic program analysis (Semgrep, CodeQL) feeds an LLM to narrow the search space before the model reasons over candidates.

| Project | Owner | Stars[^stars] | Licence | Category | Summary |
|---|---|---|---|---|---|
| [[defending-code-harness\|defending-code-harness]] | [[anthropic\|Anthropic]] (orig.), [[semgrep\|Semgrep]] (fork) | 6K | Apache 2.0 | Exploitgen | Finds C/C++ memory bugs by crashing them in a sandbox and builds a verified patch |
| [[security-audit-skill\|security-audit-skill]] | Cloudflare | 2K | MIT | Skill-boosting | Six-phase multi-agent audit with adversarial validation |
| [[trail-of-bits-skills\|trailofbits/skills]] | Trail of Bits | 6K | CC-BY-SA | Skill-boosting | ~40 plugins distilling a security consultant's expert knowledge |
| vulnhunter | Capital One | 500 | Apache 2.0 | Skill-boosting | Three composable Claude Code skills (find, fix, verify) forming an automated remediation loop |
| mantis | [[google\|Google]] | 400 | Apache 2.0 | Skill-boosting | ~15 security-focused skills, described as a starting point rather than a rigid instruction set |
| [[raptor\|RAPTOR]] | Community (Gadi Evron) | 3K | MIT | SAST+LLM, overlaps exploitgen | Static analysis, fuzzing, exploit feasibility, SCA and patching built on Claude Code |
| [[deepsec\|deepsec]] | Vercel Labs | 5K | Apache 2.0[^deepsec-licence] | SAST+LLM | Web/app-stack focus: regex prefilter, then AI investigation, then revalidation |
| [[ai-deep-sast\|ai-deep-sast]] | Cisco | 50 | Apache 2.0 | SAST+LLM | Fast SAST scanner, optionally fully local, LLM-triages Semgrep findings across 30+ languages |
| [[vvah\|VVAH]] | Visa | 600 | Apache 2.0 | SAST+LLM | Eleven-stage agentic SAST pipeline across 42 languages: threat-models, verifies adversarially, proposes and validates fixes without executing code |

## Six cross-cutting findings

Almost every LLM+SAST harness **separates discovery from validation**, many using a separate model for each step. Exploitgen harnesses tend to break that pattern and use the same model to both find and generate the exploit.

**The line between static and dynamic analysis is blurring.** [[ai-deep-sast|ai-deep-sast]], [[vvah|VVAH]] and [[deepsec|deepsec]] reason entirely statically. [[defending-code-harness|defending-code-harness]] and [[raptor|RAPTOR]] also compile and execute binaries to validate what they find; Semgrep calls that pairing "a new hybrid analysis that is much closer to how human vulnerability research looks."

**Adversarial validation is widely adopted:** a second, independent agent tries to falsify each finding before it is reported. Named instances include Cloudflare's Phase 3 in [[security-audit-skill|security-audit-skill]], VVAH's S6, Trail of Bits' `fp-check`, and defending-code-harness's fresh-container grader. Semgrep frames this as most valuable when a different model attempts to disprove the findings.

**Language support is less of a differentiator** than for traditional SAST, which took a long time to add each new language; LLMs support almost all languages without that build-out. Exploitgen stays narrower, constrained more by the language-specific toolchain needed to build a binary than by the language itself.

**The definition of a "finding" varies by harness:**

| Tool | Primary method | A "finding" is… |
|---|---|---|
| [[ai-deep-sast\|ai-deep-sast]] | Semgrep + LLM triage | A triaged static match |
| [[vvah\|VVAH]] | 11-stage agentic LLM pipeline | A verified candidate from the pipeline |
| [[raptor\|RAPTOR]] | Semgrep + CodeQL + AFL++ + LLM | A verified static finding or pipeline candidate, with or without a PoC |
| [[defending-code-harness\|defending-code-harness]] | LLM agents craft crashing inputs | A reproducible crash under ASan |
| [[deepsec\|deepsec]] | Regex prefilter + AI + revalidation | A re-validated static match |

**Patch generation remains uncommon.** The harnesses that generate exploits also execute code and produce proof-of-concept inputs, and less obviously they also produce patches; most of the SAST-plus-LLM hybrids do not.

| Tool | Executes code | PoC | Patch |
|---|---|---|---|
| [[ai-deep-sast\|ai-deep-sast]] | No | No | No, advice only |
| [[vvah\|VVAH]] | No | No | Yes, proposed and LLM-validated |
| [[raptor\|RAPTOR]] | Yes | Yes (fuzzing / feasibility) | Yes |
| [[defending-code-harness\|defending-code-harness]] | Yes | Yes | Yes, execution-verified |
| [[deepsec\|deepsec]] | No | No | No |

## Deployment shape: pipelines vs. skills

Semgrep's LLM-generated deployment-shape table splits seven of the nine projects by how they deploy. A **standalone pipeline** ([[ai-deep-sast|ai-deep-sast]], [[vvah|VVAH]], [[raptor|RAPTOR]], [[defending-code-harness|defending-code-harness]], [[deepsec|deepsec]]) installs as a program or CI job, brings its own model calls, runs unattended, and carries its own sandbox: gVisor, a microVM, or an OS-level mechanism. An **agent-native skill** ([[security-audit-skill|security-audit-skill]], [[trail-of-bits-skills|trailofbits/skills]]) deploys as a prompt pack inside Claude Code or Codex, uses the host agent's model instead of its own, runs unattended only as far as the driving agent allows, and inherits whatever sandbox the host agent has. The cost model follows the same split: per-run infrastructure plus tokens for a pipeline, tokens alone on an existing agent for a skill.

## Market structure

Semgrep asks whether a reference open-source LLM harness will emerge and answers: "We think not today." Many companies will build their own **"shop jigs"** for vulnerability finding instead, a term Semgrep credits to tptacek on Hacker News. An open-source leader may still emerge, but the field is moving too fast for one to settle now, which Semgrep names as the reason some of the nine projects ship marked "no external contributions accepted" or go unmaintained. Semgrep states it has its own announcement planned for Black Hat in August 2026 — notable because Semgrep's own static-analysis engine already feeds several of the compared harnesses.

## Ideal use cases

Semgrep's own recommendations, by scenario. Fully local or offline work favors [[ai-deep-sast|ai-deep-sast]], the only project in the comparison that runs entirely on-device, trading depth and proof for breadth and speed. A vulnerability hunter with the operational appetite for one platform reaches for [[raptor|RAPTOR]]; one who wants a starting point for custom skills reaches for Google's mantis. A C/C++ maintainer wanting high-confidence, verified-patch findings reaches for [[defending-code-harness|defending-code-harness]]. An AppSec program optimizing time-to-reviewed-fix reaches for [[vvah|VVAH]], at the cost of a frontier-model dependency and no runnable proof of concept. A Next.js or Vercel shop reaches for [[deepsec|deepsec]]: cheap regex-first triage, an AI second look, a PR-diff mode. A team already running Claude Code, Codex, or another coding harness installs skills directly: [[security-audit-skill|security-audit-skill]] for a rigorous, adversarially-validated methodology with near-zero setup; [[trail-of-bits-skills|trailofbits/skills]] for a toolbox of specialist reviewers; Capital One's vulnhunter for a remediation loop built specifically around Claude Code.

## Strengths and Weaknesses

The comparison spans nine repositories, which grounds its cross-cutting findings as observations about the category: the discovery/validation model split, the static/dynamic blur, and the spread of adversarial validation all describe patterns visible only once several harnesses sit side by side. Two limits qualify it. The per-tool detail sections and two of the four comparison tables are LLM-generated summaries rather than Semgrep's own verified reporting, a distinction the source states plainly but a reader skimming the tables would not catch. And Semgrep is a vendor in the market it surveys: its static-analysis engine is a named input to several of the compared harnesses, and it discloses its own harness announcement for Black Hat in the same post that concludes no reference open-source harness will lead the field soon.

## Relations

- Supports: [[raptor|RAPTOR]] because the LLM-generated capability matrix and repository summary add detail that page did not carry (22 vulnerability classes, Landlock+seccomp+namespace isolation, roughly 107K lines of Python).
- Contradicts: [[raptor|RAPTOR]] on one figure. The same LLM-generated summary gives a six-stage exploitability-validation pipeline, against the Stages 0–F count that page sources from the v3.0.0 README. Neither count is preferred; [[raptor|the RAPTOR page]] records both.

## Notes

[^date]: Publication date not exposed in page metadata; inferred from an embedded screenshot dated 2026-07-20 and a forward reference to a Black Hat announcement in August 2026. No author is named on the source page.
[^delimiter]: "The above was human-written; see below for LLM-generated summaries of some of the repos involved!" — the source's own delimiter, roughly two-thirds of the way through the page.
[^stars]: Star counts as reported by Semgrep in its July 2026 survey, a point-in-time figure from the human-written category listing rather than the LLM-generated sections.
[^deepsec-licence]: The category list states Apache 2.0. deepsec's own LLM-generated capability-matrix row instead reads "see repo," with a footnote telling readers to confirm the licence in the repository. See [[deepsec|deepsec]] for the disagreement.
