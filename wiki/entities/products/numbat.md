---
type: entity
entity_type: product
title: "Numbat"
address: c-000250
created: 2026-07-31
updated: 2026-07-31
tags:
  - products
  - open-source
  - agentic-coding
  - endpoint-security
  - opentelemetry
  - detection-engineering
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
origin: aggregated
vendor: "[[perplexity|Perplexity]]"
license: "Unstated — announcement says open source, identifier not published"
platforms: "macOS, Linux, Windows"
related:
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[perplexity|Perplexity]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[open-secure-ai-alliance|Open Secure AI Alliance]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[opentelemetry-gen-ai|OpenTelemetry GenAI Semantic Conventions]]"
  - "[[agentshield|AgentShield]]"
  - "[[claude-code-security|Claude Code Security]]"
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability]]"
sources:
  - "https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat"
  - ".raw/articles/perplexity-numbat-agent-security-suite-2026-07-31.md"
---

# Numbat

**Announcement:** [Securing Agents Across Perplexity's Client Endpoints with Numbat](https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat) (Perplexity Research, July 2026) · summarized at [[perplexity-numbat-agent-security|Numbat Agent Security Suite]]

Open-source agent security suite from [[perplexity|Perplexity]], distributed as a lightweight static Go binary for macOS, Linux, and Windows. Numbat detects, prevents, and investigates risky behavior by coding agents running on client endpoints, and presents one interface across the several harnesses a developer fleet has installed.

## Function

Numbat attaches to an agent harness at three points. Hooks provide real-time detection, and the pre-action variety can block an action before it executes. Session artifacts, read directly from the harness dot-directory under `$HOME` and normalized to NDJSON timelines by `numbat scan`, provide retrospective forensics — including for sessions that ran before Numbat was installed. OTLP telemetry, received locally by `numbat collect`, supplies fleet monitoring signals that hooks and artifacts do not carry.

Detection content ships as 52 built-in rules across 11 behavior categories, written as CEL expressions over normalized events, together with multi-step sequence detections for secret access, exfiltration, privilege escalation, and lateral movement. Operators add rules and tests without modifying Numbat's source.

## Deployment posture

Telemetry is local-first: the `numbat collect` receiver binds to localhost by default, and nothing leaves the endpoint until an operator configures it to. Remote shipping runs through `numbat ship`, with ClickHouse named as an analysis destination. Perplexity distributes Numbat across its own fleet by MDM and reports using it against [[claude-code-security|Claude Code]], [[codex-security|Codex]], OpenCode, and Pi.

## Position

Numbat sits between two control types the wiki already records: harness-native permission systems, which enforce inside a single harness ([[claude-code-security|Claude Code Security]]), and static configuration auditing, which reads harness config without observing execution ([[agentshield|AgentShield]]). Numbat observes and blocks at runtime, across harnesses. Its distinguishing design commitment is the [[accidental-meltdown|accidental meltdown]] threat model — an agent crossing security boundaries without any adversarial input — which is what motivates placing prevention in the harness rather than the model.

The session-artifact route has one prior production implementation: Uber's [[adr-agentic-detection-system|ADR]] sensor, which parses the same class of local harness caches at fleet scale and already pairs that route with inline-hook blocking ([arXiv:2605.17380](https://arxiv.org/abs/2605.17380)). Numbat's addition relative to ADR is the OTLP receiver, four harnesses rather than three, and availability as an open-source binary.

The OTLP path converges with the direction argued in [[genai-endpoint-observability-talk|GenAI Endpoint Observability]]: agent-hook signals routed through [[opentelemetry-gen-ai|OpenTelemetry GenAI semantic conventions]] into the SIEM, because raw EDR telemetry cannot separate a developer's shell command from an agent's.

> [!gap] Unverified release specifics
> The repository URL, license identifier, and version are not stated in the announcement, and the site was unreachable from the ingest environment. Confirm all three against the published repository before citing Numbat as a deployable control.
