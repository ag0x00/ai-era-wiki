---
type: entity
title: "Anthropic Sandbox Runtime"
address: c-000245
created: 2026-07-30
updated: 2026-08-21
tags:
  - entities
  - product
  - tool
  - sandboxing
  - claude-code
  - foss
status: seed
scope_axis:
  - sec-of-ai
entity_type: product
role: "Open-source process wrapper that applies the same Seatbelt (macOS) and bubblewrap (Linux, WSL2) isolation Claude Code uses for Bash to an entire process, bringing built-in file tools, MCP servers, and hooks inside one OS boundary. Beta research preview; configuration format subject to change. Denies all write and network access by default."
homepage: https://github.com/anthropic-experimental/sandbox-runtime
package: "@anthropic-ai/sandbox-runtime"
maintainer: "[[anthropic|Anthropic]]"
related:
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[anthropic|Anthropic]]"
  - "[[gvisor|gVisor]]"
  - "[[firecracker|Firecracker]]"
  - "[[gke-agent-sandbox|GKE Agent Sandbox]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|D4 Runtime & Guardrails]]"
  - "[[anti-patterns-and-failure-modes|RA and CMM Anti-Patterns and Failure Modes]]"
sources:
  - https://code.claude.com/docs/en/sandbox-environments
  - https://github.com/anthropic-experimental/sandbox-runtime
---

# Anthropic Sandbox Runtime

**Repository:** [anthropic-experimental/sandbox-runtime](https://github.com/anthropic-experimental/sandbox-runtime)

`@anthropic-ai/sandbox-runtime` wraps an arbitrary process in the operating-system isolation primitives that [[claude-code-security|Claude Code]]'s built-in Bash sandbox uses — Seatbelt on macOS, bubblewrap on Linux and WSL2. Launched as `npx @anthropic-ai/sandbox-runtime claude`, it puts the whole agent session inside one boundary rather than only its shell subprocesses.

## Rationale

Claude Code's built-in sandbox covers Bash commands and their children. Three things run outside it: the in-process file tools (Read, Edit, Write), MCP servers, and hooks. Each is a separate process or a separate code path executing on the host, and each has produced a real finding — the [[claude-code-github-action-credential-exposure|Microsoft Defender research]] escaped through the unsandboxed Read tool while the Bash boundary was intact.

The runtime closes that asymmetry without requiring Docker. On the isolation ladder it occupies the rung between the per-command sandbox and a container: whole-session coverage, no container runtime, host-kernel sharing.

## Configuration Posture

Default-deny on both write and network. A working configuration in `~/.srt-settings.json` must explicitly allow the project directory, Claude Code's own configuration paths (`~/.claude` and `~/.claude.json`), `/tmp`, and the model API endpoint. The agent's own configuration directory has to stay writable for the session to function, so the runtime's boundary does not by itself prevent an agent from rewriting its instructions. That is the [[cognitive-file-integrity|cognitive file integrity]] problem rather than an isolation problem.

## Status and Limits

Beta research preview, with the package documentation stating that the configuration format may change. There is no native Windows support, because the underlying primitives have none. The isolation is kernel-shared, so it is weaker than a VM or microVM boundary and appropriate for constraining a misbehaving agent rather than for containing untrusted code with kernel-exploitation capability.

## Placement

Runtime plane of the [[agentic-ai-security-reference-architecture|AAI-S RA]]. It is the cheapest control that brings MCP servers and hooks inside an enforcement boundary, which makes it the natural [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] evidence artifact for an organization that has adopted agentic coding on developer workstations and cannot mandate containers. An OS boundary degrades more gracefully than a text-matching guard, per the [[guard-canonicalization-gap|guard canonicalization gap]].

D4 evidence has to name the covered surface, and naming this package does not do that. [[anti-patterns-and-failure-modes|RA and CMM Anti-Patterns and Failure Modes]] entry B6 grades "sandboxed" recorded as a state as an anti-pattern, so the assessable artifact records which paths, destinations, and identities the boundary admits. The same entry sets whole-process isolation — this runtime, a container, or a VM — as the requirement for unattended runs, and grades per-command isolation as insufficient there.

The scope argument for it is made in [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]: the built-in Bash sandbox constrains Bash and its children, while in-process file tools, MCP servers, and hooks run on the host. Wherever a deployment shape suppresses the approval prompt, that residual surface is unmediated, which is what moves this package from a convenience to the load-bearing runtime control.
