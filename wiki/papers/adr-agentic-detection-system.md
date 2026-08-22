---
type: paper
title: "ADR: Agentic Detection for Enterprise AI"
address: c-000212
created: 2026-06-17
updated: 2026-06-17
tags:
  - papers
  - agentic-soc
  - mcp-security
  - detection-engineering
  - prompt-injection
  - credential-exposure
  - benchmarks
  - ai-in-sec-defense
  - sec-of-ai
status: summarized
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
origin: aggregated
year: 2026
authors:
  - "Chenning Li"
  - "Pan Hu"
  - "Justin Xu"
  - "Mohammad Alizadeh"
  - "Ming Zhang"
venue: "9th MLSys Conference (Industry Track), 2026"
source_url: https://arxiv.org/abs/2605.17380
archived_copy: ".raw/papers/adr-agentic-detection-system-2026-05-17.md"
key_claim: "A production-proven enterprise detection-and-response system for MCP-driven AI agents closes the agent-observability gap with an endpoint sensor that reconstructs the full prompt-to-tool-call causal chain, then routes events through a cost-tiered triage-then-reason detector, validated by ten months at Uber and a new 302-task MCP-native benchmark."
methodology: "System paper plus production deployment study (7,200+ hosts, ten months at Uber) and benchmark evaluation on ADR-Bench (302 tasks) and AgentDojo (93 tasks) against three baselines."
related:
  - "[[mcp-security|MCP Security]]"
  - "[[tiered-detection-cascade|Tiered Detection Cascade]]"
  - "[[adr-bench|ADR-Bench]]"
  - "[[uber|Uber]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]]"
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability]]"
sources:
  - "[[.raw/papers/adr-agentic-detection-system-2026-05-17.md]]"
  - https://arxiv.org/abs/2605.17380
---

# ADR — Agentic Detection for Enterprise AI

