---
type: entity
entity_type: product
title: "Mantis (Google)"
created: 2026-09-18
updated: 2026-09-18
tags:
  - products
  - google
  - google-cloud
  - mantis
  - ai-vuln-discovery
  - agent-skills
  - sandboxing
  - open-source
status: developing
origin: aggregated
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
vendor: "Google"
parent_org: "[[google]]"
role: "Open-source multi-agent code-review skill set and ADK reference harness"
homepage: "https://github.com/google/mantis"
first_mentioned: "[[semgrep-oss-ai-security-harness-comparison]]"
related:
  - "[[google]]"
  - "[[google-cloud-autonomous-sdlc-security]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[big-sleep]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[agent-sandbox-isolation-landscape]]"
  - "[[gvisor]]"
  - "[[gemini-cli]]"
  - "[[adversarial-reflexion]]"
  - "[[autonomous-exploit-generation]]"
  - "[[vulnops]]"
  - "[[frontier-ai-for-vuln-discovery]]"
sources:
  - "https://github.com/google/mantis"
  - "https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-how-google-cloud-security-uses-ai-internally"
  - "[[.raw/reports/google-mantis-repository-2026-09-18.md]]"
  - "[[.raw/articles/cloud-ciso-perspectives-how-google-cloud-security-uses-ai-internally-2026-09-18.md]]"
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
verified: 2026-09-18
verified_against:
  - ".raw/articles/cloud-ciso-perspectives-how-google-cloud-security-uses-ai-internally-2026-09-18.md"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
  - ".raw/reports/google-mantis-repository-2026-09-18.md"
verified_findings: 0
verified_note: "Repo extract and article read whole, repository consulted at the pinned commit; Semgrep read for its Mantis rows only. Host-target count corrected to three, Semgrep category corrected to skill-boosting, workspace and patch-check claims bounded."
---

# Mantis (Google)

