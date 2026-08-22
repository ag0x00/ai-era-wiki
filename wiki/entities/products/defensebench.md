---
type: entity
entity_type: product
title: "DefenseBench"
address: c-000145
created: 2026-05-25
updated: 2026-06-21
tags:
  - entities
  - product
  - benchmark
  - agentic-soc
  - blue-team
  - ai-in-sec-defense
status: developing
scope_axis: [ai-in-sec-defense]
homepage: https://defensebench.ai
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[agentdojo|AgentDojo]]"
  - "[[evaluating-ai-soc-agents|Evaluating AI SOC Agents]]"
  - "[[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]]"
sources:
  - https://defensebench.ai
  - https://defensebench.ai/benchmarks/botsv3
---

# DefenseBench

**Sources:** [DefenseBench](https://defensebench.ai) · [BOTSv3 benchmark](https://defensebench.ai/benchmarks/botsv3)

DefenseBench is a public benchmark platform that evaluates how well AI agents perform defensive (blue-team) cybersecurity tasks: alert triage and incident investigation in production-like environments. As of mid-2026 it is a research preview with one active benchmark.

## BOTSv3

The active benchmark, **BOTSv3**, is built on Splunk's *Boss of the SOC v3* dataset. Agents investigate security incidents by running Splunk searches and answering forensic questions under time constraints. The leaderboard scores each run on average and best score and on "best correct" — the number of incidents correctly solved. As of a May 2026 snapshot it ranked general coding-agent configurations run in interactive mode (Claude Opus 4.6 and GPT-5.x Codex variants near the top, smaller models lower); it therefore measures off-the-shelf coding agents on a SOC-investigation task rather than purpose-built defender agents. The leaderboard is live, so any specific ranking is a snapshot.

## Significance

DefenseBench is the defender-side counterpart to offensive agent benchmarks such as [[agentdojo|AgentDojo]] and the suites catalogued in the [[ai-vuln-discovery-benchmark-landscape|AI vuln-discovery benchmark landscape]]. Its existence narrows a gap the [[agentic-soc-state-of-the-field|Agentic SOC]] thesis had recorded as open: that no public benchmark existed for defender agents. The gap is narrowed, not closed. BOTSv3 is a single Splunk-derived dataset (forensic question-answering under time limits); the platform is a research preview with thin published methodology; and the leaderboard currently measures general coding agents rather than the purpose-built AI-SOC agents that Gartner's [[evaluating-ai-soc-agents|evaluation framework]] addresses. Even so, it is the first public, reproducible scoreboard for agentic SOC investigation, a comparator the field previously lacked.

## See Also

- [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] — the thesis whose benchmark gap this narrows
- [[agentdojo|AgentDojo]] — the analogous benchmark on the attacker / agent-security side
- [[adr-bench|ADR-Bench]] — a different defender-side benchmark: detecting attacks against MCP agents (302 tasks, 133 servers) rather than scoring agents on SOC investigation; together they cover the two defender-benchmark axes (detect attacks-on-agents vs. agents-do-defense-work)
- [[evaluating-ai-soc-agents|Evaluating AI SOC Agents]] — Gartner's buyer-side criteria (complementary: criteria versus scores)
- [[ai-vuln-discovery-benchmark-landscape|AI Vuln-Discovery Benchmark Landscape]] — the offensive-side benchmark catalogue
