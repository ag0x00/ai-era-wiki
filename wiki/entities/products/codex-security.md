---
type: entity
title: "Codex Security"
address: c-000064
created: 2026-05-15
updated: 2026-09-29
tags:
  - entities
  - product
  - tool
  - vuln-discovery
  - openai
  - codex
  - daybreak
  - appsec
status: developing
scope_axis:
  - ai-in-sec-defense
entity_type: product
aliases:
  - "Aardvark"
role: "OpenAI's application-security agent, formerly Aardvark: a Codex plugin, a cloud service over connected GitHub repositories and a CLI that scan code against a threat model, validate candidate findings and generate patches for human review; the workflow layer of OpenAI Daybreak."
homepage: https://learn.chatgpt.com/docs/security
maintainer: "[[openai|OpenAI]]"
first_mentioned: "[[codex-security-announcement|Introducing Aardvark]]"
related:
  - "[[openai]]"
  - "[[openai-daybreak]]"
  - "[[codex-security-announcement]]"
  - "[[claude-code-security-announcement]]"
  - "[[claude-code-security]]"
  - "[[openant]]"
  - "[[adversarial-reflexion]]"
  - "[[mdash]]"
  - "[[big-sleep]]"
  - "[[codemender]]"
  - "[[google-cloud-codemender-preview]]"
  - "[[aisle-openssl-12-of-12]]"
  - "[[harness-config-as-supply-chain-artifact]]"
  - "[[vulnops]]"
  - "[[frontier-ai-for-vuln-discovery]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
sources:
  - https://openai.com/index/introducing-aardvark/
  - https://openai.com/index/codex-security-now-in-research-preview/
  - https://openai.com/policies/outbound-coordinated-disclosure-policy/
  - https://openai.com/index/daybreak-securing-the-world/
  - https://openai.com/business/solutions/cybersecurity/
  - https://learn.chatgpt.com/use-cases/scan-code-changes-for-security
  - https://learn.chatgpt.com/use-cases/deep-security-scan
  - https://learn.chatgpt.com/use-cases/remediate-vulnerability-backlog
  - https://openai.com/daybreak/
  - "[[.raw/articles/openai-aardvark-codex-security-2026-05-15.md]]"
  - "[[.raw/articles/openai-daybreak-securing-the-world-2026-09-28.md]]"
  - "[[.raw/articles/cybersecurity-2026-09-28.md]]"
  - "[[.raw/articles/openai-codex-security-scan-code-changes-2026-09-28.md]]"
  - "[[.raw/articles/openai-codex-security-deep-scan-2026-09-28.md]]"
  - "[[.raw/articles/openai-codex-security-remediate-backlog-2026-09-28.md]]"
  - "[[.raw/articles/openai-daybreak-hub-2026-09-28.md]]"
verified: 2026-09-29
verified_against:
  - ".raw/articles/cybersecurity-2026-09-28.md"
  - ".raw/articles/openai-codex-security-deep-scan-2026-09-28.md"
  - ".raw/articles/openai-codex-security-remediate-backlog-2026-09-28.md"
  - ".raw/articles/openai-codex-security-scan-code-changes-2026-09-28.md"
  - ".raw/articles/openai-daybreak-hub-2026-09-28.md"
  - ".raw/articles/openai-daybreak-securing-the-world-2026-09-28.md"
verified_findings: 0
verified_note: "verify2, diff-scoped: cloud and threat-model re-sourcing checked against June post and solutions page; 'durable' (from the dropped guide) cut, 1 low"
---

# Codex Security (formerly Aardvark)

