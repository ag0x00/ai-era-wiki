---
type: concept
title: "Control-Efficacy Gate"
address: c-000057
created: 2026-05-15
updated: 2026-05-15
tags:
  - concepts
  - controls
  - ci-cd
  - assurance
status: seed
no_public_url: "wiki-original control construct; no single external canonical source"
scope_axis: [sec-of-ai, sec-against-ai]
related:
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[agentshield|AgentShield]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|CMM Measurement Protocol]]"
  - "[[supply-chain-security-for-agents|Supply Chain Security for Agents]]"
  - "[[ai-spm|AI-SPM]]"
sources:
  - "[[agentshield-announcement|AgentShield README]]"
---

# Control-Efficacy Gate

A control-efficacy gate is a CI-time assurance instrument that asserts a *control still catches what it is supposed to catch* — distinct from a control-existence check, which asserts only that the control is configured. Two complementary forms appear in current practice:

- **Positive-corpus regression gate.** A curated attack corpus is replayed against the current detector / scanner / guardrail on every commit; the build fails if the corpus is no longer fully detected. [[agentshield|AgentShield]]'s `--corpus-gate` is the canonical worked example: a built-in attack corpus covering env proxy / DNS exfiltration / runtime import mutation / env-token exfiltration / credential-store access / clipboard access must stay fully detected, and a corpus-gate failure emits a *prioritized accuracy improvement plan* by category, missing rule, and missed config so maintainers can act rather than just disable the gate.
- **Time-bound exception lifecycle.** Organizational waivers carry an `expires_at` and an owner; the audit surfaces total / active / expiring-within-N-days / expired exceptions on every run. AgentShield's organization-policy exception audit is the worked example: temporary exemptions stay visible in branch-protection evidence instead of becoming silent permanent bypasses.

## Rationale

Conventional CMMs and audit programs score whether a control is *in place*. They do not, by design, score whether the control still *works* against an evolving threat surface, and they do not, by design, score whether the control's *exceptions* have outlived their justification. Both gaps are routes by which a maturity grade can be true on paper while the program degrades in production:

- A detector can pass a control-existence check forever while its true-positive rate decays as adversary tradecraft evolves.
- A waiver granted for a legitimate three-month migration can persist for three years if no lifecycle observability instrument forces re-justification.

A control-efficacy gate addresses both decays mechanically — at CI time, with a deterministic pass/fail signal — rather than relying on periodic human review.

## Generalization Beyond AgentShield

The corpus-gate construct is general to any classifier-style control: prompt-injection filter, egress policy, DLP regex bank, RAG-source-trust scorer, behavioral-anomaly detector. The construct needs three components — a curated and versioned attack corpus, a deterministic replay harness, and a regression criterion (typically *100% detection of the previously-detected subset*) — plus a remediation-plan emitter to keep the gate from being disabled the first time it fails.

The exception-lifecycle construct is general to any policy framework with override semantics — IAM allowlists, firewall rules, Cedar / OPA policies, code-scanning suppressions, security-baseline waivers. The construct needs three components — owner-attributed exceptions with explicit `expires_at`, a periodic enumeration in audit output, and escalation paths for expired-but-still-relied-on exceptions.

## Relationship to Maturity Modeling

Neither construct has a dedicated slot in the current [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] domain framework. The closest existing analogs:

- [[agentic-ai-security-cmm-2026|CMM]] D7 (Observability & Detection) measures whether *detectors are deployed*; the corpus-gate measures whether they *still hit*.
- [[agentic-ai-security-cmm-2026|CMM]] D9 (Operations & Human Factors) measures HITL-fatigue and decommission lifecycle; the exception-lifecycle is a structurally similar continuous-attestation primitive applied to *policy waivers*.

These are candidates for revision-pass additions when a second sourced peer instrument lands (currently AgentShield is the only sourced operational implementation of either pattern on the wiki).

## See Also

- [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] — the sibling generalization from the same AgentShield ingest; the corpus-gate operates *over* the config tree this concept names.
- [[agentshield|AgentShield]] — concrete worked example of both forms.
- [[agentshield-announcement|AgentShield README]] — source.
- [[agentic-ai-security-cmm-measurement-protocol|CMM Measurement Protocol]] — assessment workflow this concept is a candidate addition to.
