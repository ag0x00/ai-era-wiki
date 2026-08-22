---
type: paper
title: "GuardFall Shell-Injection Audit"
address: c-000239
created: 2026-07-30
updated: 2026-08-16
tags:
  - papers
  - agentic-coding
  - prompt-injection
  - supply-chain
  - sandboxing
  - adversa
status: summarized
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: aggregated
year: 2026
authors:
  - "Omer Ben Simon"
venue: "Adversa AI research blog"
source_url: "https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/"
no_public_url: ""
key_claim: "Ten of eleven surveyed open-source coding and computer-use agents can be driven to execute arbitrary shell commands because their command guards inspect raw strings while bash executes post-expansion text."
methodology: "Static review of each agent's guard implementation plus live end-to-end exploitation using Claude Sonnet 4.6 against malicious MCP servers, injected READMEs, and compromised Makefiles."
contradicts: []
supports:
  - "[[guard-canonicalization-gap]]"
related:
  - "[[guard-canonicalization-gap|Guard Canonicalization Gap]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
sources:
  - https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/
---

# GuardFall Shell-Injection Audit

**Source:** [Adversa AI — Open-Source AI Coding Agents Shell Injection Vulnerability](https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/) (2026-06-30), by Omer Ben Simon.

## Key Claim

Command allowlists and blocklists in agentic coding harnesses evaluate a string that is not the string the shell runs. Bash performs quote removal, parameter expansion, and command substitution before execution, so a guard that pattern-matches raw text can be walked past with syntax that has existed for decades. Ten of the eleven agents surveyed were exploitable; one was not.

## Methodology

Two passes. Static review of each agent's guard implementation identified where the check ran and what it matched. Live end-to-end exploitation then drove each agent with Claude Sonnet 4.6 against three realistic delivery channels: a malicious MCP server, an injected `README`, and a compromised `Makefile`. The delivery channels are the point — the operator never types the payload, so this is an [[indirect-prompt-injection|indirect prompt injection]] result, not a misuse result.

## Notable Findings

**Agents surveyed (11).** Aider, Cline, Continue, Goose, NousResearch Hermes, Open Interpreter, OpenHands, opencode, Plandex, Roo-Code, SWE-agent. Only **Continue** resisted.

**Five bypass classes.**

| Class | Mechanism | Illustration |
| --- | --- | --- |
| A | Quote removal | `r''m` reaches the shell as `rm` |
| B | `$IFS` expansion for field splitting | separator supplied by the environment |
| C | Command substitution computing a binary name | the name never appears literally |
| D | Base64 decoded into a shell interpreter | payload opaque to text matching |
| E | Alternative destructive `argv` combinations | same effect, unmatched spelling |

**The defending design.** Continue runs five sequential steps rather than one match: tokenize with `shell-quote`, detect variable-expansion patterns, recursively evaluate substitutions, check pipe destinations for interpreters, then match an explicit disabled-pattern list. Against the twelve canonical destructive cases the audit reports that none reached the agent's `allowedWithoutPermission` path.

**Framing.** The author declines to file the result as a set of bugs and calls it *"not a bug, but a dangerous convention and a class of problems."* No CVEs were assigned and no coordinated-disclosure timeline is published.

**Recommended mitigations** split into immediate and quarterly. Immediate: relocate `$HOME` behind a wrapper so credential paths fall outside the agent's reach, capture multi-line scripts for out-of-band review, audit repository-shipped agent configuration, disable fork-PR execution in CI, and remove auto-execution flags. Quarterly: adopt the tokenize-and-canonicalize evaluator, keep a guard test harness in CI, separate operational filters from security filters, and re-run the probes against each new model.

## Strengths and Weaknesses

The static-plus-live design is the strength: a static finding that also lands end-to-end through a realistic delivery channel is not a theoretical bypass. Publishing the one architecture that held, with its five steps enumerated, converts the result from a scare into a design specification.

The audit carries three limits. The vendor is a commercial AI-security firm publishing on its own blog, so the result carries no independent replication. No CVEs and no disclosure timeline means the reader cannot tell which findings were fixed before publication. The survey covers open-source agents only — [[claude-code-security|Claude Code]], Cursor, and Codex are absent, so the result says nothing directly about the harnesses with the largest enterprise install base, and the absence should not be read as a clean bill for them. [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] is one disclosure from that excluded group, and it fails a step earlier than anything in this audit: [[gemini-cli|Gemini CLI]]'s `--yolo` mode suppressed the tool allowlist entirely rather than evaluating it against the wrong representation. A guard defeated by shell syntax and a guard removed by an autonomy flag are separated in [[guard-canonicalization-gap|Guard Canonicalization Gap]].

## Relations

- Supports [[guard-canonicalization-gap|Guard Canonicalization Gap]] — the audit is the empirical anchor for that concept.
- Supports [[agent-sandboxing|Agent Sandboxing]] — every bypass class is defeated by an OS-level boundary that constrains the process regardless of which string was approved, which is the argument for enforcement below the guard rather than inside it.
- Extends [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] — "audit repository-shipped agent configuration" is the same position arrived at from the attack side.
- Grounds a [[agentic-ai-security-cmm-d3-control-least-agency|D3]] observation: a text-matching command guard is not a policy decision point, and scoring it as one overstates control maturity.
- Supplies the empirical basis for the ranking that organizes [[securing-agentic-coding|Securing Agentic Coding]]: controls that constrain the process outrank controls that inspect a string, because ten of eleven string-inspecting guards here were walked past with shell syntax older than the tools.
