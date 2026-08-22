---
type: concept
title: "Behavioral Anomaly Detection for Agents"
address: c-000304
created: 2026-05-03
updated: 2026-08-18
tags:
  - concepts
  - observability
  - behavioral-detection
  - agentic-ai
  - soc
  - anomaly-detection
status: developing
origin: aggregated
no_public_url: "wiki synthesis of a non-published talk; generic detection technique with no single canonical source"
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
related:
  - "[[agent-observability]]"
  - "[[multi-agent-runtime-security]]"
  - "[[prompt-volume-to-alert-ratio]]"
  - "[[non-human-identity]]"
  - "[[oversight-layer]]"
  - "[[1-8m-prompts-30-alerts-talk]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[nist-ai-rmf]]"
  - "[[nist-ai-800-4]]"
  - "[[mitre-atlas]]"
  - "[[owasp-ai-exchange]]"
sources:
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
---

# Behavioral Anomaly Detection for Agents

Behavioral anomaly detection for agents is the practice of profiling what normal looks like for autonomous AI agents — at the agent level, the user level, and (in multi-tenant contexts) the organization level — and alerting on statistically significant deviations from that baseline. It is the operational form of the [[agent-observability|Agent Observability]] practice's insider-threat framing, applied specifically to agentic AI workloads.

**Why this differs from traditional UEBA.** Classical User and Entity Behavior Analytics (UEBA) profiles humans and machines at the user level. Agentic AI introduces a structurally new identity axis: the **agent itself**. Agents have more constrained, predictable behavior than humans (their logic is typically custom-built per deployment), which means agent-level baselines yield lower false-positive rates than user-level baselines alone. Combining three levels — user / agent / org — into an ensemble model produces higher-fidelity signals than any single axis.

## The three-level context model

First described in production by [[matt-rittinghouse|Matt Rittinghouse]] at [[unprompted-conference-march-2026|Unprompted March 2026]], the model defines three identity axes for anomaly detection:

| Level | Entity | What the level adds |
|---|---|---|
| 1 | User | Established UEBA baseline — what does this human normally do? |
| 2 | Agent | New axis — what does this specific agent normally do, given its deployment context? |
| 3 | Organization | Multi-tenant context — what is normal for agents in this tenant organization? |

Anomaly signals from each level are combined in an **ensemble model**, where correlated deviations across multiple axes substantially reduce noise. A single-axis anomaly may reflect legitimate edge-case behavior; a three-axis anomaly is statistically near-impossible by chance.

## Feature design principles

Effective behavioral features for agentic AI detection focus on data access patterns along two dimensions:

**Depth features** (sensitive data targeting):
- Frequency of database calls per session
- Proportion of total data accessed vs. normal baseline
- Column/field-level access to PII-flagged or rarely-accessed attributes

**Breadth features** (exfiltration indicators):
- Number of distinct tables, columns, or records touched
- Ratio of accessed data to normal session scope
- Sensitivity scoring of accessed fields based on access rarity

**Temporal features:**
- Incremental historical profiling (rolling daily baselines)
- Standard-deviation-based scoring against the rolling baseline
- New-agent warm-up period (typically ~14 days) during which baselines are established

## Ineffective approaches

Two documented failures from the Salesforce production deployment inform current best practice:

1. **Query complexity scoring** — Intuitive but misleading in agentic contexts because agents construct their own queries. Complexity reflects agent design choices, not malicious intent. Measuring query complexity confuses the agent identity layer with the malicious-actor layer.

2. **Multi-table joins for feature context** — Joining metadata from multiple tables to build features introduces expensive computational overhead. Refactoring to single-table operation reduced model training time by ~67% at Salesforce.

The meta-principle: **features must measure what you think they are measuring**. Validate feature predictive contribution (e.g., PCA-style feature importance analysis) before investing in implementation.

## Adversarial limits

The two failures above are design errors. A third limit is structural, and the [[owasp-ai-exchange|OWASP AI Exchange]] states it for its own series-level detector: attackers can distribute inputs across multiple identities or sources specifically to reduce detectability.[^aix-unwanted] The three-level model is keyed on identity at every level, so an adversary who spreads activity across user accounts, agent registrations, or tenants attacks the correlation the ensemble depends on. The near-impossible-by-chance property above holds against an adversary who does not know the detector exists; it fails against one who does and can register a second agent. The 14-day warm-up compounds it, because a fresh agent identity has no baseline to deviate from for two weeks.

