---
type: entity
title: "AgentShield"
address: c-000055
created: 2026-05-15
updated: 2026-08-21
tags:
  - entities
  - product
  - tool
  - scanner
  - claude-code
status: seed
scope_axis: [sec-of-ai, ai-in-sec-offense, sec-against-ai]
entity_type: product
role: "Security auditor for AI agent configurations — CLI, GitHub Action, ECC plugin, and GitHub App for scanning `.claude/` directories for hardcoded secrets, permission misconfigurations, hook injection, MCP server risks, and agent prompt-injection vectors."
homepage: https://github.com/affaan-m/agentshield
license: MIT
language: TypeScript
package: ecc-agentshield
author: "[[affaan-m|Affaan M]]"
first_mentioned: "[[agentshield-announcement|AgentShield README]]"
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[affaan-m|Affaan M]]"
  - "[[agentshield-announcement|AgentShield paper page]]"
  - "[[anthropic|Anthropic]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agents]]"
  - "[[ai-spm|AI Security Posture Management]]"
  - "[[mcp-security|MCP Security]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[endor-labs-ai-code-governance]]"
  - "[[guardfall-shell-injection-audit]]"
  - "[[securing-agentic-coding]]"
sources:
  - https://github.com/affaan-m/agentshield
  - https://www.npmjs.com/package/ecc-agentshield
  - https://github.com/apps/ecc-tools
  - https://cerebralvalley.ai/e/claude-code-hackathon
  - "[[.raw/articles/agentshield-2026-05-15.md]]"
---

# AgentShield

