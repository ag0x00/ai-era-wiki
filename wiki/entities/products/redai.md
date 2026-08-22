---
type: entity
title: "RedAI"
address: c-000206
created: 2026-06-04
updated: 2026-06-04
tags:
  - entities
  - product
  - tool
  - vuln-discovery
  - red-team
  - live-validation
  - open-source
status: developing
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
entity_type: product
role: "Open-source terminal-UI workbench for AI-driven vulnerability discovery with live-environment validation. Three-phase pipeline (discover, validate, report) in which scanner agents (Claude Code or Codex) produce candidate findings and validator agents prove or disprove them inside a running instance of the target. Validation environments are plugins, with Chrome (via agent-browser) and iOS Simulator shipped in the box."
homepage: https://github.com/kpolley/redai
github: https://github.com/kpolley/redai
license: MIT
maintainer: "[[kyle-polley|Kyle Polley]]"
runtime: "Bun >= 1.2; TypeScript + Python"
package: "@kpolley/redai (npm)"
first_mentioned: "[[redai-readme-2026-06-04|RedAI GitHub README (snapshot 2026-06-04)]]"
related:
  - "[[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]"
  - "[[openant|OpenAnt]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[codex-security|Codex Security]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
  - "[[plan-validate-execute|Plan-Validate-Execute]]"
sources:
  - "[[redai-readme-2026-06-04|RedAI GitHub README (snapshot 2026-06-04)]]"
---

# RedAI

Open-source terminal workbench for **AI-driven vulnerability discovery with live-environment validation**, authored by [[kyle-polley|Kyle Polley]] under MIT. RedAI distinguishes itself from rule-based static analysis and from candidate-only LLM scanners by running every finding through a *live target* before it reaches the report: scanner agents produce candidates, then validator agents act inside a running instance of the application (a real browser, an iOS Simulator, or any plugin a user implements) and attempt to prove or disprove each finding with reproducible evidence.

**Repository:** [github.com/kpolley/redai](https://github.com/kpolley/redai) — open-source (MIT). Source snapshot: [[redai-readme-2026-06-04|RedAI GitHub README (2026-06-04)]].

Architecturally, RedAI sits alongside [[openant|OpenAnt]] as the open-source counterpart to the proprietary vendor stacks [[claude-code-security|Claude Code Security]], [[codex-security|Codex Security]], and [[mdash|MDASH]]. The structural distinction is the **environment-as-plugin** model: where OpenAnt validates inside Docker sandboxes against a fixed verification trace, RedAI exposes the validation environment as a plugin interface so the same scanner-validator pipeline can target whatever runtime the application actually runs in.

## Mechanism

The pipeline runs in three phases.

- **Discover.** A scanner agent (Claude Code or OpenAI Codex, selected at scan time) threat-models the project, prioritizes files, and produces candidate findings. Output is a list of *candidates*, not confirmed bugs.
- **Validate.** A validator agent receives each candidate plus the validation environment its plugin provides. It drives the UI, hits endpoints, writes proof-of-concept scripts, hosts helper servers, and collects artifacts (screenshots, HTTP transcripts, logs). Each candidate ends with a verdict: confirmed, disproved, or unable-to-test.
- **Report.** Findings, verdicts, and evidence write to `~/.redai/runs/<runId>/` in Markdown, HTML, and JSON. Confirmed findings ship with the artifacts the validator produced, so the report is reviewable end-to-end rather than a list of model assertions.

The full pipeline runs nine stages under the hood; the three phases are the user-facing view.

## Validation environments

Two ship in the box as reference implementations.

| Environment | Driver | Use case |
|---|---|---|
| Browser | `agent-browser` (a Chrome driver from Vercel Labs) | Web applications |
| iOS Simulator | `xcrun simctl` (per-scan template simulator) | iOS apps on macOS hosts with Xcode CLI |

Any other target (a Linux VM, an Android emulator, a remote staging cluster, a Kubernetes namespace, an embedded device shim) is reachable by writing a plugin against the documented validator-plugin interface. The plugin contract is the load-bearing surface: it is what lets one project apply the discover-validate-report pipeline against runtimes the maintainers did not anticipate.

## Placement in the field

RedAI is the **open-source, live-validation** entry in the production-path landscape catalogued by [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]]. Three observations follow.

1. **The harness-over-model argument applies.** RedAI is mechanism-agnostic about the scanner — Claude Code or Codex at user discretion — which restates the [[frontier-ai-for-vuln-discovery|thesis's]] convergent argument: the model is one input, and the engineering that distinguishes systems is the harness. RedAI's harness is the validator-plus-environment plane.
2. **Validation is the load-bearing stage.** RedAI's pipeline routes through a live environment before any finding reaches the report, the same architectural commitment [[adversarial-reflexion|Adversarial Reflexion]], MDASH's prover, and AISLE's hybrid PoC discrimination represent on the proprietary side.
3. **The plugin model lowers the cost of new targets.** Most of the production paths in the thesis target source code in a host language; RedAI generalizes by decoupling *what the scanner reads* from *where the validator acts*, which is what lets a plugin author take the pipeline somewhere the maintainers did not.

## Limitations

- **Standalone benchmark numbers are not provided.** The README does not claim a CyberGym score or recall figure on a public dataset; what it ships is the live-validation pipeline and two reference environments, with example reports against intentionally vulnerable demo apps.
- **No autonomous remediation.** RedAI reports findings; it does not generate or apply patches the way [[google-codemender-deepmind|CodeMender]] or [[claude-code-security|Claude Code Security]] do.
- **No managed service or coalition.** Unlike [[anthropic-glasswing-announcement|Project Glasswing]], RedAI runs locally on a developer or red-team operator's terminal, against targets they own or are authorized to assess.

## Relations

- The open-source counterpart in the [[frontier-ai-for-vuln-discovery|Frontier AI for Vulnerability Discovery]] thesis's "Open-source maintainer-side tooling" production path, alongside [[openant|OpenAnt]].
- Uses [[claude-code-security|Claude Code]] or [[codex-security|Codex]] as the scanner-agent input.
- Embodies the [[plan-validate-execute|plan-validate-execute]] discipline at the pipeline level: the validator is the verifier of the scanner's plan, not a separate output channel.
- Source: [[redai-readme-2026-06-04|RedAI GitHub README (snapshot 2026-06-04)]].
