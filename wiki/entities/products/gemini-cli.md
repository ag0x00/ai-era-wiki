---
type: entity
entity_type: product
title: "Gemini CLI"
address: c-000291
created: 2026-08-16
updated: 2026-08-16
tags:
  - products
  - gemini-cli
  - google
  - coding-agent
  - cli
status: seed
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: aggregated
parent_org: "[[google]]"
role: "Google's terminal coding agent, distributed as @google/gemini-cli, with a first-party GitHub Action wrapper for CI use"
homepage: "https://github.com/google-gemini/gemini-cli"
related:
  - "[[google|Google]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]"
  - "[[cedar|Cedar]]"
  - "[[cursor-ide|Cursor]]"
  - "[[claude-code-security|Claude Code Security]]"
sources:
  - "https://github.com/google-gemini/gemini-cli"
  - "https://github.com/google-github-actions/run-gemini-cli"
  - "https://github.com/advisories/GHSA-wpqr-6v78-jr5g"
---

# Gemini CLI

**Sources:** [gemini-cli (GitHub)](https://github.com/google-gemini/gemini-cli) · [run-gemini-cli action](https://github.com/google-github-actions/run-gemini-cli) · [GHSA-wpqr-6v78-jr5g](https://github.com/advisories/GHSA-wpqr-6v78-jr5g)

Google's terminal-resident coding agent, published on npm as `@google/gemini-cli`. Its CI wrapper, the `google-github-actions/run-gemini-cli` Action, carries the wiki's only maximum-severity coding-agent advisory.

Three surfaces matter here.

**As a policy-enforcement target.** [[sondera|Sondera]]'s [[hooking-coding-agents-with-cedar-talk|Cedar hook harness]] enumerates Gemini CLI as one of three intercepted coding-agent surfaces. It is the only one of the three offering model-level hooks — before and after the model, permitting individual tokens to be streamed — where [[claude-code-security|Claude Code]] exposes none and [[cursor-ide|Cursor]] intercepts at the tool and shell layer instead. The multi-turn information-flow-control demo in that talk runs on Gemini CLI.

**As a configuration tree.** The harness reads a workspace-local `.gemini/` directory for settings and environment, and a user-level `~/.gemini/settings.json` carrying the fine-grained tool allowlist. Both are instances of [[harness-config-as-supply-chain-artifact|harness config as supply-chain artifact]], and the workspace-local one is where [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] landed.

**As a CI-runner agent.** `run-gemini-cli` places the harness in the [[generative-coding-deployment-shape-2026|CI-runner shape]] — event-triggered, no human positioned to see an action before it executes. Two autonomy controls define its posture there: workspace trust, which since 0.39.1 must be granted explicitly through `GEMINI_TRUST_WORKSPACE` rather than inferred in headless mode, and `--yolo`, which auto-approves tool calls and, before the same release, also suppressed the tool allowlist.

## Security Record

| Date | Item | Severity |
|---|---|---|
| 2026-04-24 | [[gemini-cli-workspace-trust-rce\|Workspace-trust and `--yolo` allowlist bypasses]] (GHSA-wpqr-6v78-jr5g) | CVSS 10.0, patched in 0.39.1 / action 0.1.22 |
