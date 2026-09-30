---
type: entity
entity_type: product
title: "Claude Cowork"
created: 2026-09-18
updated: 2026-09-30
tags:
  - entities
  - products
  - anthropic
  - cowork
  - desktop-agent
  - productivity-assistant
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
vendor: "Anthropic"
parent_org: "[[anthropic]]"
role: "Anthropic's agentic knowledge-work assistant in Claude Desktop, holding connected local folders, connectors over MCP, a built-in browser and scheduled tasks; governed through organization settings, Enterprise custom roles and managed desktop configuration"
homepage: "https://claude.com/docs/cowork/overview"
related:
  - "[[anthropic|Anthropic]]"
  - "[[claude-code|Claude Code]]"
  - "[[productivity-assistant-deployment-shape]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[indirect-prompt-injection|Indirect Prompt Injection]]"
  - "[[mcp-security|MCP Security]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[shadow-ai|Shadow AI]]"
  - "[[cyera-agent-guardian-release|Cyera Agent Guardian Release]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - "https://claude.com/docs/cowork/overview"
  - "https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans"
  - "https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview"
  - "https://claude.com/docs/cowork/monitoring"
  - "https://support.claude.com/en/articles/13364135-use-claude-cowork-safely"
  - "https://support.claude.com/en/articles/13854387-schedule-recurring-tasks-in-cowork"
  - "https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork"
  - "https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities"
  - "https://support.claude.com/en/articles/13930458-set-up-role-based-permissions-on-enterprise-plans"
  - "https://support.claude.com/en/articles/14477985-monitor-claude-cowork-activity-with-opentelemetry"
  - "https://academy.claude.com/tutorials/claude-cowork-enterprise-administrator-guide"
  - "https://claude.com/docs/cowork/guide/dispatch"
  - "https://claude.com/docs/cowork/3p/overview"
  - "https://claude.com/docs/third-party/claude-desktop/local-access"
  - "https://claude.com/docs/third-party/claude-desktop/mdm"
  - "https://claude.com/blog/compliance-api-cowork-and-claude-code"
verified: 2026-09-30
verified_against: []
verified_findings: 0
verified_note: "Whole-page source read against live Anthropic documentation, including local and remote scheduled-task variants."
---

# Claude Cowork