**Sources:** [Codex Security documentation](https://learn.chatgpt.com/docs/security) · [Introducing Aardvark](https://openai.com/index/introducing-aardvark/) · [Daybreak announcement, Codex Security section](https://openai.com/index/daybreak-securing-the-world/#from-findings-to-fixes-with-codex-security) · [AI for Cybersecurity Teams](https://openai.com/business/solutions/cybersecurity/)

Codex Security is [[openai|OpenAI]]'s application-security agent. It builds a threat model of a repository, searches the code for plausible vulnerabilities, validates candidates in an isolated environment, and generates patches that a human reviews before anything lands.[^aardvark][^june-cs] OpenAI announced it as Aardvark, an agentic security researcher powered by GPT-5, and renamed it Codex Security on 2026-03-06, when it was built into Codex as a research preview for ChatGPT Enterprise, Business and Edu customers.[^aardvark] It is now the workflow layer of [[openai-daybreak|OpenAI Daybreak]]: the Daybreak models supply the cyber capability, and Codex Security supplies the context, tools, validation, workflow and review that apply it to code.[^solutions]

Humans keep three decisions: which findings to investigate, which changes to apply and what information to share.[^june-cs]

## Surfaces

Codex Security runs in three places:

- **Plugin.** Runs inside Codex, suited to trying the workflow on a branch or one codebase. The June 2026 update gave it out-of-the-box defensive workflows.[^solutions][^june-cs]
- **Cloud.** Runs managed, ongoing scans across connected GitHub repositories. It launched in research preview in March 2026.[^solutions][^june-cs]
- **CLI.** Runs scans from a terminal and inside CI/CD. OpenAI's solutions page calls it open source.[^solutions]

## Workflows

The plugin exposes three skills:

| Workflow | Invoked as | Scope | Output |
|---|---|---|---|
| Change review | `$codex-security:security-diff-scan` | A pull request, commit, branch diff or working-tree patch, with discovery and validation kept to the diff and its supporting code | A Markdown report and inline comments on affected lines |
| Deep scan | `$codex-security:deep-security-scan` | A whole repository or one named folder, with repeated discovery passes before validation | A findings workspace, a report, the surfaces reviewed and the proof gaps |
| Fix one finding | `$codex-security:fix-finding` | One validated or plausible finding, from Codex Security or an external source | A minimal patch, regression evidence and the remaining uncertainty |

> [!contradiction] Conflict with [[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]
> The landscape states that no commercial programme ships in the agent-native skill shape. The plugin installs into Codex, and the three workflows above run as skills invoked from a Codex prompt, on OpenAI's own agent and models.[^diff][^deep][^fix][^solutions] The landscape carries the matching flag beside its claim.

### Change review

The change review is for a pull request, commit, branch or local patch that touches a sensitive path: authentication, authorization, parsing, file access, secrets or privileged workflows.[^diff] OpenAI positions it beside ordinary code review, for evidence about security regressions. The guide asks the user to run it first without requesting a fix, so the result stays a review artifact, and to check each affected line, validation result and stated proof gap before deciding to remediate.[^diff] A useful report separates a reachable, supported finding from a suspicion that still needs confirmation.[^diff]

### Deep scan

The deep scan trades runtime and tokens for coverage. It repeats discovery passes over a repository or named folder before it validates and prioritizes, so it runs longer and costs more than an ordinary scan.[^deep] Beyond opening the repository in Codex and completing the plugin quickstart, the guide prepares the scan in three steps:

1. Confirm that the user owns the repository or is authorized to assess it.[^deep]
2. Write architecture, trust-boundary, security-invariant, finding-criteria, exclusion and severity guidance into `SECURITY.md`, with nested `SECURITY.md` files for directory-specific policy.[^deep]
3. Keep the supported build, test and validation commands in `AGENTS.md`.[^deep]

The guide expects the final result to name the affected locations, why the behavior is reachable, what validation Codex performed, the remaining proof gaps and a bounded remediation direction, and to separate validated findings from unvalidated ones. Remediation starts only on a finding the user has selected and reviewed.[^deep]

### Fixing findings

The fix workflow takes one finding at a time. A finding can come from Codex Security, Linear or Jira, GitHub Security Advisories, HackerOne or Bugcrowd, a penetration test or an internal review.[^fix] The guide advises against handing Codex a broad backlog to change at once, because a single-finding loop keeps the security invariant, the patch and the validation evidence reviewable.[^fix] Each fix follows the same rules:

- Confirm that the issue still reproduces before changing code, where feasible.[^fix]
- Make the smallest change that enforces the intended security invariant.[^fix]
- Add focused regression coverage or the strongest repeatable validation available.[^fix]
- Verify that legitimate behavior still works and the original issue no longer reproduces.[^fix]
- Keep unrelated findings and refactors out of the change.[^fix]

If the issue is already fixed or does not reproduce, the guide has the agent record that evidence and leave the code unchanged. Each completed item keeps its original ticket or advisory reference, the exact change, the checks run and any proof gap.[^fix]

## Scan context

### Threat model

The threat model is the artifact the scans run against. Codex Security reads a team's code and its threat model, generating one where none exists, and OpenAI's solutions page describes it as an editable model built from a repository that focuses analysis on realistic attack paths and high-impact code.[^june-cs][^solutions] A solutions-page card under Daybreak Blue maps assets, entry points, trust boundaries, sensitive data paths and abuse cases, so that teams can prioritize security work before incidents occur.[^solutions] The design is continuous with Aardvark's, whose pipeline began from a whole-repository threat model and scanned each commit against it.[^aardvark]

### Repository files

`SECURITY.md` and `AGENTS.md` carry the rest of the context. The deep scan reads its finding criteria, exclusions and severity guidance from the first and its validation commands from the second.[^deep] In Daybreak's defense loop, `SECURITY.md` is the shared system context that every stage reads and extends, so later passes start from the established map, ownership and evidence.[^hub] Both files arrive with the repository and change what the agent reports and runs, which places them in the artifact class [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] describes.

## Findings from other tools

The June 2026 plugin update opens the pipeline at both ends. It can triage and validate existing findings from scanners, advisories, bug-bounty reports or ticketing systems, then generate patches at scale to close a backlog, and it can export a completed scan to an existing vulnerability-management system or into other tools through SARIF files and CodeQL queries.[^june-cs] Automated pipelines run it through Codex CLI.[^june-cs] That makes Codex Security a validation-and-remediation stage for findings it did not produce, the triage position [[vulnops|VulnOps: Vulnerability Operations]] describes.

## Original pipeline

Aardvark's announcement describes a four-stage pipeline:[^aardvark]

1. **Analysis.** The agent analyzes the full repository and produces a threat model of its security objectives and design.
2. **Commit scanning.** Each new commit is inspected against the repository and the threat model, and the history is scanned when a repository is first connected; findings are explained step by step and annotated for human review.
3. **Validation.** The agent tries to trigger each candidate in an isolated, sandboxed environment to confirm exploitability.
4. **Patching.** A Codex-generated patch, scanned by Aardvark, is attached to each finding for human review and one-click application.

The announcement frames the method against the prior generation of tools: Aardvark "does not rely on traditional program analysis techniques like fuzzing or software composition analysis" and uses LLM reasoning and tool use, reading code, writing and running tests, and using tools as a human researcher would.[^aardvark] The sandboxed validation stage is the dynamic-execution form of the false-positive control that [[openant|OpenAnt]] formalizes as [[adversarial-reflexion|Adversarial Reflexion]] and that [[mdash|MDASH (Microsoft Agentic Scanning Harness)]] implements as a prover stage. The [[claude-code-security-announcement|Claude Code Security Announcement]] describes the same control as Claude attempting to prove or disprove its own findings.

## Reported figures

All figures are OpenAI's own.

- **Recall.** In benchmark testing on "golden" repositories, Aardvark found 92% of known and synthetically introduced vulnerabilities.[^aardvark] The benchmark set is undisclosed, so the figure does not compare with the CyberGym scores [[cybergym|CyberGym Benchmark]] records for other systems; [[aisle-openssl-12-of-12|AISLE: 12 of 12 OpenSSL Vulnerabilities]] cites it as one side of that non-comparison.
- **Open-source disclosure.** Ten of the vulnerabilities Aardvark found in open-source projects received CVE identifiers, and OpenAI planned pro-bono scanning for select non-commercial repositories.[^aardvark]
- **Base rate.** OpenAI cites over 40,000 CVEs reported in 2024, and its testing found that around 1.2% of commits introduce bugs.[^aardvark]
- **Usage.** Between the cloud research preview's March 2026 launch and OpenAI's 2026-06-22 announcement, Codex Security scanned over 30 million commits across more than 30,000 codebases; human reviewers marked more than 70,000 findings fixed, and over 500,000 findings were determined fixed automatically.[^june-cs] OpenAI's solutions page shows "500K+ findings fixed", a figure that matches the automatic count, without saying which count it reports.[^solutions]

OpenAI updated its outbound coordinated disclosure policy alongside the Aardvark launch, toward collaboration and scalable impact and away from rigid timelines that pressure developers, anticipating that tools like Aardvark will raise the number of bugs found.[^aardvark]

## Competitive position

Google's [[codemender|CodeMender (Google DeepMind)]] entered managed preview in July 2026, as [[google-cloud-codemender-preview|CodeMender Preview on Google Cloud]] records, with the same three-part shape: reason-over-code discovery, sandboxed exploit validation, and patch generation under human approval, with the same rejection of pattern matching as the prior generation. Sandboxed validation, which Aardvark could claim as a differentiator at its launch, is now the shared baseline across the vendor previews.

Two differences remain from Aardvark's design. Its stable whole-repository threat model, with commit deltas as the event stream, has no counterpart in Google's published description, and CodeMender composes with a cloud asset graph and an offensive agent on one platform, a composition that neither OpenAI's Codex Security documentation nor Anthropic's Claude Code Security announcement describes. The June 2026 update adds a third: Codex Security now ingests other tools' findings and exports its own, so it sits in the middle of a scanning stack as well as at its head. Aardvark's golden-repository figure is still the only published recall figure of the three.

## Limits of the public record

The statements below are scoped to the Aardvark announcement and to the OpenAI Daybreak and Codex Security pages fetched on 2026-09-28.

- **False-positive rate.** None of them publishes one. The Aardvark post describes its output as low false-positive without a figure, and the June 2026 counts measure findings fixed.[^aardvark][^june-cs]
- **Validation environment.** The sources describe an isolated, sandboxed environment and name no isolation technology.[^aardvark][^solutions]
- **Recall benchmark.** The golden repositories are not disclosed, so the 92% figure has no public comparison.[^aardvark]
- **Cost.** The March 2026 preview rolled out to Enterprise, Business and Edu customers with a month of free usage, and none of the Daybreak sources states pricing.[^aardvark][^solutions]

## See also

- [[openai-daybreak|OpenAI Daybreak]] — the programme Codex Security serves as the workflow layer, with the June 2026 usage figures and the solutions page's workflow cards.
- [[codex-security-announcement|Aardvark / Codex Security Announcement]] — the original announcement.
- [[claude-code-security|Claude Code Security]] — the Anthropic counterpart.
- [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] — the thesis placing the vendor pipelines.

## Notes

[^aardvark]: [OpenAI — Introducing Aardvark](https://openai.com/index/introducing-aardvark/), fetched 2026-05-15, with its 2026-03-06 update banner: the four-stage pipeline, 92% recall on golden repositories (known and synthetically introduced vulnerabilities found), ten CVE identifiers, pro-bono scanning, the disclosure-policy update, and the 40,000-CVE and 1.2%-of-commits base rates. Local copy: `.raw/articles/openai-aardvark-codex-security-2026-05-15.md`.
[^june-cs]: [OpenAI — Daybreak, "From findings to fixes with Codex Security"](https://openai.com/index/daybreak-securing-the-world/#from-findings-to-fixes-with-codex-security), 2026-06-22: commits and codebases scanned since the March 2026 research-preview launch, findings marked fixed by human reviewers, findings determined fixed automatically, and the plugin update. Summarized at [[openai-daybreak|OpenAI Daybreak]].
[^solutions]: [OpenAI — AI for Cybersecurity Teams](https://openai.com/business/solutions/cybersecurity/), undated, fetched 2026-09-28: the three surfaces, the FAQ on models and Codex Security, and the "500K+ findings fixed" figure. Summarized at [[openai-daybreak|OpenAI Daybreak]].
[^diff]: [OpenAI — Scan code changes for security](https://learn.chatgpt.com/use-cases/scan-code-changes-for-security), undated, fetched 2026-09-28. Local copy: `.raw/articles/openai-codex-security-scan-code-changes-2026-09-28.md`.
[^deep]: [OpenAI — Run a deep security scan](https://learn.chatgpt.com/use-cases/deep-security-scan), undated, fetched 2026-09-28. Local copy: `.raw/articles/openai-codex-security-deep-scan-2026-09-28.md`.
[^fix]: [OpenAI — Remediate a vulnerability backlog](https://learn.chatgpt.com/use-cases/remediate-vulnerability-backlog), undated, fetched 2026-09-28. Local copy: `.raw/articles/openai-codex-security-remediate-backlog-2026-09-28.md`.
[^hub]: [OpenAI — Daybreak](https://openai.com/daybreak/), programme page, undated, fetched 2026-09-28: the defense loop and `SECURITY.md` as shared context. Local copy: `.raw/articles/openai-daybreak-hub-2026-09-28.md`.