**Sources:** [GitHub — `affaan-m/agentshield`](https://github.com/affaan-m/agentshield) · [npm — `ecc-agentshield`](https://www.npmjs.com/package/ecc-agentshield) · [ECC Tools GitHub App](https://github.com/apps/ecc-tools) · [Cerebral Valley × Anthropic Claude Code Hackathon](https://cerebralvalley.ai/e/claude-code-hackathon)

## Description

AgentShield is an open-source security scanner whose unit of analysis is the agent-configuration tree itself — `~/.claude/` or any `.claude/`-shaped directory containing hooks, MCP server manifests, subagents, slash commands, skill `.md` files, and `CLAUDE.md` instructions. 102 rules across five categories (secrets, permissions, hooks, MCP servers, agents) produce a 0–100 score with an A–F grade. Provenance-aware `runtimeConfidence` labels distinguish active runtime config from project-local overrides, template catalogs, docs examples, plugin manifests, and manifest-resolved non-shell hook implementations. Optional layered analyses: active prompt-injection probing (`--injection`), sandboxed hook execution (`--sandbox`), data-flow taint tracking (`--taint`), and a three-agent [[red-team-blue-team-auditor-pipeline|Red Team / Blue Team / Auditor]] adversarial pipeline powered by Claude Opus 4.6 (`--opus`).

## Relevance to This Wiki

First open-source scanner the wiki has cataloged whose unit of analysis is the *Claude Code config surface itself*. Operationalizes practices the wiki has previously discussed in the abstract: skill-marketplace controls and MCP-package provenance for [[supply-chain-security-for-agents|Supply Chain Security for Agents]], asset inventory and misconfiguration detection for [[ai-spm|AI-SPM]], hardened single-agent runtime design for [[agent-sandboxing|Agent Sandboxing]] (via the bundled MiniClaw runtime), and static recognition of the structural conditions for [[lethal-trifecta|Lethal Trifecta]] composition (`Bash(*)` + `curl ${file}` + remote MCP transports).

Natural evidence instrument for [[agentic-ai-security-cmm-2026|CMM]] domains D3 (Control & Least-Agency via permission rules), D4 (Runtime & Guardrails via hook + agent + prompt-injection rules and the bundled MiniClaw sandboxed runtime), D5 (Egress & Network via MCP remote-transport rules and network-exposure rules), D8 (Supply Chain & AI-BOM via MCP-package provenance and skill-marketplace controls), and D9 (Operations & Human Factors via baseline-drift gate, organization-policy presets, and time-bound exception-lifecycle audit). See [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] and [[control-efficacy-gate|Control-Efficacy Gate]] for the two design disciplines parked as candidate CMM revision-pass additions.

## Outputs

- **CLI** — `npm install -g ecc-agentshield` or `npx ecc-agentshield scan`. Subcommands: `scan`, `init` (secure baseline config), `baseline write`, `evidence-pack verify`, `policy init`, `runtime install / status / repair`, `miniclaw start`.
- **GitHub Action** — `uses: affaan-m/agentshield@v1`. Inputs: `path`, `min-severity`, `fail-on-findings`, `format`, `sarif-output`, `baseline`, `save-baseline`, `policy`, `fail-on-policy`. Outputs: score, grade, finding counts, SARIF path, baseline drift, policy status. Writes Markdown job summaries and emits GitHub annotations inline.
- **ECC plugin** — distribution channel into the *Everything Claude Code* ecosystem.
- **ECC Tools GitHub App** — integrated scanning across a GitHub organization. Paid *Pro* tier (\$19 / seat / month, Stripe-billed) for automated repo analysis.
- **MiniClaw** — bundled minimal sandboxed agent runtime exposed as a single HTTP endpoint and importable as `ecc-agentshield/miniclaw`. Four independently enforced security layers (server / prompt-router / tool-whitelist / sandbox-FS), zero external runtime dependencies (Node built-ins only).
- **Output formats** — terminal, JSON, Markdown, HTML, SARIF, evidence pack (deterministic, SHA-256-digested, redacted), and stable-fingerprint JSON remediation plan.

## Notable Design Choices

- **Provenance-aware false-positive control.** Same rule, different score weight by source kind: `template-example` and `docs-example` at `0.25×`, `project-local-optional` at `0.75×`, `plugin-manifest` at `0.5×`. Real secrets stay at full weight everywhere. Single template file capped at 10 deduction points per category so one catalog cannot dominate the grade.
- **Cross-file hook-manifest awareness.** Settings-only `hooks-no-pretooluse` suppressed when a companion `hooks/hooks.json` manifest declares `PreToolUse` hooks. Manifest-referenced non-shell hook implementations discovered through indirection and analyzed under the `hook-code` confidence label.
- **Corpus-gate with prioritized improvement plan.** `agentshield scan --corpus-gate` fails CI if the built-in attack corpus (env proxy / DNS exfiltration / runtime import mutation / env-token exfiltration / credential-store access / clipboard access) is not fully detected, and produces a per-category / per-missing-rule improvement plan when it regresses.
- **Time-bound policy exceptions.** Organization-policy files declare exceptions with `id`, `rule`, `owner`, `reason`, `expires_at`, `scope`, `ticket`. The exception lifecycle audit surfaces total / active / expiring-within-7-days / expired exceptions so temporary waivers stay visible in branch-protection evidence.
- **Stable-fingerprint remediation plans.** Plans omit raw evidence and fix-before/after values so they can be attached to CI tickets without copying token-shaped strings.
- **Evidence pack redaction default-on.** Local paths, usernames, emails, and token-shaped strings are redacted unless `--no-evidence-redact` is set for private internal bundles.
- **MiniClaw narrowness.** Single HTTP endpoint, isolated per-session FS, prompt-router strips 12+ injection-pattern categories (system-prompt overrides, identity reassignment, jailbreaks, data-exfil URLs, zero-width Unicode, base64 payloads), three-tier tool whitelist (Safe / Guarded / Restricted; restricted disabled by default), 10 KB request cap, 10 req/min/IP rate limit, 10 MB file cap, 5-min timeout, no network by default.

## Peer-instrument status

The scanner's unit of analysis — the harness configuration tree — now has independent corroboration from two directions. [[endor-labs-ai-code-governance|Endor Labs AI Code Governance]] applies the same unit commercially and across harness vendors, and [[guardfall-shell-injection-audit|the GuardFall audit]] arrived at *"audit repository-shipped agent configuration"* from exploitation rather than static analysis. Two disciplines converging on that surface is the evidence [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] required before treating the position as more than vendor-specific.

## Adjacent Gaps

- **Harness coverage beyond Claude Code.** Rule corpus is tuned for `.claude/` shape; other harnesses (OpenCode, Codex, Gemini, dmux, terminal-agent wrappers) currently surface only as local adapter-evidence markers.
- **Freeform `skill-md` prompt text** has narrower coverage than `agent-md` / `CLAUDE.md`.
- **Non-shell hook execution beyond the narrow `hook-code` signals** (context injection, transcript access, child-process execution) still needs language-aware analysis.
- **No GA library API yet** — the package root export is the CLI entrypoint, not a semver-stable scanner module. Consumers should treat JSON / SARIF reports as the supported automation surface.
