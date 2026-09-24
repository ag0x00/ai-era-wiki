---
type: entity
entity_type: product
title: "Claude Code"
created: 2026-09-17
updated: 2026-09-24
tags:
  - entities
  - products
  - claude-code
  - anthropic
  - coding-agent
  - cli
status: seed
scope_axis:
  - sec-of-ai
  - sec-against-ai
  - ai-in-sec-offense
origin: aggregated
parent_org: "[[anthropic]]"
role: "Anthropic's agentic coding harness, run from a terminal, IDE extension, desktop app or cloud session and distributed as @anthropic-ai/claude-code, with a GitHub Action wrapper for CI use and a hook surface used for in-path enforcement"
homepage: "https://github.com/anthropics/claude-code"
related:
  - "[[anthropic|Anthropic]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[claude-cowork|Claude Cowork]]"
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
  - "[[manifold-security|Manifold Security]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[agentshield|AgentShield]]"
  - "[[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[claude-code-control-sheet|Claude Code Control Sheet]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]"
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[cursor-ide|Cursor]]"
  - "[[mexican-government-ai-breach|Mexican Government Multi-Agency AI-Assisted Breach]]"
  - "[[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]]"
sources:
  - "https://github.com/anthropics/claude-code"
  - "https://code.claude.com/docs/en/monitoring-usage"
  - "https://code.claude.com/docs/en/claude-code-on-the-web"
  - "https://code.claude.com/docs/en/claude-tag"
  - "https://code.claude.com/docs/en/slack"
  - "https://code.claude.com/docs/en/desktop"
  - "https://code.claude.com/docs/en/sandbox-environments"
  - "https://claude.com/docs/cowork/overview"
  - "https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code"
  - "https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/"
  - "https://www.manifold.security/blog/ai-coding-agents-git-hijack"
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified: 2026-09-24
verified_against: []
verified_findings: 0
verified_note: "Diff-scoped 2026-09-24: l.67 cloud entry points gain the Slack mention and [^surfaces] extended, checked against the saved cloud, claude-tag and slack docs (item 8). V5's whole-page read earlier today, the GitSpawn raw among its sources, left nothing open."
---

# Claude Code

