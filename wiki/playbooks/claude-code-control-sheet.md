---
type: playbook
title: "Claude Code Control Sheet"
created: 2026-09-24
updated: 2026-09-24
tags:
  - playbooks
  - claude-code
  - agentic-coding
  - cmm
  - control-mapping
status: developing
scope_axis:
  - sec-of-ai
origin: produced
audience: "Platform or security engineer configuring Claude Code across an enterprise estate, and the assessor grading that estate against the Agentic AI Security CMM"
length: "~25 pages; nine domain tables mapping 91 of the CMM's 224 criteria, each claim footnoted to a Claude Code documentation section"
related:
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]"
  - "[[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]"
  - "[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]"
  - "[[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]]"
  - "[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]]"
  - "[[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[claude-code|Claude Code]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]]"
  - "[[security-guidance-plugin|Security Guidance Plugin]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[cognitive-file-integrity|Cognitive File Integrity (CFI)]]"
  - "[[guard-canonicalization-gap|Guard Canonicalization Gap]]"
  - "[[decision-rights|Decision Rights for AI Agents]]"
  - "[[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]]"
  - "[[cmm-known-limitations|CMM Known Limitations (current state)]]"
sources:
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]"
  - "[[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]"
  - "[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]"
  - "[[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]]"
  - "[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]]"
  - "[[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[claude-code|Claude Code]]"
  - "https://code.claude.com/docs/en/monitoring-usage"
  - "https://code.claude.com/docs/en/permission-modes"
  - "https://code.claude.com/docs/en/sandboxing"
  - "https://code.claude.com/docs/en/desktop"
  - "https://code.claude.com/docs/en/hooks"
  - "https://code.claude.com/docs/en/managed-settings"
  - "https://code.claude.com/docs/en/self-hosted-environments-identity"
  - "https://code.claude.com/docs/en/mcp"
  - "https://code.claude.com/docs/en/server-managed-settings"
  - "https://code.claude.com/docs/en/admin-setup"
  - "https://code.claude.com/docs/en/claude-apps-gateway"
  - "https://code.claude.com/docs/en/github-actions"
  - "https://code.claude.com/docs/en/permissions"
  - "https://code.claude.com/docs/en/cloud-environments"
  - "https://code.claude.com/docs/en/feature-availability"
  - "https://code.claude.com/docs/en/memory"
  - "https://code.claude.com/docs/en/plugin-marketplaces"
  - "https://code.claude.com/docs/en/security"
  - "https://code.claude.com/docs/en/settings-reference"
  - "https://code.claude.com/docs/en/auto-mode-config"
  - "https://code.claude.com/docs/en/claude-code-on-the-web"
  - "https://code.claude.com/docs/en/claude-directory"
  - "https://code.claude.com/docs/en/code-review"
  - "https://code.claude.com/docs/en/data-usage"
  - "https://code.claude.com/docs/en/env-vars"
  - "https://code.claude.com/docs/en/sandbox-environments"
  - "https://code.claude.com/docs/en/settings"
  - "https://code.claude.com/docs/en/sub-agents"
  - "https://code.claude.com/docs/en/agent-teams"
  - "https://code.claude.com/docs/en/authentication"
  - "https://code.claude.com/docs/en/claude-tag"
  - "https://code.claude.com/docs/en/cli-reference"
  - "https://code.claude.com/docs/en/computer-use"
  - "https://code.claude.com/docs/en/devcontainer"
  - "https://code.claude.com/docs/en/discover-plugins"
  - "https://code.claude.com/docs/en/legal-and-compliance"
  - "https://code.claude.com/docs/en/managed-mcp"
  - "https://code.claude.com/docs/en/mobile"
  - "https://code.claude.com/docs/en/network-config"
  - "https://code.claude.com/docs/en/remote-control"
  - "https://code.claude.com/docs/en/routines"
  - "https://code.claude.com/docs/en/security-guidance"
  - "https://code.claude.com/docs/en/third-party-integrations"
  - "https://code.claude.com/docs/en/zero-data-retention"
verified: 2026-09-24
verified_against: []
verified_findings: 0
verified_note: "Read whole vs the 44 cited code.claude.com docs saved 2026-09-24, the nine deep dives, protocol, dependency rules, core evidence list, issues #328/329/331/333/334/342; 0 open: the D9 unmapped-criteria summary was completed after the read, against D9 l.91, l.138 and l.140"
---

# Claude Code Control Sheet

The configurable controls of [[claude-code|Claude Code]], Anthropic's agentic coding harness, map to the criteria of the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] (AAI-S CMM) in one section per domain. Each row pairs a criterion, quoted as its deep dive words it, with the setting key, managed-only key, hook event, CLI flag or environment variable that implements it, the evidence an assessor collects and the harness's coverage, for the engineer who configures the control and the assessor who grades it. Each harness statement is footnoted to the section of Anthropic's Claude Code documentation, as published on 2026-09-24, that states it.

The harness is strongest in the control domain, where a managed permission policy the session cannot widen is the decision point D3 L3 names, and weakest in identity, where a local session acts as the developer. With the organization's own records counted as present, Claude Code's controls reach L2 at most in any domain.

## Scope

### Surfaces and settings sources

The sheet covers six surface groups:

- **Local sessions.** The terminal CLI, the VS Code and JetBrains extensions, and the Desktop app's Code tab with Local selected run on the developer's machine,[^web-intro] and Desktop reads the CLI's settings files.[^desk-modes]
- **Desktop SSH and WSL sessions.** The SSH environment runs Claude Code on a remote machine the user manages, under that host's managed settings file, and on Windows the WSL environment runs it in a WSL 2 distribution on the same machine.[^desk-env][^desk-managed] WSL reads only `/etc/claude-code` unless an administrator-only Windows source sets `wslInheritsWindowsSettings: true`, and Desktop turns WSL sessions off by default on devices it detects as organization-managed.[^as-wsl]
- **Non-interactive runs.** `claude -p` and the Agent SDK run without the interactive interface, which is all the Desktop app offers.[^desk-cli]
- **Cloud sessions.** Started from claude.ai/code, the Claude mobile app, the Desktop app with Cloud selected, `claude --cloud`, a routine or a Claude Tag channel, they share one set of environments, hosted by Anthropic or self-hosted, the latter in public beta on Team and Enterprise plans.[^web-intro][^web-env][^shi-beta]
- **CI integrations.** The Claude Code GitHub Action and GitLab CI/CD.
- **Remote Control.** A browser or the mobile app steers a session that keeps running on the user's machine.[^rc-vs]

The Desktop app's Cowork tab is graded under each deep dive's desktop-agent row.

Settings reach a session from ranked scopes: managed policy, the command line (`--settings`), then local project, shared project and user files. List keys merge across scopes, so a lower scope can add entries to a managed list it cannot override.[^set-prec] Managed policy reaches a device from four sources, highest priority first:[^as-deliver]

- Server-managed settings, from the claude.ai admin console or a Claude apps gateway.
- A macOS plist in the `com.anthropic.claudecode` domain, or Windows policy under `HKLM\SOFTWARE\Policies\ClaudeCode`.
- The file-based `managed-settings.json`, at `/etc/claude-code/` on Linux and WSL.
- The Windows user registry under `HKCU\SOFTWARE\Policies\ClaudeCode`, writable without elevation.

Under the default `managedSourcesBehavior`, `"first-wins"`, Claude Code applies the highest-ranked source delivering a policy key, apart from a few keys it reads from every admin source.[^ms-combine]

### Deployment shapes

[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] defines the five shapes, and a row applies to every shape unless its bullets name one. The surface groups fall under them as follows:

| Shape | Surfaces | Settings that reach the session | Identity the session acts under |
|---|---|---|---|
| Interactive local | Local sessions, Desktop SSH and WSL sessions, Remote Control | Every scope, and managed policy from the host's own sources | The developer's account |
| Sandboxed autonomous local | The same surfaces with prompts suppressed, and non-interactive runs on a workstation | As interactive local, with the `sandbox` keys load-bearing | The developer's account |
| Delegated cloud | Cloud sessions | Server-managed settings, the repository's `.claude/settings.json` in a one-repository session, and a self-hosted runner image's managed file[^set-cloud] | The developer's account, or the organization's shared identity under Claude Tag[^tag] |
| CI-runner agent | CI integrations, and non-interactive runs on a build runner | The workflow's `settings` and `claude_args` inputs, and a managed file on the runner[^gha-params][^dc-policy] | A Console service account through federation, or a stored token[^gha-wif] |
| Fleet / parallel | Many of the above at once | The union of the above | Per shape |

Workload identity federation credentials do not trigger the server-managed settings fetch, so a CI run on federation takes its policy from the workflow or the runner.[^sms-platform]

### Provider routes

The route to the model decides which administrative channels exist:[^fa-admin]

- **Claude for Teams or Enterprise sign-in.** The Owner or Primary Owner role sets server-managed settings, and settings-change audit events come through the compliance API or audit log export.[^sms-req][^sms-audit] Enterprise sessions start in Manual mode, and Team sessions in a terminal or the VS Code extension in auto mode, from v2.1.228 on macOS, Linux and WSL and v2.1.233 on native Windows.[^pm-start] The extension's own `claudeCode.initialPermissionMode` outranks a managed `permissions.defaultMode`.[^pm-switch]
- **Claude Console API key.** Server-managed settings apply where the key belongs to a Team or Enterprise organization.[^fa-summary]
- **Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry and Claude Platform on AWS.** Server-managed settings and the analytics dashboard are unavailable, so policy arrives through device management, a managed file or a Claude apps gateway.[^fa-admin] Access is restricted through the provider's IAM, which `forceLoginOrgUUID` does not reach, and metrics and error reports to Anthropic are off by default.[^as-enforce][^du-provider]
- **Claude apps gateway.** A self-hosted OIDC gateway built into the `claude` binary delivers managed settings per IdP group and stamps telemetry with the IdP identity, and it has no sign-in flow for unattended pipelines, so CI authenticates to the provider directly.[^gw-why][^gw-ci]
- **Another LLM gateway set through `ANTHROPIC_BASE_URL`.** Server-managed settings and Remote Control are off, so policy arrives through device management and OpenTelemetry export carries D7.[^fa-admin][^sms-platform]

Cloud sessions and Remote Control need a claude.ai account, so neither exists on a Console API key or a third-party provider.[^mob-signin]

## Instructions for use

### Reading a row

A criterion is one bullet in a deep dive's level detail list, or one item of a level statement where the deep dive grades that level from its statement, and alternatives satisfying one criterion, such as D1's three L5 assurance schemes, count once. The coverage column takes four values:

- **Native**: Claude Code enforces the control, or emits the evidence, once the row's setting is in place.
- **Partial**: Claude Code covers part of the criterion, and a bullet names the rest and its supplier: an organization record, a hook the organization writes, or a control outside the harness.
- **External**: the control sits outside Claude Code, and the row names any input Claude Code supplies to it.
- **Not applicable**: the harness holds no instance of what the criterion governs in the shapes the bullets name.

Coverage grades the harness, and a natively covered criterion is still not met on a device without the setting. Bullets follow the table's levels in ascending order, with a whole-domain bullet, such as a dependency cap, last.

OpenTelemetry events appear under their short names, such as `tool_decision`, which Claude Code emits with a `claude_code.` prefix, and span and metric names appear in full. A named environment variable reaches every session through the managed `env` block,[^env-set] and sandbox keys sit under `sandbox`, `sandbox.network` or `sandbox.filesystem`, whether or not the text writes the prefix.[^sbx-enforce][^sbx-network][^sbx-fs]

### Columns each role adds