**Sources:** [google/mantis on GitHub](https://github.com/google/mantis) · [Cloud CISO Perspectives: Our path to autonomous SDLC security (Google Cloud, 2026-06-29)](https://cloud.google.com/blog/products/identity-security/cloud-ciso-perspectives-how-google-cloud-security-uses-ai-internally) · [[semgrep-oss-ai-security-harness-comparison|Semgrep's July 2026 harness survey]]

**Mantis** is Google's open-source framework for agentic code security review. The repository describes it as "a set of skills along with an ADK reference harness for building secure software in the new AI era of software development." Google Cloud's security organization names it as the core of its internal code-scanning stage: "our core multi-agent orchestration framework designed specifically for scalable, context-aware repository analysis," with the core skills released publicly and "a more full-fledged version running internally and securing our customers."

The public repository carries 21 skills at commit `21ef4b4c` (2026-09-17), an Apache-licensed reference harness built on Google's Agent Development Kit, evaluation datasets, and deliberately vulnerable sample applications. Google disclaims it: it is not an officially supported Google product, it is ineligible for the Google Open Source Software Vulnerability Rewards Program, and the README states it is intended for demonstration and not for production use.

## Pipeline and skills

The README states a thirteen-step review loop that runs from repository history to a secure-development advisory. The skills implement it, and the review stages write named artifacts into a shared `workspace/` directory that the next stage reads. Nineteen skills sit at the repository root and install as a set; `mantis-configure` and `mantis-launch` ship under `reference/skills/` with the harness and drive a campaign rather than contribute to the workspace.

| Phase | Skills | Artifact written |
|---|---|---|
| Prime | `mantis-history`, `mantis-summarize`, `mantis-structural-index`, `mantis-architecture` | `historical_learnings.jsonl`, per-directory `mantis-summary.md`, a content-addressed semantic-unit index, an interlinked knowledge base |
| Model | `mantis-threat-model` | A living threat model of trust boundaries, attack surfaces and attacker profiles |
| Plan | `mantis-plan` | `plan.json`, a targeted review plan drawn from the threat model and past findings |
| Research | `mantis-researcher` | Raw findings from per-file static analysis and deep-dive review |
| Filter | `mantis-dedupe`, `mantis-review`, `mantis-critic` | Consolidated findings, with false positives and debug-only features removed |
| Prove | `mantis-reproduce`, `mantis-chain` | Crash reproducers, and higher-impact chains built from low-severity findings |
| Fix and grade | `mantis-patch`, `mantis-calibrate`, `mantis-report` | Minimal patches under transactional isolation, risk scores, a stakeholder review packet |
| Learn | `mantis-reflect`, `mantis-advise` | `learnings.jsonl`, queried before later code edits |
| Operate | `mantis-meta-agent`, `mantis-pipeline-adapter`, `mantis-configure`, `mantis-launch` | `workflow.json`, campaign supervision, preflight checks |

Two design commitments show in that list. Calibration is a first-class stage, scoring findings against an established rubric to counter what the README calls LLM inflation of severity, so the pipeline surfaces the most critical risks instead of "spewing thousands of 'criticals'." And the loop is meant to be rebuilt: the README calls the default workflow a starting point rather than a rigid set of instructions, `reference/workflow.json` encodes the default deep review, and passing an objective to `run.sh` synthesizes a custom research-graph topology at run time.

## Distribution and host agents

Skills install with `npx skills add google/mantis`. The repository states that Mantis is platform agnostic and works with Gemini CLI, Antigravity CLI, Google ADK, and other coding agent frameworks. The reference harness under `reference/` uses Vertex AI application-default credentials.

That naming settles a gap in the earlier record. [[semgrep-oss-ai-security-harness-comparison|Semgrep's July 2026 survey]] filed Mantis under skill-boosting research, counted roughly fifteen skills, and named no host agent for it where it named one for each of the other three skill sets it recorded. The repository at the September 2026 commit names three host targets — Gemini CLI, Antigravity CLI and Google ADK — alongside other coding agent frameworks, carries 21 skills, and ships a harness of its own rather than depending entirely on a host. Both readings of the delivery model now apply: a team can install the skills into a coding agent it already runs, or run the ADK harness directly.

## Sandboxing posture

The repository treats the execution of model-generated code as the primary risk it carries, and opens with a caution before any installation instruction: the suite generates and executes autonomously generated code that may be unstable or perform unexpected actions, and it must run only in isolated, restricted environments, never on a machine with access to production systems, sensitive data, or internal networks.

The stated prerequisites are Docker for testing containers and [[gvisor|gVisor]] for enhanced security when executing untrusted AI-generated crash-reproducer code, registered for networkless execution through `sudo runsc install -- --network=none` or the equivalent `runtimeArgs` in the Docker daemon configuration. The `mantis-configure` skill exposes four selectable sandbox mechanisms: static-only, gVisor, microsandbox, and a Google Compute Engine remote sandbox. Cloud SDKs cover the remote case.

The README then declines to treat its own sandboxes as sufficient. It instructs an operator using the most advanced frontier models to add a further sandboxing layer that itself carries strong monitoring for escape attempts. [[agent-sandbox-isolation-landscape|The sandbox isolation landscape]] records what other harness builders assembled for the same workload; Mantis is the one that publishes a layering instruction above its own default.

## Responsible-use constraints

A second admonition governs what an operator does with the output. Google states that AI models are non-deterministic and can hallucinate findings or generate incorrect patches, that all findings must be manually verified by a security expert before being reported, and that unverified AI-generated reports must not be mass-filed to open-source maintainers. It qualifies the reproduction stage in both directions: a failed automatic reproduction does not establish a false positive, and a successful reproducer does not guarantee the bug is exploitable in all contexts.

That second qualification bounds the triage claim [[autonomous-exploit-generation|autonomous exploit generation]] carries as a defensive control. A reproducer establishes exploitability under the configuration it ran in, and Google's own open-source harness states the limit that vendor product descriptions leave implicit.

## Relationship to CodeMender

Mantis and [[codemender|CodeMender]] run the same stages, and no published source states a relationship between them. Both scan a repository with a multi-agent pipeline, both run AI-generated proof-of-concept exploits in an isolated sandbox to establish exploitability before alerting a developer, and both check a generated patch before a human sees it, CodeMender with an LLM judge screening for functional disruption and Mantis with an adversarial loop confirming the flaw is really fixed. Google attributes them to different organizations: the 2026-06-29 Cloud CISO post credits Google Cloud security engineering with building Mantis, and the 2026-07-21 preview post traces CodeMender to Google DeepMind research. Neither post names the other system, and the repository names neither CodeMender nor DeepMind.

Two readings stay open. Mantis may be the open demonstration of the skills that CodeMender's harness carries, which would match the preview post's statement that the harness is updated with "up-to-date agent skills, security tools, and system prompts." Or the two are separate programmes that converged on one architecture, which would match the separate attributions. Both readings stay live for anyone counting Google's security artifacts. Big Sleep, CodeMender and Mantis are three on the evidence Google has published, and a source stating a shared lineage would collapse two of the three into one.

## Placement

Mantis is the one Google code-security artifact a team can adopt without a Google Cloud relationship. [[big-sleep|Big Sleep]] stays vendor-internal and CodeMender ships as a managed preview, so the skill set and its harness are the open path. [[oss-ai-vuln-discovery-harness-landscape|The open-source harness landscape]] carries the per-project comparison against the commercial set, and [[vulnops|VulnOps]] carries the function the pipeline feeds.

The self-reflection stage is where it diverges from the rest of that landscape. `mantis-reflect` extracts successes, failures and false assumptions from a completed campaign into `learnings.jsonl`, and `mantis-advise` queries that store, the threat model, verified patch patterns and triaged false positives before a later code edit. Google Cloud describes the internal counterpart as a compounding-interest effect on security engineering. [[adversarial-reflexion|Adversarial Reflexion]] covers the within-run verification discipline the open field converged on; this store is the across-run one, and it carries no published measurement.

## See also

- [[google-cloud-autonomous-sdlc-security|Google Cloud Autonomous SDLC Security]] — the post that names Mantis as the internal code-scanning framework.
- [[google-cloud-codemender-preview|CodeMender Preview on Google Cloud]] — the managed product with the same stage shape.
- [[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]] — where Mantis sits against eight peers.
- [[agent-sandbox-isolation-landscape|Agent Sandbox Isolation Landscape]] — the isolation classes its four sandbox options draw from.
- [[google|Google]] — the parent organization.
