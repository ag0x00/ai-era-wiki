---
type: practice
title: "Coding-Agent Investigation Evidence"
created: 2026-10-21
updated: 2026-10-21
tags:
  - practices
  - dfir
  - incident-response
  - detection-engineering
  - coding-agent
  - claude-code
  - observability
  - opentelemetry
status: developing
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
origin: produced
maturity: emerging
addresses_threat: "Coding-agent actions that cannot be reconstructed after an incident: who or what approved a tool call, what it ran, and what it changed."
related:
  - "[[following-the-ai-audit-trail-talk|Following the AI Audit Trail]]"
  - "[[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[numbat|Numbat]]"
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability for Detection Engineers]]"
  - "[[claude-code|Claude Code]]"
  - "[[claude-cowork|Claude Cowork]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[claude-code-control-sheet|Claude Code Control Sheet]]"
sources:
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides-text.md"
  - ".raw/articles/ai-dfir-toolkit-2026-10-21.md"
  - ".raw/articles/ai-dfir-toolkit-artifact-catalog-2026-10-21.md"
  - ".raw/articles/perplexity-numbat-agent-security-suite-2026-07-31.md"
  - ".raw/papers/adr-agentic-detection-system-2026-05-17.md"
  - ".raw/talks/2026-03-03_Mika-Ayenson_GenAI-Endpoint-Observability_transcript.md"
  - "https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf"
  - "https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md"
  - "https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat"
  - "https://arxiv.org/abs/2605.17380"
  - "https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans"
  - "https://academy.claude.com/tutorials/claude-cowork-enterprise-administrator-guide"
  - "https://www.rfc-editor.org/rfc/rfc3227#section-2.1"
  - "https://code.claude.com/docs/en/monitoring-usage"
verified: 2026-10-21
verified_against:
  - ".raw/articles/ai-dfir-toolkit-2026-10-21.md"
  - ".raw/articles/ai-dfir-toolkit-artifact-catalog-2026-10-21.md"
  - ".raw/articles/perplexity-numbat-agent-security-suite-2026-07-31.md"
  - ".raw/papers/adr-agentic-detection-system-2026-05-17.md"
  - ".raw/talks/2026-03-03_Mika-Ayenson_GenAI-Endpoint-Observability_transcript.md"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides-text.md"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf"
verified_findings: 0
verified_note: "Raw sources read whole; RFC 3227 s2.1, Anthropic Cowork Team/Enterprise article and admin guide read live 2026-10-21; Cowork local-history export claim corrected to live article (Enterprise Compliance API retrieves it); none open."
---

# Coding-Agent Investigation Evidence

This practice tells an incident responder or SOC analyst which records establish what a coding agent did on a developer's machine, what to switch on before an incident so those records exist, and how to reconstruct one tool call from approval to effect. The worked example is [[claude-code|Claude Code]], whose OpenTelemetry events a BSides NYC 2026 talk captured and annotated field by field ([[following-the-ai-audit-trail-talk|Following the AI Audit Trail]]). The same sequence applies to other agents with the per-product differences listed below. The full per-feed collection matrix is on the talk page.

## Questions and the record that answers each

No single record answers every investigative question. Four classes of evidence each answer a different one, and a fifth question, whether a person or an agent acted, needs two of them together:[^four]

| Question | Record | Written by | Default state |
| --- | --- | --- | --- |
| Who changed access, keys or policy | Platform audit log | Vendor platform | Plan-gated |
| What the agent tried, and who approved it | Client runtime events | Agent client | Off until enabled |
| Which instructions or content were involved | Transcripts, content capture | Client or vendor | Usually off |
| What happened to the system or data | Endpoint, network, destination logs | EDR, proxies, target services | Independent of the agent |
| Whether a person or the agent ran a process | Runtime approval record plus EDR | Agent client and EDR | Needs both |

EDR alone cannot settle the last question. A developer and an agent running the same shell command produce near-identical host telemetry, with the same process ID, user and command line ([[genai-endpoint-observability-talk|GenAI Endpoint Observability for Detection Engineers]]).[^ayenson] The agent's own approval record supplies the missing decision, and EDR supplies the effect.