- **The platform engineer** copies each domain table into the assessment workbook and adds an *As deployed* column: the value set, its scope and the device's Claude Code version, with one entry per policy variant, keyed by its `managed_settings.resolved_sha256` digest, for a fleet.
- **The assessor** adds a *Verdict* column: the CMM verdict, its assurance class and, for inspected or attested evidence, the artifact record. A criterion is met only where every policy variant in the assessed scope meets it, the rule D2 states and D3 leaves open ([#329](https://github.com/ag0x00/ai-era/issues/329)).

### Verdicts and evidence

The deep dives grade each criterion met, not met, not applicable or unanswerable, and [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] records an assurance class beside each met or not-met verdict:

- **Tested**: the organization or the assessor exercised the control, such as by attempting a denied tool call.
- **Inspected**: vendor tooling the customer can reach, such as `/status`, `claude doctor` or the fleet's OpenTelemetry events, or a record the organization keeps, such as a register or a runbook, shows the control's state in this deployment.
- **Attested**: a vendor document whose own scope names the control, such as a Claude Code documentation page, which reads the same in every deployment.

An inspected or attested verdict names its artifact by issuer, title and version, date, service and tenant, and period covered. Grading is cumulative, an unanswerable criterion holds its level and those above open, and a not-applicable verdict leaves the denominator with its reason in the strategic-rationale field. Every cited behavior belongs to a release, so the assessment names the harness version beside each artifact, and a device below a footnoted version floor lacks the control.

An external row is graded on the external control's evidence, recorded with the input Claude Code supplied and the harness version, and is not met where no external control operates, because the instance exists and nothing controls it. An instance inside Anthropic's service, such as the request and response screening, is graded on Anthropic's evidence instead, the deployment's own events or documentation whose scope names the control, and is unanswerable where neither exists, as for input canonicalization.

## Domain summary

The harness ceiling is the highest level at which every mapped criterion can be met from Claude Code's own controls, counting hooks the organization writes as the harness's and the organization's records at that level, such as registers, runbooks and matrices, as present. *Ceiling set by* names up to two of the lowest criteria that need a control outside the harness, the domain sections name any others, and the last column names that control.

| Domain | Criteria mapped | Harness ceiling | Ceiling set by | External control that lifts it |
|---|---|---|---|---|
| D1 Governance and Accountability | 4 of 17 | L2 | L3 shadow-agent inventory, L3 harness configuration | Endpoint discovery of unsanctioned installs, and device management that restores the managed source |
| D2 Identity and Authorization | 11 of 33 | L1 local, L2 cloud and CI | D2-IDENTITY locally, D2-DELEGATE-EXCHANGE in cloud and CI | A workload identity per agent that the called services verify |
| D3 Control and Least-Agency | 14 of 23 | L2 | L3 decision-rights matrix, L3 fail-closed gate | An approval service naming the approver per action class |
| D4 Runtime and Guardrails | 11 of 21 | L2 | L3 input canonicalization, L3 output data-class scope | An inference gateway that canonicalizes input and scans output by data class |
| D5 Egress and Network | 9 of 22 | L2 | L3 gateway in path | An agent-aware gateway on every egress leg, with TLS inspection |
| D6 Data, Memory and RAG | 10 of 30 | L2 | L3 per-source trust attribution, L3 augmentation store | Per-source trust labels, and encryption at rest over the transcript store |
| D7 Observability and Detection | 10 of 20 | L2 | L3 semantic-convention pin, L3 sandbox escape indicators | A collector that pins the `gen_ai.*` convention and forwards sandbox violations |
| D8 Supply Chain and AI-BOM | 11 of 28 | L2 | L3 dependency scanning in CI, L3 artifact signing | Dependency scanning with lockfile enforcement, pre-install scanning, signing |
| D9 Operations and Human Factors | 11 of 30 | L2 | L3 system-prompt trip-wire, L3 approval categories | A tamper-evident approval store, and a system-prompt trip-wire |

Four ceilings need their basis stated:

- **D1's L2 rests on records alone.** No D1 L2 criterion bears on the harness, so the ceiling holds where the owner, the AI-use policy, the risk-tier scheme and the signed RACI exist.
- **D2 decides two other domains.** Rules DR-001 and DR-002 cap egress and observability at D2's raw score, so a local estate whose sessions act as the developer reports both at L1 effective whatever it spends on proxies and collectors, and identity is the first control to add, as a CI run on federation with a custom GitHub App lifts all three to L2.
- **D3 stops at L2 on two criteria.** The decision-rights matrix needs an approver the session holder does not choose, and the fail-closed gate fails as written on an absent managed source, pending the decision in [#342](https://github.com/ag0x00/ai-era/issues/342) on whether delivery assurance can stand in.
- **D5's L2 needs every egress leg allowlisted.** Claude Code allowlists shell commands through the sandbox, the fetch tool through `WebFetch` rules, remote MCP servers by `serverUrl` and the Desktop Browser pane by site, and only a whole-process boundary, such as the sandbox runtime or a dev container, confines hooks and stdio MCP servers, so without one a host or network allowlist carries those legs, or D5 stays at L1.[^se-bash]

### Coding-tool evidence items

[[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] adds four evidence items for a coding tool at L3 and above, drawn from [[ai-coding-agent-governance|AI Coding Agent Governance]]. The sheet grades each against the criterion that names its object, and the domain rows carry the controls:

| Evidence item | Criterion that grades it | Coverage |
|---|---|---|
| Agent rules-file integrity | D6 L3 hashing for cognitive file integrity, and D8 L4 cognitive-file integrity baselines | partial |
| IDE extension provenance | D6 L2 extension review | partial |
| Typosquat / dependency-hijack defense | D8 L3 dependency scanning in CI with lockfile enforcement | external |
| Destructive-action classification | D3 L3 action-risk tiering, with the per-action tier documentation and the decision-rights matrix beside it | partial |

D8's level definitions, as read on 2026-09-24, name no editor extension, so an editor-extension allowlist is graded at D6's L2 extension review, which names them, and a Claude Code plugin, which bundles skills, agents, hooks and MCP servers, at D8's provenance item.

## 1 — D1 Governance and Accountability

The sheet maps 4 of D1's 17 criteria, all at [[agentic-ai-security-cmm-d1-governance|D1]] L3, where one criterion names the coding harness itself. The other 13 are ownership, policy, oversight and assurance documents no harness setting produces.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L3 | Decision rights and operational boundaries documented per agent type. | Managed `permissions.deny` and `autoMode.hard_deny` entries | The governance record, citing the rule set by version | partial |
| L3 | A shadow-agent inventory and reaper SLA in operation. | `forceLoginOrgUUID` and `forceLoginMethod` | Endpoint inventory reconciled against Claude Code users | external |
| L3 | A provider responsibility matrix. | None | Matrix rows for Anthropic as model, hosting and extension provider | external |
| L3 | Harness configuration under managed policy and review, for agentic coding. | Managed policy with `allowManagedPermissionRulesOnly`, `allowManagedHooksOnly`, `allowManagedMcpServersOnly`, `strictPluginOnlyCustomization` | The policy resolved on an enrolled device, the restore mechanism, the review record | partial |

- **The harness criterion asks for three artifacts, and Claude Code produces the first.** `/status` names the managed source in force on its `Setting sources` line, and from v2.1.242 a `Skipped sources` line names any source a higher one overrode.[^ms-status] `claude doctor` lists each dropped entry with its source.[^ms-drop] With `OTEL_LOG_MANAGED_SETTINGS=1`, `managed_settings_resolved`, from v2.1.274, carries `managed_settings.resolved_sha256`, and machines reporting one digest run one policy.[^mon-msr] Desktop SSH and WSL sessions each resolve policy as a device of their own.[^desk-managed][^as-wsl]
- **Four managed-only locks close permission rules, hooks, MCP servers and user- or project-sourced customization.**[^ms-managedonly] The record names the list keys a local scope can still extend:
  - `sandbox.excludedCommands`, which has no managed-only lockdown.[^sbx-widen]
  - The `autoMode` lists, where a developer's `allow` entry can override an organization `soft_deny` entry.[^amc-scope]
  - `allowedHttpHookUrls` and `httpHookAllowedEnvVars`, which merge across settings files.[^ms-failclosed]
- **The restore mechanism sits outside the harness.** A developer who administers the machine can edit the managed source, so device management redeploys it on a schedule.[^ms-dev] A user on an unmanaged device bypasses server-managed settings, a client-side control, without administrator rights, for example by exporting a provider variable or a non-default `ANTHROPIC_BASE_URL`, which skips the fetch.[^sms-sec][^sms-platform]
- **The prohibited-action list has an enforced half.** Managed `permissions.deny` rules act before the auto-mode classifier in every mode and cannot be overridden, and the per-agent-type record stays organizational.[^amc-scope]
- **Login restriction prevents, and the endpoint discovers.** `forceLoginOrgUUID` is checked for claude.ai logins in the terminal, the VS Code extension and the Agent SDK, not for Console logins, gateway sign-in or cloud-provider sessions, and the section read names no such check for the Desktop app.[^as-enforce] Endpoint inventory finds installs on unmanaged devices, and inside a WSL 2 distribution only a sensor running there sees processes.[^as-wsl]
- **Anthropic's side of the responsibility matrix comes from its own documents.** The Trust Center holds the SOC 2 Type 2 report and the ISO 27001 certificate.[^sec-found] Managed settings bind Claude Code only, so model API calls from another tool stay residue on the organization's side.[^ms-dev]

## 2 — D2 Identity and Authorization

Claude Code's answer to [[agentic-ai-security-cmm-d2-identity|D2]] L2, an identity of the agent's own, turns on where the session runs. The sheet maps 11 of D2's 33 criteria. The other 22 are identity-platform controls, records the organization keeps and the L5+ program items, which no harness setting implements.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | D2-IDENTITY | CI: federation through `anthropic_federation_rule_id`, and a custom GitHub App's `github_token` | The federation rule, and who can use each credential | partial |
| L2 | D2-DELEGATE | Local: the developer's own account. Cloud: the GitHub proxy's repository scope | A sampled session's effective access against its developer's own | partial |
| L3 | D2-IDENTITY-VERIFY | Self-hosted: `CLAUDE_CODE_SESSION_ACCESS_TOKEN`, an ES256 JWT verifiable against a published key set | Service-side verification configuration, and an accepted token | partial |
| L3 | D2-DELEGATE-EXCHANGE | Self-hosted: the token's `act` chain names the creating user and the session | A token sample showing `act.sub` and `ccr:session_id` | partial |
| L3 | D2-TRACE | Identity attributes on every OpenTelemetry event, and `git_commit_id` on `tool_result` | Commits re-traced to a session and a developer | partial |
| L4 | D2-NOCRED | `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`, `sandbox.credentials`, and gateway sign-in or federation | The credentials each route leaves readable to the process | partial |
| L4 | D2-AUTHZ | None | A policy export naming the calling agent | external |
| L4 | D2-KILL | Gateway: disabling the user in the IdP ends access within `ttl_hours` | An execution record against a running session | partial |
| L4 | D2-TASKBIND | Self-hosted: a token bound to `ccr:session_id` with a four-hour default lifetime | A token sample, and a token refused after its session ended | partial |
| L4 | D2-MUTUAL | `CLAUDE_CODE_CLIENT_CERT` and `CLAUDE_CODE_CLIENT_KEY`, and OAuth on remote MCP servers | Each called service's authentication configuration | partial |
| L5 | D2-AUDIT | Settings-change events through the compliance API, and `ConfigChange` hooks | An audit-log sample covering each event type | partial |

- **Local shapes fail D2-IDENTITY.** Claude Code acts under no separate service account: each event carries the developer's Claude account, or the developer's IdP identity through a gateway,[^mon-attr] so both local shapes stop at L1.
- **Anthropic-hosted cloud sessions meet D2's vendor-held identity condition.** A customer-verifiable session identity exists only for self-hosted environments, in public beta, and Anthropic-hosted sessions' tokens carry an `sk-ant-si-` prefix and a separate key set,[^shi-format] so, with no agent principal of the organization's own, D2-IDENTITY, D2-IDENTITY-VERIFY, D2-LIFECYCLE and D2-IDENTITY-ATTEST are not applicable. The GitHub proxy swaps a scoped in-VM credential for the user's GitHub token and limits API calls to the session's repositories, which holds D2-DELEGATE,[^ce-github] while the downstream call carries the human's own token, where D2-DELEGATE-EXCHANGE wants one naming both parties.
- **A CI run holds two credentials, and D2-IDENTITY reads both.** Federation exchanges the workflow's GitHub OIDC token for Claude API access through a Console service account, leaving no long-lived key in the workflow.[^gha-wif] The repository credential defaults to the Claude GitHub App, shared by every Claude feature that integrates with GitHub, Code Review and cloud auto-fix included, and able to write to Actions, Workflows and repository hooks among others,[^gha-app] so the sheet reads D2-IDENTITY against a custom app whose `github_token` covers only the action.[^gha-params]
- **A human-triggered CI run acts for that human.** An `@claude` mention starts it, and the action requires the triggering user's write access and rejects a bot actor unless `allowed_bots` lists it,[^gha-trigger] so D2-DELEGATE sets the app's unchanged grant against that user's own access.
- **Self-hosted environments are the one route to the L3 identity criteria.** The session token proves that Anthropic issued it for a specific session in a specific environment and how the session was created, and not which runner process presents it.[^shi-proves] `act.sub` names the creating user or service identity, and `act.attested_by.sub` the SSO provider's subject.[^shi-act] With no revocation feed, a token stays valid until its `exp`, four hours by default and eight at most, so a service scopes what it derives from it,[^shi-claims][^shi-scope] and the beta route counts from its documented production date under the CMM's rule 2.
- **The harness supplies D2-TRACE's human half from outside the model.** Each event carries the human identity the D7 identity bullet lists per route, and with `OTEL_LOG_TOOL_DETAILS=1` `tool_result` records `git_commit_id` for a commit Claude makes.[^mon-toolresult] The `attribution.commit` trailer is no evidence, because the model writes the line even where a managed value outranks repository instructions.[^sr-attr]
- **D2-NOCRED turns on the route.** The environment scrub strips credentials from the Bash tool, hooks and MCP stdio servers while the parent process keeps them for its own API calls,[^env-vars] so a stored API key, or a long-lived token from `claude setup-token`, sits in the agent's process.[^auth-token] Gateway sign-in keeps the upstream credential in the organization's infrastructure behind a short-lived token, and federation leaves CI no stored key.[^gw-why][^gha-wif]
- **D2-KILL has a documented path on the gateway route only.** A user disabled in the IdP loses gateway access at the next failed refresh, within `ttl_hours`, and `/logout` sends a best-effort revocation from v2.1.275, while the gateway server advertises no revocation endpoint, so JWT secret rotation forces its sessions out.[^gw-enforced]
- **D2-AUTHZ finds no agent to name.** Permission rules name tools and paths, a subagent's `tools` and `disallowedTools` bind a tool list to a subagent definition, and a gateway policy binds settings to an IdP group of people, so no rule keys on an agent identity.[^sub-fields][^gw-avail]
- **D2-MUTUAL covers two legs.** The client certificate authenticates the model connection, which cloud sessions leave to the hosting environment by ignoring the variables in a settings file.[^nc-mtls] OAuth covers the remote MCP servers that use it.[^mcp-auth] Calls from shell commands, hooks and stdio MCP servers rest on each called service's own authentication.
- **D2-AUDIT covers grant changes.** Server-managed changes reach the compliance API, and `ConfigChange` hooks record local settings-file changes.[^sms-audit][^hk-config] Identity and credential events come from the identity provider's and the device-management system's own logs.

## 3 — D3 Control and Least-Agency

In [[agentic-ai-security-cmm-d3-control-least-agency|D3]] the harness is the decision point itself: it resolves a permission policy and returns permit or deny before each tool call runs. The sheet maps 14 of D3's 23 criteria. The other 9, L4 non-transferable sessions and delegation-chain validation, L5 approval tokens, anomaly-driven step-up and cryptographic segregation of duties, and the four L5+ research items, have no control in the Claude Code pages read.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | Per-agent tool allowlist | `permissions.allow` with `--tools`, and `dontAsk` where no human approves | The resolved rule set, and the session's tool list | native |
| L2 | HITL on destructive actions defined informally | `permissions.ask` rules, which no mode auto-approves | The ask-rule list, and a prompt under the most permissive mode | native |
| L3 | A PDP outside the model context mediating every tool call | Managed permission rules under `allowManagedPermissionRulesOnly` | The four substitute artifacts listed below | partial |
| L3 | Least-agency action-risk tiering implemented. | `allow`, `ask` and `deny` rules as the auto, confirm and block tiers | The rule set mapped to the four tiers | partial |
| L3 | Each action's risk tier documented. | The managed rule set, versioned where it is authored | A tier register over every built-in tool and MCP server in scope | partial |
| L3 | A decision-rights matrix operationalized in the PDP | None | A matrix naming the approver per action class | external |
| L3 | A synchronous, fail-closed gate. | Start refused on a malformed managed source, and `forceRemoteSettingsRefresh` | The behavior per policy source, and starts with the source corrupted and removed | partial |
| L4 | Four-stage progressive-autonomy promotion with documented criteria | Permission modes as stages, gated by `permissions.disableAutoMode` and `permissions.disableBypassPermissionsMode` | The promotion runbook, and `permission_mode_changed` events | partial |
| L4 | A continuous lethal-trifecta breaker | None native. A stateful `PreToolUse` hook can track the three legs | Hook code and its detection log | external |
| L4 | Time-bounded JIT elevation that auto-reverts. | None | Elevation records from a broker outside the harness | external |
| L4 | Segregation of duties | Cloud push restricted to the session's working branch | Branch protection requiring a second principal to merge | external |
| L4 | Task-scope authorization at invocation. | A `PreToolUse` hook deciding against the declared task scope | Hook code, and a denied out-of-scope call | partial |
| L4 | A session action ledger alongside the per-call decision | `CLAUDE_CODE_SCRIPT_CAPS`, and stateful `PreToolUse` hooks | A ledger sample with a blocked threshold crossing | partial |
| L5 | A deny-by-default policy compiled and reviewed every release with no drift. | `dontAsk` with an explicit allow list, and the fleet digest `managed_settings.resolved_sha256` | The per-release review record, and digest equality across devices | partial |

- **Bash rules match command text, which D3 grades advisory.** Anthropic states that a Bash rule is no security boundary: `Bash(git push *)` stops `git push origin main` and misses `git -C . push origin main`.[^perm-bash] [[guard-canonicalization-gap|Guard Canonicalization Gap]] explains the advisory grade, and the OS sandbox is the harness control independent of command text.[^perm-sbx] The L2 allowlist therefore counts over tool names and MCP tools, and the sheet reads a bare `Bash` entry, which admits every command, as failing it, a question [#331](https://github.com/ag0x00/ai-era/issues/331) leaves open.
- **The allowlist is consulted in every mode and bounds the tool set only in some.** In `auto` the classifier approves calls outside the allow list and entering the mode drops broad rules such as `Bash(*)`,[^pm-classifier] `bypassPermissions` runs everything outside the deny and ask rules,[^pm-bypass] and `acceptEdits` adds file edits and `mkdir`, `touch`, `rm`, `rmdir`, `mv`, `cp` and `sed` inside the working directories to what runs unprompted.[^pm-accept] D3 treats an allowlist an autonomy mode suppresses as no decision point, so the sheet reads the rule set under the most permissive mode the surface offers and the deployment permits, and records the criterion not met under `auto` or `bypassPermissions` unless another control, such as `--tools`, bounds the tool set.[^cli-flags] D3 states this test and the advisory rule outside its criterion lists, a placement [#333](https://github.com/ag0x00/ai-era/issues/333) leaves open, and the sheet applies both at L2. The surfaces offer different modes:
  - Managed `permissions.disableAutoMode` and `permissions.disableBypassPermissionsMode`, set to `"disable"`, remove the two permissive modes in the CLI and the Desktop app, and `permissions.defaultMode` sets only the starting mode.[^as-enforce][^desk-managed][^pm-start]
  - The Desktop app has no `dontAsk` and no equivalent of the tool flags, and shows Bypass permissions only once a Settings toggle on Pro and Max, or organization policy on Team and Enterprise, enables it.[^desk-modes][^desk-cli]
  - Cloud sessions offer Accept edits, Plan and Auto, pre-approve file edits in every mode, and ignore `bypassPermissions` and `dontAsk` from settings files.[^pm-switch][^pm-bypass]
- **Destructive-action classification is a managed rule set, with the classifier as a second gate in auto mode.** The classifier blocks by default, among others, force push, production deploys and migrations, `terraform destroy` and irreversible destruction of files that existed before the session.[^pm-blocks] User intent and `allow` exceptions can clear a `soft_deny` rule, and a `hard_deny` rule blocks unconditionally.[^amc-rules] The managed ask and deny lists, which act before the classifier, are therefore the evidence item's confirm and block tiers.[^amc-scope] `rm` and `rmdir` on a critical path prompt even in `bypassPermissions`, and `acceptEdits` runs them unprompted inside the working directories unless an ask rule names them.[^pm-noauto][^pm-accept] A routine has no mode picker and runs without stopping for approval.[^rt-create] The assessor names which of the HITL clause's three recorded readings ([#328](https://github.com/ag0x00/ai-era/issues/328)) applies, and the ask-rule list is the artifact under each.
- **The four in-process substitute artifacts each have a harness instrument:**
  1. The policy as resolved on an enrolled device, with its scope: the `Setting sources` line in `/status`, and `managed_settings.sources` on `managed_settings_resolved`.[^ms-status][^mon-map]
  2. The record that the model cannot write that scope. The plist and HKLM sources need administrator rights to write.[^as-deliver] The sandbox denies sandboxed commands any write to `.claude` settings files, skills, agents, hooks and `.mcp.json`, with no exemption, including the target of a symlink planted at a protected path.[^sbx-protected] `ConfigChange` hooks audit in-session changes to user, project, local and policy settings files and to skills, and do not run for server-managed, MDM or registry changes.[^hk-config]
  3. A denied action under the most permissive mode the deployment permits, since deny rules block in every mode, `bypassPermissions` included.[^pm-modes]
  4. The documented behavior when the policy source is absent or malformed, which the fail-closed bullet below reads.[^ms-drop]
- **The second artifact leans on the managed-only locks.** `bypassPermissions` allows writes to protected paths, so the model can edit a project `.claude/settings.json`, and native Windows has no sandbox to stop a shell command writing it.[^pm-protected][^sbx-platform] Under `allowManagedPermissionRulesOnly` and `allowManagedHooksOnly` that file holds no permission rule or hook the harness honors.[^ms-managedonly] The audit half needs a `ConfigChange` hook the organization writes, which makes the decision-point row partial.
- **A hook that routes decisions to a policy engine withdraws the substitution for the calls it decides**, which then need the direct-invocation and unreachability tests. A timed-out `command`, `http` or `mcp_tool` hook lets the call continue through the normal permission flow,[^hk-timeout] which in `dontAsk` denies every call outside `permissions.allow` and the read-only set, so the engine's failure becomes a deny.[^pm-dontask]
- **Three records outside the harness narrow the circularity** the protocol records for these artifacts:
  - The managed settings as the device-management system or the admin console holds them.
  - The target system's own log of a denied action, such as the Git host's audit log.
  - The OpenTelemetry events in the organization's store.
- **The four tiers map onto three rule kinds and a hook.** An allow rule, or auto mode's approved set, is the auto tier, an ask rule the confirm tier and a deny rule the block tier, and notify, which has no rule kind, is a `PostToolUse` hook that sends the notification. With sandboxing on and `autoAllowBashIfSandboxed` at its default, a bare `Bash` ask rule stops prompting for sandboxed commands, while content-scoped ask rules keep prompting.[^perm-sbx] `dontAsk` denies an ask-rule call, and from v2.1.259 `--permission-prompts none` denies every prompt in print mode.[^pm-dontask][^cli-flags]
- **The tier register is the organization's record.** The rule set records a tier only for calls a rule names, and other calls take their mode's default, so the register assigns a tier to every built-in and MCP tool in scope.
- **The decision-rights matrix has no field in the harness.** The approval prompt reaches whoever holds the session, a `PermissionRequest` hook can answer it for the user, and a session that cannot show a prompt denies the call when no hook decides.[^hk-permreq] A hook forwarding the request to an approval service moves the matrix into that service, graded on its own record against [[decision-rights|Decision Rights for AI Agents]].
- **The fail-closed gate is not met as written.** D3 reads an in-process enforcement point through its policy input, and one that proceeds when that source is absent or malformed has not met the level. A managed file, plist or HKLM value that does not parse refuses start, even beside a valid source, and an absent one is no failure.[^ms-drop] Schema-invalid entries are skipped with a warning, and listed keys fall back to a stricter value.[^ms-failclosed] Server-managed delivery proceeds on cached or absent settings unless `forceRemoteSettingsRefresh` is set, which exits when no fresh policy arrives and covers first launch from an endpoint-managed source.[^sms-failclosed] A shell that exports a provider variable skips the fetch and starts without waiting, and a gateway-signed-in session exits when the gateway is unreachable.[^sms-platform][^sms-failclosed][^gw-enforced] Until [#342](https://github.com/ag0x00/ai-era/issues/342) decides whether device management, fleet digests and `forceRemoteSettingsRefresh` can stand in for the absent case, the sheet records the criterion not met, which holds every Claude Code estate below D3 L3.
- **Live observation at L3 draws its three records from the harness.** For the protocol's live trace, decision and gate fire, a denied call emits `tool_decision` with `source` set to `config`, and a prompt appears as a `claude_code.tool.blocked_on_user` span in the beta traces.[^mon-decision][^mon-traces]
- **Progressive autonomy has stages and no promotion record.** The modes run from Manual through `acceptEdits` and `auto` to `dontAsk` and `bypassPermissions`, each change emits `permission_mode_changed` with `from_mode`, `to_mode` and `trigger`, and the promotion criteria are the organization's document.[^pm-modes][^mon-map]
- **Task scope is a hook the organization writes, and the trifecta breaker needs more.** Claude Code holds no task scope, so the organization declares one and writes the `PreToolUse` hook that denies an out-of-scope call and logs it as an agent-escape event, and the harness enforces the deny.[^hk-pretool] The trifecta row stays external, because D3 evaluates its external-communications leg across every permitted dependency, which the harness does not hold.
- **JIT elevation has no native control.** A grant given at a prompt lasts for the session.
- **The session ledger has one native cap.** `CLAUDE_CODE_SCRIPT_CAPS` limits how often named scripts run per session when the environment scrub is set, and Anthropic calls it defense in depth that misses fan-out through `xargs` or `find -exec`,[^env-vars] so a stateful `PreToolUse` hook the organization writes carries the cumulative threshold.
- **Deny-by-default runs in the CLI only,** in `dontAsk`, the one mode that denies what no rule allows.[^desk-modes][^pm-dontask] Digest equality across the fleet shows no drift, and the per-release review record is the organization's.

## 4 — D4 Runtime and Guardrails

[[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] grades screening on the model's input and output, interception of the agent loop, and the sandbox around high-risk actions. Claude Code supplies lifecycle hooks and a shell sandbox, and Anthropic's service runs the screening. The sheet maps 11 of D4's 21 criteria. The other 10, reasoning-layer auditing, groundedness, post-hoc tool narrowing, four L5 items on platform-wide enforcement, language coverage, leak scanning and budgets, and the three L5+ items, have no control in the Claude Code pages read.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | A default safety filter runs on input | Anthropic's request-side safety classifier, and `switchModelsOnFlag` | `api_refusal` events carrying a `category` | partial |
| L2 | a content-safety classifier on output | Vendor-side refusal on the response stream | `api_refusal` events, and a vendor statement of the classifier's scope | partial |
| L3 | Input canonicalization ahead of the classifier. | None stated in the pages read | A canonicalizing gateway's configuration | external |
| L3 | In-path injection detection and agent-loop interception. | `UserPromptSubmit`, `PreToolUse` and `PostToolUse` hooks, and in auto mode a server-side probe over tool results | Hook configuration, and a crafted tool output on the path a tool reads | partial |
| L3 | Output content-safety classifier with its data-class scope recorded. | `MessageDisplay` hook redaction, which changes the display only | A data-class scope record for a scanner outside the harness | external |
| L3 | Per-task sandbox on high-risk-tier actions, meeting the Exchange specification. | `sandbox.enabled`, `failIfUnavailable`, `allowUnsandboxedCommands: false`, `sandbox.credentials` | The resolved sandbox configuration, and a denied write outside the workspace | partial |
| L4 | Code-safety static analysis | The Security Guidance plugin, `/security-review`, and Code Review (research preview) | Static-analysis findings from CI on agent-authored changes | external |
| L4 | Injection-resistant context boundaries. | WebFetch runs in a separate context window | A trust-segmentation record covering every untrusted source | partial |
| L4 | Semantic tool validation on high-impact calls | The auto-mode classifier, on Claude Sonnet 5 by default | Judge findings recording the judge's model family | partial |
| L4 | Human approval on configured high-blast-radius operations, mandatory even where every automated check passes. | `permissions.ask` rules, and a hook's `"ask"` decision | A prompt raised on a call the classifier would allow | native |
| L5 | Enumeration of the services every sandbox shares | Documentation naming the proxy, MCP servers and hooks as outside the sandbox | The enumeration, with the residual risk per shared service | partial |

- **The L2 filter is vendor-side, and the organization reads it in its own telemetry.** Claude Code emits `api_refusal` when a request ends in a refusal, and with `OTEL_LOG_TOOL_DETAILS=1` its `category` names `cyber`, `bio`, `frontier_llm` or `reasoning_extraction`.[^mon-refusal] `switchModelsOnFlag` chooses whether the session switches model or pauses when a safety classifier flags a request.[^sr-switch] The pages read state no scope for either classifier.
- **Injection screening runs inside Anthropic's service, and hooks intercept the loop.** In auto mode a server-side probe flags suspicious content in incoming tool results before Claude reads it, and tool results are stripped from the classifier's own requests.[^pm-classifier] Subagent reports arrive under a header marking them as subagent output, after a scan that marks instruction-shaped text.[^sub-scan] No event in the pages read records the probe's flag, so detection stays attested, and D4's indirect test plants the crafted file on the path a tool reads. A `PreToolUse` hook exiting with code 2 blocks the call as a `"deny"` decision does.[^hk-pretool]
- **Output scanning by data class has only a display hook.** `MessageDisplay` can redact API keys or internal hostnames from what renders on screen, while the transcript and what Claude sees keep the original, and a failed hook displays the original.[^hk-msgdisplay] The criterion rests on a scanner at an inference gateway or the collector.
- **The sandbox covers the shell.** It applies to Bash, PowerShell and Monitor commands and their child processes.[^perm-sbx] Read, Edit and Write use the permission system, and MCP servers and command hooks are separate processes that run unconstrained on the host.[^sbx-scope][^se-bash] The whole-process `@anthropic-ai/sandbox-runtime`, a beta research preview, confines every tool, hook and MCP server in the session.[^se-runtime] [[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]] tracks it, and [[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]] records an exfiltration through the unsandboxed Read tool while the shell boundary held. The L5 enumeration starts from this scope, with the organization's written residual risk per shared service.
- **Computer use runs outside the sandbox.** A research preview in the Desktop app and the CLI, it exists only on Pro and Max plans and is off by default, and once enabled it acts on the actual desktop outside the sandboxed Bash tool, so a device signed in to a personal plan can hold an action path no sandbox row confines.[^desk-cu][^cu-cli]
- **Three settings make the sandbox mandatory.** `sandbox.enabled`, `failIfUnavailable` and `allowUnsandboxedCommands: false` enable it, refuse start when it cannot initialize, and remove the model's unsandboxed retry.[^sbx-enforce] Commands typed at the `!` prompt still run outside it, and native Windows has no sandbox.[^sbx-enforce][^sbx-platform]
- **The Exchange specification asks for more than the sandbox confines.** The default read policy covers the whole machine, including `~/.aws/credentials` and `~/.ssh/`, until `sandbox.credentials` or `denyRead` removes them.[^sbx-fs] Protected paths confine agent self-configuration.[^sbx-protected] The sandboxing pages read state no compute or wall-clock ceiling for a local session, and cloud sessions run in isolated VMs.[^sec-cloud]
- **The trust table answers the start-ordering question for repository content.** D4 reads Claude Code's start ordering for repository content from the documentation cited here, and records it unanswerable for the other harnesses the wiki tracks. In a folder never trusted, a `claude -p` or SDK run uses the settings-file hooks, the `env` block and helpers such as `apiKeyHelper`, and connects `.mcp.json` servers without asking,[^perm-trust] since trust verification is off under `-p`.[^sec-add] `--setting-sources user`, `--bare`, `--settings '{"disableAllHooks": true}'` and `disabledMcpjsonServers` keep repository configuration out of such a run.[^perm-trust]
- **Code-safety analysis is advisory inside the harness.** The [[security-guidance-plugin|Security Guidance Plugin]] reviews each edit, turn and commit and blocks none of them.[^sg-limits] Code Review, a research preview on Team and Enterprise plans, posts findings and always completes its check run neutral, so it never blocks a merge.[^cr-note][^cr-check] A warn-only authoring-time instrument carries no D4 level alone, so static analysis in CI carries the criterion.
- **Isolated context covers fetched pages.** WebFetch uses a separate context window to keep hostile page content out of the main conversation,[^sec-add] which covers fetched web content alone of the untrusted sources the criterion segments.
- **The classifier is a same-family judge.** Auto mode's classifier runs on Claude Sonnet 5 by default, and a model Anthropic configures server-side takes precedence.[^pm-auto] D4 wants an independent judge from another model family, so the classifier informs the decision and leaves that clause open.
- **Human approval stays mandatory where a rule says so.** No mode auto-approves an explicit ask rule: the call prompts, or `dontAsk` denies it, apart from the bare `Bash` rule the D3 tier bullet names.[^pm-noauto][^pm-dontask] A hook's `"ask"` forces a prompt in auto mode, where the classifier can still deny the call and cannot approve it silently.[^hk-pretool]
- **Rule DR-003 caps D4 at D3's raw score.**

## 5 — D5 Egress and Network

[[agentic-ai-security-cmm-d5-egress-network|D5]] grades the agent's outbound path. Claude Code's sandbox sends shell traffic through a proxy enforcing a hostname allowlist, and every other leg takes its own path: the model connection, MCP servers, hooks, the in-process fetch tool and the Desktop app's Browser pane. The sheet maps 9 of D5's 22 criteria. The other 13 grade inter-agent channels, per-call token exchange, MCP CVE feeds, orchestrator and mesh topology, and the L5+ items, which sit in the network around the harness.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | Each agent has an outbound destination allowlist (DNS- or proxy-level) | `sandbox.network.allowedDomains` with `allowManagedDomainsOnly`, `WebFetch(domain:...)` rules, the cloud environment's access level, `disableBrowserExternalNavigation` | The resolved allowlist, and a host blocked for a sandboxed command | partial |
| L2 | the egress reach of each allowlisted internal destination recorded | None | A reach record per allowlisted host, package mirrors included | external |
| L3 | Gateway in path. | A Claude apps gateway on the model leg, required through a managed `forceLoginMethod` of `"gateway"` | Gateway configuration for each leg: model, MCP, shell and fetch | partial |
| L3 | MCP calls brokered with OAuth/JWT identity authorization. | OAuth for remote MCP servers, `allowedMcpServers` by `serverUrl`, `managedMcpServers` | Authentication configuration per MCP server | partial |
| L3 | Tool fingerprinting active. | None | A fingerprint store in a gateway | external |
| L3 | The resolver closed as an independent channel | None stated for local sessions. Cloud sessions keep a DNS-level audit trail | Resolver configuration on the runner or VM | external |
| L3 | Per-agent ceilings on outbound API call volume and tool-invocation count | Gateway spend limits per user or group | A ceiling enforced at the gateway | partial |
| L4 | Rug-pull / tool-poisoning detection active | None native | Detection findings from a gateway product | external |
| L5 | SSRF and direct-egress paths are closed at the network layer | A custom proxy through `httpProxyPort` and `socksProxyPort`, `network.tlsTerminate` (experimental) | A network-layer egress policy for the host and every process on it | partial |

- **The sandbox allowlist covers shell commands and their children.** Claude Code pre-allows no domain, prompts on a new one, and blocks instead under `strictAllowlist`, which a repository's settings files cannot set, or a managed `allowManagedDomainsOnly`.[^sbx-network]
- **The Desktop Browser pane is a further leg.** Outside Auto and Bypass permissions a domain allowlist check precedes navigation to a new site, and safety classifiers review Claude's write actions on external pages in every mode. The pane follows the organization's Claude in Chrome site allowlist and blocklist, `browserExternalPageTools` set to `"disabled"` removes Claude's tools from external pages, and `disableBrowserExternalNavigation` set to `true` blocks external navigation for users and Claude.[^desk-browser][^desk-managed]
- **Cloud sessions bypass the environment allowlist on four paths.** GitHub traffic through its proxy, enabled MCP connectors, the hosts of the environment's API credentials, and Claude Code's own calls to the Anthropic API all skip it.[^ce-levels] No organization-level allowlist reaches every member's environments, and an Owner standardizes one through an organization-shared environment.[^ce-allow]
- **A hostname allowlist is a misconfiguration control.** The built-in proxy decides from the client-supplied hostname without terminating TLS, so code in the sandbox can use domain fronting past a broad entry such as `github.com`.[^sbx-limits] The experimental `network.tlsTerminate` terminates TLS for credential masking without content filtering, and Anthropic directs a stronger threat model to a custom proxy that inspects traffic.[^sbx-limits][^sbx-proxy] D5 grades a hostname-only allowlist at L2 to L3 for the coding shape, TLS-terminating inspection comes before L4, and L5's closure needs a network-layer policy over every process on the host.
- **The reach record belongs to each allowlisted host.** The cloud environment's default Trusted level allows package registries, GitHub and cloud SDKs,[^ce-levels] and D5's L2 reach clause records what each of them can itself reach, a package mirror with open internet access being the case D5 records.
- **The gateways sit on the model leg.** A Claude apps gateway sits between the client and the model provider and disables WebSearch on its sessions,[^gw-why][^gw-avail] and Claude Code honors `forceLoginMethod` set to `"gateway"` only from a managed source on the machine, and a cloud-provider environment variable skips the gateway sign-in.[^sr-login] Another LLM gateway sits in the same position,[^as-data] and D5's L3 gateway criterion also covers tools and MCP servers, so either meets it in part.
- **MCP brokering stops at per-server OAuth.** Claude Code connects to each MCP server itself and supports OAuth 2.0 for remote servers, so the broker the criterion names is a gateway outside the harness, except that a cloud session's proxy authenticates to delivered connectors.[^mcp-auth] A managed allowlist by `serverUrl`, or a deployed `managed-mcp.json`, bounds which remote servers the agent reaches, and server-managed settings cannot carry that file, delivering servers through `managedMcpServers` without its exclusive control.[^mmcp-excl] In the Desktop app's local and SSH sessions the claude.ai connectors arrive in-process, beyond every MCP setting, and only the organization's `blocked` connector tool controls reach them.[^mcp-connectors]
- **Call ceilings sit on the model leg as well.** A Claude apps gateway's per-user and per-group spend limits bound model calls.[^gw-avail] The gateway pages read describe no tool-invocation ceiling, and `CLAUDE_CODE_SCRIPT_CAPS` runs inside the agent, where D5 wants enforcement at the gateway.[^env-vars]
- **Agent teams carry an inter-agent channel on local files.** The experimental agent teams, disabled by default, exchange messages through a mailbox file per agent under `~/.claude/teams/`.[^teams] D5's A2A criterion, as read on 2026-09-24, names network transport and no file channel, so an estate enabling agent teams records the channel as one no criterion grades.
- **Tool definitions move at runtime.** Claude Code accepts `list_changed` notifications and refreshes a server's tools without reconnecting.[^mcp-dynamic] Rug-pull detection and fingerprinting sit in a gateway that compares definitions over time.
- **Rule DR-001 caps D5 at D2's raw score.**

## 6 — D6 Data, Memory and RAG

[[agentic-ai-security-cmm-d6-data-rag|D6]] treats a coding agent's repository as the retrieved corpus, at the grain of repository and branch grants. The sheet maps 10 of D6's 30 criteria. Two of the other 20 are records the organization keeps over repository and branch grants, the L2 first assessment of what the agent reaches and for whom and the L3 oversharing remediation, and the rest, such as ingest poisoning scans, groundedness, validation corpora, trust-weighted retrieval and label-aware response gating, grade controls no harness setting implements.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | A retrieval names its origin in the terms the corpus indexes | `tool_input` on `tool_result` with `OTEL_LOG_TOOL_DETAILS=1`, and `vcs.*` attributes with `OTEL_METRICS_INCLUDE_REPOSITORY` | An event sample naming repository, ref and path | partial |
| L2 | the extensions that widen what the agent retrieves are reviewed by a person before use | Project MCP servers held until the folder is trusted, `allowedMcpServers`, `strictKnownMarketplaces` | A review record per MCP server, plugin and editor extension | partial |
| L2 | a classification scheme covers the reachable corpus on paper, at the grain the authorization layer grants on | `Read` deny rules and sandbox `denyRead` over the register's excluded paths | The repository register, with classification and excluded paths | partial |
| L3 | Per-source trust attribution. | Subagent reports marked as subagent output, and `InstructionsLoaded` | A trust label per source entering the context | partial |
| L3 | RAG-injection scanning. | The auto-mode server-side probe over incoming tool results | A probe flag on a planted repository file | partial |
| L3 | Hashing for cognitive file integrity over identity and system-prompt files. | Managed `claudeMd`, `ConfigChange` and `InstructionsLoaded` hooks, sandbox protected paths | A hash baseline over the instruction set, and a drift alert | partial |
| L3 | The augmentation store inside the classification and protection scope | `cleanupPeriodDays`, `desktopSessionCleanupPeriodDays`, `autoMemoryDirectory`, `CLAUDE_CODE_SKIP_PROMPT_HISTORY` | The retention setting, and the store's access and encryption record | partial |
| L3 | Corpus and fine-tuning scope. | Working directories, `permissions.additionalDirectories`, `permissions.blockReadsOutsideWorkingDirectories` | The corpus scope decision per repository | partial |
| L3 | Answer-time access enforcement resolves the asking principal's read authorization. | The developer's grants locally, and the GitHub proxy's repository scope in the cloud | The trim for two developers with different grants | partial |
| L4 | Memory partitioning, write authorization, and provenance. | Auto memory per project directory, `autoMemoryEnabled`, a subagent's `memory` scope | A memory write log with writer, session and partition | partial |

- **A retrieval event names the repository and the path.** `OTEL_METRICS_INCLUDE_REPOSITORY=true`, from v2.1.269, stamps events with the `vcs.*` identity of the `origin` remote, and `OTEL_LOG_TOOL_DETAILS=1` adds the path in `tool_input`.[^mon-repo][^mon-toolresult] On the events, only a successful `git commit` records a ref, so a hook the organization writes logs the checked-out ref.
- **Extension review has a trust gate for MCP servers.** From v2.1.196 a cloned repository cannot approve its own `.mcp.json` servers, which wait for the workspace trust dialog.[^mcp-trust] Under `claude -p` in a folder never trusted they connect without asking, as D4's start-ordering bullet sets out. Editor extensions follow the IDE's own policy, and the review record is the organization's.
- **Excluded paths are enforced for file tools and sandboxed commands.** Read and Edit deny rules cover the built-in file tools and the file commands Claude Code recognizes in Bash, and miss a script that opens files itself.[^perm-read] The sandbox's default read policy covers the whole machine until `denyRead` narrows it, and `permissions.blockReadsOutsideWorkingDirectories`, from v2.1.257, makes recognized file-reading Bash commands prompt even in auto and bypass modes.[^sbx-fs][^pm-noauto]
- **Trust attribution covers two sources.** Subagent reports arrive marked, as the D4 injection bullet sets out, and `InstructionsLoaded` records which instruction files load and why.[^hk-instr] File contents, command output, fetched pages and MCP results carry no trust label in the pages read, so their labels come from outside the harness.
- **Injection scanning of repository content runs only in auto mode.** The probe the D4 injection bullet describes records no event, so a deployment outside auto mode, or one needing an inspectable record, scans repository content with a tool outside the harness.[^pm-classifier]
- **Rules-file integrity has enforcement pieces, and the hash comes from the organization.** The core page's file set for Claude Code is CLAUDE.md, the settings file at every scope, the managed-settings directory, hooks and MCP manifests, and a managed CLAUDE.md, deployed as a file or through the `claudeMd` key, cannot be excluded.[^mem-managed][^mem-exclude] `ConfigChange` hooks and the protected paths the D3 second artifact describes cover the settings files, skills, agents, hooks and `.mcp.json`, and neither reaches a CLAUDE.md at the repository root.[^hk-config][^sbx-protected] The baseline and drift alert that [[cognitive-file-integrity|Cognitive File Integrity (CFI)]] names come from a CI job or a hook the organization writes, and the same baseline evidences D8's L4 item.
- **The augmentation store is plaintext on disk.** Transcripts and history are not encrypted at rest, and operating-system file permissions are their only protection, so encryption comes from outside the harness.[^cd-plain] Local transcripts stay under `~/.claude/projects/` for 30 days by default, adjustable with `cleanupPeriodDays`, and `desktopSessionCleanupPeriodDays` gives Desktop transcripts an age limit.[^du-retention][^cd-plain] Auto memory lives at `~/.claude/projects/<project>/memory/`, and a repository-chosen `autoMemoryDirectory` is honored only under workspace trust.[^mem-auto]
- **Answer-time enforcement follows where the session runs.** A local session reads the developer's clone under the developer's own grants, and a cloud session reaches only its attached repositories through the GitHub proxy, as the D2 cloud bullet sets out. A CI run with no asking principal, such as a scheduled one, reads what the GitHub App installation or the passed `github_token` reaches, the non-human scope D6 records, and an `@claude` run sets that reach against the triggering user's grants.
- **Memory is partitioned, and write authorization sits outside the harness.** Auto memory is partitioned per repository, and a subagent's `memory` field scopes its persistent memory to user, project or local.[^mem-auto][^sub-fields] The pages read describe no per-write authorization or provenance record, which D6's L4 item names.

## 7 — D7 Observability and Detection

[[agentic-ai-security-cmm-d7-observability|D7]] grades whether every agent action is reconstructable from telemetry the organization holds. Claude Code emits OpenTelemetry metrics and events, and traces in beta, as a raw event stream only, and Anthropic assigns anomaly detection, baselining, correlation and alerting to the SIEM.[^mon-map] The sheet maps 10 of D7's 20 criteria. The other 10 grade SIEM analytics, posture management and eval programs built on the stream, and the L5+ research items.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | A tool-call audit log records action history with user attribution. | `CLAUDE_CODE_ENABLE_TELEMETRY=1`, `OTEL_LOGS_EXPORTER`, a managed `OTEL_EXPORTER_OTLP_ENDPOINT` | `tool_result` and `tool_decision` events carrying `user.email` in the organization's store | native |
| L3 | OpenTelemetry `gen_ai.*` spans from every agent | `CLAUDE_CODE_ENHANCED_TELEMETRY_BETA=1` with `OTEL_TRACES_EXPORTER` (beta) | A trace sample with its GenAI convention attributes | partial |
| L3 | Per-agent identity multiplexing of logs | `session.id`, `user.account_uuid`, the gateway's IdP identity, `OTEL_RESOURCE_ATTRIBUTES` | Every sampled event traced to a session and a human | partial |
| L3 | A minimum action-log schema over tool calls and memory writes. | `tool_result` and `tool_decision` with `OTEL_LOG_TOOL_DETAILS=1` | A schema check that includes the rollback reference | partial |
| L3 | Sandbox escape indicators forwarded. | The `dangerouslyDisableSandbox` parameter on Bash tool events | Denied-syscall and blocked-host records in the backend | partial |
| L3 | Ingest poisoning-scan alerts forwarded. | None | The D6 L3 verdict on an ingest scan | not applicable |
| L3 | The semantic-convention version pinned. | None | A collector transform pinning a stated version | external |
| L4 | Per-agent behavioral baselines and drift detection | The event stream only | A SIEM baseline per session population, and its alert | external |
| L4 | Control-state change monitored alongside agent behavior | `permission_mode_changed`, `managed_settings_resolved`, `hook_registered`, `ConfigChange` hooks | An alert on a mode escalation or a policy digest change | partial |
| L4 | Log integrity verified under adversarial conditions. | A managed OTLP endpoint that removes developer-set endpoints | A test showing a session cannot suppress its own events | partial |

- **Identity on the events depends on the route.** With a Claude account the events carry `user.email`, `user.account_uuid` and `organization.id`, and on a gateway `user.id` is the IdP subject. With an API key or a cloud-provider route only `user.id` and `session.id` exist, and `OTEL_RESOURCE_ATTRIBUTES` set through managed settings attaches the user.[^mon-attr] The multiplexing criterion is not met on the local shapes, because it asks for an agent identity and `session.id` names only a session, and elsewhere it follows D2-IDENTITY.
- **The spans are Claude Code's, with GenAI attributes.** Each prompt starts a `claude_code.interaction` root span with `claude_code.llm_request`, `claude_code.tool` and, under detailed beta tracing, `claude_code.hook` children, and a tool span carries `claude_code.tool.blocked_on_user` and `claude_code.tool.execution` children.[^mon-traces] Attributes such as `gen_ai.system`, `gen_ai.request.model` and `gen_ai.tool.call.id` follow the OpenTelemetry GenAI semantic convention, and the documentation names no convention version, so the pin is a collector-side mapping.[^mon-traces] Traces are beta, so a trace-based verdict records the date they entered production.
- **Content capture is off by default.** Prompts, tool details, tool content and raw API bodies each need their own opt-in, and without `OTEL_LOG_TOOL_DETAILS` an event redacts an MCP tool's name to `"mcp_tool"`.[^mon-common][^mon-mcp] The action-log schema therefore needs `OTEL_LOG_TOOL_DETAILS=1` in managed settings, and the pages read describe no rollback reference on a tool event.
- **Sandbox escape indicators are thin.** Bash tool events carry the `dangerouslyDisableSandbox` parameter, and the monitoring pages read name no event for a denied syscall or blocked host,[^mon-toolresult] so violations reach the backend through a collector on the host, if at all.
- **Ingest-scan forwarding is not applicable where no ingest scan runs.** D7 records a missing scan as a D6 L3 finding.
- **Managed settings pin the destination and leave the selectors per key.** A managed `OTEL_EXPORTER_OTLP_*` variable removes conflicting developer-set endpoints and credentials, while the exporter selectors follow normal precedence, so a developer's setting can disable a signal unless managed settings set the selectors too.[^mon-lock] Spawned processes receive no `OTEL_*` variables.[^mon-admin] The adversarial test disables export from user settings, project settings and a Bash command, and confirms the events still arrive.
- **Control-state signals are native.** `permission_mode_changed` reports mode escalation, `managed_settings_resolved` the sources, the policy helper's health and the digest, and `plugin_installed` and `hook_registered` additions to the configuration,[^mon-map][^mon-hookreg] beside the two policy-change sources the D2-AUDIT bullet names.
- **A cloud-provider route removes the analytics dashboard, and OpenTelemetry stays.** OpenTelemetry metrics work on every provider,[^fa-every] and on those routes D7 asks the assessor to confirm that the SIEM receives session-correlated prompt, tool-result and decision events before crediting the level.
- **Rule DR-002 caps D7 at D2's raw score.**

## 8 — D8 Supply Chain and AI-BOM

[[agentic-ai-security-cmm-d8-supply-chain|D8]] sets the coding shape's highest target, L4, because slopsquatting lands on the dependency channel, and its commentary puts the harness configuration tree, as [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] describes it, in scope. The sheet maps 11 of D8's 28 criteria. The other 17 sit with the organization's records and build pipeline: the L2 vendor model cards and documentation register, the build-time AI-BOM, build provenance, per-workload write scoping on artifact repositories, the producer-only items, and the L5 and L5+ programs.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | Model and library versions tracked | `service.version` on every export, `requiredMinimumVersion`, `requiredMaximumVersion`, `availableModels` | A fleet version report, and the resolved model allowlist | native |
| L2 | an AI-component inventory exists (models, skills, MCP servers, framework deps, third-party AI APIs, and acquired datasets and corpora) with source / version / hash / maintainer / date | `plugin_installed`, `plugin_loaded`, `skill_activated`, `mcp_server_connection` and `hook_registered` events | An inventory export reconciled against the event stream | partial |
| L3 | dependency/SCA scanning runs in CI with lockfile enforcement (the slopsquatting control) | None at install time. A sandbox allowlist can restrict installs to a registry mirror | The CI scanning configuration, and a blocked install | external |
| L3 | acquired models pass a recorded pre-execution assessment before they are loaded, or a Safetensors-only load policy is enforced | None | The trust basis, recorded | not applicable |
| L3 | skills and MCP servers pass registry-provenance plus a pre-install scan | `strictKnownMarketplaces`, `blockedMarketplaces`, `allowManagedMcpServersOnly`, `strictPluginOnlyCustomization`, `disableSideloadFlags` | The resolved allowlists, and a pre-install scan record | partial |
| L3 | suppliers of models, datasets, hosting and abilities are assessed against a recorded dimension set | None | A supplier assessment for Anthropic and each marketplace | external |
| L3 | the org's own agent artifacts are signed | Git `sha` pins and archive `sha256` digests, which check integrity without a signature | A signing record for internal plugins, skills and hooks | external |
| L4 | Every acquired and produced artifact is signature-verified at load/deploy | Archive `sha256` verification refuses a mismatched install | A verification log per artifact | partial |
| L4 | a runtime AI-BOM reconciles against the build/deploy AI-BOM (drift detection) | The same load events, and `managed_settings.resolved_sha256` | A reconciliation report | partial |
| L4 | cognitive-file integrity baselines cover identity / system-prompt files | As D6's hashing row | A baseline and its drift alert | partial |
| L4 | AI-dependency disclosures are consumed and acted on within an SLA | `requiredMinimumVersion` raises the fleet's floor after an advisory | An advisory-to-floor timeline | partial |

- **Version tracking is in the telemetry.** Every metric and event carries `service.version`, the running Claude Code version or, for a Desktop Code tab session, the Desktop app's, and `requiredMinimumVersion` and `requiredMaximumVersion` refuse start outside an approved range.[^mon-service][^as-enforce]
- **Fleet inventory comes from the event stream.** D8's commentary names fleet inventory, which harnesses, versions and MCP servers run for which human, as evidence its L3 and L4 criteria omit. `plugin_installed` records `plugin.name`, `marketplace.name` and `marketplace.is_official`, the names for a third-party marketplace only under `OTEL_LOG_TOOL_DETAILS=1`, and `mcp_server_connection` records `is_plugin`.[^mon-map][^mon-plugin] Aggregated per device, these events and the settings digest are the runtime half of the reconciliation.
- **Typosquat defense sits on the dependency channel.** The pages read describe no check on the package itself at install time. By default the auto-mode classifier blocks downloading and executing code, such as `curl | bash`, and routing a package install around an internal registry to a public one, and an ask rule on an install command matches text and is advisory.[^pm-blocks][^perm-bash] A sandbox allowlist naming only an internal registry mirror bounds where a sandboxed install fetches from, leaving the control to the mirror's own policy.
- **The model load has no instance to assess.** The model is first-party and hosted.
- **Plugin and MCP provenance is allowlisting with integrity pins.** A git-based plugin source pins an exact commit with `sha`, and an archive source's `sha256` makes Claude Code refuse an install whose download does not match.[^pmk-sources][^pmk-archive] The plugin pages read describe no signature check on a plugin, marketplace or MCP server. Managed marketplace restrictions limit which sources users can add, and from v2.1.277 a managed `strictKnownMarketplaces` that fails validation is enforced as an empty allowlist.[^pmk-restrict][^ms-failclosed] `disableSideloadFlags` rejects the CLI flags that sideload plugins, agents and MCP servers for one run, and `strictPluginOnlyCustomization` blocks skills, agents, hooks and MCP servers from user and project sources.[^as-enforce] On Team and Enterprise plans only admins add claude.ai connectors, which in Desktop sessions bypass the MCP allowlist, as the D5 MCP bullet sets out.[^mcp-claudeai]
- **The pre-install scan is the organization's.** Anthropic states that plugins and marketplaces are highly trusted components that execute arbitrary code with the user's privileges.[^dp-sec] The allowlist records who approved a source, and the scan runs before the source enters it.
- **The supplier assessment starts from Anthropic's documents**, the Trust Center reports the D1 matrix bullet names among them. Zero Data Retention disables cloud sessions and Remote Control where it is enabled,[^zdr] and a BAA extends to Claude Code traffic where ZDR is enabled.[^lc-baa]
- **Disclosure response raises a floor that fails open.** Both version keys drop an invalid value by design.[^ms-failclosed] The [[claude-code|Claude Code]] entity page tracks the advisories a floor responds to.

## 9 — D9 Operations and Human Factors

[[agentic-ai-security-cmm-d9-operations|D9]] grades the operating model around the agents. For a coding deployment its load-bearing question is approval fatigue, which D9 names as the mechanism that converts an interactive deployment into an unattended one. The sheet maps 11 of D9's 30 criteria. The other 19 are runbooks, roles, policies, drills, disclosures and closed-loop programs kept outside the harness, and three measures it does not produce: basic system-prompt protection at L2, and at L4 the involvement measure and the separation of benign from adversarial drift.

| Level | Criterion | Claude Code control | Evidence | Coverage |
|---|---|---|---|---|
| L2 | A runbook documents guardrail fail behavior | Documented fail modes for hook timeouts, malformed managed sources and `failIfUnavailable` | Runbook entries citing each fail mode by version | partial |
| L2 | decommission and credential rotation on owner departure | Gateway deprovisioning through the IdP, `/logout`, and `claude project purge` | A departure checklist executed for a leaver | partial |
| L2 | HITL-queue monitoring | `tool_decision` events, and `claude_code.tool.blocked_on_user` spans (beta) | A queue view in the organization's store | native |
| L3 | Guardrail latency and cost measured per agent | `claude_code.hook` spans (detailed beta), `hook_execution_complete`, `claude_code.cost.usage` | Per-session latency and cost, with a tested fail mode | partial |
| L3 | An orphan reaper on a scheduled SLA | Gateway session expiry within `ttl_hours`, cloud VMs reclaimed after inactivity | A reaper log covering sessions and credentials | partial |
| L3 | HITL approval-rate and queue-age tracked. | `tool_decision` `source` values `user_temporary`, `user_permanent`, `user_reject` and `user_abort` | Approval-rate and queue-age series | native |
| L3 | A system-prompt trip-wire deployed | None | The canary's alert path to the SIEM | external |
| L3 | High-risk approval categories defined in advance | `permissions.ask` categories, and `tool_decision` records carrying the user's identity | The category definition, and a tamper-evident approval extract | partial |
| L4 | Quantitative HITL-fatigue indicators tracked | The same decision events and prompt spans | Rubber-stamp rate and queue p95, per quarter | partial |
| L4 | The oversight path exercised adversarially | None native | An oversight red-team report, and enforced approval-rate limits | external |
| L4 | Model versions pinned in production. | `availableModels` with `enforceAvailableModels`, and provider pins such as `ANTHROPIC_DEFAULT_OPUS_MODEL` | The resolved model allowlist per device | partial |

- **The fail modes are documented per mechanism.** A timed-out `command`, `http` or `mcp_tool` hook lets the call proceed through the permission flow.[^hk-timeout] A malformed managed source refuses start.[^ms-drop] `failIfUnavailable` refuses start when the sandbox cannot initialize.[^sbx-enforce] Auto mode pauses and resumes prompting after 3 consecutive or 20 total classifier blocks, thresholds that are not configurable.[^pm-fallback]
- **Reaping differs by route.** A gateway session whose user the IdP disables expires within `ttl_hours`, and cloud session VMs are reclaimed after inactivity.[^gw-enforced][^sec-cloud] A self-hosted session token stays valid until its expiry.[^shi-scope] `claude project purge` deletes the state Claude Code holds for one project on a device.[^cd-clear]
- **The decision source separates configuration from people.** `tool_decision` reports `config` for a decision a rule, mode or earlier session grant made, `hook` for a hook's decision, and `user_temporary`, `user_permanent`, `user_reject` or `user_abort` for a person's answer at a prompt.[^mon-decision] The rubber-stamp rate is the share of prompts a person accepted, and queue age the duration of `claude_code.tool.blocked_on_user` spans in the beta traces.[^mon-traces] A `config` decision does not name the rule that matched, so rule attribution comes from the policy export.
- **Guardrail cost and latency arrive per session.** `hook_execution_complete` carries the wall-clock duration of the hooks on an event, and `claude_code.cost.usage` the session's cost, so the organization aggregates both per agent.[^mon-hookdone][^mon-metrics] Because a timed-out hook proceeds, a high-risk action fails closed only under an ask or deny rule or `dontAsk`, and the organization runs the fail-mode test.
- **A system-prompt trip-wire has no native control.** A canary string in the managed CLAUDE.md is an organization artifact.
- **The approval record needs a tamper-evident store.** D9's record holds the request, the parameters shown, the approver's identity and authentication method, the decision and the outcome, of which `tool_decision` with `OTEL_LOG_TOOL_DETAILS=1` supplies the parameters, identity and decision, and `tool_result` the outcome.[^mon-decision][^mon-toolresult] The authentication method and the tamper evidence come from the organization's store.
- **Approval volume changes the shape.** D9 reads a falling prompt count against a rising action count as a shape change, which rising `config` decisions against falling decisions from people, alongside `permission_mode_changed` events into `auto` or `bypassPermissions`, record in the organization's own data.[^mon-map]
- **The oversight path has no native rate limit.** Auto mode's fallback thresholds count classifier blocks, and the criterion's session-level approval limits against each session's baseline come from the SIEM, through an alert or a hook.
- **Model pinning covers the session model and reaches the classifier's only through `availableModels`.** `availableModels` with `enforceAvailableModels` constrains the model picker and the default, and cloud-provider deployments pin versions through `ANTHROPIC_DEFAULT_OPUS_MODEL` and its siblings.[^as-enforce][^tpi-pin] The auto-mode classifier runs on Sonnet 5 unless Anthropic configures another model server-side, and falls back to a model the session sets where `availableModels` excludes Sonnet 5.[^pm-auto]

## Adaptation by deployment shape

- **Interactive local.** Every row applies, and the D9 approval rows carry the most weight, because the developer is the approver and prompt volume is the documented pressure on the shape.
- **Sandboxed autonomous local.** The D4 sandbox and D5 allowlist rows carry the weight. Set the three D4 sandbox settings in managed settings, and wrap the whole process in the sandbox runtime where MCP servers or hooks handle untrusted content.[^se-runtime]
- **Delegated cloud.** A one-repository session reads the repository's `.claude/settings.json`, hooks and permission rules included, so that file is policy and belongs under the D1 review record.[^set-cloud] A routine takes the CI-runner posture on the D3 and D9 approval rows.
- **CI-runner agent.** Keep repository hooks, `env` and `.mcp.json` servers out of the run with the flags the D4 start-ordering bullet lists. Run in `dontAsk` or with `--permission-prompts none`, so a call that would prompt is denied.[^pm-dontask][^cli-flags] Authenticate through federation with a custom GitHub App, and deliver policy through the `settings` input or a managed file on the runner.[^gha-wif] The fail-closed clause holds the run below D3 L3 under any of the five readings [#334](https://github.com/ag0x00/ai-era/issues/334) records, and [[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]] is the case the shape is assessed against.
- **Fleet and parallel.** The D1 digest, the D8 load events and the D9 decision stream carry the population view, `disableAgentView` turns off background sessions, and `processWrapper` prefixes the background supervisor and its workers with a required corporate launcher.[^as-enforce]

## Scoring and reporting

Score each domain with the protocol's rubric, whose score of 3 adds operational ID tagging and live observation, which static configuration never satisfies. The D3 section names the live records. An estate where no call can prompt, such as one running every session in `dontAsk`, records the gate fire not applicable, while one where an ask rule or a mode's default prompts and no fire is produced has not met the requirement.

Resolve effective scores and the headline under [[agentic-ai-security-cmm-dependency-rules|CMM: Effective-Score Dependency Rules]], whose active rules the D4, D5 and D7 sections apply. On Claude Code's controls alone, an interactive-local estate reports a typical L2, a weakest D2 at L1 raw with D5 and D7 at L1 effective, and a strongest L2 across the other eight domains.

The ceilings bound the harness's contribution and grade nothing. [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] grades a Claude Code estate for a regulated bank on the same domains, and a re-grade reads its evidence against these rows.

## See also

- [[securing-agentic-coding|Securing Agentic Coding]]: the same controls by reference-architecture plane.
- [[cmm-known-limitations|CMM Known Limitations (current state)]]: the recorded readings behind the in-process decision point.
- [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]]: the pattern behind the cloud session's GitHub proxy.

## Notes

[^web-intro]: [Anthropic — Use Claude Code in the cloud](https://code.claude.com/docs/en/claude-code-on-the-web), 2026. The surfaces that start a cloud session, and the terminal, IDE and Desktop Local sessions that run on the user's own machine.
[^desk-modes]: [Anthropic — Desktop application, "Choose a permission mode"](https://code.claude.com/docs/en/desktop#choose-a-permission-mode), 2026. Settings files shared with the CLI, `dontAsk` in the CLI only, the modes cloud sessions offer, and Bypass permissions enablement per plan.
[^desk-env]: [Anthropic — Desktop application, "Environment configuration"](https://code.claude.com/docs/en/desktop#environment-configuration), 2026. The Local, Cloud, SSH and WSL environments.
[^desk-managed]: [Anthropic — Desktop application, "Managed settings"](https://code.claude.com/docs/en/desktop#managed-settings), 2026. The Desktop managed keys, the managed settings each kind of session reads, and connectors delivered outside every MCP setting.
[^as-wsl]: [Anthropic — Set up Claude Code for your organization, "WSL sessions in Claude Code Desktop"](https://code.claude.com/docs/en/admin-setup#wsl-sessions-in-claude-code-desktop), 2026. Policy resolution inside WSL, `wslInheritsWindowsSettings`, WSL sessions off on organization-managed devices, and Windows-side endpoint sensors.
[^desk-cli]: [Anthropic — Desktop application, "CLI flag equivalents"](https://code.claude.com/docs/en/desktop#cli-flag-equivalents), 2026. Desktop as interactive only, with no per-session equivalent of the tool flags.
[^web-env]: [Anthropic — Use Claude Code in the cloud, "Cloud environments"](https://code.claude.com/docs/en/claude-code-on-the-web#cloud-environments), 2026. One set of environments for every entry point, and organization-level environments for Claude Tag channel sessions.
[^shi-beta]: [Anthropic — Verify session identity in self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments-identity), 2026. Self-hosted environments are in public beta on Team and Enterprise plans.
[^rc-vs]: [Anthropic — Continue local sessions from any device with Remote Control, "Remote Control vs cloud sessions"](https://code.claude.com/docs/en/remote-control#remote-control-vs-cloud-sessions), 2026. A Remote Control session executing on the user's machine.
[^set-prec]: [Anthropic — Settings files and precedence, "Settings precedence"](https://code.claude.com/docs/en/settings#settings-precedence), 2026. The order of settings scopes, and list keys merging across them.
[^as-deliver]: [Anthropic — Set up Claude Code for your organization, "Decide how settings reach devices"](https://code.claude.com/docs/en/admin-setup#decide-how-settings-reach-devices), 2026. The four managed sources, their priority, and the tamper resistance of each.
[^ms-combine]: [Anthropic — Deploy managed settings, "How Claude Code combines managed sources"](https://code.claude.com/docs/en/managed-settings#how-claude-code-combines-managed-sources), 2026. `managedSourcesBehavior` and the default `"first-wins"` behavior.
[^set-cloud]: [Anthropic — Settings files and precedence, "Settings in cloud sessions"](https://code.claude.com/docs/en/settings#settings-in-cloud-sessions), 2026. Which settings files reach a cloud session.
[^tag]: [Anthropic — Claude Tag](https://code.claude.com/docs/en/claude-tag), 2026. `@Claude` in Slack channels as the organization's shared identity, on Team and Enterprise plans.
[^gha-params]: [Anthropic — Claude Code GitHub Actions, "Action parameters"](https://code.claude.com/docs/en/github-actions#action-parameters), 2026. The `settings`, `claude_args` and `github_token` inputs.
[^dc-policy]: [Anthropic — Development containers, "Enforce organization policy"](https://code.claude.com/docs/en/devcontainer#enforce-organization-policy), 2026. Claude Code reading `/etc/claude-code/managed-settings.json` on Linux, copied into place from a Dockerfile.
[^gha-wif]: [Anthropic — Claude Code GitHub Actions, "Set up for an organization"](https://code.claude.com/docs/en/github-actions#set-up-for-an-organization), 2026. Workload identity federation inputs and the `id-token: write` permission.
[^sms-platform]: [Anthropic — Configure server-managed settings, "Platform availability"](https://code.claude.com/docs/en/server-managed-settings#platform-availability), 2026. The credentials that trigger the fetch, and provider variables that skip it.
[^fa-admin]: [Anthropic — Feature availability, "Admin and analytics"](https://code.claude.com/docs/en/feature-availability#admin-and-analytics), 2026. Analytics, server-managed settings and ZDR availability by provider, and the gateway note on `ANTHROPIC_BASE_URL`.
[^sms-req]: [Anthropic — Configure server-managed settings, "Requirements"](https://code.claude.com/docs/en/server-managed-settings#requirements), 2026. The Teams or Enterprise plan and the Owner or Primary Owner role.
[^sms-audit]: [Anthropic — Configure server-managed settings, "Audit logging"](https://code.claude.com/docs/en/server-managed-settings#audit-logging), 2026. Settings-change events through the compliance API or audit log export.
[^pm-start]: [Anthropic — Choose a permission mode, "Which mode a session starts in"](https://code.claude.com/docs/en/permission-modes#which-mode-a-session-starts-in), 2026. The built-in starting mode per plan, provider and run type.
[^pm-switch]: [Anthropic — Choose a permission mode, "Switch permission modes"](https://code.claude.com/docs/en/permission-modes#switch-permission-modes), 2026. The modes each interface offers, the VS Code extension's starting-mode order, and the modes cloud sessions offer.
[^pm-auto]: [Anthropic — Choose a permission mode, "Eliminate permission prompts with auto mode"](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode), 2026. The classifier's default model, server-side model precedence, and the fallback where `availableModels` excludes Sonnet 5.
[^fa-summary]: [Anthropic — Feature availability, "Summary by provider"](https://code.claude.com/docs/en/feature-availability#summary-by-provider), 2026. What each provider route lacks, and server-managed settings for Console keys of Team or Enterprise organizations.
[^mon-attr]: [Anthropic — Monitoring, "Attribute actions to users"](https://code.claude.com/docs/en/monitoring-usage#attribute-actions-to-users), 2026. The identity attributes per route, and the absence of a separate service account.
[^as-enforce]: [Anthropic — Set up Claude Code for your organization, "Decide what to enforce"](https://code.claude.com/docs/en/admin-setup#decide-what-to-enforce), 2026. The enforcement surfaces and the setting keys that drive each.
[^du-provider]: [Anthropic — Data usage, "Default behaviors by API provider"](https://code.claude.com/docs/en/data-usage#default-behaviors-by-api-provider), 2026. Metrics, error reports and feedback reports off by default on the cloud-provider routes.
[^gw-why]: [Anthropic — Claude apps gateway, "Why Claude apps gateway"](https://code.claude.com/docs/en/claude-apps-gateway#why-claude-apps-gateway), 2026. Upstream credentials held in the organization's infrastructure, and deprovisioning within the session lifetime.
[^gw-ci]: [Anthropic — Claude apps gateway, "CI pipelines and remote machines"](https://code.claude.com/docs/en/claude-apps-gateway#ci-pipelines-and-remote-machines), 2026. No service-token flow for unattended pipelines.
[^mob-signin]: [Anthropic — Claude Code on mobile, "Get the app"](https://code.claude.com/docs/en/mobile#get-the-app), 2026. Cloud sessions and Remote Control requiring a claude.ai account.
[^env-set]: [Anthropic — Environment variables, "In settings files"](https://code.claude.com/docs/en/env-vars#in-settings-files), 2026. Variables under a settings file's `env` key, applied however `claude` is launched.
[^sbx-enforce]: [Anthropic — Configure the sandboxed Bash tool, "Enforce sandboxing with managed settings"](https://code.claude.com/docs/en/sandboxing#enforce-sandboxing-with-managed-settings), 2026. `failIfUnavailable`, `allowUnsandboxedCommands` and the `!` prompt.
[^sbx-network]: [Anthropic — Configure the sandboxed Bash tool, "Network isolation"](https://code.claude.com/docs/en/sandboxing#network-isolation), 2026. Domain restrictions, `strictAllowlist` and the managed lockdown.
[^sbx-fs]: [Anthropic — Configure the sandboxed Bash tool, "Filesystem isolation"](https://code.claude.com/docs/en/sandboxing#filesystem-isolation), 2026. Default write and read scope.
[^ms-status]: [Anthropic — Deploy managed settings, "Check that a policy is in force"](https://code.claude.com/docs/en/managed-settings#check-that-a-policy-is-in-force), 2026. The `Setting sources` line in `/status`, and `Skipped sources` from v2.1.242.
[^ms-drop]: [Anthropic — Deploy managed settings, "Find entries Claude Code dropped"](https://code.claude.com/docs/en/managed-settings#find-entries-claude-code-dropped), 2026. Refusal to start on an unparseable source, absent sources, skipped invalid entries, and `claude doctor`.
[^mon-msr]: [Anthropic — Monitoring, "Managed settings resolved event"](https://code.claude.com/docs/en/monitoring-usage#managed-settings-resolved-event), 2026. `managed_settings.resolved_sha256` under `OTEL_LOG_MANAGED_SETTINGS=1`.
[^ms-managedonly]: [Anthropic — Deploy managed settings, "Keys only a managed source can set"](https://code.claude.com/docs/en/managed-settings#keys-only-a-managed-source-can-set), 2026. The managed-only keys, including the four locks.
[^sbx-widen]: [Anthropic — Configure the sandboxed Bash tool, "Keep developers from widening the policy"](https://code.claude.com/docs/en/sandboxing#keep-developers-from-widening-the-policy), 2026. Merging array keys, and `excludedCommands` without a managed lock.
[^amc-scope]: [Anthropic — Configure auto mode, "Where the classifier reads configuration"](https://code.claude.com/docs/en/auto-mode-config#where-the-classifier-reads-configuration), 2026. The `autoMode` scopes, additive lists, and managed deny rules acting first.
[^ms-failclosed]: [Anthropic — Deploy managed settings, "Keys that fail closed"](https://code.claude.com/docs/en/managed-settings#keys-that-fail-closed), 2026. The stricter fallback per key, the merging HTTP-hook lists, and the version keys failing open.
[^ms-dev]: [Anthropic — Deploy managed settings, "What a developer can change"](https://code.claude.com/docs/en/managed-settings#what-a-developer-can-change), 2026. Local administrator rights over the managed source, and managed settings binding Claude Code only.
[^sms-sec]: [Anthropic — Configure server-managed settings, "Security considerations"](https://code.claude.com/docs/en/server-managed-settings#security-considerations), 2026. Server-managed settings as a client-side control, per tampering scenario.
[^sec-found]: [Anthropic — Security, "Security foundation"](https://code.claude.com/docs/en/security#security-foundation), 2026. The SOC 2 Type 2 report and ISO 27001 certificate in the Trust Center.
[^shi-format]: [Anthropic — Verify session identity in self-hosted environments, "Token format"](https://code.claude.com/docs/en/self-hosted-environments-identity#token-format), 2026. The `sk-ant-cc-` ES256 token, and the `sk-ant-si-` prefix and separate key set for Anthropic-hosted sessions.
[^ce-github]: [Anthropic — Configure cloud environments, "GitHub proxy"](https://code.claude.com/docs/en/cloud-environments#github-proxy), 2026. Credential swap, push protection and repository scope.
[^gha-app]: [Anthropic — Claude Code GitHub Actions, "GitHub App permissions"](https://code.claude.com/docs/en/github-actions#github-app-permissions), 2026. The shared app's permission set, and the custom-app alternative.
[^gha-trigger]: [Anthropic — Claude Code GitHub Actions, "Who can trigger runs"](https://code.claude.com/docs/en/github-actions#who-can-trigger-runs), 2026. The write-access and human-actor checks.
[^shi-proves]: [Anthropic — Verify session identity in self-hosted environments, "What the token proves"](https://code.claude.com/docs/en/self-hosted-environments-identity#what-the-token-proves), 2026. What a valid session token establishes, and that it does not prove which process presents it.
[^shi-act]: [Anthropic — Verify session identity in self-hosted environments, "The act chain"](https://code.claude.com/docs/en/self-hosted-environments-identity#the-act-chain), 2026. `act.sub` and `act.attested_by.sub` in the RFC 8693 delegation chain.
[^shi-claims]: [Anthropic — Verify session identity in self-hosted environments, "Claims reference"](https://code.claude.com/docs/en/self-hosted-environments-identity#claims-reference), 2026. `exp` carries a four-hour default lifetime and an eight-hour maximum.
[^shi-scope]: [Anthropic — Verify session identity in self-hosted environments, "Scope derived credentials"](https://code.claude.com/docs/en/self-hosted-environments-identity#scope-derived-credentials), 2026. Offline verification, no revocation feed, and scoping of derived credentials.
[^mon-toolresult]: [Anthropic — Monitoring, "Tool result event"](https://code.claude.com/docs/en/monitoring-usage#tool-result-event), 2026. Tool parameters and `tool_input`, `git_commit_id` and the commit's `vcs.ref.head.*` identity, and the `dangerouslyDisableSandbox` parameter.
[^sr-attr]: [Anthropic — All settings, "attribution"](https://code.claude.com/docs/en/settings-reference#attribution), 2026. Attribution lines passed to Claude, and managed values outranking CLAUDE.md.
[^env-vars]: [Anthropic — Environment variables](https://code.claude.com/docs/en/env-vars#variables), 2026. `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` and `CLAUDE_CODE_SCRIPT_CAPS`.
[^auth-token]: [Anthropic — Authentication, "Generate a long-lived token"](https://code.claude.com/docs/en/authentication#generate-a-long-lived-token), 2026. The long-lived token `claude setup-token` issues.
[^gw-enforced]: [Anthropic — Claude apps gateway, "What's enforced on developers"](https://code.claude.com/docs/en/claude-apps-gateway#whats-enforced-on-developers), 2026. Startup exit when unreachable, deprovisioning within `ttl_hours`, and sign-out revocation.
[^sub-fields]: [Anthropic — Create custom subagents, "Frontmatter reference"](https://code.claude.com/docs/en/sub-agents#supported-frontmatter-fields), 2026. The `tools`, `disallowedTools` and `memory` frontmatter fields.
[^gw-avail]: [Anthropic — Claude apps gateway, "Availability and limitations"](https://code.claude.com/docs/en/claude-apps-gateway#availability-and-limitations), 2026. Policy by IdP group, WebSearch disabled, OIDC only.
[^nc-mtls]: [Anthropic — Enterprise network configuration, "mTLS authentication"](https://code.claude.com/docs/en/network-config#mtls-authentication), 2026. The client certificate variables, ignored from a settings file in cloud sessions.
[^mcp-auth]: [Anthropic — Connect Claude Code to tools via MCP, "Authenticate with remote MCP servers"](https://code.claude.com/docs/en/mcp#authenticate-with-remote-mcp-servers), 2026. OAuth 2.0 for remote servers, and the cloud session's proxy authenticating to delivered connectors.
[^hk-config]: [Anthropic — Hooks reference, "ConfigChange"](https://code.claude.com/docs/en/hooks#configchange), 2026. The configuration sources that fire the hook and those that do not.
[^perm-bash]: [Anthropic — Configure permissions, "What a Bash rule doesn't match"](https://code.claude.com/docs/en/permissions#bash-rule-limits), 2026. Bash rules as text matching, with examples that escape them.
[^perm-sbx]: [Anthropic — Configure permissions, "How permissions interact with sandboxing"](https://code.claude.com/docs/en/permissions#how-permissions-interact-with-sandboxing), 2026. Sandbox scope, and `autoAllowBashIfSandboxed`.
[^pm-classifier]: [Anthropic — Choose a permission mode, "How the classifier evaluates actions"](https://code.claude.com/docs/en/permission-modes#how-the-classifier-evaluates-actions), 2026. The decision order, dropped broad rules, stripped tool results, and the server-side probe.
[^pm-bypass]: [Anthropic — Choose a permission mode, "Skip all checks with bypassPermissions mode"](https://code.claude.com/docs/en/permission-modes#skip-all-checks-with-bypasspermissions-mode), 2026. What the mode disables and what still prompts.
[^pm-accept]: [Anthropic — Choose a permission mode, "Auto-approve file edits with acceptEdits mode"](https://code.claude.com/docs/en/permission-modes#auto-approve-file-edits-with-acceptedits-mode), 2026. The file edits and filesystem commands the mode approves inside the working directories.
[^cli-flags]: [Anthropic — CLI reference](https://code.claude.com/docs/en/cli-reference#cli-flags), 2026. `--permission-prompts`, `--tools`, `--bare` and `--setting-sources`.
[^pm-blocks]: [Anthropic — Choose a permission mode, "What the classifier blocks by default"](https://code.claude.com/docs/en/permission-modes#what-the-classifier-blocks-by-default), 2026. The default block list.
[^amc-rules]: [Anthropic — Configure auto mode, "Override the block and allow rules"](https://code.claude.com/docs/en/auto-mode-config#override-the-block-and-allow-rules), 2026. `hard_deny`, `soft_deny` and `allow` precedence.
[^pm-noauto]: [Anthropic — Choose a permission mode, "Actions no mode auto-approves"](https://code.claude.com/docs/en/permission-modes#actions-no-mode-auto-approves), 2026. Ask rules, critical-path removals, and reads outside the working directories.
[^rt-create]: [Anthropic — Automate work with routines, "Create a routine"](https://code.claude.com/docs/en/routines#create-a-routine), 2026. Routines running as cloud sessions with no permission-mode picker and no stop for approval.
[^mon-map]: [Anthropic — Monitoring, "Map security questions to events"](https://code.claude.com/docs/en/monitoring-usage#map-security-questions-to-events), 2026. Security signals mapped to events, and the SIEM's responsibility for analytics.
[^sbx-protected]: [Anthropic — Configure the sandboxed Bash tool, "Protected paths"](https://code.claude.com/docs/en/sandboxing#protected-paths), 2026. Write denial over configuration paths, with no exemption.
[^pm-modes]: [Anthropic — Choose a permission mode, "Available modes"](https://code.claude.com/docs/en/permission-modes#available-modes), 2026. The six modes, and deny rules blocking in every mode.
[^pm-protected]: [Anthropic — Choose a permission mode, "Protected paths"](https://code.claude.com/docs/en/permission-modes#protected-paths), 2026. Protected-path writes per mode, and the protected directories and files.
[^sbx-platform]: [Anthropic — Configure the sandboxed Bash tool, "Platform and tool compatibility"](https://code.claude.com/docs/en/sandboxing#platform-and-tool-compatibility), 2026. macOS, Linux and WSL2 supported, native Windows not.
[^hk-timeout]: [Anthropic — Hooks reference, "Timeouts"](https://code.claude.com/docs/en/hooks#timeouts), 2026. Timed-out `PreToolUse` hooks letting the call continue.
[^pm-dontask]: [Anthropic — Choose a permission mode, "Allow only pre-approved tools with dontAsk mode"](https://code.claude.com/docs/en/permission-modes#allow-only-pre-approved-tools-with-dontask-mode), 2026. What `dontAsk` runs and denies.
[^hk-permreq]: [Anthropic — Hooks reference, "PermissionRequest"](https://code.claude.com/docs/en/hooks#permissionrequest), 2026. Deciding permission requests on the user's behalf, and the deny when no hook decides.
[^sms-failclosed]: [Anthropic — Configure server-managed settings, "Enforce fail-closed startup"](https://code.claude.com/docs/en/server-managed-settings#enforce-fail-closed-startup), 2026. `forceRemoteSettingsRefresh`, and its effect from an endpoint-managed source.
[^mon-decision]: [Anthropic — Monitoring, "Tool decision event"](https://code.claude.com/docs/en/monitoring-usage#tool-decision-event), 2026. The `decision` and `source` attributes and their values.
[^mon-traces]: [Anthropic — Monitoring, "Traces (beta)"](https://code.claude.com/docs/en/monitoring-usage#traces-beta), 2026. The beta span hierarchy and its GenAI semantic-convention attributes.
[^hk-pretool]: [Anthropic — Hooks reference, "PreToolUse decision control"](https://code.claude.com/docs/en/hooks#pretooluse-decision-control), 2026. The four decisions, their precedence, exit code 2, and `"ask"` in auto mode.
[^mon-refusal]: [Anthropic — Monitoring, "API refusal event"](https://code.claude.com/docs/en/monitoring-usage#api-refusal-event), 2026. The refusal event and its `category` values.
[^sr-switch]: [Anthropic — All settings, "switchModelsOnFlag"](https://code.claude.com/docs/en/settings-reference#switchmodelsonflag), 2026. The response when a safety classifier flags a request.
[^sub-scan]: [Anthropic — Create custom subagents, "Subagent output scanning"](https://code.claude.com/docs/en/sub-agents#subagent-output-scanning), 2026. The scan and header applied to a subagent's report, from v2.1.210.
[^hk-msgdisplay]: [Anthropic — Hooks reference, "MessageDisplay"](https://code.claude.com/docs/en/hooks#messagedisplay), 2026. Display-only replacement, and fallback to the original text.
[^sbx-scope]: [Anthropic — Configure the sandboxed Bash tool, "Scope"](https://code.claude.com/docs/en/sandboxing#scope), 2026. Built-in file tools under the permission system.
[^se-bash]: [Anthropic — Choose a sandbox environment, "Sandboxed Bash tool"](https://code.claude.com/docs/en/sandbox-environments#sandboxed-bash-tool), 2026. MCP servers and command hooks running unconstrained on the host.
[^se-runtime]: [Anthropic — Choose a sandbox environment, "Sandbox runtime"](https://code.claude.com/docs/en/sandbox-environments#sandbox-runtime), 2026. Whole-process isolation, as a beta research preview.
[^desk-cu]: [Anthropic — Desktop application, "Let Claude use your computer"](https://code.claude.com/docs/en/desktop#let-claude-use-your-computer), 2026. Computer use as a research preview on Pro and Max plans, off by default, outside the sandboxed Bash tool.
[^cu-cli]: [Anthropic — Let Claude use your computer from the CLI](https://code.claude.com/docs/en/computer-use), 2026. The CLI's computer use as a research preview on Pro and Max plans, off by default, acting on the actual desktop outside the sandboxed Bash tool.
[^sec-cloud]: [Anthropic — Security, "Cloud execution security"](https://code.claude.com/docs/en/security#cloud-execution-security), 2026. Isolated VMs, audit logging and reclamation after inactivity.
[^perm-trust]: [Anthropic — Configure permissions, "What runs before you trust a folder"](https://code.claude.com/docs/en/permissions#what-runs-before-you-trust-a-folder), 2026. Repository content used before trust, and the flags that keep it out.
[^sec-add]: [Anthropic — Security, "Additional safeguards"](https://code.claude.com/docs/en/security#additional-safeguards), 2026. Trust verification disabled under `-p`, and isolated context windows for web fetch.
[^sg-limits]: [Anthropic — Catch security issues as Claude writes code, "Review independence and limits"](https://code.claude.com/docs/en/security-guidance#review-independence-and-limits), 2026. No layer of the plugin blocks writes or commits.
[^cr-note]: [Anthropic — Code Review](https://code.claude.com/docs/en/code-review), 2026. Research preview on Team and Enterprise, unavailable under ZDR.
[^cr-check]: [Anthropic — Code Review, "Check run output"](https://code.claude.com/docs/en/code-review#check-run-output), 2026. The neutral check-run conclusion.
[^desk-browser]: [Anthropic — Desktop application, "Browse external sites"](https://code.claude.com/docs/en/desktop#browse-external-sites), 2026. Safety classifiers on external write actions in every mode, the domain allowlist check outside Auto and Bypass permissions, and the organization's controls on external browsing.
[^ce-levels]: [Anthropic — Configure cloud environments, "Access levels"](https://code.claude.com/docs/en/cloud-environments#access-levels), 2026. The four network levels, and the paths that bypass them.
[^ce-allow]: [Anthropic — Configure cloud environments, "Allow specific domains"](https://code.claude.com/docs/en/cloud-environments#allow-specific-domains), 2026. Per-environment allowlists, and no organization-level list.
[^sbx-limits]: [Anthropic — Configure the sandboxed Bash tool, "Security limitations"](https://code.claude.com/docs/en/sandboxing#security-limitations), 2026. Hostname-based filtering, domain fronting, and the experimental `network.tlsTerminate`.
[^sbx-proxy]: [Anthropic — Configure the sandboxed Bash tool, "Custom proxy configuration"](https://code.claude.com/docs/en/sandboxing#custom-proxy-configuration), 2026. `httpProxyPort` and `socksProxyPort`.
[^sr-login]: [Anthropic — All settings, "forceLoginMethod"](https://code.claude.com/docs/en/settings-reference#forceloginmethod), 2026. The `"gateway"` value, honored only from a managed source on the machine.
[^as-data]: [Anthropic — Set up Claude Code for your organization, "Review data handling"](https://code.claude.com/docs/en/admin-setup#review-data-handling), 2026. A gateway between developers and the provider, and the Claude apps gateway's per-request audit log.
[^mmcp-excl]: [Anthropic — Control MCP server access for your organization, "Exclusive control with managed-mcp.json"](https://code.claude.com/docs/en/managed-mcp#exclusive-control-with-managed-mcp-json), 2026. The exclusive server set, and `managedMcpServers` for server-managed delivery.
[^mcp-connectors]: [Anthropic — Connect Claude Code to tools via MCP, "How connectors reach Claude Code"](https://code.claude.com/docs/en/mcp#how-connectors-reach-claude-code), 2026. The settings that govern claude.ai connectors in terminal, cloud and Desktop sessions.
[^teams]: [Anthropic — Orchestrate teams of Claude Code sessions, "Architecture"](https://code.claude.com/docs/en/agent-teams#architecture), 2026. Experimental agent teams, and the mailbox file per agent.
[^mcp-dynamic]: [Anthropic — Connect Claude Code to tools via MCP, "Dynamic tool updates"](https://code.claude.com/docs/en/mcp#dynamic-tool-updates), 2026. `list_changed` refreshes of a server's tools.
[^mon-repo]: [Anthropic — Monitoring, "Repository attributes"](https://code.claude.com/docs/en/monitoring-usage#repository-attributes), 2026. The `vcs.*` repository identity on metrics and events under `OTEL_METRICS_INCLUDE_REPOSITORY`, from v2.1.269.
[^mcp-trust]: [Anthropic — Connect Claude Code to tools via MCP, "Project server approvals and workspace trust"](https://code.claude.com/docs/en/mcp#project-server-approvals-and-workspace-trust), 2026. Repository approvals ignored in an untrusted folder from v2.1.196.
[^perm-read]: [Anthropic — Configure permissions, "Read and Edit"](https://code.claude.com/docs/en/permissions#read-and-edit), 2026. What Read and Edit deny rules cover and miss.
[^hk-instr]: [Anthropic — Hooks reference, "InstructionsLoaded"](https://code.claude.com/docs/en/hooks#instructionsloaded), 2026. The event fired as instruction files load.
[^mem-managed]: [Anthropic — How Claude remembers your project, "Deploy organization-wide CLAUDE.md"](https://code.claude.com/docs/en/memory#deploy-organization-wide-claude-md), 2026. The managed CLAUDE.md path and the `claudeMd` key.
[^mem-exclude]: [Anthropic — How Claude remembers your project, "Exclude specific CLAUDE.md files"](https://code.claude.com/docs/en/memory#exclude-specific-claude-md-files), 2026. Managed CLAUDE.md files cannot be excluded.
[^cd-plain]: [Anthropic — Explore the .claude directory, "Plaintext storage"](https://code.claude.com/docs/en/claude-directory#plaintext-storage), 2026. Transcripts unencrypted at rest, and the retention controls.
[^du-retention]: [Anthropic — Data usage, "Data retention"](https://code.claude.com/docs/en/data-usage#data-retention), 2026. Local transcript caching for 30 days by default under `cleanupPeriodDays`.
[^mem-auto]: [Anthropic — How Claude remembers your project, "Storage location"](https://code.claude.com/docs/en/memory#storage-location), 2026. The auto memory directory and trust rule for `autoMemoryDirectory`.
[^mon-common]: [Anthropic — Monitoring, "Common configuration variables"](https://code.claude.com/docs/en/monitoring-usage#common-configuration-variables), 2026. The content opt-ins, each disabled by default.
[^mon-mcp]: [Anthropic — Monitoring, "Audit MCP activity"](https://code.claude.com/docs/en/monitoring-usage#audit-mcp-activity), 2026. MCP detail on events, and its redaction without `OTEL_LOG_TOOL_DETAILS`.
[^mon-lock]: [Anthropic — Monitoring, "How managed settings lock the OTLP destination"](https://code.claude.com/docs/en/monitoring-usage#how-managed-settings-lock-the-otlp-destination), 2026. Removal of developer-set endpoints, and exporter selectors following per-key precedence.
[^mon-admin]: [Anthropic — Monitoring, "Administrator configuration"](https://code.claude.com/docs/en/monitoring-usage#administrator-configuration), 2026. Managed telemetry configuration, and `OTEL_*` variables withheld from subprocesses.
[^mon-hookreg]: [Anthropic — Monitoring, "Hook registered event"](https://code.claude.com/docs/en/monitoring-usage#hook-registered-event), 2026. One event per configured hook at session start, naming the hook's source.
[^fa-every]: [Anthropic — Feature availability, "Features available on every provider"](https://code.claude.com/docs/en/feature-availability#features-available-on-every-provider), 2026. OpenTelemetry metrics and the managed settings file on every provider.
[^mon-service]: [Anthropic — Monitoring, "Service information"](https://code.claude.com/docs/en/monitoring-usage#service-information), 2026. `service.version` on every exported metric and event, the Desktop app's version for Code tab sessions.
[^mon-plugin]: [Anthropic — Monitoring, "Plugin installed event"](https://code.claude.com/docs/en/monitoring-usage#plugin-installed-event), 2026. Plugin and marketplace names for a third-party marketplace only under `OTEL_LOG_TOOL_DETAILS=1`.
[^pmk-sources]: [Anthropic — Create and distribute a plugin marketplace, "Plugin sources"](https://code.claude.com/docs/en/plugin-marketplaces#plugin-sources), 2026. The `sha` pin as the effective commit.
[^pmk-archive]: [Anthropic — Create and distribute a plugin marketplace, "Zip archives"](https://code.claude.com/docs/en/plugin-marketplaces#zip-archives), 2026. `sha256` verification and refusal on mismatch.
[^pmk-restrict]: [Anthropic — Create and distribute a plugin marketplace, "Managed marketplace restrictions"](https://code.claude.com/docs/en/plugin-marketplaces#managed-marketplace-restrictions), 2026. Managed limits on marketplace sources.
[^mcp-claudeai]: [Anthropic — Connect Claude Code to tools via MCP, "Use MCP servers from claude.ai"](https://code.claude.com/docs/en/mcp#use-mcp-servers-from-claude-ai), 2026. Admin-only addition of claude.ai connectors on Team and Enterprise plans.
[^dp-sec]: [Anthropic — Discover and install prebuilt plugins through marketplaces, "Security"](https://code.claude.com/docs/en/discover-plugins#security), 2026. Plugins and marketplaces executing arbitrary code with user privileges.
[^zdr]: [Anthropic — Zero data retention, "Features disabled under ZDR"](https://code.claude.com/docs/en/zero-data-retention#features-disabled-under-zdr), 2026. Features blocked when ZDR is enabled.
[^lc-baa]: [Anthropic — Legal and compliance, "Healthcare compliance (BAA)"](https://code.claude.com/docs/en/legal-and-compliance#healthcare-compliance-baa), 2026. The BAA extending to Claude Code traffic under ZDR.
[^pm-fallback]: [Anthropic — Choose a permission mode, "When auto mode falls back"](https://code.claude.com/docs/en/permission-modes#when-auto-mode-falls-back), 2026. The fixed thresholds of 3 consecutive and 20 total blocks.
[^cd-clear]: [Anthropic — Explore the .claude directory, "Clear local data"](https://code.claude.com/docs/en/claude-directory#clear-local-data), 2026. `claude project purge`.
[^mon-hookdone]: [Anthropic — Monitoring, "Hook execution complete event"](https://code.claude.com/docs/en/monitoring-usage#hook-execution-complete-event), 2026. `total_duration_ms` across the hooks matching an event.
[^mon-metrics]: [Anthropic — Monitoring, "Metrics"](https://code.claude.com/docs/en/monitoring-usage#metrics), 2026. `claude_code.cost.usage` as the cost of the session.
[^tpi-pin]: [Anthropic — Enterprise deployment overview, "Pin model versions for cloud providers"](https://code.claude.com/docs/en/third-party-integrations#pin-model-versions-for-cloud-providers), 2026. The `ANTHROPIC_DEFAULT_*_MODEL` pins.
