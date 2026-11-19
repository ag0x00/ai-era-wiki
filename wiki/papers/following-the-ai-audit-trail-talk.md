---
type: talk
title: "Following the AI Audit Trail"
created: 2026-10-21
updated: 2026-10-21
tags:
  - papers
  - talks
  - observability
  - opentelemetry
  - detection-engineering
  - dfir
  - claude-code
  - coding-agent
  - audit-logging
status: summarized
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
origin: aggregated
year: 2026
authors: ["Darwin Salazar"]
venue: "BSides NYC 2026, October 17, 2026"
source_url: "https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf"
archived_copy: ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf"
no_public_url: ""
key_claim: "Coding-agent activity is reconstructed from four evidence classes held by different parties: platform audit records, client runtime events, captured content, and evidence outside the agent. Each tool exposes a different subset under different plans and defaults, and Claude Code's runtime record shows who approved a tool call and what the tool reported, but not whether the action had its intended effect."
methodology: "Practitioner talk by Monad's Head of Growth, based on Monad's book A Security Field Guide to AI Audit Trails (October 2026). The deck compares nine audit and telemetry feeds from vendor documentation reviewed October 1 to 10, 2026, and captures three Claude Code 2.1.300 runs (manual approval, manual rejection, auto mode) through a local OTLP receiver on October 15, 2026. Summary built from the 30-slide deck and the talk repository README; no transcript or recording was available."
contradicts: []
supports:
  - "[[agent-observability]]"
  - "[[claude-code-control-sheet]]"
related:
  - "[[coding-agent-investigation-evidence|Coding-Agent Investigation Evidence]]"
  - "[[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]]"
  - "[[agent-observability|Agent Observability]]"
  - "[[opentelemetry-gen-ai|OpenTelemetry gen_ai.* Semantic Conventions]]"
  - "[[claude-code|Claude Code]]"
  - "[[claude-code-control-sheet|Claude Code Control Sheet]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[claude-cowork|Claude Cowork]]"
  - "[[cursor-ide|Cursor]]"
  - "[[mitre-atlas|MITRE ATLAS]]"
  - "[[agentic-soc-ra-detection-engineering|Agentic SOC RA: Detection Engineering]]"
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability for Detection Engineers]]"
sources:
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides-text.md"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_readme.md"
  - "https://github.com/DarwinSec/bsides-nyc-2026"
  - "https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf"
  - "https://code.claude.com/docs/en/monitoring-usage"
  - "https://platform.claude.com/docs/en/manage-claude/compliance-activity-feed"
  - "https://github.com/monad-inc/community-transformations"
  - "https://monad.com/blog/detection-engineering-for-claude-code-part-1"
  - "https://monad.com/blog/detection-engineering-for-claude-code-part-2"
verified: 2026-10-21
verified_against:
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_readme.md"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides-text.md"
  - ".raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf"
verified_findings: 0
verified_note: "All 30 slides rasterized and read with text layer and README; 9 fixes (start-sequence order, EDR overstatement, Gemini scope, Cursor record, tool_input, truncation default, run counts, transform attribution, slide 9 cite); none open."
---

# Following the AI Audit Trail