## Collection prerequisites before an incident

Client runtime telemetry does not backfill: the Claude Code client pushes each event to a collector as it happens, and a pipeline cannot recover an event the client never sent.[^push] Every item below therefore has to be in place before the session under investigation runs.

- **Inventory the agents.** List each agent, where it runs and which plan it is on, using the talk's [[following-the-ai-audit-trail-talk#Collection requirements by feed|collection requirements by feed]] as the checklist.[^start] The [[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]] osquery pack answers which hosts have a given tool.[^osquery]
- **Turn on client telemetry through managed settings.** Claude Code needs `CLAUDE_CODE_ENABLE_TELEMETRY=1` and an exporter, set in user or managed settings because a repository's settings file cannot set exporter variables. Managed settings keep it on.[^a3]
- **Decide on content capture.** Without `OTEL_LOG_TOOL_DETAILS=1` the Claude Code record holds no command text, so the sensitive-path hunt below has nothing to match. Captured command text exposes the paths and data the commands touched, so the logs need the same protection as the source data.[^start]
- **Enable the platform feeds that are off by default.** Anthropic's Activity Feed needs the Compliance API enabled, its transcripts need Claude Enterprise with Compliance on, and OpenAI's API Platform audit log needs an owner to enable it. Cursor and Cowork feeds require their Enterprise or Team plans.[^a9]
- **Keep the raw payload.** Store each OTLP batch as received alongside the flattened copy analysts query, because flattening changes the structure and field names move between client releases.[^a4]
- **Record client versions.** The approval record changed between Claude Code 2.1.300 and 2.1.301, so an analyst reading a past event needs the version that wrote it.[^approval]
- **Baseline collection health.** Track the last event seen per device and user. A quiet client is not broken by itself, and a silent client on a machine that should be busy warrants a collection-health check.[^a10]

## Reconstructing one tool call

A tool call leaves up to three records: a decision, a reported result and an effect on the host or a remote service. The steps below take them in that order, using Claude Code's field names.[^model]

1. **Scope the session.** Every Claude Code event carries `session.id`, which lasts from start to exit, and `prompt.id`, which groups the events that answer one user message. Pull all events for the session, then the turn.
2. **Read the decision.** The `tool_decision` event records `accept` or `reject` and the approval source: `user_temporary` for a one-time Yes, `user_permanent` for an always-allow answer, `user_reject` for a refusal, and `config` where no person was prompted. On 2.1.300, `config` covers both the auto-mode classifier and configured rules. On 2.1.301 and later, `permission_mode` and `classifier_verdict` separate them.[^approval]
3. **Join the result on `tool_use_id`, not on time.** A rejected call never has a `tool_result`, so every rejected attempt is visible only in step 2. An accepted call with no result can mean a long-running action, an interruption or a collection gap, and the pipeline is checked before the person.[^a10]
4. **Treat `success` as the tool's report.** A `tool_result` whose `success` field holds the string `"true"` records what the tool reported, not what changed on disk or in a remote service.[^a5]
5. **Confirm the effect outside the agent.** An EDR record of the process, its parent and the file write independently supports the action, and the file contents need a separate comparison.[^a6] For a cloud or SaaS action, the destination service's audit log carries the effect. Match on host, user, command text and time. The sources reviewed here name no identifier shared between the agent's events and EDR records.
6. **Fall back when client telemetry was off.** Agent session artifacts on disk still hold the conversation and tool calls. [[numbat|Numbat]] reads them from the agent-specific dot-directory under the home directory where most harnesses keep them, and normalizes them to timelines, including for sessions that ran before it was installed.[^numbat] Uber's [[adr-agentic-detection-system|ADR]] sensor parses the SQLite and JSONL caches of Cursor, Cline and Claude Code into prompt, reasoning, tool-call and outcome records.[^adr] On Claude Enterprise, compliance transcripts return the request and tool input without client telemetry, and endpoint evidence is still needed for the effect.[^a6]

## Endpoint artifacts and collection order

