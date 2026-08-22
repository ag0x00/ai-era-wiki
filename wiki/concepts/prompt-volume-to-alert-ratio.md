---
type: concept
title: "Prompt-Volume-to-Alert Ratio"
address: c-000305
created: 2026-05-03
updated: 2026-08-18
tags:
  - concepts
  - observability
  - soc
  - metrics
  - agentic-ai
status: developing
origin: aggregated
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
no_public_url: "Wiki-coined metric derived from an ingested talk; no external canonical source."
aliases:
  - "Signal-to-noise ratio for agentic AI SOC"
  - "AI SOC detection ratio"
related:
  - "[[behavioral-anomaly-detection-for-agents]]"
  - "[[agent-observability]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[owasp-ai-exchange]]"
sources:
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
---

# Prompt-Volume-to-Alert Ratio

The prompt-volume-to-alert ratio is a signal-to-noise metric for agentic AI security operations: the number of AI prompts processed per time period divided by the number of actionable security alerts generated over the same period. It is the agentic-AI SOC analog to the traditional SIEM signal-to-noise ratio.

## Definition

```
prompt_volume_to_alert_ratio = total_prompts_processed / actionable_alerts_generated
```

A high ratio indicates an effective behavioral detection layer — one that can process large volumes of agent activity and surface only the anomalies that require human attention. A low ratio indicates either insufficient detection (many anomalies missed) or, more commonly, excessive noise (analysts reviewing low-confidence alerts and burning out).

The ratio counts alerts raised rather than attacks present, so a high reading is consistent with two different states. The [[owasp-ai-exchange|OWASP AI Exchange]] names the evasion path that separates them: an attacker can distribute inputs across multiple identities or sources specifically to reduce detectability.[^aix-unwanted] Activity spread beneath an identity-keyed baseline adds prompts to the numerator and no alerts to the denominator, which raises the ratio. A rising ratio therefore reports either improving precision or successful evasion, and the metric alone does not separate them. Read it alongside a recall measure — red-team injections that the detection layer was expected to raise, and did — rather than as an efficacy figure on its own.

## Production benchmark

The only quantified production example in this wiki as of August 2026 is from Salesforce's Agentforce deployment, reported by [[matt-rittinghouse|Matt Rittinghouse]] and [[millie-rittinghouse|Millie Rittinghouse]] at [[unprompted-conference-march-2026|Unprompted March 2026]]:

| Metric | Value |
|---|---|
| Daily prompts processed | ~1,800,000 |
| Daily actionable alerts | <30 |
| Ratio | ~60,000:1 |
| Detection model | Three-level ensemble (user / agent / org) behavioral anomaly detection |
| Detection latency | 12–24 hours (batch mode, as of March 2026) |

See [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]] for full methodology.

## Significance of the metric

Traditional SIEM deployments are notoriously noisy — security teams frequently reduce alert volumes through aggressive suppression, at the cost of missing real events. Agentic AI workloads amplify the problem: agents generate **10–20× the log volume of humans** over the same time window (per Miggo Security's analysis of agent telemetry patterns, cited in [[agent-observability|Agent Observability]]).

Without purpose-built behavioral detection, a naive alerting policy on 1.8M daily prompts would produce an unworkable alert queue. The prompt-volume-to-alert ratio quantifies how effectively a detection stack tames this noise problem.

## Relationship to maturity levels

In the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]], the ratio is graded in one place. [[agentic-ai-security-cmm-d7-observability|D7 Observability and Detection]] L5 requires that per-agent baselines hold a documented prompt-volume-to-alert ratio for at least a quarter, alongside a measured and high analyst-actionable alert rate. The capability the ratio reads arrives one rung earlier: D7 L4 puts per-agent behavioral baselines and drift detection into production and wires them to the SIEM/SOAR. A program below L4 has no baseline to compute the ratio against, and a program at L4 can measure the ratio without yet holding it to a documented figure across a quarter.

Hot-path inline scoring and automatic containment above a threshold are the Salesforce deployment's stated roadmap and appear in no D7 rung. Reading them as maturity levels reverses the direction of evidence, since the CMM grades capabilities that operate in production.

> [!gap] No cross-vendor benchmarks available
> The Salesforce 60,000:1 figure is the first and only published production benchmark for this metric as of August 2026. Whether this ratio is achievable across other agentic platforms, or what the lower bound of a "good" ratio is, remains an open empirical question. The ratio is sensitive to platform scale, agent behavioral diversity, and the aggressiveness of the detection threshold. The Exchange's evasion path sets the other end: a high reading has no upper bound that indicates success, because sustained evasion and high precision produce the same figure.[^aix-unwanted]

## Notes

[^aix-unwanted]: [OWASP AI Exchange — UNWANTED INPUT SERIES HANDLING](https://owaspai.org/go/unwantedinputserieshandling/), retrieved 2026-08-18.
