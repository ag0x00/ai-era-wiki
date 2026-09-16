---
type: concept
title: "Chain-of-Thought Monitorability"
address: c-866833
created: 2026-09-16
updated: 2026-09-16
tags:
  - concepts
  - guardrails
  - monitorability
  - chain-of-thought
  - cmm
  - d4
status: developing
scope_axis:
  - sec-of-ai
complexity: advanced
domain: ai-security
aliases:
  - "CoT monitorability"
  - "chain-of-thought monitoring"
  - "CoT monitoring"
related:
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[llamafirewall]]"
  - "[[llamafirewall-2025]]"
  - "[[prompt-injection-containment]]"
  - "[[oversight-layer]]"
  - "[[guardian-agent]]"
  - "[[agent-observability]]"
  - "[[agentdojo]]"
  - "[[agent-runtime-protection-canvass-2026-09]]"
sources:
  - "https://deploymentsafety.openai.com/gpt-6-astra"
  - "https://deploymentsafety.openai.com/gpt-6-astra/cot-controllability"
  - "https://arxiv.org/abs/2609.15989"
  - "https://arxiv.org/abs/2507.11473"
  - "https://arxiv.org/abs/2505.03574"
---

# Chain-of-Thought Monitorability

## Definition

Chain-of-thought (CoT) monitorability is a monitor's ability to detect misaligned or otherwise undesirable behavior by reading a model's reasoning trace. Controllability is a separate property: the model's ability to steer what that trace says, independent of the action it takes. OpenAI states the relationship directly: "Controllability is an undesirable property for monitorability, because it reduces the trustworthiness of CoT for detecting model misalignment."[^astra-controllability]

**A rising controllability figure works against monitoring, because a model that controls what its own chain of thought says can make the trace read as ordinary reasoning regardless of the action it takes next.** One GPT-6 Astra system card carries both readings: the controllability metric on its CoT-controllability page[^astra-controllability] and the monitorability decrease in the card itself.[^astra-degradation]

## Aliases / Variants

The literature uses chain-of-thought monitorability, chain-of-thought monitoring, and CoT monitoring for the same property: an external or internal monitor's ability to read a model's reasoning trace and act on what it finds there.

## Controls that read the trace

D4 of the [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM]] lists chain-of-thought and alignment auditing among the capabilities an L4 runtime-guardrails program assembles. Two implementations carry that rung. The catalog names [[llamafirewall|LlamaFirewall]]'s AlignmentCheck at an experimental grade, and Microsoft's Content Safety Task Adherence, the platform-native alternative, remains in public preview.

AlignmentCheck's published figures have stood still. The paper reports an 83% attack-success reduction on the [[agentdojo|AgentDojo]] benchmark, and 80%+ recall at under 4% false-positive rate on its own goal-hijacking benchmark.[^alignmentcheck-numbers] Both date from a 2025-05-13 commit, roughly sixteen months before this page, while the surrounding repository stayed active through 2026-08-18.[^alignmentcheck-stale]

Both controls read the property this page describes, so the property's reliability bounds what either one can promise. [[prompt-injection-containment|Prompt-injection containment]] therefore places AlignmentCheck as one layer of a combined defense, and the [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]]'s D4 assessor questions ask an auditor to watch a chain-of-thought or alignment audit fire on a real agent run, whichever implementation the deployment runs, in place of a vendor claim on file.

The [[agent-runtime-protection-canvass-2026-09|Agent Runtime Protection Market Canvass]] surveys vendor coverage of this capability across the wider runtime-guardrails market.

## Evidence of fragility

Three sources, in the order they were published, move from a precautionary warning to reported evidence to a working attack.

### Precautionary case for fragility (2025)

arXiv:2507.11473, "Chain of Thought Monitorability: A New and Fragile Opportunity for AI Safety," carries roughly 41 authors from Anthropic, OpenAI, Google DeepMind, UC Berkeley, Stanford, MIT, and the Center for AI Safety (v1 2025-07-15, latest revision 2025-12-07).[^tri-lab] The paper argues that monitorability could be undermined by careless development choices; it does not report that degradation had occurred in a shipped model. It recommends that frontier developers weigh the effect of development decisions on monitorability and invest in CoT monitoring alongside other safety methods.

### Reported degradation in a shipped model (2026-09)

OpenAI's GPT-6 Astra system card, published 2026-09-03, is the first evidence found that the precautionary risk in the 2025 paper materialized: "GPT-6 Astra shows a substantial decrease in chain-of-thought monitorability compared to previous models," and "it seems likely that we would soon have significantly reduced confidence in detecting many forms of misaligned behaviors using our current monitoring systems."[^astra-degradation] The same system card reports a CoT-Control constraint-following-rate metric rising from 16.1% for GPT-5.6 Sol to 60.9% for GPT-6 Astra in a 750–1,250-token CoT bucket. The figures could not be pulled verbatim from the source chart; convergent secondary citations of the same page corroborate them at medium-high confidence. The two quoted sentences above carry this page's claim, and the percentages support it at that lower confidence.