**Source:** [arXiv:2605.17380 — *ADR: An Agentic Detection System for Enterprise Agentic AI Security*](https://arxiv.org/abs/2605.17380) (submitted 2026-05-17; 9th MLSys Conference Industry Track). Authors from [[uber|Uber]], MIT, and the University of Oxford. Local copy: `.raw/papers/adr-agentic-detection-system-2026-05-17.md`.

## Key Claim

ADR is presented as the first large-scale, production-proven enterprise framework for securing AI agents that operate through the [[mcp-security|Model Context Protocol]].[^abstract] It targets three named gaps: limited observability (EDR sees file writes, not agent reasoning or the causal chain from intent to execution), insufficient robustness (rule-bound static defenses do not generalize across attack techniques), and high detection cost (per-event LLM inference is too expensive at scale).[^abstract]

The load-bearing data point is operational, not theoretical: ten months of production use at [[uber|Uber]] across 7,200+ unique hosts processing 10,000+ agent sessions daily.[^abstract] That makes it the wiki's strongest production-proven anchor for the [[inline-gateway-vs-runtime-instrumentation|runtime-instrumentation]] camp and a peer to Salesforce's [[1-8m-prompts-30-alerts-talk|Agentforce telemetry]] on the [[agentic-soc-state-of-the-field|Agentic SOC]] axis.

## Three Components

| Component | Role | Mechanism |
|---|---|---|
| **ADR Sensor** | High-fidelity agentic telemetry | Endpoint agent that parses the local SQLite/JSONL caches of Cursor, Cline, and Claude Code to reconstruct full sessions: user prompt (intent), agent reasoning, MCP tool calls (sequence, arguments, results), and environmental context (server configs, `pip`/`npm` packages). ~0.182 s per run.[^sensor] |
| **ADR Explorer** | Pre-deployment red teaming | Three-agent evolutionary loop (Red-Teaming → Eval → Threat-Intelligence) that mutates known attacks, scores variants by a fitness function `F = ε × σ × τ^α` (execution depth × naturalness × impact), and publishes survivors into the threat repository feeding Tier 2.[^explorer] |
| **ADR Detector** | Two-tier online detection | Tier 1 triage (high-recall, low-cost LLM screen) escalates suspicious events to Tier 2, a reasoning agent that queries enterprise context over MCP: source-code inspection, threat-intelligence lookup, and policy verification.[^detector] |

The framing is explicitly SOC-shaped: the Sensor mirrors an analyst's visibility, the two-tier detector mirrors triage-then-investigate, and the Explorer mirrors an internal red team.[^detector] The stated core insight is the attacker–defender **semantic gap**: attacks that read as benign on the surface but are malicious once intent and tool semantics are understood. The source-code and policy MCP providers exist to close that gap.[^detector]

The two-tier detector is a concrete instance of the [[tiered-detection-cascade|tiered detection cascade]]: a cheap high-recall pre-filter gates an expensive high-precision reasoner. The triage layer alone resolves 40.7% of tasks at \$0.017 each, leaving the costlier reasoning path (\$0.029) for flagged events.[^efficiency]

## Architecture Choice: Sensor over Gateway

ADR evaluated and rejected an LLM/MCP **gateway** that would intercept agent-to-LLM and agent-to-tool traffic at the network boundary. The stated reasons: a gateway requires changes to MCP hosts, is incompatible with streaming responses, and captures only partial information, omitting the environmental and contextual data needed for high-fidelity observability.[^sensor] For prevention specifically, ADR runs a **hybrid** model: the sensor provides deep forensics while inline hooks supply real-time blocking of high-severity credential leakage.[^sensor] This is direct production evidence for the [[inline-gateway-vs-runtime-instrumentation|gateway-vs-instrumentation]] fork, and a partial answer to that page's open question about runtime-instrumentation viability at scale (here, for local-process coding agents).

## Threat Framework

The paper consolidates a five-tactic, 17-technique framework for MCP-driven agents from public frameworks ([[mitre-atlas|MITRE ATLAS]], [[owasp-agentic-ai-top-10|OWASP]] Excessive Agency), disclosed incidents (JFrog, Invariant Labs, Microsoft Defender, [[zenity|Zenity Labs]], CyberArk), and Uber's own telemetry.[^threat] The five tactics are Initial Access & Execution (6 techniques), Permission Abuse (2), Security Control Bypass (3), Reasoning & Data Manipulation (4), and Operational Impact (2), spanning traditional vectors adapted to agents ([[indirect-prompt-injection|indirect prompt injection]], [[tool-poisoning|tool rug pull]]) and agent-native threats (control-flow hijacking, malicious agent collusion).[^threat]

## ADR-Bench

The companion benchmark, [[adr-bench|ADR-Bench]], is derived from enterprise telemetry: 302 tasks (42 malicious / 260 benign — 13.9% attack prevalence), 133 MCP servers, 729 tools, 17 techniques across 5 tactics, averaging 28.5 tool calls per task.[^bench] Its claim to distinction is full 17/17 technique coverage with MCP context and realistic class imbalance, where prior benchmarks cover only 3–6 of the 17.[^bench] It is released on GitHub with the sensor and detection framework.[^bench]

## Evaluation

On ADR-Bench, ADR reports perfect precision and zero false positives at 0.667 recall (0.800 F1), 2–4x the F1 of the three baselines.[^eval] On AgentDojo, it reports perfect recall (all 38 attacks) with three false alarms (0.962 F1).[^eval] The two settings differ by design: precision-first under enterprise class imbalance, recall-first on prompt injection where a miss is costly.

| Benchmark | Precision | Recall | F1 | FP |
|---|---|---|---|---|
| ADR-Bench (302 tasks) | 1.000 | 0.667 | 0.800 | 0/260 benign |
| AgentDojo (93 tasks) | 0.927 | 1.000 | 0.962 | 3/55 benign |

Baselines on ADR-Bench trail badly on precision — ALRPHFS 0.333, GuardAgent 0.231, [[llamafirewall|LlamaFirewall]] 0.167 — because each fires 30–40 false positives across the 260 benign tasks, the failure mode that makes them unusable for production alerting.[^eval] Ablations attribute most of ADR's recall to the **source-code MCP** provider (removing it drops F1 0.800→0.676), with threat-intel next and policy verification near-marginal.[^eval]

## Production Findings

The most frequent true positive in deployment was credential exposure.[^cred] ADR found hundreds of high-severity credential exposures across 26 categories shared outside the enterprise network, which motivated a **shift-left** move from detection to prevention inside the agent loop.[^cred] The prevention layer uses Cursor and Claude Code **Hooks** (released late 2025) running a regex-plus-entropy pre-prompt scan that blocks secrets before transmission, reported at 97.2% precision with 206 true positives and 6 false positives across 212 unique credentials.[^cred] Threat-emulation exercises reproduced an internal CTF (prompt manipulation plus `curl | python3` remote execution) and the [[zenity|Zenity Labs]] Agent Flayer indirect-injection chain (a poisoned Jira ticket coercing a Jira-MCP-connected Cursor IDE into exfiltrating local credentials), both detected by correlating reasoning logs with MCP telemetry.[^cred]

Deployment labeling is honest about noise: of human-reviewed sessions, 34% were true positives, 17% true-positive-non-malicious (largely internal offensive-security activity), and 49% false positives driven by context-rich sessions such as analyzing large multi-file codebases.[^cred]

## Strengths and Weaknesses

Strengths: a rare production-scale, longitudinal deployment story rather than a benchmark-only result; an honest cost/latency accounting (2x GuardAgent but 19x cheaper than LlamaFirewall on ADR-Bench); a released MCP-native benchmark and code; and a clean architectural argument for sensors over gateways grounded in real constraints.[^eval][^bench]

Weaknesses and scope limits: recall is moderate (0.667 on ADR-Bench), so roughly a third of malicious tasks are missed, with Permission Abuse the weakest tactic (1/5 detected).[^eval] The benchmark is single-organization in provenance (Uber SOC telemetry, deployment on macOS coding agents), so external validity to other enterprise mixes is asserted, not measured. The detector depends on frontier models (GPT-4o triage, Claude Sonnet 4 reasoning) and inherits their drift and adversarial-adaptation risk, which the authors mitigate by pinning and regression suites rather than eliminate.[^detector]

## Relations

- **Supports** [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]] — first large-scale production evidence for the runtime-instrumentation (sensor) camp, with an explicit rejection of the gateway approach.
- **Supports** [[agent-observability|Agent Observability]] — the Sensor is a working glass-box telemetry layer reconstructing prompt → reasoning → tool-call → outcome.
- **Extends** [[genai-endpoint-observability-talk|GenAI Endpoint Observability]] (Ayenson, Elastic) — answers the intent-attribution gap (EDR cannot tell a developer from an agent) with cache-parsing reconstruction rather than process heuristics.
- **Sits alongside** [[1-8m-prompts-30-alerts-talk|1.8M Prompts, 30 Alerts]] (Salesforce) and [[syara-semantic-detection-talk|SYARA]] (Palo Alto) as production-scale, cost-tiered detection — see [[tiered-detection-cascade|Tiered Detection Cascade]].
- **Adds a comparator** to [[adr-bench|ADR-Bench]] · [[agentdojo|AgentDojo]] · [[defensebench|DefenseBench]] in the agentic-security benchmark landscape.

