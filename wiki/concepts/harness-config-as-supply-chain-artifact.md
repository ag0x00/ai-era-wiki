---
type: concept
title: "Harness Config as Supply-Chain Artifact"
address: c-000058
created: 2026-05-15
updated: 2026-09-01
tags:
  - concepts
  - supply-chain
  - sast
  - agent-config
  - claude-code
status: seed
no_public_url: "Wiki-original analytical framing; no single external canonical source"
scope_axis: [sec-of-ai, sec-against-ai]
related:
  - "[[control-efficacy-gate|Control-Efficacy Gate]]"
  - "[[agentshield|AgentShield]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agents]]"
  - "[[ai-spm|AI-SPM]]"
  - "[[ai-bom|AI-BOM]]"
  - "[[mcp-security|MCP Security]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[endor-labs-ai-code-governance|Endor Labs AI Code Governance]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[security-audit-skill|security-audit-skill]]"
  - "[[trail-of-bits-skills|trailofbits/skills]]"
  - "[[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
  - "[[semgrep|Semgrep]]"
sources:
  - "[[agentshield-announcement|AgentShield README]]"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# Harness Config as Supply-Chain Artifact

The agent harness configuration tree — `~/.claude/` and analogous directories for OpenCode, Codex, Gemini, dmux, and similar agentic developer environments — is itself a software-supply-chain artifact. Hooks, MCP server manifests, subagents, slash commands, skill `.md` files, and `CLAUDE.md` instructions compose a runtime that takes attacker-influenced inputs (community skills, public MCP servers, retrieved documents, repository templates) and exposes sensitive primitives (shell, filesystem, network, model API tokens). Treating that tree as out-of-scope of conventional supply-chain assurance leaves a category of attacks undetectable at build time.

## The Argument

Three observations compose the position:

1. **Composition without provenance.** Developers install community skills, connect MCP servers, and configure hooks without any automated way to audit the security of their setup. The composition step is identical in structure to `npm install` or `pip install` — third-party code becomes runtime behavior — but the artifacts in question (skill manifests, MCP server definitions, hook scripts) are not covered by the existing AI-BOM / SBOM / SLSA primitives, which target *model artifacts* and *library dependencies* rather than *harness configuration*. The comparison to `npm install` is no longer an analogy for one class of these artifacts. Security methodology now ships as an installable skill pack: Cloudflare's audit skill installs through `npx skills add`, Trail of Bits distributes roughly forty plugins for Claude Code and Codex, and Google and Capital One each publish skill sets of their own. Semgrep's comparison of the deployment shapes states the inheritance directly — a skill pack makes no model calls of its own and runs on the host agent's model, and carries no sandbox and inherits the host agent's.[^semgrep] The artifact is methodology and every security property under it is the host's, which is the composition-without-provenance problem with the third-party code removed and the third-party *instructions* left in place.
2. **Static-analyzable risk surface.** The same risks that classical SAST detects in application code (hardcoded secrets, command injection via interpolation, wildcard permissions, dangerous network primitives, container-escape primitives, reverse shells, credential-store reads, log tampering) reproduce verbatim inside hook scripts and `Bash(...)` permission rules. The risks that classical SAST cannot detect ([[prompt-injection|prompt injection]], hidden Unicode instructions, time-bombs in `CLAUDE.md`) are detectable by analogous rule families when applied to the harness-config tree.
3. **Provenance-aware confidence is required.** A naive scanner treating every `.claude/`-shaped file as live runtime drowns in noise from template catalogs (`mcp-configs/`), docs examples (`docs/`, `commands/`), plugin manifests (`hooks/hooks.json`), and manifest-referenced hook implementations. The scanner must distinguish source kinds and weight findings accordingly, because *"the repo ships this risky template"* is a fundamentally different finding from *"this is enabled right now"* and ought to grade differently. Real secrets stay critical regardless of source kind.

The structural conclusion: the agent harness configuration tree belongs in the same supply-chain assurance flow as the rest of the build, with a SAST-style scanner, CI gating, baseline-drift comparison, SARIF integration with code-scanning UIs, and evidence packs suitable for audit handoff.

## Worked Example

[[agentshield|AgentShield]] is the first sourced open-source implementation of this position. 102 rules across five categories (Secrets / Permissions / Hooks / MCP Servers / Agents) with provenance-aware `runtimeConfidence` labels operate over the Claude Code `.claude/` tree, with optional layered analyses (`--injection` for active prompt-injection probing, `--sandbox` for hook sandbox execution, `--taint` for data-flow tracking, and a three-agent Claude Opus 4.6 adversarial pipeline). The scanner produces SARIF output for GitHub code-scanning UIs, baseline-drift comparison for regression gates, deterministic redacted evidence packs for audit handoff, and stable-fingerprint remediation plans for CI ticketing. See [[agentshield-announcement|the source page]] for the full instrument.

## Scope Limit