**Sources:** [Cowork documentation](https://claude.com/docs/cowork/overview) · [Cowork on Team and Enterprise plans](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) · [Cowork architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview) · [Cowork monitoring reference](https://claude.com/docs/cowork/monitoring)

Cowork is [[anthropic|Anthropic]]'s agentic assistant for knowledge work. [Anthropic's documentation](https://claude.com/docs/cowork/overview) states that it runs the same agentic architecture as [[claude-code|Claude Code]] inside Claude Desktop without a terminal, reads and writes local files, divides work across sub-agents, and produces spreadsheets, presentations and formatted documents. [The getting-started article](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) puts it on the paid Pro, Max, Team and Enterprise plans, on macOS and Windows desktop, on the web, on iOS and Android, and in a Chrome side panel, with Enterprise availability on each surface gated on an administrator enabling it.

Four reaches define the surface: files in folders the user connects, connectors that call tools over MCP, a browser that can open sites and fill forms, and scheduled tasks that run remotely or locally according to the selected route. Those four hold all three legs of [[lethal-trifecta|the lethal trifecta]] at once — private data, untrusted content, and a channel that leaves the trust boundary. The desktop placement adds local files and browser paths to the employee's connected work systems.

## Execution model

### Session placement

[The architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview) states that Cowork sessions run in the cloud by default, with the agent loop and code execution on Anthropic's servers, and that a desktop deployment can also run local sessions in which the agent loop runs natively on the device and code execution runs in an isolated virtual machine. A cloud session reaches a local file or the browser by asking the Claude Desktop app, and that reach is limited to the folders the member has connected on the desktop.

### Sandbox boundary

The local session's virtual machine uses the platform hypervisor, [Apple Virtualization.framework on macOS and Hyper-V on Windows](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview), and applies network egress filtering, syscall restrictions and per-session user isolation. The same overview states that the cloud sandbox [cannot reach private, internal, link-local or cloud-metadata addresses, and that egress is enforced outside the sandbox through a proxy the sandbox cannot reconfigure or bypass](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview). Two credential properties follow from that boundary: the sandbox holds only session-scoped tokens that expire within hours, and connector authorization tokens never enter the sandbox because connector calls are made server side. [[agent-sandboxing|Agent Sandboxing]] carries the general comparison of isolation strengths.

### Network egress policy

Cowork respects the organization's existing network egress permissions, set under [Organization settings, Capabilities, Code execution](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans). The same article records the exclusion an assessor has to price: [network egress permissions do not apply to the web fetch or web search tools, or to MCPs, including Claude in Chrome](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans). An egress allowlist therefore constrains code the agent runs and leaves the agent's own retrieval and tool paths outside its scope. Two qualifications sit against that exclusion. [Web fetch runs server side and is limited to search results and to URLs the member has shared](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans), and an owner can [turn web search off for Cowork and Chat, and Claude in Chrome off under its own organization setting](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans), so the excluded channels carry an on-off control rather than an allowlist. The allowlist itself is read when a session starts, so [a change made during an active conversation does not reach that session](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans).

Two Anthropic pages state different scope for that exclusion on a local session. [The architecture overview](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview) places a local session's agent loop on the member's device, lists web fetches and local plugin MCP servers inside that loop, and states that an application-layer permission system gates their access against the organization's network egress settings — the same permissions [the Team and Enterprise article](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) states do not apply to those tools. Neither article reconciles the two statements, and neither states where web search egress originates. An assessor establishes session placement in the fleet before relying on either statement.

## Administrative control surface

### Organization settings

Five settings govern Cowork for Team and Enterprise organizations, each under Organization settings.

| Setting | Effect | Default |
|---|---|---|
| [Enable for your organization](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) | Turns Cowork on or off for every member | On |
| [Run Cowork in the cloud](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) | Governs cloud sessions | On for Team, off for Enterprise |
| [Built-in browser](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) | When off, members cannot open the built-in browser and Claude cannot use it | On for Team; on for Enterprise from 10 September 2026 |
| [Allow "Automatically approve" mode](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) | When off, the mode is absent from the member's selector | On |
| [Allow "Always allow" for connector tools](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) | When off, the per-task blanket approval is greyed out | Off |

The first toggle is organization-wide: [either all members have access or none do](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans). Plugins ride on the same toggle and hold no access setting of their own, and their distribution is governed separately: an owner curates a plugin marketplace that sets each plugin to [installed by default, available, required or not available](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans), and an Enterprise plan overrides that setting per group. The article also states that projects carry no separate admin control, so an owner cannot restrict project creation at the organization level.

### Role-based permissions

Enterprise organizations narrow that all-or-nothing shape with groups and custom roles. [The role-based permissions article](https://support.claude.com/en/articles/13930458-set-up-role-based-permissions-on-enterprise-plans) names Cowork, Cowork in the cloud, Claude Code, connectors, skills, plugins, projects, code execution, memory, web search, Claude in Chrome, and model access with effort-level caps among the capabilities a custom role can grant. The default is closed: a member holding a custom role inherits no organization-enabled capability automatically, and every capability that member needs must be granted explicitly. Roles compose additively across a member's groups. Team plans hold none of this, so [Cowork there stays all-or-nothing](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans).

### Permission modes

[Three modes](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) set how far the agent acts before a human sees the action. Manual pauses for approval. Automatic keeps working and reviews each action for safety before it runs, checking for data exfiltration and prompt injection and blocking what it judges unsafe. Skip removes both the pause and the automatic check. Deletion is carved out of all of this: Cowork requires explicit permission before it permanently deletes a file, and the [safety article](https://support.claude.com/en/articles/13364135-use-claude-cowork-safely) states that the member must select Allow before a deletion runs.

### Connector authorization

Connectors follow a two-gate model. An Owner or Primary Owner [enables a connector for the organization](https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities) before any member can use it, and enabling grants nobody access on its own: each person authenticates individually. Authorization scope is then set twice. Owners can [limit which actions a connected service can take across the organization](https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities), allowing a connector to read from a service while preventing writes, and can set each permission category to Always allow, Needs approval or Blocked. Beneath that, Claude inherits each person's permissions from the connected service, so a record the member cannot open in the source system stays out of reach through the connector. [The enterprise administrator guide](https://academy.claude.com/tutorials/claude-cowork-enterprise-administrator-guide) states that this organization-wide enablement has no per-group equivalent, so an organization needing different connector sets per team is directed to separate organizations or a manual policy layer. [[mcp-security|MCP Security]] holds the threat model for the tool channel itself.

### Filesystem scope

The member selects the folders Cowork can reach, and [the getting-started article](https://support.claude.com/en/articles/13345190-get-started-with-claude-cowork) states that Claude can read and write files only in connected folders. Inside a connected read-write folder the agent reaches every file the member's operating-system account reaches, so the folder selection is the whole of the scope. Anthropic's guidance is to [create a dedicated working folder rather than granting broad access](https://support.claude.com/en/articles/13364135-use-claude-cowork-safely) and to keep backups.

An administrator-side path restriction exists in the third-party deployment mode. [`allowedWorkspaceFolders`](https://claude.com/docs/third-party/claude-desktop/local-access) in managed configuration restricts which paths a user may attach, takes a per-entry `mode` of `rw` or `ro`, and is enforced against the resolved path so that symlinks and parent-directory traversal cannot escape an allowed root. An empty list blocks every attachment while leaving the sandbox scratch space usable. No equivalent key appears on the first-party Team and Enterprise articles cited here.

### Scheduled tasks and background delegation

A member creates a scheduled task in conversation or by hand, choosing name, prompt, approval mode, cadence of hourly, daily, weekly, weekdays or manual, an optional model and an optional folder. [Anthropic's scheduled-task guide](https://support.claude.com/en/articles/13854387-schedule-recurring-tasks-in-cowork) describes remote tasks that fire while the member's computer is asleep and the desktop app is closed, using connectors and files saved to the Claude account. The same guide's manual setup permits an optional local folder and says a task requiring local files or apps runs only locally. Its general statement that scheduled tasks cannot be tied to a computer folder conflicts with that manual option, so an assessor records and tests the actual placement before relying on remote execution or local file access. The article states that Team and Enterprise administrators govern scheduled-task access through the Cowork toggle; the articles cited here name no separate per-task approval workflow, quota or rate limit.

Dispatch is the second unattended path. [The Dispatch reference](https://claude.com/docs/cowork/guide/dispatch) describes a long-running agent that splits an instruction into child tasks, each running as its own Cowork or Code session, and states that a permission prompt a child task raises is forwarded to the member and **automatically denied after ten minutes**, with the task continuing without that action. Dispatch also registers the desktop as a host for tasks started from the Claude mobile app.

## Observability and records

### Telemetry export

[OpenTelemetry export](https://claude.com/docs/cowork/monitoring) is the event-level channel, available on Team and Enterprise plans and configured under Admin settings, Cowork with a collector endpoint, protocol and authentication headers. Settings load at session start, and nothing is exported until an administrator sets the endpoint. Six event types carry the session: `user_prompt`, `assistant_response`, `tool_result`, `api_request`, `api_error` and `tool_decision`. Every event carries the session, organization and user identifiers, the member's email address on first-party deployments, and `workspace.host_paths`, the host directories selected in the desktop app. The tool events are the ones an investigation needs, because each records the tool name, success, duration, the MCP server scope for an MCP tool, and both the decision and its source, which distinguishes a configured allow from a hook decision and from a human approval.

Two Anthropic pages state different defaults for content capture. [The monitoring reference](https://claude.com/docs/cowork/monitoring) states that events carry metadata only by default and that prompt text, model response text and tool details arrive only when an administrator enables them through the `otlpContentCapture` setting. [The help-center article on the same feature](https://support.claude.com/en/articles/14477985-monitor-claude-cowork-activity-with-opentelemetry) states that user prompt content is included in events by default and advises filtering at the collector. An assessor should establish which holds for the app version in the fleet before routing events into a SIEM.

### Compliance API and local storage

[Anthropic's compliance announcement](https://claude.com/blog/compliance-api-cowork-and-claude-code) puts Cowork across the desktop app, web and mobile onto the Compliance API under an Enterprise organization's existing Compliance Access Key, returning transcript content — prompts and responses, tool call content for web and MCP, skills and artifact content — alongside verified user identity, organization, session and message identifiers and timestamps. The same announcement excludes Claude Code on the web, Claude Code through the Claude Platform, and sessions running on Amazon Bedrock, Google Cloud Vertex AI or Microsoft Foundry. Two Anthropic pages state different availability for that channel. The announcement calls the Cowork endpoints generally available; [the enterprise administrator guide](https://academy.claude.com/tutorials/claude-cowork-enterprise-administrator-guide) records Cowork session transcripts on the Compliance API as in beta for Enterprise organizations. An assessor establishes which holds for the organization's own key before relying on the channel.

Two gaps in that record bear directly on retention and legal hold. [The Team and Enterprise article](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) states that a local session stores its conversation history on the member's own computer, that this history falls outside Anthropic's standard data-retention policies, and that an administrator can neither manage nor export it centrally; it also states that local session deletion endpoints are not yet available. Separately, [the enterprise administrator guide](https://academy.claude.com/tutorials/claude-cowork-enterprise-administrator-guide) states that Audit Logs do not yet cover Cowork, leaving the Compliance API and the telemetry export as the two session-activity channels those pages name.

## Managed configuration on third-party deployments

An organization that cannot route inference through Anthropic's first-party products runs [Claude Desktop on third-party providers](https://claude.com/docs/cowork/3p/overview), which sends every model request to Google Cloud's Agent Platform, Amazon Bedrock, Microsoft Foundry, a gateway the organization operates, or the Anthropic API, bundles the web application into the app, and stores conversation history on local disk. Data residency in that mode follows the inference region selected and the physical location of the device where conversations persist.

Configuration then arrives as policy rather than as a console setting. [The MDM deployment reference](https://claude.com/docs/third-party/claude-desktop/mdm) places the profile at `/Library/Managed Preferences/com.anthropic.claudefordesktop.plist` on macOS and under `HKLM\SOFTWARE\Policies\Claude` on Windows, and states that a managed profile setting any key beyond a small group of app-behavior keys takes ownership of the device: the in-app configuration window becomes read-only and locally authored values are ignored. The profile covers which of Cowork, Code and Chat are available, the sandbox egress allowlist, disabled built-in tools, allowed workspace folders, managed MCP servers pushed to all users, whether members may add their own local MCP servers, whether desktop extensions are permitted and whether unsigned ones are rejected, the telemetry collector endpoint, a per-device token cap, and retention periods after which idle Cowork tasks are deleted.

## Residual risk stated by Anthropic

[The safety article](https://support.claude.com/en/articles/13364135-use-claude-cowork-safely) states that two things must be true at the same time for a prompt-injection attack to succeed: Claude can read information outside the trusted boundary, and Claude can take actions that compromise the user. The four reaches above satisfy both. The same article states that Claude working in Automatic mode can read malicious content mid-task and act on those instructions before the member notices, that web content is a primary vector, and that installing a plugin can significantly expand the agent's scope of action because a plugin bundles skills, connectors and sub-agents together. Anthropic states the residual plainly on that article and on [the Team and Enterprise article](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans): the risk of prompt injection attacks is non-zero. See [[indirect-prompt-injection|Indirect Prompt Injection]] for the delivery mechanism.

Discovery of unsanctioned installations sits with the endpoint rather than with the tenant. [[cyera-agent-guardian-release|Cyera Agent Guardian]] names Cowork among the desktop agent harnesses its endpoint component covers, which is the pattern [[shadow-ai|Shadow AI]] records for agents that a suite administrator cannot see.

## Placement in the deployment-shape taxonomy

The [[productivity-assistant-deployment-shape|Productivity Assistant Deployment Shape]] treats Cowork as the desktop-agent placement of an employee productivity assistant, alongside in-suite and enterprise-app placements. Its threat model locates the crossings that matter for this placement. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] records the actual deployment shape in the assessment scope and specifies evidence and applicability rules. The [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] grades the defined deployment by domain; it assigns no fixed score to Cowork or another product class.

The placements differ in where enforcement lives. An in-suite assistant draws on source permissions and tenant policy where those controls cover its actual retrieval and action routes. Cowork adds a connected local filesystem, connectors the member authorizes individually, a browser, and scheduled tasks on remote or local routes. An action can therefore land outside the tenant that granted the data. The organization console, Enterprise role model, connector authorization, and, in third-party deployments, device profile each control part of that path. An assessor tests the effective routes and grades supply controls under [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]] and egress criteria under [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]] against their own evidence.

Cowork acts through the member and inherits that member's source-system permissions on connected paths. For [[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]], the assessor checks the documented vendor-held identity condition before recording any identity criterion as not applicable. Human delegation, ownership, and downstream trace remain assessable under the protocol. The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] locates those decisions at the identity and action boundaries.
