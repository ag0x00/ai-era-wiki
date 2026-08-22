---
type: concept
title: "Guard Canonicalization Gap"
address: c-000243
created: 2026-07-30
updated: 2026-08-16
tags:
  - concepts
  - agentic-coding
  - policy-enforcement
  - sandboxing
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
no_public_url: "Wiki-original generalization; empirical anchor is the Adversa AI GuardFall audit"
aliases:
  - "Canonicalization gap"
related:
  - "[[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[least-agency-principle|Least-Agency Principle]]"
  - "[[capability-based-authorization|Capability-Based Authorization]]"
  - "[[plan-validate-execute|Plan-Validate-Execute Pattern]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]]"
  - "[[hooking-coding-agents-with-cedar-talk|Hooking Coding Agents with Cedar]]"
  - "[[numbat|Numbat]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
sources:
  - https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/
---

# Guard Canonicalization Gap

A **guard canonicalization gap** exists wherever an agent's policy check evaluates a representation of an action that the executing subsystem will transform before performing it. The check and the execution disagree about what the action *is*, so an attacker who controls the representation controls the outcome without ever contradicting the policy.

> [!gap] Single-source generalization
> The name and the framing are wiki-original. The empirical basis is one vendor audit ([[guardfall-shell-injection-audit|GuardFall]], Adversa AI) plus the Claude Code CVE record. The further instances listed below are argued by structural analogy, not measured. A second independent audit — ideally covering the commercial harnesses GuardFall did not survey — is the evidence this concept is missing. [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] reaches into that excluded group but is a single disclosure rather than a survey, and its allowlist defect is the adjacent failure below rather than an instance of this gap.

The gap is not a matching-quality problem. Adding patterns to a blocklist that inspects pre-expansion text does not close it, because the attacker is not evading the patterns — the attacker is supplying a string whose meaning changes after the check has finished.

## The Canonical Instance

The [[guardfall-shell-injection-audit|GuardFall audit]] (Adversa AI, 2026-06-30) demonstrates the gap in agentic coding harnesses. Command guards inspect the raw command string; bash then performs quote removal, parameter expansion, and command substitution before execution. Ten of eleven surveyed open-source agents were exploitable. `r''m` passes a check for `rm` and executes as `rm`; `$IFS` supplies a separator the guard never saw; command substitution computes a binary name that appears nowhere in the inspected text; base64 piped to an interpreter makes the payload opaque to text matching entirely.

## Other occurrences

The same structure recurs whenever a policy layer sits above a transforming executor:

- **Path checks above symlink resolution.** A deny rule on a settings file that resolves to a different inode after the check is the shape behind CVE-2026-39861 and CVE-2026-25725 in [[claude-code-security|Claude Code]]; the second case is subtler still, because a read-only bind mount cannot be applied to a path that does not yet exist, so the guard was absent rather than wrong.
- **Tool-name allowlists above dynamic dispatch.** An MCP tool approved by name whose server later rewrites the description or arguments behind that name.
- **Diff review above build execution.** A reviewed patch that is safe as text and hostile once a build script interprets it.
- **Prompt-level instruction filters above model interpretation.** The general case, and the reason instruction-level filtering grades weaker than a structural control.

## The Adjacent Failure

A guard that never runs produces the same outcome and is not this gap. In [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]], Gemini CLI's `--yolo` flag suppressed the fine-grained tool allowlist in `settings.json` outright rather than merely skipping confirmation prompts, so a workflow that had enumerated two permitted commands executed anything the model was steered into ([Pillar Security](https://www.pillar.security/blog/my-agentic-trust-issues-from-prompt-injection-to-supply-chain-compromise-on-gemini-cli)). Nothing was canonicalized incorrectly, because nothing was evaluated.

Keeping the two apart matters because the closures differ. The canonicalization gap is closed by making the guard evaluate the executor's form, which is expensive and technical. A suppressed guard is closed by making autonomy modes orthogonal to policy — a flag that removes confirmation must not also remove authorization — which costs nothing and is a design decision a harness makes once. The assessment order follows: confirm the guard is consulted under every autonomy mode the deployment allows, then ask what it matches. An organization that inspects allowlist contents without checking the first condition has graded an artifact that a flag removed from the path.

## Two Closures

**Canonicalize before deciding.** Evaluate the action in the form the executor will act on. The audit's one defending implementation runs five sequential steps rather than one match: tokenize with a shell grammar, detect expansion constructs, recursively evaluate substitutions, inspect pipe destinations for interpreters, then apply the disabled-pattern list. This is a real closure and it is expensive — it requires the guard to model the executor's grammar and to keep modeling it as that grammar changes.

**Enforce below the representation.** An OS-level boundary constrains the process regardless of which string was approved. Seatbelt, bubblewrap, seccomp, a container, or a VM does not care how the command was spelled, because it never reads the command. This closure is cheaper to reason about and does not decay as shell grammar or tool syntax evolves.

> The first closure makes the guard smarter. The second makes the guard's mistakes survivable. Only the second degrades gracefully, which is why it belongs underneath the first rather than instead of it.

## Consequence for Maturity Scoring

A text-matching command guard is not a policy decision point in the [[agentic-ai-security-cmm-d3-control-least-agency|D3]] sense, and an organization scoring it as one has overstated its control maturity by a level. The distinguishing question for an assessor is whether the enforcement mechanism reads the same artifact the executor does. If it reads a string that a shell, a filesystem, or a tool server will rewrite, the control is advisory.

An assessor should also establish which threat model a text-matching control was built for before scoring it, and should not let coverage against the non-adversarial case be reported as coverage against the adversarial one. See [Scope limit under a non-adversarial threat model](#scope-limit-under-a-non-adversarial-threat-model) below.

The corollary is a limit on [[plan-validate-execute|Plan-Validate-Execute]]: a deterministic gatekeeper is only as sound as its equivalence between the validated plan and the executed action. The pattern remains correct; the validator must operate on canonicalized actions for its determinism to mean anything.

This gap is what orders the control catalog in [[securing-agentic-coding|Securing Agentic Coding]]. That page ranks process constraints above string inspection above model instruction, and cites the second closure as the reason: OS enforcement holds regardless of how a command was spelled, so it is the layer that makes the guard's mistakes survivable rather than the layer that makes the guard smarter.

[[capability-based-authorization|Capability-based authorization]] inherits the same precondition. An attenuated capability is only as narrow as the equivalence between the action it names and the action the executor performs, so a capability that authorizes a command string a shell will rewrite is subject to this gap exactly as a text-matching guard is.

## Scope limit under a non-adversarial threat model

The gap is a statement about evasion, and evasion presumes something trying to evade. Under the [[accidental-meltdown|accidental meltdown]] model — an agent crossing a boundary while pursuing the user's actual goal — the agent is not disguising its commands, so a string-matching guard sees the literal form and fires. [[numbat|Numbat]] takes this position explicitly, shipping pattern-matching pre-action rules over commands and file paths as blocking controls.

This narrows the gap's scope rather than contradicting it. A guard tuned only for meltdowns may inspect strings and stop there; the same guard is not evidence of maturity against an injected agent, where the ranking applies unchanged.

> [!gap] Canonicalization behavior of Numbat's event layer
> Numbat's preventive rules are CEL pattern matches over command strings and file paths. [[perplexity-numbat-agent-security|The announcement]] does not state how the normalized event layer handles shell obfuscation, aliasing, or indirect invocation, so those rules cannot be graded against the adversarial case until that is established.