The position is general; the AgentShield rule corpus is harness-specific. The rules are tuned for the `.claude/` shape, and other harnesses (OpenCode, Codex, Gemini, dmux, terminal-agent wrappers) currently surface only as local adapter-evidence markers rather than first-class rule targets. The *generalizable* contribution from the AgentShield ingest is the design discipline — config tree as artifact, provenance-aware confidence weighting, cross-file manifest awareness — not the specific 102-rule corpus. A peer non-Claude-Code instrument is the precondition for treating the position as standard rather than vendor-specific.

That precondition is still open, and the evidence base under it has changed shape. [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] (2026-04-24, CVSS 10.0) is an exploited instance in a `.gemini/` tree: headless [[gemini-cli|Gemini CLI]] trusted a workspace-supplied configuration directory and executed from it, so the artifact this concept names was the delivery vehicle for a maximum-severity finding in a harness no scanner here covers. An attack instance is not an instrument and does not close the precondition. It does move the position's status: the argument that harness configuration is executable content is now demonstrated outside the `.claude/` tree, and the gap that remains is tooling rather than generality.

Distribution has crossed harnesses even though tooling has not. Trail of Bits publishes its plugin set for Claude Code and for Codex under a single licence, and the four skill packs Semgrep surveys carry differing terms among themselves — MIT, CC-BY-SA, and Apache 2.0.[^semgrep] A licence is provenance metadata on an executable artifact, and no AI-BOM or SBOM primitive records it for a config-tree artifact today. The precondition at the head of this section stays open, because a distributed skill pack is another instance of the artifact class rather than the peer instrument that would audit it.

AgentShield's rule corpus assumes the config tree is a *persistent* artifact on a developer's machine, where a finding describes what an installed hook or MCP manifest can do. In the CI-runner shape the tree arrives with the repository under review, so provenance-aware confidence weighting inverts: a `.gemini/` directory appearing in a fork's pull request is the highest-confidence finding the scanner can produce, because it arrived with the code under review rather than from a trusted template catalog.

## Relationship to Existing Wiki Coverage

- [[supply-chain-security-for-agents|Supply Chain Security for Agents]] has historically covered skill-marketplace controls (Aguara Watch, SecureClaw), MCP server provenance, and incident anchors ([[clawhavoc|ClawHavoc]], [[sandworm-mode-npm-worm|SANDWORM_MODE]], [[litellm-supply-chain-compromise|LiteLLM]]). This concept extends the practice's scope: not just the *content* (skills, MCP packages) but the *composition manifest* (hooks, permissions, `CLAUDE.md`) is a supply-chain artifact.
- [[ai-spm|AI-SPM]] tracks AI asset inventory and misconfiguration detection at the enterprise control-plane level (Microsoft Agent 365, Wiz AI-SPM, Knostic Kirin). Harness-config scanners are a narrower per-harness instance of the same pattern, added to the AI-SPM tooling categories.
- [[ai-bom|AI-BOM]] covers build-time and runtime bills of materials for *model and skill artifacts*. Harness configuration sits adjacent to AI-BOM but is not currently part of any AI-BOM spec (CycloneDX 1.6 ML-BOM, SPDX 3.0 AI extension).
- [[mcp-security|MCP Security]] covers the threat surface of MCP itself. Harness-config audit is the *static-analysis layer* over MCP server definitions inside the host harness — complementary to the gateway / runtime / IAM controls MCP Security focuses on.
- [[endor-labs-ai-code-governance|Endor Labs AI Code Governance]] is the first catalogued **commercial** instrument operating on this artifact class, extending the unit of analysis from one developer's config tree to a fleet inventory of harnesses, versions, MCP servers, skills, and hooks with attribution back to a human operator. Its enforcement half is regular-expression matching over shell commands, which the [[guardfall-shell-injection-audit|GuardFall audit]] shows is the construction that fails — see [[guard-canonicalization-gap|Guard Canonicalization Gap]]. The inventory half is the durable contribution.
- The attack-side confirmation arrived independently: GuardFall's first immediate mitigation is *"audit repository-shipped agent configuration,"* reached from exploitation rather than from static analysis. Two disciplines converging on the same artifact is the peer evidence this concept was parked pending.
- [[securing-agentic-coding|Securing Agentic Coding]] places config audit in the data plane of the RA and lists it among the eight deployment steps.
- [[agentic-ai-security-cmm-2026|CMM]] D8 (Supply Chain & AI-BOM) houses this position for now; harness-config audit becomes a candidate evidence dimension there once a second sourced peer instrument lands.

## See Also

- [[control-efficacy-gate|Control-Efficacy Gate]] — sibling generalization from the same AgentShield ingest; corpus-gate and time-bound exception lifecycle as continuous-efficacy primitives that operate *over* the scanner this concept names.
- [[agentshield|AgentShield]] · [[agentshield-announcement|AgentShield README]] — concrete worked example.
- [[lethal-trifecta|Lethal Trifecta]] — the structural composition that harness-config rules catch at static-analysis time (`Bash(*)` + `curl ${file}` + remote MCP transports as configuration-time signal of trifecta exposure).

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The ~40-plugin figure and the category-list licences are human-written; the `npx skills add` install command, the deployment-shape inheritance table, and the per-tool descriptions are from Semgrep's LLM-generated repository summaries. Summarized at [[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]].
