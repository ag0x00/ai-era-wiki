---
type: concept
title: "Cognitive File Integrity (CFI)"
address: c-000115
created: 2026-05-24
updated: 2026-07-30
tags:
  - concepts
  - integrity
  - agent-identity
  - cfi
status: seed
no_public_url: "wiki-original construct; no single external canonical source"
scope_axis: [sec-of-ai]
complexity: intermediate
domain: agent-security
aliases:
  - CFI
  - Cognitive File Integrity
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[vibe-coding|Vibe Coding]]"
  - "[[agent-memory-isolation|Agent Memory Isolation]]"
  - "[[citizen-coders|Citizen Coders]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agentic AI]]"
  - "[[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
sources:
  - "[[vibe-coding|Vibe Coding]]"
---

# Cognitive File Integrity (CFI)

## Definition

Cognitive File Integrity is an integrity control for the files that define an agent's behavior: system prompts, identity files such as `IDENTITY.md` and `SOUL.md`, and rules files. The control maintains cryptographic hash baselines for those files and verifies them at session start and after any configuration change, so an unreviewed edit to an identity file is detected rather than silently shipped.

## Rationale

[[vibe-coding|Vibe-coded]] changes to identity files can introduce subtle behavioral shifts an operator does not notice, because the operator iterates on intent rather than reviewing each diff. CFI treats those files as security-relevant assets with a verifiable baseline, the way file-integrity monitoring treats system binaries. It is adjacent to [[agent-memory-isolation|agent memory isolation]]: one protects the agent's defining files, the other protects its runtime memory.

The control's scope widened with agentic coding. `CLAUDE.md`-class instruction files, subagent definitions, skill manifests, and hook scripts are simultaneously repository content and runtime configuration: they arrive with a clone, they are edited by the agent itself, and they execute. Sandbox write-protection covers the harness settings file but not the instruction files beside it, which makes CFI the control that closes the remainder. See [[securing-agentic-coding|Securing Agentic Coding]] §Data plane and [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]].

> [!gap] Provenance unconfirmed
> CFI is used on this wiki by [[vibe-coding|Vibe Coding]] and in the conventions style examples, but a primary external source for the term has not been recorded here. Confirm origin (vendor, paper, or wiki-coinage) before treating the term as established.