Coding agents, model runtimes and MCP servers leave dot-directories holding plaintext API tokens, configuration files that start child processes at login, unauthenticated listeners on well-known ports, and session transcripts that can contain whole source trees.[^catalog] The [[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]] artifact catalog documents [622 artifacts across 51 tools](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md#contents), each with a forensic-value rating, and gives each tool entry a collection block carrying a triage priority.[^catalog] The repository also exports KAPE targets and Velociraptor artifacts.[^exports] Collect in this order:

1. **Volatile state.** Running agent processes, their parents and listening ports first, following the order of volatility in RFC 3227, because they disappear when the session ends or the host reboots.[^rfc3227] The catalog records each tool's ports and process relationships.
2. **Session artifacts and transcripts.** These are the endpoint record of sessions that client telemetry missed. Claude Cowork stores a local session's history on the member's computer, outside Anthropic's standard retention, and administrators cannot centrally manage or delete it. Claude Enterprise administrators can retrieve it through the Compliance API, so on other plans the endpoint copy may be the only copy.[^cowork-local]
3. **Credential and MCP configuration locations.** The catalog lists [164 credential locations and 66 MCP configuration locations](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md#contents). They scope which secrets the agent could reach and which MCP servers it could call.
4. **Remaining catalog targets by triage priority**, through the KAPE or Velociraptor export or the catalog site's generated triage script.

Weight each artifact by the catalog's confidence rating: a low rating means single-source or inferred and carries `unverified: true`. The presence of an agent on an endpoint is not by itself an incident.[^catalog] Transcripts and content logs need the access controls of the source data they contain.[^start]

## Known blind spots by product, dated

Each entry is dated to the source that reports it. Field names and defaults move between releases, so re-check an entry against current vendor documentation before relying on it in an investigation.[^names]

- **Claude Code, 2.1.300 (October 2026).** No backfill, a rejected call leaves no result event, and `config` does not separate the auto-mode classifier from a rule. Command text and prompts are absent unless their content switches are on.[^approval]
- **Claude Cowork (October 2026).** The deck reports that local and cloud sessions differ and need separate verification.[^a1] Anthropic's administrator guide states that Audit Logs do not cover Cowork yet, and its Team and Enterprise article states that a local session's history stays on the member's computer, retrievable centrally only through the Compliance API on Claude Enterprise.[^cowork-local]
- **OpenAI Codex (October 2026, not rechecked by the deck).** Project-local telemetry settings are ignored, and runs started with the yolo flag or similar approval-skipping flags need endpoint logs.[^a1]
- **Cursor (October 2026).** The team-command record is not shell-command telemetry, and conversation content is a separate opt-in covering only Cloud Agents and Grok Bot.[^a1]
- **GitHub Copilot (October 2026).** Client OpenTelemetry content capture is off by default, and the audit log's Copilot actor filter covers the last 180 days.[^a1]
- **Anthropic Activity Feed (October 2026).** It records account and organization activity, not local commands, is not retroactive, and has reported file and artifact names as null since September 24, 2026.[^a2]
- **EDR (March 2026).** It records what ran and not the decision behind it, so it cannot separate a developer's command from an agent's without the agent-side record.[^ayenson]

## Starting hunts

The talk frames four queries over the flattened Claude Code record as starting points that open an investigation, each followed by a review of the rest of the session:[^a10]

- **Sensitive path in the command.** `full_command` matching AWS credential files, SSH keys, `.env` files, kubeconfig or Azure configuration, keyed on `tool_decision` so rejected attempts count. A matched path shows where to look next and does not prove a read or an exfiltration.
- **Accepted decision with no result.** Usually a collection or pipeline check before it is a person check.
- **Automatic approval against policy.** A `config` source on an action the team's policy reserves for a person, read with `permission_mode` and `classifier_verdict` on 2.1.301 and later.
- **Silence.** No events from a device that should be active.

For host-side behaviour, the [[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]] endpoint set holds Sigma rules scoped to agent behaviour across every catalogued tool. Its README reports their count and ATLAS and OWASP mappings, and reports no measured detection rate.[^osquery]

## Notes

[^four]: [Salazar — Following the AI Audit Trail, slide 5](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), BSides NYC, 2026-10-17. The fifth row combines slide 5 with the Ayenson talk.
[^ayenson]: Mika Ayenson (Elastic), "GenAI Endpoint Observability for Detection Engineers", [Unprompted Conference abstracts, March 2026](https://unpromptedcon.org/abstract-march2026/); transcript at `.raw/talks/2026-03-03_Mika-Ayenson_GenAI-Endpoint-Observability_transcript.md`: "Same PID, same user, same command line, nearly identical telemetry."
[^push]: [Salazar — Following the AI Audit Trail, slides 7 and 10](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^start]: [Salazar — Following the AI Audit Trail, slide 16, "Where to start"](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^osquery]: [ai-dfir-toolkit README, "Coverage overview"](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md#coverage-overview), commit `1a5963d`, captured 2026-10-21: the endpoint set is scoped to agent behaviour on a host, and its osquery pack answers which hosts have a tool.
[^a3]: [Salazar — Following the AI Audit Trail, slides A3 and 16](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17; the deck reports checking these settings against [Claude Code monitoring documentation](https://code.claude.com/docs/en/monitoring-usage) on 2026-10-10.
[^a9]: [Salazar — Following the AI Audit Trail, slides 6 and A9](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17; values rechecked 2026-10-10 where the deck says so.
[^a4]: [Salazar — Following the AI Audit Trail, slides A4 and 17](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17: "Keep the original payload. Flattening changes its structure."
[^approval]: [Salazar — Following the AI Audit Trail, slides 12, 14 and 15](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17. Captures on 2.1.300; the 2.1.301 fields are reported by the deck from the monitoring documentation.
[^a10]: [Salazar — Following the AI Audit Trail, slide A10](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^model]: [Salazar — Following the AI Audit Trail, slide 9](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^a5]: [Salazar — Following the AI Audit Trail, slide A5](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17: "Verified is not in these logs."
[^a6]: [Salazar — Following the AI Audit Trail, slide A6](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17. The deck reports that the transcripts cover local Claude Code and Cowork sessions and exclude cloud Claude Code, Console API-key, third-party and Zero Data Retention sessions.
[^numbat]: [Perplexity Research — Securing Agents Across Perplexity's Client Endpoints with Numbat, "Session artifacts"](https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat), July 2026.
[^adr]: [arXiv:2605.17380 — ADR: An Agentic Detection System for Enterprise Agentic AI Security, §3.1](https://arxiv.org/abs/2605.17380), 2026-05-17.
[^catalog]: [ai-dfir-toolkit — AI Agent Artifact Catalog README](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/artifacts/README.md), commit `1a5963d`, captured 2026-10-21. Counts are the repository's generated Contents table.
[^exports]: [ai-dfir-toolkit README, "Artifact Catalog"](https://github.com/depalmar/ai-dfir-toolkit/blob/1a5963d9225d3577d4500f3e1ec8a1f0b8664b3b/README.md#artifact-catalog), commit `1a5963d`, captured 2026-10-21: generated feeds for ForensicArtifacts, KAPE, Velociraptor and CSV or JSON consumers.
[^rfc3227]: [IETF RFC 3227 — Guidelines for Evidence Collection and Archiving, §2.1 Order of Volatility](https://www.rfc-editor.org/rfc/rfc3227#section-2.1), 2002.
[^cowork-local]: [Anthropic — Use Claude Cowork on Team and Enterprise plans](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) and [Anthropic Academy — Claude Cowork enterprise administrator guide](https://academy.claude.com/tutorials/claude-cowork-enterprise-administrator-guide), both read 2026-10-21; the Team and Enterprise article's "Local conversation storage" section and the administrator guide's Phase 1 security-review step.
[^a1]: [Salazar — Following the AI Audit Trail, slide A1](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17, reviewed 2026-10-01 to 2026-10-07; Cowork OpenTelemetry settings, the Cursor plan and GitHub's 180-day window rechecked 2026-10-10; Codex values not rechecked.
[^a2]: [Salazar — Following the AI Audit Trail, slides A2, A6 and A7](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17, rechecked 2026-10-10.
[^names]: [Salazar — Following the AI Audit Trail, slides 3 and 17](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17: field names change between versions and are re-read each release.