**Source:** [Darwin Salazar — Following the AI Audit Trail, BSides NYC 2026 (slides PDF)](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf) (2026-10-17), from the [talk repository](https://github.com/DarwinSec/bsides-nyc-2026). Local copy: `.raw/talks/2026-10-17_Darwin-Salazar_Following-the-AI-Audit-Trail_slides.pdf`.

Darwin Salazar, Head of Growth at Monad, a security data pipeline vendor, gave this talk at BSides NYC 2026. The deck discloses that the research comes from Monad's work, and the talk repository names Monad's book *A Security Field Guide to AI Audit Trails* (October 2026) as its basis. An investigator can use the talk as an evidence map for coding-agent activity: which record answers which question, what each vendor feed requires before it records anything, and what Claude Code's runtime events show about a single tool call and its approval.

## Findings that change an investigation

### Approval and outcome are separate records

Claude Code emits two runtime events for one tool call. A `tool_decision` event records whether the call was accepted or rejected and the approval source. A `tool_result` event, emitted only when the tool ran, records the result the tool reported. The two events share a `tool_use_id`, and the deck directs analysts to join them on that identifier, not on timestamps. The deck separates three states: *approved* is the decision event, *reported* is a result event whose `success` field holds the string `"true"`, and *verified* appears in neither. A file-write event from endpoint telemetry supports a write, and the file contents need a separate comparison.[^a5]

### A rejected call leaves only a decision

In the manual-rejection run, the analyst answered No at the permission prompt. The session's 15 events include a `tool_decision` recording a `reject` decision from source `user_reject`, and no `tool_result`.[^a5] The rejected decision still carries the full command in `tool_parameters` when tool details are enabled. A detection that reads only result events therefore misses every attempt a person refused, and a hunt keyed on the decision event counts them.

### The `config` source covers several approval paths

The approval source distinguishes a one-time Yes (`user_temporary`), an always-allow answer (`user_permanent`) and a refusal (`user_reject`). Claude Code 2.1.300 records an auto-mode approval, in which a classifier model reviews the action without prompting the user, as `config`. That version groups several permission paths under that value, so a `config` approval does not show whether the classifier or a configured rule allowed the call. The deck reports that 2.1.301 adds `classifier_verdict`, the classifier's decision, and `permission_mode`, the active mode, which separate the two.[^approval] The deck also reports auto mode as the built-in starting mode from 2.1.283.[^a4]

### Runtime telemetry starts at enablement

Claude Code delivers runtime events only by push once `CLAUDE_CODE_ENABLE_TELEMETRY=1` is set: the client on the developer's machine sends each event to a collector as it happens and does not backfill. Vendor audit APIs work the other way, returning history within their retention window, and the deck states that none of them carries Claude Code tool events. A pipeline between client and SIEM can split, flatten, normalize and redact the stream, and it cannot recover an event the client never sent.[^push] Telemetry enabled after an incident therefore records nothing about it. After inventorying the agents in use, the deck's starting sequence turns on client telemetry first, because it does not backfill, and deploys it through managed settings so it stays on.[^start]

## Evidence map

### Four evidence classes

The talk assigns each investigative question to the evidence class that answers it:[^four]

- **Platform audit records**, from vendor audit APIs, answer who changed access, keys or policy.
- **Runtime events**, usually delivered over OpenTelemetry, answer what the agent tried to do and who or what approved it.
- **Transcripts and content capture** answer which instructions or content were involved. Prompt text, tool inputs and responses are usually off by default.
- **Evidence from outside the agent**, in endpoint, network and destination logs, answers what happened to the system or data. None of it appears in the agent's own telemetry.

The deck lists what a coding agent can reach: the repository, every local file the user can read (AWS and SSH credentials, `.env` files and kubeconfig), the shell, APIs and MCP servers through the tokens it holds, cloud infrastructure, and SaaS data, where one tool call can perform a bulk export. Commits carry the developer's git identity, and AI attribution on a commit can be turned off.[^touch]

### Collection requirements by feed

The deck's checklist states what each feed delivers and what an organization needs before it records anything. Values are as the deck reports them after its October 2026 review, and the deck cites each vendor's documentation in its reference slide.[^a9]

| Feed | Evidence | Delivery | Plan or setting | Enabled by | Command and prompt text |
| --- | --- | --- | --- | --- | --- |
| Claude Code | Runtime events | Client push | Any install | Developer or managed settings | Off by default |
| Claude Cowork | Runtime events | Push | Team, Enterprise | Org admin | Version- and config-dependent |
| OpenAI Codex | Runtime events | Push | Any, opt-in | Developer | Prompts redacted |
| Cursor | Audit and runtime | Poll and push | Enterprise | Cursor admin | Separate opt-in |
| GitHub Copilot | Audit and runtime | Poll and push | Varies by feed | Org, enterprise or client | Client OTel content off by default |
| Anthropic Activity Feed | Audit | Poll | Compliance API enabled | Compliance activities key | None |
| Anthropic session transcripts | Content | Poll | Claude Enterprise, Compliance on | Compliance Access Key | The content itself |
| OpenAI API Platform audit logs | Audit | Poll | Owner must enable | Org owner | None |
| Gemini for Workspace activity | Feature usage | Poll | Workspace edition | Workspace admin with audit read scope | None |

The deck's per-tool comparison adds the conditions that change collection:[^a1]

- **Claude Code.** Exporter variables come from user or managed settings, and a repository's own settings file cannot set them. `tool_parameters` arrives as a JSON string that a pipeline must parse.
- **Claude Cowork.** The deck reports Desktop minimums of 1.1.4173 for local sessions and 1.22209.3 for cloud sessions, and states that local and cloud sessions differ, so both need verification.[^a1]
- **OpenAI Codex.** Telemetry is configured in the user-level Codex configuration file, and project-local settings are ignored. Runs started with the yolo flag or a similar approval-skipping flag need endpoint logs. The deck did not recheck its Codex values.
- **Cursor.** Audit logs cover the control plane, and the Enterprise OpenTelemetry export is sent from Cursor's side. Conversation content is a separate opt-in that the deck reports covers only Cloud Agents and Grok Bot, and the team-command record is not shell-command telemetry.
- **GitHub Copilot.** Usage records have a 48-hour pull window, and the audit log's Copilot actor filter covers the last 180 days.[^a1]

### Enterprise platform feeds

Three platform feeds record account, administrative and feature-usage activity, not local commands. The deck reports their retention and timing as rechecked on October 10, 2026:[^a2]

- **Anthropic Activity Feed** is polled from the Compliance API activities endpoint. The deck reports it as queryable within 1 minute, kept 6 years and not retroactive, with organization fields that can be null and file and artifact names null since September 24, 2026.[^a2]
- **OpenAI API Platform audit logs** are polled from the organization audit-log endpoint with an Admin key. Tenant events need a tenant-only query parameter, retention is not fixed, and audit logging cannot be disabled from settings once an owner enables it.[^a2]
- **Google Workspace Gemini activity** is polled through the Admin SDK Reports API under the Gemini in Workspace apps activity type. The deck reports availability from 2025-06-20 with a rolling 180-day history, and notes that Google Vault covers Gemini app conversations only.[^a2]

Anthropic's compliance transcripts return the request and tool input without client telemetry. The deck reports that they cover local Claude Code and Cowork sessions and exclude cloud Claude Code, Console API-key sessions, third-party sessions and Zero Data Retention, and that Admin keys cannot read them.[^a6] An EDR agent on the host records the process, its parent and the file write, and a matching file-write event independently supports the write.[^a6]

## Claude Code record model

### Session, turn and tool call

Every Claude Code event is flat and carries three identifiers. `session.id` persists from start to exit, survives a resume, and changes when the user clears the conversation. `prompt.id` groups everything the agent does to answer one user message. `tool_use_id` joins the decision and result events of one tool call. The deck compares the model with cloud audit logs: AWS CloudTrail's session context describes a credential session, `session.id` describes a conversation, and `prompt.id` describes one turn in it.[^model]

### Content switches

Telemetry is off until `CLAUDE_CODE_ENABLE_TELEMETRY=1` is set in user or managed settings, and each content class has its own switch. The deck reports these defaults, checked on October 10, 2026:[^a3]

| Setting | Effect | Default |
| --- | --- | --- |
| `OTEL_LOG_USER_PROMPTS=1` | Prompt text | Off (redacted) |
| `OTEL_LOG_TOOL_DETAILS=1` | `tool_parameters`, including `full_command`, and `tool_input` | Off |

Assistant response text follows `OTEL_LOG_USER_PROMPTS` unless set separately, raw API request and response bodies and tool-output span events each have their own switch and are off by default, and captured content is truncated at 61,440 UTF-16 units by default.[^a3] The demonstration left `OTEL_LOG_USER_PROMPTS` unset, so its prompts appear as `<REDACTED>`.[^a4]

Without `OTEL_LOG_TOOL_DETAILS=1` the record holds no `full_command`, and the deck advises protecting the resulting logs like the source data they expose.[^start]

### The three demonstration runs

Each run gave Claude Code 2.1.300 the same instruction, to copy a fake AWS credentials file into a backup directory, in a fresh session:[^a5]

| Run | Mode | User action | `tool_decision` | `tool_result` |
| --- | --- | --- | --- | --- |
| 1 | Manual | Yes, once | `accept`, `user_temporary` | Emitted, `success` `"true"` |
| 2 | Manual | No | `reject`, `user_reject` | Not emitted |
| 0 | Auto | No prompt | `accept`, `config` | Emitted, `success` `"true"` |

Runs 1 and 0 ran the same command and reported the same success, and only run 1 shows that a person approved it. Run 1 captured 18 events, run 2 captured 15, and run 0, with two tool calls, captured 26.[^a5]

## Starting hunts

The deck frames its queries as hunts and collection checks rather than alerts, each followed by a review of the rest of the session:[^a10]

- **Sensitive path in the command.** Match `full_command` against AWS credential files, SSH keys, `.env` files, kubeconfig and Azure configuration paths, keyed on `tool_decision` so rejected calls count. A path in the command shows where to look next and does not prove the file was read or that data left.
- **Accepted decision with no result.** An `accept` with no `tool_result` on the same `tool_use_id` can mean a long-running action, an interruption or a collection gap, so the deck checks the pipeline before the person.
- **Automatic approval against policy.** A `config` source on an action the team's policy reserves for a person, read on 2.1.301 and later together with `permission_mode` and `classifier_verdict`.
- **Silence.** The last event seen per device and user email, compared with expected activity. A quiet client on a machine that should be busy warrants a look.

The deck attributes queries for the sensitive-path and rejected-command cases, and an unknown-MCP-server rule, to Monad's March 26, 2026 detection-engineering post, a series published in [part 1](https://monad.com/blog/detection-engineering-for-claude-code-part-1) and [part 2](https://monad.com/blog/detection-engineering-for-claude-code-part-2). Monad's [community-transformations repository](https://github.com/monad-inc/community-transformations) holds the split-and-flatten transform the demonstration used. The deck's guidance is to check their field names against the receiving pipeline.[^a10]

## Source caveats

- **Vendor authorship.** The research comes from a security data pipeline vendor, and the deck names Monad as one pipeline option. The record findings come from Claude Code's own events, captured by a local OTLP receiver and flattened with Monad's open-source transform, so any OTLP/HTTP receiver can reproduce them without a Monad pipeline.
- **Field names move.** The deck states that field names change between versions and that every field name it uses comes from [Claude Code's monitoring documentation](https://code.claude.com/docs/en/monitoring-usage), to be re-read at each release. The record above is Claude Code 2.1.300, and 2.1.301 already adds two approval fields.[^approval]
- **Self-corrections in the deck.** The deck records that Monad's March 2026 post stated Bash commands appear by default, and that current documentation and the book require `OTEL_LOG_TOOL_DETAILS=1`. It also reports that the Cowork setup path differs between the help center (Organization settings) and the book (Admin settings).[^a7]
- **Product churn.** The deck reports Cowork merging into Claude, announced September 16, 2026, with a unified beta since October 8, so the Cowork feed's scope may change.[^a7]

## Relations

- [[coding-agent-investigation-evidence|Coding-Agent Investigation Evidence]] turns the talk's evidence map into an investigation sequence: collection prerequisites, reconstruction of one tool call, and endpoint collection order.
- [[agent-observability|Agent Observability]] treats hooks, artifact parsing and OpenTelemetry as instruments. The talk supplies the record an investigator reads from the OpenTelemetry instrument on one client and the blind spots it carries.
- [[genai-endpoint-observability-talk|GenAI Endpoint Observability for Detection Engineers]] describes the same separation from the endpoint side: EDR records what ran and not the decision behind it, which is the gap the runtime approval record fills.
- [[claude-code-control-sheet|Claude Code Control Sheet]] C07 holds tool decisions as evidence. The talk's runs show which events a denied, approved and auto-approved action produce.
- [[securing-agentic-coding|Securing Agentic Coding]] catalogs Claude Code's telemetry and content gates on its observability plane.
- [[ai-dfir-toolkit|depalmar/ai-dfir-toolkit]] is the artifact catalog the deck recommends for what agents leave on an endpoint and the order to collect it.
- [[opentelemetry-gen-ai|OpenTelemetry gen_ai.* Semantic Conventions]] is the transport family the runtime feeds use, while the field names shown are vendor-specific and change between releases.
- [[claude-code|Claude Code]], [[claude-cowork|Claude Cowork]] and [[cursor-ide|Cursor]] are the products whose feeds the deck compares.
- [[mitre-atlas|MITRE ATLAS]] and the [[owasp-agentic-ai-top-10|OWASP Top 10 for Agentic Applications]] are the deck's recommended threat taxonomies, and [[agentic-soc-ra-detection-engineering|Agentic SOC RA: Detection Engineering]] lists its starting hunts among the detection approaches for agent fleets.

## Notes

[^a5]: [Salazar — Following the AI Audit Trail, slide A5 and slides 9 and 13–15](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), BSides NYC, 2026-10-17. Three runs captured on Claude Code 2.1.300 on 2026-10-15; event counts are per captured session.
[^approval]: [Salazar — Following the AI Audit Trail, slides 12 and 15](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17. The 2.1.301 fields are reported by the deck from [Claude Code monitoring documentation](https://code.claude.com/docs/en/monitoring-usage); the demonstration captures predate them.
[^a4]: [Salazar — Following the AI Audit Trail, slides 11 and A4](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17. Manual runs started with `claude --permission-mode default`; the auto run started with plain `claude`.
[^push]: [Salazar — Following the AI Audit Trail, slides 7 and 10](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^start]: [Salazar — Following the AI Audit Trail, slide 16](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17, "Where to start".
[^four]: [Salazar — Following the AI Audit Trail, slide 5](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^touch]: [Salazar — Following the AI Audit Trail, slide 4](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^a9]: [Salazar — Following the AI Audit Trail, slides 6 and A9](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17; vendor documentation listed on slide A8, rechecked 2026-10-10 where slides A1 and A2 say so. The OpenAI API Platform feed needs an Admin API key to read; Anthropic transcripts need the `read:compliance_user_data` scope.
[^a1]: [Salazar — Following the AI Audit Trail, slide A1](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17, reviewed 2026-10-01 to 2026-10-07; Cowork OpenTelemetry settings, the Cursor plan and GitHub's 180-day window rechecked 2026-10-10; Codex values not rechecked. The deck cites [Claude Cowork monitoring](https://claude.com/docs/cowork/monitoring), [Cursor compliance and monitoring](https://cursor.com/docs/enterprise/compliance-and-monitoring) and [GitHub Copilot agentic audit log events](https://docs.github.com/en/copilot/reference/agentic-audit-log-events) for these rows.
[^a2]: [Salazar — Following the AI Audit Trail, slides A2 and A7](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17, rechecked 2026-10-10. The deck cites [Anthropic — Compliance Activity Feed](https://platform.claude.com/docs/en/manage-claude/compliance-activity-feed), [OpenAI Help Center article 9687866](https://help.openai.com/en/articles/9687866) and [Google Workspace — Gemini in Workspace apps activity events](https://developers.google.com/workspace/admin/reports/v1/appendix/activity/gemini-in-workspace-apps). Retention and timing values are the deck's, not rechecked here.
[^a6]: [Salazar — Following the AI Audit Trail, slide A6](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^model]: [Salazar — Following the AI Audit Trail, slides 7 and 9](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17.
[^a3]: [Salazar — Following the AI Audit Trail, slide A3](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17, checked 2026-10-10 against [Claude Code monitoring documentation](https://code.claude.com/docs/en/monitoring-usage). The deck also reports a 5000 ms event batch interval.
[^a10]: [Salazar — Following the AI Audit Trail, slide A10](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17. The deck attributes the queries to Monad's 2026-03-26 post by Jane and Redgate.
[^a7]: [Salazar — Following the AI Audit Trail, slides 3 and A7](https://github.com/DarwinSec/bsides-nyc-2026/blob/39be1d7fdb1a210203fe1ebf67b6c32533d49f1c/Following_the_AI_Audit_Trail_BSides_NYC_2026.pdf), 2026-10-17; the deck read the Cowork changelog on 2026-10-15.