**Sources:** [claude-code (GitHub)](https://github.com/anthropics/claude-code) · [Claude Code documentation](https://code.claude.com/docs/en/monitoring-usage) · [npm download volume, 2026-07-28 to 2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code)

[[anthropic|Anthropic]]'s agentic coding harness, published on npm as `@anthropic-ai/claude-code` and wrapped for continuous integration by a first-party GitHub Action. [[manifold-security|Manifold Security]] cites over 77 million npm downloads across the 31 days ending [2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code) for the package, the only distribution figure it gives for any agent in its [[gitspawn-coding-agent-git-config-rce|GitSpawn]] set; the count bounds distribution rather than installed base, because it includes mirrors, re-installs and continuous-integration fetches.[^npm]

The harness runs on the developer's machine from a terminal, the VS Code and JetBrains extensions, or the Desktop app's Local and WSL environments, on a remote machine the developer manages through the Desktop app's SSH environment, and as a cloud session started from claude.ai/code, the Claude mobile app, the Desktop app's Cloud environment, `claude --cloud`, a routine or an `@Claude` mention in Slack.[^surfaces] Sessions on the developer's machine or over SSH belong to the two local shapes of [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]], and every cloud-session entry point to its delegated cloud shape.

The coding agent is distinct from [[claude-code-security|Claude Code Security]], a separate Anthropic product that reads and reasons about codebases for vulnerability discovery. It is also distinct from [[claude-cowork|Claude Cowork]], which runs the same agentic architecture for knowledge work inside Claude Desktop and loads the skills and plugins enabled for the member's claude.ai account, because Cowork [does not read the Claude Code CLI's `~/.claude` directory on the machine](https://claude.com/docs/cowork/overview). The configuration tree below is therefore the coding agent's alone.

Four roles matter here.

**As a configuration tree.** The harness reads a workspace-local `.claude/` directory and a user-level `~/.claude/` tree carrying hooks, MCP server manifests, subagents, slash commands, skills and instruction files. That tree is the original worked instance of [[harness-config-as-supply-chain-artifact|harness config as supply-chain artifact]], and [[agentshield|AgentShield]]'s rule corpus is tuned to its shape.

**As an enforcement surface.** Claude Code exposes hooks that run in path around tool calls, and Uber's [[adr-agentic-detection-system|ADR]] deployment uses them for real-time blocking of high-severity credential leakage while its sensor reconstructs sessions from the local caches the harness writes. [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] carries the architectural argument that deployment settles.

**As a CI-runner agent.** The GitHub Action places the harness in the CI-runner variant of [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]], where no human is positioned to see an action before it executes. Microsoft Defender researchers extracted a model API key from that shape through a prompt injection in issue or pull-request content, which drove the unsandboxed Read tool to `/proc/self/environ` past a shell-only sandbox boundary ([[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]). The harness's documentation states which repository content a headless run uses before a folder is trusted, which [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]] reads as this harness's answer to the question of when isolation starts.

**As offensive tooling.** Two catalogued campaigns drove the agent against third-party targets: [[gtg-1002-ai-orchestrated-espionage|GTG-1002]], where it orchestrated reconnaissance and intrusion across roughly thirty targets, and the [[mexican-government-ai-breach|Mexican government multi-agency breach]], where it carried about 75 percent of the operator's remote command execution.

[[claude-code-control-sheet|Claude Code Control Sheet]] maps the harness's configurable controls to criteria in all nine domains of the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]], with the evidence an assessor collects for each, and [[securing-agentic-coding|Securing Agentic Coding]] catalogs the same controls by reference-architecture plane.

## Security Record

| Date | Item | Reported effect |
|---|---|---|
| 2026-06-05 | [[claude-code-github-action-credential-exposure\|GitHub Action credential exposure]] | Model API key extraction from a CI workflow; unsandboxed file-read tool |
| 2026-09-01 | [[gitspawn-coding-agent-git-config-rce\|GitSpawn startup `core.fsmonitor` execution]] | Host command execution before the workspace-trust prompt; patched in 2.1.196 |
| 2026-09-01 | [[gitspawn-coding-agent-git-config-rce\|GitSpawn `ultrareview` execution]] | Host command execution on start-up through an undisclosed git config key; unpatched at 2.1.252 as of that date |

[[anthropic-sandbox-runtime|`@anthropic-ai/sandbox-runtime`]] is Anthropic's whole-process wrapper for closing the coverage asymmetry the first of those findings exploited. The hook surface does not reach the context-gathering subprocess the GitSpawn findings use, because that subprocess belongs to the harness rather than to a tool call. The wrapper confines that subprocess without stopping it, because a Claude Code process launched through the runtime starts inside the runtime's filesystem and network boundary.[^srt]

## Notes

[^surfaces]: [Anthropic — Use Claude Code in the cloud](https://code.claude.com/docs/en/claude-code-on-the-web), fetched 2026-09-24: the cloud-session entry points, Claude Tag among them under "Cloud environments", and terminal, IDE and Desktop Local sessions running on the developer's machine. A Slack mention starts a cloud session through [Claude Tag](https://code.claude.com/docs/en/claude-tag), which runs as the organization's shared identity on Team and Enterprise plans, or through the earlier [Claude Code in Slack](https://code.claude.com/docs/en/slack), which runs each session under the individual user's account and remains the route on Pro and Max plans. The Desktop app's Local, Cloud, SSH and WSL environments are defined in [Desktop application, "Environment configuration"](https://code.claude.com/docs/en/desktop#environment-configuration).
[^srt]: [Anthropic — Choose a sandbox environment, "Sandbox runtime"](https://code.claude.com/docs/en/sandbox-environments#sandbox-runtime), fetched 2026-09-24: the package "wraps an entire process", running Claude Code through it "constrains every tool, hook, and MCP server in the session, not only shell commands", and "Claude Code starts inside the sandbox with the filesystem and network boundaries you configured".
[^npm]: [npm registry downloads API, `@anthropic-ai/claude-code`, 2026-07-28 to 2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code). Package downloads over a 31-day window, the endpoint cited by [Manifold Security](https://www.manifold.security/blog/ai-coding-agents-git-hijack) for the figure.