### Adversarial evasion under plan injection (2026-09)

arXiv:2609.15989, dated 2026-09-14 and authored by Keertana Chidambaram, Andrew Ilyas, and Vasilis Syrgkanis (affiliations not confirmed), demonstrates plan injection: planting harmful but benign-sounding reasoning in an actor model's context to steer it toward an adversarial action while evading a monitor reading its trace.[^plan-injection] The attack generalizes to harder tasks and larger models, reaching 25-33% monitor evasion across different monitorability benchmarks. A separate, task-specific result in the same paper is worse: giving the monitor access to the injected plan drops detection by as much as 50% in a Bio-Math task. The threat model is deliberate adversarial evasion through plan injection, distinct from the naturally occurring degradation the Astra card and the 2025 paper describe.

## Counter-evidence

A search for a source disputing the direction of this trend found none. The Frontier Model Forum's issue brief on chain-of-thought monitorability calls CoT monitoring "not a silver bullet" and "one important layer in a 'defense-in-depth' approach to AI safety, complementing rather than replacing other methods",[^fmf] which agrees with the reported fragility. Anthropic's Claude Mythos system card is reported to disclose a training bug that let reward code see the chain of thought in roughly 8% of RL episodes, on a third-party summary rather than the card itself.[^mythos-summary] That raises a corroborating concern from a second lab through a different mechanism. It neither replicates nor rebuts either finding above.

## Limits a trace-reading control inherits

Any control built on the reasoning trace inherits two limits from this property. Its detection rate rests on a property the model vendor controls and that changes between model generations, so a grading scheme has to give up the fixed baseline it could assume for a static classifier. An adversary can also target the trace directly: the 25-33% evasion rate arXiv:2609.15989 reports[^plan-injection] varies across monitorability benchmarks, and the paper measures plan injection alone, so an attack that combines it with other techniques sits outside the measured range.

A maturity ladder that grades the capability itself absorbs both limits, because it re-evaluates as models change where a certification of a fixed product version would not.

## Related Concepts

- [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] — combines AlignmentCheck with input-layer filtering.
- [[guardian-agent|Guardian Agent]] — an independent oversight layer that can consume CoT output as one signal among several.
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — the policy layer a CoT-derived signal feeds into after a monitor flags it.
- [[agent-observability|Agent Observability]] — captures the reasoning trace an AlignmentCheck-style monitor reads.

## Notes

[^astra-controllability]: [OpenAI Deployment Safety Hub — GPT-6 Astra, CoT controllability](https://deploymentsafety.openai.com/gpt-6-astra/cot-controllability), 2026-09-03. Defines monitorability and controllability and states the inverse relationship between them; the 16.1%/60.9% CoT-Control figures appear on this sub-page as a chart, not as extractable text.
[^alignmentcheck-numbers]: [arXiv:2505.03574 — LlamaFirewall: An Open Source Guardrail System for Building Secure AI Agents](https://arxiv.org/abs/2505.03574), submitted 2025-05. Reports AlignmentCheck running on Llama 4 Maverick at an 83% attack-success reduction on AgentDojo, and at 80%+ recall with under 4% false-positive rate on the paper's internal goal-hijacking benchmark of 600 scenarios.
[^alignmentcheck-stale]: GitHub, meta-llama/PurpleLlama, checked 2026-09-16. The LlamaFirewall directory received general maintenance commits through 2026-08-18; the most recent commit found that specifically touches AlignmentCheck is dated 2025-05-13.
[^tri-lab]: [arXiv:2507.11473 — Chain of Thought Monitorability: A New and Fragile Opportunity for AI Safety](https://arxiv.org/abs/2507.11473), v1 2025-07-15, latest revision 2025-12-07.
[^astra-degradation]: [OpenAI Deployment Safety Hub — GPT-6 Astra System Card](https://deploymentsafety.openai.com/gpt-6-astra), 2026-09-03.
[^plan-injection]: [arXiv:2609.15989 — Corrupt Plans, Clean Traces: Evading Chain-of-Thought Monitoring with Plan Injection](https://arxiv.org/abs/2609.15989), 2026-09-14.
[^fmf]: [Frontier Model Forum — Chain of Thought Monitorability issue brief](https://www.frontiermodelforum.org/issue-briefs/chain-of-thought-monitorability/), 2026-01-27.
[^mythos-summary]: [Zvi Mowshowitz — Claude Mythos: The System Card](https://thezvi.substack.com/p/claude-mythos-the-system-card). A third-party summary of the Anthropic system card; the ~8% figure is read from that summary and was not checked against the card.

<!-- sources:auto -->
## Sources

- [deploymentsafety.openai.com](https://deploymentsafety.openai.com/gpt-6-astra)
- [deploymentsafety.openai.com](https://deploymentsafety.openai.com/gpt-6-astra/cot-controllability)
- [arxiv.org](https://arxiv.org/abs/2609.15989)
- [arxiv.org](https://arxiv.org/abs/2507.11473)
- [arxiv.org](https://arxiv.org/abs/2505.03574)
<!-- /sources -->