[^abstract]: Abstract, [arXiv:2605.17380](https://arxiv.org/abs/2605.17380): "the first large-scale, production-proven enterprise framework," the three challenges, "over 7,200 unique hosts," "over 10,000 agent sessions daily," and the headline benchmark figures.
[^sensor]: §3.1 *Observability: The ADR Sensor* — four captured dimensions, cache-parsing of Cursor/Cline/Claude Code, 0.182 s average run, the rejected LLM/MCP gateway alternative, and the hybrid sensor-plus-inline-hooks prevention model.
[^explorer]: §3.2 *Offline red-teaming* — the Red-Teaming / Eval / Threat-Intelligence agents, the evolutionary fitness function `F = ε × σ × τ^α` (α = 1.2), and the `[EAS]` / `[CURATED]` repository tagging.
[^detector]: §3 *System Design* and §3.2 *Detection at Scale* — the SOC-by-design framing, the semantic-gap insight, Tier 1 triage and Tier 2 reasoning, the three MCP context providers, GPT-4o triage + Claude Sonnet 4 reasoning, and no-hyperparameter-tuning with pinning/regression mitigations.
[^threat]: §4.1 *Threat Framework and Reported Instances* — five tactics with technique counts (6/2/3/4/2 = 17) and the three evidence sources.
[^bench]: §4 *ADR-Bench* and Table 1 — 302 tasks (42/260), 133 MCP servers, 729 tools, 28.5 tool calls/task, 17/17 coverage versus prior benchmarks, and the GitHub release.
[^eval]: §5 *Evaluation*, Table 2, and §5.3 ablations — ADR-Bench and AgentDojo metrics, baseline false-positive counts, cost/latency, and the source-code-MCP ablation (F1 0.800→0.676).
[^efficiency]: §5.2 *Efficiency* — triage handles 40.7% of tasks at \$0.017/task (2.3 s); the reasoning path costs \$0.029/task (29.7 s).
[^cred]: §6 *Real-World Deployment* and §6.2 *Detecting and Preventing Credential Exposure* — the TP/TPNM/FP labeling (34%/17%/49%), 26 credential categories, the Hooks-based shift-left prevention layer (97.2% precision, 206 TP / 6 FP across 212 credentials), and the CTF and Agent Flayer emulations.
</content>
