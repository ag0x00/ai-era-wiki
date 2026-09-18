---
type: entity
entity_type: product
title: "Claude Code"
created: 2026-09-17
updated: 2026-09-18
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
role: "Anthropic's terminal coding agent, distributed as @anthropic-ai/claude-code, with a GitHub Action wrapper for CI use and a hook surface used for in-path enforcement"
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
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[cursor-ide|Cursor]]"
  - "[[mexican-government-ai-breach|Mexican Government Multi-Agency AI-Assisted Breach]]"
  - "[[gtg-1002-ai-orchestrated-espionage|GTG-1002: AI-Orchestrated Espionage Campaign]]"
sources:
  - "https://github.com/anthropics/claude-code"
  - "https://docs.claude.com/en/docs/claude-code/monitoring-usage"
  - "https://claude.com/docs/cowork/overview"
  - "https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code"
  - "https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/"
  - "https://www.manifold.security/blog/ai-coding-agents-git-hijack"
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
verified: 2026-09-18
verified_against: []
verified_findings: 0
verified_note: "Fresh-eyes and source read of the desktop-agent productivity-assistant row against Anthropic's live Cowork documentation: the Team/Enterprise, architecture, OTel and enterprise-administrator articles, the Cowork overview and monitoring reference, and the Compliance API announcement. Nothing archived to .raw/. Scoped to the desktop-agent content this pass added; the rest of the page was not re-read."
---

# Claude Code

**Sources:** [claude-code (GitHub)](https://github.com/anthropics/claude-code) · [Claude Code documentation](https://docs.claude.com/en/docs/claude-code/monitoring-usage) · [npm download volume, 2026-07-28 to 2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code)

[[anthropic|Anthropic]]'s terminal-resident coding agent, published on npm as `@anthropic-ai/claude-code` and wrapped for continuous integration by a first-party GitHub Action. [[manifold-security|Manifold Security]] cites over 77 million npm downloads across the 31 days ending [2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code) for the package, the only distribution figure it gives for any agent in its [[gitspawn-coding-agent-git-config-rce|GitSpawn]] set; the count bounds distribution rather than installed base, because it includes mirrors, re-installs and continuous-integration fetches.[^npm]

The coding agent is distinct from [[claude-code-security|Claude Code Security]], a separate Anthropic product that reads and reasons about codebases for vulnerability discovery. It is also distinct from [[claude-cowork|Claude Cowork]], which runs the same agentic architecture for knowledge work inside Claude Desktop and loads the skills and plugins enabled for the member's claude.ai account, because Cowork [does not read the Claude Code CLI's `~/.claude` directory on the machine](https://claude.com/docs/cowork/overview). The configuration tree below is therefore the coding agent's alone.

Four surfaces matter here.

**As a configuration tree.** The harness reads a workspace-local `.claude/` directory and a user-level `~/.claude/` tree carrying hooks, MCP server manifests, subagents, slash commands, skills and instruction files. That tree is the original worked instance of [[harness-config-as-supply-chain-artifact|harness config as supply-chain artifact]], and [[agentshield|AgentShield]]'s rule corpus is tuned to its shape.

**As an enforcement surface.** Claude Code exposes hooks that run in path around tool calls, and Uber's [[adr-agentic-detection-system|ADR]] deployment uses them for real-time blocking of high-severity credential leakage while its sensor reconstructs sessions from the local caches the harness writes. [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] carries the architectural argument that deployment settles.

**As a CI-runner agent.** The GitHub Action places the harness in the CI-runner variant of [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]], where no human is positioned to see an action before it executes. [[claude-code-github-action-credential-exposure|Microsoft Defender research]] extracted a model API key from that shape through an HTML-comment injection in a pull request, reaching `/proc/self/environ` past a shell-only sandbox boundary.

**As offensive tooling.** Two catalogued campaigns drove the agent against third-party targets: [[gtg-1002-ai-orchestrated-espionage|GTG-1002]], where it orchestrated reconnaissance and intrusion across roughly thirty targets, and the [[mexican-government-ai-breach|Mexican government multi-agency breach]], where it carried about 75 percent of the operator's remote command execution.

## Security Record

| Date | Item | Reported effect |
|---|---|---|
| 2026-06-05 | [[claude-code-github-action-credential-exposure\|GitHub Action credential exposure]] | Model API key extraction from a CI workflow; unsandboxed file-read tool |
| 2026-09-01 | [[gitspawn-coding-agent-git-config-rce\|GitSpawn startup `core.fsmonitor` execution]] | Host command execution before the workspace-trust prompt; patched in 2.1.196 |
| 2026-09-01 | [[gitspawn-coding-agent-git-config-rce\|GitSpawn `ultrareview` execution]] | Host command execution on start-up through an undisclosed git config key; unpatched at 2.1.252 as of that date |

[[anthropic-sandbox-runtime|`@anthropic-ai/sandbox-runtime`]] is Anthropic's whole-process wrapper for closing the coverage asymmetry the first of those findings exploited. Neither it nor the hook surface sits on the path of the context-gathering subprocess the GitSpawn findings use, because that subprocess belongs to the harness rather than to a tool call.

## Notes

[^npm]: [npm registry downloads API, `@anthropic-ai/claude-code`, 2026-07-28 to 2026-08-27](https://api.npmjs.org/downloads/point/2026-07-28:2026-08-27/@anthropic-ai/claude-code). Package downloads over a 31-day window, the endpoint cited by [Manifold Security](https://www.manifold.security/blog/ai-coding-agents-git-hijack) for the figure.
