---
type: product
title: "Firecracker"
created: 2026-05-03
updated: 2026-05-03
tags:
  - products
  - sandboxing
  - runtime
  - virtualization
status: stub
license: Apache 2.0
org: AWS (Amazon Web Services)
related:
  - "[[agent-sandboxing]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
---

# Firecracker

**Firecracker** is an open-source Virtual Machine Monitor (VMM) created by AWS and open-sourced under Apache 2.0. It is purpose-built for **serverless and container workloads** that require hardware-level isolation with minimal overhead. Firecracker is used by AWS Lambda and AWS Fargate in production.

## Basis for Firecracker in agent sandboxing

Agent sandboxing requires isolating the agent process so that a compromised or malicious agent cannot escape to the host, access other agents' state, or persist beyond its task boundary. Firecracker's design matches these requirements:

- **Hardware-level isolation** — Firecracker MicroVMs use KVM for true hardware virtualization, not just container namespaces. A container escape cannot escape the VM.
- **Fast startup** — MicroVMs boot in 125ms on commodity hardware. Per-task VM creation (a new MicroVM per agent task) is operationally practical.
- **Minimal attack surface** — Firecracker's device model is deliberately minimal: no USB, no video, no audio. The codebase is ~50K lines of Rust. The reduced attack surface limits guest-to-host exploits.
- **Clean termination** — A MicroVM is destroyed when the task ends; no persistent filesystem, no cached credentials, no residual state.
- **Open source** — Apache 2.0; no vendor license for the VMM itself.

## Firecracker vs alternatives

| Sandbox primitive | Type | Isolation level | Startup time | Notes |
|---|---|---|---|---|
| **Firecracker** | MicroVM (KVM) | Hardware (VM) | ~125ms | Best isolation; AWS-battle-tested; Linux only |
| gVisor | Kernel interposer | OS syscall interception | <10ms | Google OSS; weaker isolation than VM; broader OS compatibility |
| WebAssembly sandbox | Wasm runtime | Process-level | <1ms | Lowest overhead; limited capabilities for complex agents |
| Docker cgroup | Linux namespaces | Process-level | <1ms | Least isolation; container escapes are real; easiest to operate |

For **high-risk-tier agent actions** (code execution, file mutations, network calls to external APIs), Firecracker or gVisor are the recommended choices. Docker cgroup isolation is insufficient for agents that are actively targeted by prompt injection.

## In the RA / CMM

- **RA Runtime Plane:** Firecracker is the reference implementation for "Sandbox / containment — per-task VM" row, classified as `OSS`.
- **CMM D4 L3:** "Per-task sandbox for high-risk-tier actions" — a Firecracker MicroVM per task is the canonical evidence artifact.
- **CMM D4 L5 (CaMeL pattern):** When running the [[camel-pattern|CaMeL]] privileged/quarantined LLM split, Firecracker can isolate the quarantined LLM process from the privileged LLM.
- **FOSS/small-team stack:** Firecracker is the recommended OSS sandbox for high-risk-tier task isolation.

## See also

- [[agent-sandboxing|Agent Sandboxing]] — the wiki practice page on OS-level agent isolation
- [[camel-pattern|CaMeL Pattern]] — the compartmentalized LLM pattern that can use Firecracker for isolation
- [[agentic-ai-security-reference-architecture|Agentic AI Security RA]] §Runtime plane