The Exchange states the false-positive side as well: legitimate systematic testers and researchers resemble attack patterns.[^aix-unwanted] Agent-level baselines are lower-noise than user-level baselines for the reason the definition above gives, and the residual noise concentrates on exactly the accounts that do broad, repetitive, programmatic work.

Neither limit argues against the ensemble. Both set what a measured signal-to-noise figure means: the [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]] counts alerts raised rather than attacks present, and identity-distributed activity is consistent with a high ratio.

## Production evidence

The primary production case study is Salesforce's [[agentforce|Agentforce]] deployment, described in [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]] (Unprompted March 2026):

- **Scale:** ~1.8M daily prompts, 12,000+ unique daily active agents, 55,000 tenant organizations
- **Output:** fewer than 30 actionable daily alerts
- **Signal-to-noise ratio:** approximately 60,000:1
- **Detection latency (current):** 12–24 hours (batch processing)
- **Detection latency (target):** real-time / in-flight scoring via hot-path architecture

## Mapping to frameworks

Behavioral anomaly detection sits at the intersection of three standards the wiki tracks. It is the runtime instance of the [[nist-ai-rmf|NIST AI RMF]] MEASURE function, which calls for analyzing and tracking AI system behavior in operation; agent-level baselines are how MEASURE is made continuous for an autonomous system. The barriers to doing it well — telemetry underutilization, the difficulty of detecting deceptive or monitor-evading behavior — are catalogued by [[nist-ai-800-4|NIST AI 800-4]] as open post-deployment monitoring challenges. The malicious behaviors the anomalies ultimately map to are the adversarial techniques [[mitre-atlas|MITRE ATLAS]] catalogs: an exfiltration-shaped access pattern is `AML.T0086` (Exfiltration via AI Agent Tool Invocation) observed through its behavioral signature rather than a static indicator.

## Relationship to the observability plane

Behavioral anomaly detection occupies the **detection layer** of the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]'s Observability plane. It depends on:

- **Identity telemetry** (§3 of [[agent-observability|Agent Observability]]) — structured logs linking `invoking_user_id` → `agent_id` → action. Without this, behavioral signals cannot be attributed.
- **Baseline infrastructure** — incremental historical profiling at the agent, user, and org levels. Identity linkage is a prerequisite: without it a baseline profiles the fleet and not the agent.

It feeds:

- **Alert triage pipeline** — the scored alerts feed a secondary system (potentially an LLM explainer agent) for SOC consumption.
- **Auto-containment tier** (roadmap) — when deviation crosses a "statistically impossible" threshold, automated response (session kill, token revocation, bot lockdown) executes without SOC triage.

## Auto-containment roadmap

The Salesforce architecture illustrates a three-stage progression toward autonomous response:

1. **Batch detection** → alert queue for SOC triage (current state)
2. **Hot-path inference** → real-time session scoring against cached baselines
3. **Inline auto-containment** → automated kill / revoke / lockdown when threshold crossed; purple-team exercise required before rollout

This progression is the operational instantiation of the "confident automated response" goal: moving from knowing what happened yesterday to stopping what is happening now.

> [!gap] Warm-up period formalization
> The 14-day warm-up period for new agents is mentioned by Salesforce practitioners as a SOC-playbook requirement, but no published study has characterized the optimal warm-up duration across different agent types and workloads. This is an open calibration question.

## See also

- [[agent-observability|Agent Observability]] — parent practice; this concept is its §7 (Agent Behavioral Monitoring) instantiation
- [[multi-agent-runtime-security|Multi-Agent Runtime Security]] — multi-agent extension of the same principles
- [[prompt-volume-to-alert-ratio|Prompt-Volume-to-Alert Ratio]] — the metric this practice produces
- [[non-human-identity|Non-Human Identity (NHI)]] — identity architecture that feeds the attribution layer
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] — policy enforcement layer that acts on detection outputs

## Notes

[^aix-unwanted]: [OWASP AI Exchange — UNWANTED INPUT SERIES HANDLING](https://owaspai.org/go/unwantedinputserieshandling/), retrieved 2026-08-18.
