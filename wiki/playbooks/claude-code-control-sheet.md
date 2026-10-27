---
type: playbook
title: "Claude Code Control Sheet"
created: 2026-09-24
updated: 2026-10-27
tags:
  - playbooks
  - claude-code
  - agentic-coding
  - control-mapping
status: developing
scope_axis:
  - sec-of-ai
origin: produced
audience: "Platform and security engineers defining a Claude Code deployment; assessors testing its controls"
related:
  - "[[claude-code|Claude Code]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]"
  - "[[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]"
  - "[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]"
  - "[[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]]"
  - "[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]"
sources:
  - "https://code.claude.com/docs/en/feature-availability"
  - "https://code.claude.com/docs/en/managed-settings"
  - "https://code.claude.com/docs/en/server-managed-settings"
  - "https://code.claude.com/docs/en/permissions"
  - "https://code.claude.com/docs/en/permission-modes"
  - "https://code.claude.com/docs/en/sandboxing"
  - "https://code.claude.com/docs/en/managed-mcp"
  - "https://code.claude.com/docs/en/hooks"
  - "https://code.claude.com/docs/en/cloud-environments"
  - "https://code.claude.com/docs/en/github-actions"
  - "https://code.claude.com/docs/en/monitoring-usage"
  - "https://code.claude.com/docs/en/data-usage"
  - "https://code.claude.com/docs/en/sub-agents"
  - "https://platform.claude.com/docs/en/about-claude/models/model-ids-and-versions"
  - "https://platform.claude.com/docs/en/resources/overview"
  - "https://genai.owasp.org/llmrisk/llm01-prompt-injection/"
  - "https://genai.owasp.org/llmrisk/llm062025-excessive-agency/"
  - "https://airc.nist.gov/airmf-resources/airmf/5-sec-core/"
verified: 2026-10-27
verified_against: []
verified_findings: 0
verified_note: "Whole-page D8 integration and current official Anthropic documentation check; no archived source opened."
---

# Claude Code Control Sheet

This control sheet pairs requirements with enforcers and evidence tests for [[claude-code|Claude Code]] across five enterprise deployment shapes. Record each deployed shape separately. These requirements describe a proposed deployment without attesting to an organization's implementation. The [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] explain why the execution host, decision point, identity, and evidence change with the shape. A product feature is a possible mechanism; a resolved configuration and a refused test call are deployment evidence.

The control objectives apply the risk framing of the [NIST AI Risk Management Framework](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/) and OWASP's [prompt-injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) and [excessive-agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) risks. Neither source certifies a Claude Code configuration. The requirements below name an accountable enforcer and an observable result so that an assessor can test the deployment instead.

## Scope and deployment shapes

Use one control record per materially distinct shape, provider route, and execution boundary. A fleet record points to its constituent records. The term *provider route* means the actual model endpoint and authentication path: a Claude subscription, Anthropic Console, Amazon Bedrock, Claude Platform on AWS, Google Cloud's Agent Platform, Microsoft Foundry, or an intervening gateway. [Feature availability differs by route](https://code.claude.com/docs/en/feature-availability); a route change therefore reopens the record.

| Shape | Surface and execution boundary | Identity and provider route to record | Principal control plane |
| --- | --- | --- | --- |
| Interactive local | Terminal CLI or VS Code/JetBrains session on a managed workstation | Human sign-in, organization, endpoint, and any gateway | Endpoint policy, Claude Code permissions, host controls |
| Sandboxed autonomous local | Local CLI or unattended `-p` run inside a task container or VM | Human or workload principal, credential issuer, endpoint | Task runtime, host network, Claude Code policy |
| Delegated cloud | Cloud session or routine in an Anthropic-hosted VM or self-hosted environment | Subscription organization, session initiator, model route, repository grant | Cloud environment, server or runner policy, code host |
| CI runner | GitHub Action, GitLab integration, or generic `-p` job on a runner | Trigger actor, workload identity, model route, repository token | CI workflow, runner isolation, code-host rules |
| Fleet / parallel | Multiple sessions, subagents, agent teams, or concurrent jobs across the preceding shapes | Parent and child principals and every child route | Fleet inventory, per-runtime controls, central response |

Interactive local includes a Desktop Code *Local* session only when its device, identity, and policy source are recorded as a distinct surface. Desktop Cowork, Remote Control, and mobile control introduce other access paths that require separate variant records. A cloud session launched from an IDE is delegated cloud, because code executes in the cloud environment. A CI job is not interactive local merely because it uses the same CLI. [Managed-settings delivery differs among local, Desktop, and cloud surfaces](https://code.claude.com/docs/en/managed-settings).

## Control record

For each row below, retain a record with these fields:

- **Boundary:** shape, surface, host or cloud environment, repository and data class, network paths, Claude Code version, requested model identifier and resolved version evidence where disclosed, provider endpoint, authentication route, and responsible human.
- **Decision:** control ID, required result, enforcing component and owner, approved configuration or policy version, and any dependency on a plan or client version.
- **Proof:** resolved policy or platform configuration, a positive authorized test, a negative unauthorized test, timestamped runtime result, and the location of the resulting audit record.
- **Disposition:** pass, fail, not applicable with reason, or an exception with owner, expiry, compensating measure, and retest date.

The organization owns the requirement and the decision record. Anthropic supplies documented client or service capabilities. The endpoint, identity provider, code host, CI platform, cloud account, and telemetry backend enforce controls that Claude Code does not. A screenshot of an intended setting is insufficient where a lower scope, a different provider route, or a changed permission mode can alter runtime behavior. Test the most permissive mode the deployment permits, with the same version and surface used in production.

Keep a component inventory for each route and a history of the versions it ran. Reconcile the running model, hosting service, harness, installed abilities, dependencies, and security-relevant configuration with their approved sources and owners. Save the matching card for each named model from Anthropic's [model-card listing](https://platform.claude.com/docs/en/resources/overview), or record the supplier evidence gap. A requested mutable alias is not a resolved version without a run record or provider mapping. Anthropic documents that [Claude model IDs from the 4.6 generation onward name fixed snapshots](https://platform.claude.com/docs/en/about-claude/models/model-ids-and-versions); record the actual ID and the route's version scheme. Record an undisclosed supplier-held version as such rather than inferring one from a product label.

## Common controls

These are outcome-level controls. The mechanism named for one shape is not evidence for another.

### C01 — Bound identity and route

**Requirement.** Every session must be attributable to a human or approved workload, a repository scope, a model-provider route, and a deployment owner. The run must not silently switch to an unapproved endpoint or credential.

**Enforcer.** Identity and endpoint administrators own sign-in, device or workload enrollment, and routing; Claude Code merely consumes that configuration.

**Mechanism and proof.** Record the identity-provider grant, endpoint or runner configuration, model connection, and code-host grant. A direct Claude Enterprise sign-in can use server-managed settings, but API-key helpers, third-party provider variables, and custom endpoints can change whether that channel is fetched. [Server-managed settings documents its eligibility and fallback behavior](https://code.claude.com/docs/en/server-managed-settings).

**Test.** Start with an unauthorized account or route override; the independent identity or network boundary must refuse it, and the attempt must be attributable in its log.

### C02 — Resolve and protect policy

**Requirement.** Every runtime must load the organization's approved policy before it can process repository-controlled configuration. Developers and repositories must not widen protected permission rules, hooks, MCP servers, or customization beyond the approved set.

**Enforcer.** The platform team owns managed Claude Code policy where supported; endpoint, image, or CI administrators own its delivery and restoration.

**Mechanism and proof.** For local clients, capture `/status`, `claude doctor`, the managed source and resolved policy digest or equivalent. Check `allowManagedPermissionRulesOnly`, `allowManagedHooksOnly`, and `strictPluginOnlyCustomization` where those surfaces must be locked. Test list merging: a managed setting's presence does not prevent every lower scope from adding entries. Anthropic server-managed delivery is unavailable on Bedrock, Claude Platform on AWS, Agent Platform, and Foundry routes. Use a managed device or runner source, or a separately validated gateway policy channel. `forceRemoteSettingsRefresh` can make a fetch failure block startup on a route that actually fetches remote policy. It cannot repair a skipped fetch. [Managed-settings precedence and exceptions](https://code.claude.com/docs/en/managed-settings), [provider availability](https://code.claude.com/docs/en/feature-availability), and [server-delivery conditions](https://code.claude.com/docs/en/server-managed-settings) determine the test.

Reconcile the complete enrolled device and runner population by configuration class, managed source, and exception. Record each managed list key that a local scope can extend and its approved additions. Keep the complete change history of the configuration tree, including settings, hooks, MCP definitions, subagents, skills, and instruction files. Endpoint, image, or code-host controls must prevent each security-relevant change from taking effect before review, including a project change in the pull request under review. Hold the pre-use review claim where a local change can load before approval. The customer supplies the fleet reconciliation, lock values, restore test, and review-gate evidence; Anthropic's settings documentation defines the available controls.

**Test.** Add a conflicting project rule and an unapproved MCP server, then try a route that skips server policy. Attempt a local or pull-request configuration change before review and alter a managed file to test restoration. Record the resolved source, review gate, restore result, and refused operation; a policy file alone is not a pass.

### C03 — Bound tool authority

**Requirement.** The effective tool set must enumerate what a run can execute and where it can write or send data. Destructive or cross-boundary actions require a human decision or an external gate that refuses them in unattended operation.

**Enforcer.** The platform owner sets Claude Code permissions; code-host and service owners enforce branch protection, API scopes, and approval at the target system.

**Mechanism and proof.** Capture effective `allow`, `ask`, and `deny` rules, enabled MCP tools, hooks, permission mode, and downstream grants. Avoid a blanket shell or network allow rule as the only boundary: [Bash rules match command text, and permission modes change decisions](https://code.claude.com/docs/en/permissions). From v2.1.283, `auto` is the built-in starting mode for interactive terminal and VS Code sessions on every plan and provider when available. A session that selects `auto` but cannot use it starts in Manual. `permissions.defaultMode` selects a start mode, while `permissions.disableAutoMode` can remove `auto` from managed deployments. JetBrains and IDE-originated sessions require their own observed mode. [Permission-mode precedence and surface differences](https://code.claude.com/docs/en/permission-modes) govern the record.

**Test.** Try an unlisted tool, an alternate spelling of a dangerous shell action, a direct default-branch push, and a mode change. The relevant boundary must refuse each action that policy prohibits.

### C04 — Contain execution and egress

**Requirement.** The run must be confined to approved files, processes, destinations, and resource limits before untrusted workspace content can affect execution. The egress inventory must include model traffic and paths outside a shell sandbox.

**Enforcer.** The host, container, cloud, or CI operator owns whole-runtime isolation and network policy; Claude Code may add a narrower shell boundary.

**Mechanism and proof.** Capture a running process and mount profile, allowed domains, DNS or proxy rules, credential mounts, and CPU, memory, time, and teardown limits. Claude Code's built-in sandbox covers Bash, PowerShell, and Monitor child processes, not built-in Read/Edit, hooks, MCP servers, or the parent process. It uses Seatbelt on macOS and bubblewrap on Linux/WSL2; native Windows does not have that built-in sandbox. A passing built-in sandbox test therefore proves only the covered tool path. [Sandbox scope and operating-system support](https://code.claude.com/docs/en/sandboxing) fix this boundary.

**Test.** Attempt a forbidden file read and write through both shell and built-in tools, an unauthorized MCP or hook connection, a network call, and a sandbox escape or unsandboxed retry. Collect the denial from the component that actually blocks each path.

### C05 — Govern untrusted inputs and extensions

**Requirement.** Repository instructions, retrieved content, command output, MCP responses, and third-party extensions remain untrusted inputs. Only reviewed sources may add executable configuration or external tools; their content must not grant authority.

**Enforcer.** Each control boundary has an owner:

- Repository owners review instruction and configuration files.
- Platform administrators govern MCP and plugins.
- Policy or target-system owners hold sensitive actions at a separate gate.

**Mechanism and proof.** Version the allowed MCP server command or URL, plugin source, hooks, skill files, and repository instruction files. From v2.1.273, `allowedMcpServers` with `allowManagedMcpServersOnly: true` prevents lower-scope allowlists from broadening the managed catalog. Match a server URL or command: a name-only entry can admit another endpoint under the same name. Inventory `managedMcpServers`, `managed-mcp.json`, and host-provided in-process servers separately, because the managed allowlist does not screen all of them. [Managed MCP documentation](https://code.claude.com/docs/en/managed-mcp) defines these exceptions. A timed-out command, HTTP, or MCP `PreToolUse` hook lets the call continue through ordinary permissions; an independent gate is needed when hook failure must stop the action. [Hook timeout semantics](https://code.claude.com/docs/en/hooks) differ for Agent SDK callback hooks.

Approve each ability by publisher and exact installation source or endpoint, then compare that record with what the run can use. For an ability installed on an organization-run system, retain a malicious-code and instruction scan for each version before installation and clear or dispose of positive findings before use. Keep a reviewed baseline for each instruction file and evidence that the running harness checks it at load, including a mismatch record or test. Managed allowlists are source controls; they are not scan results or instruction-file integrity evidence.

**Test.** Put an instruction to export a canary secret in a file, page, and MCP response, and add an unapproved extension. Attempt to install an unscanned version and alter an instruction file after baseline approval. Verify that no outbound effect occurs, the extension is absent or refused, and the baseline mismatch is recorded. This tests the [prompt-injection boundary](https://genai.owasp.org/llmrisk/llm01-prompt-injection/), not the model's verbal refusal alone.

### C06 — Protect data and credentials

**Requirement.** The approved data class, recipients, retention, and credential exposure must be explicit for each route. A credential inside a run's environment is available to code the agent can invoke unless an independent boundary removes or proxies it.

**Enforcer.** The data and credential boundaries have separate owners:

- Data owners classify repositories and connected systems.
- Identity and runtime operators issue scoped credentials.
- Provider administrators validate contractual processing and retention.

**Mechanism and proof.** Record repository and MCP data classes, provider agreement, model endpoint, environment variables and mounts, secret injection path, local transcript settings, and deletion or retention rule. [Claude Code data usage](https://code.claude.com/docs/en/data-usage) distinguishes commercial provider handling from local plaintext transcript caching; a qualified Enterprise zero-data-retention arrangement is separate from the standard plan. Cloud environment variables are ordinary variables visible to sessions using that environment. The cloud API-credential proxy described in [cloud environments](https://code.claude.com/docs/en/cloud-environments) is currently a Pro/Max feature, not a Team/Enterprise control to assume.

For each model, hosting, harness, or ability supplier, retain a route-scoped security assessment with cited development, test, disclosure, incident-notice, and audit evidence. For the provider that actually processes deployment data, attach applicable permitted-use, retention, deletion, and subcontractor terms. For an inaccessible supplier-held system prompt, obtain a scoped assertion of secret absence and sensitivity treatment; a customer leak test can detect disclosure but cannot prove what that prompt contains. Record missing supplier evidence and a reassessment date. An Anthropic capability page does not establish a Bedrock, Agent Platform, Foundry, or gateway contract.

**Test.** From a sandboxed command and an unsandboxed extension, attempt to read a canary credential; then try to send classified canary content to a disallowed recipient. Verify both the runtime denial and the downstream service result.

### C07 — Hold evidence and respond

**Requirement.** The organization must hold searchable records of policy resolution, tool decisions, target-system effects, and emergency disablement for the approved retention period. A runbook must name who triages an alert, how the run or credential is stopped, and what happens when a guardrail errors or times out.

**Enforcer.** Observability and incident-response owners operate the collector, correlation, alerting, retention, and revocation; product events are inputs.

**Mechanism and proof.** On supported clients, Claude Code can emit `tool_decision`, `permission_mode_changed`, and `managed_settings_resolved` events. Detail flags determine whether tool arguments and MCP names appear; enabling them can also expose sensitive content. Claude Code emits raw events, while the SIEM must correlate sessions and alert. [Monitoring's event schema and explicit SIEM boundary](https://code.claude.com/docs/en/monitoring-usage) govern the design. Where that stream is unavailable or insufficient, use host, provider, CI, code-host, and target-service records.

**Test.** Cause a denied action, a policy-source change, a hook timeout, and a permitted write. Find their identities and outcomes in the organization's store, trigger the alert, revoke the principal or route, and verify the next call fails.

## Shape-specific controls

Each overlay adds a distinct gate and evidence set to C01–C07. A control that cannot be implemented on the chosen provider route needs an alternative independent enforcer or a recorded exception.

### Interactive local — L01

**Requirement and owner.** Endpoint management must keep the approved policy and host protections on every eligible terminal, VS Code, and JetBrains workstation. The platform owner decides whether interactive work requires a prompt, permits `auto`, or allows some actions only through an external review gate.

**Mechanism.** Record the actual sign-in and provider route for each surface. For a direct Claude Enterprise organization route, server-managed settings can supply policy, but endpoint delivery and a fleet enrollment record protect against skipped fetch, local administrator changes, and offline startup. Capture `/status`, `claude doctor`, device enrollment, IDE setting, and runtime mode. A Manual-mode design that relies on prompting for shell writes must set sandbox `autoAllowBashIfSandboxed` to `false` or accept that sandboxed commands run without prompts; that setting defaults to `true` even in Manual mode. [Sandbox modes](https://code.claude.com/docs/en/sandboxing) and [IDE mode rules](https://code.claude.com/docs/en/permission-modes) are version-sensitive.

**Negative test.** On each IDE and CLI surface, try a sandboxed write, a built-in Edit, a mode escalation, an unapproved MCP server, and a route override. Record the enforcing component and result for each, not one combined “managed” verdict.

### Sandboxed autonomous local — L02

**Requirement and owner.** The runtime owner must create a task-scoped boundary before reading an untrusted checkout and destroy it on completion or forced stop. No local approval prompt may be the only protection for a run that has no human at the keyboard.

**Mechanism.** Use a container or VM around the whole CLI process, hooks, MCP servers, built-in file tools, and shell children; give it a disposable workspace, constrained network, resource ceilings, and a task-scoped identity. Claude Code's built-in sandbox can narrow shell access with `enabled`, `failIfUnavailable`, and `allowUnsandboxedCommands: false`, but cannot replace the whole-process boundary. Check inherited environment variables and the parent process's model credential. [Sandbox scope, credential handling, and unsandboxed fallback](https://code.claude.com/docs/en/sandboxing) set the limits.

**Negative test.** Use a poisoned checkout and verify each boundary:

- A project hook or MCP process cannot start before isolation.
- A shell command cannot escape the sandbox or retry unsandboxed.
- Built-in Read/Edit and an MCP tool cannot reach forbidden host files.
- A forced stop leaves no reusable task state.

Record host policy and denial logs.

### Delegated cloud — L03

**Requirement and owner.** The cloud-environment owner must select who can start sessions, which repository and environment they reach, what credentials are supplied, and which network paths remain open. The code host owns merge and branch gates after Claude produces a change.

**Mechanism.** Record whether the session uses an Anthropic-hosted VM or a self-hosted environment. An Anthropic-hosted session gets a fresh VM, a cloud environment and a security proxy. Team and Enterprise sessions can receive server-managed settings. A self-hosted environment uses the organization's runner image and network boundary. Do not infer local MDM policy inside a hosted VM. An owner-managed shared environment can standardize its network list. Each personal environment needs its own review. The cloud environment's network access level does not cover its separate GitHub proxy, MCP connector path, API-credential hosts, or Claude API path. [Cloud environment access paths](https://code.claude.com/docs/en/cloud-environments) define the inventory. Cloud sessions require a Claude subscription. Check [provider availability](https://code.claude.com/docs/en/feature-availability) before assuming another route.

**Negative test.** For each approved cloud environment, test its most permissive production access level against a disallowed external host through ordinary shell traffic. Separately test each enabled connector and proxy path against its own destination and authorization boundary. Try a repository outside the grant and a write that bypasses branch protection. Retain the environment version, session ID, code-host result, and provider audit evidence.

### CI runner — L04

**Requirement and owner.** The CI owner must isolate untrusted pull-request content before Claude Code reads project configuration, bind the run to a constrained workload identity, and keep writeback behind code-host review.

**Mechanism.** Pin the CLI, action and runner image. Record trigger, actor authorization, fork treatment, token scope, model route, and workflow permissions. In `-p` mode, repository settings can supply hooks, environment values, and MCP definitions before an interactive trust prompt. `--setting-sources user` excludes project settings and `.mcp.json`, while `--bare` alone still permits some project environment and helper settings. [Print-mode trust behavior](https://code.claude.com/docs/en/permissions) makes an isolated runner or explicit source exclusion necessary. For the Claude Code GitHub Action, scheduled triggers differ from interactive actor checks. The official GitHub App carries permissions for other Claude features. A custom app can narrow this action's grants. Inspect the app and workflow token to establish least privilege. [GitHub Actions setup and permissions](https://code.claude.com/docs/en/github-actions) supply the route-specific details.

Where the organization assembles an agent release, keep a pipeline-generated AI bill of materials reconciled to the deployed configuration. For packages the agent or its setup installs, retain the governed registry path, exact version and hash lock, and vulnerability scan with blocking dispositions. Bind review and static-analysis results for agent-generated code and security-relevant configuration to the shipped revision. Sign organization-built artifacts and authenticate build provenance where a build produces them. The CI owner supplies these records; Claude Code does not attest to the surrounding pipeline.

**Negative test.** A forked PR containing a hostile settings file must not execute its hook, read a secret, install an MCP server, or write to the protected branch. Try an unapproved package, altered lock hash, unsigned artifact, and generated-code finding on each applicable install or build route. Save the workflow run, resolved settings, runner log, scanner and gate decisions, token grant, and code-host denial.

### Fleet and parallel — L05

**Requirement and owner.** The fleet owner must be able to enumerate every live run and child agent, identify its parent and provider route, cap its authority and aggregate effect, and stop it without waiting for a parent session to cooperate.

**Mechanism.** Treat this as an overlay, not a sixth runtime: every worker inherits the applicable local, cloud, or CI record. Inventory delegated tools, subagent definitions, concurrency and spend limits, shared MCP and credential paths, and the code-host or service limits that bound simultaneous writes. A parent session's permission or sandbox setting is not evidence that a separately launched child on another host has the same boundary. [Subagent and agent-team behavior](https://code.claude.com/docs/en/sub-agents) must be checked against the installed version.

**Negative test.** Launch concurrent children with one revoked principal and one out-of-scope tool request. The former must lose access and the latter must be denied; the organization's evidence store must join each child to the initiating human and show the total effects. Exercise the stop path for one child and for the whole fleet.

## Validation and decision

Before release, the platform and security owners run the same test pack against each recorded shape and provider route. A permitted repository edit and approved outbound call prove that the route works; an unauthorized call proves that its boundary exists. Use benign canaries and staging resources. The minimum negative pack is:

- A lower-scope policy, extension, or route override against the approved policy.
- A tool and permission-mode escalation, including a shell spelling that differs from the allow-rule example.
- A forbidden file or credential read through both a covered shell process and an uncovered built-in or extension path.
- A prompt-injection payload in repository content and a remote result that requests exfiltration.
- A disallowed network destination, a protected code-host write, and an incident-response revocation.

Store a test manifest with client and action versions, resolved model ID where disclosed, policy hash, provider and identity route, shape, host image, canary IDs, commands or tool calls, observed decisions, downstream effects, telemetry query, assessor, and date. Link each material design risk to an assembled-route test, including repeated model outcomes where behavior varies, and retain findings and retests. Probe protected functionality and rules before a material prompt or model change; record the unacceptable disclosure or effect and each finding's disposition. Preserve the raw refusal or target-system record: a model reply saying it will not act does not establish enforcement. Repeat tests after a version, provider, policy-source, extension, environment-image, or permission-mode change.

Before each production release or material configuration promotion, record a security decision for the exact deployed revision or digest. The decision links the design review, first-party change checks, assembled-route tests, open findings, supplier limitations, and any D1-authorized bounded exception. Test that CI, manual, emergency, and cloud or console promotion paths refuse a missing decision or a version other than the one approved. Hold a route when no enforceable prepublication gate covers one of those paths; a later audit entry is not a release gate.

Assign a disposition to each shape and provider route:

- **Pass:** every required test ran, and inspectable evidence of configuration, decisions, and downstream effects shows that every observed outcome meets its requirement.
- **Hold:** absent a Reject condition, a required test is unrun, evidence is incomplete or uninspectable, or a failed requirement lacks a valid Conditional exception. Hold release for any such gap.
- **Conditional exception:** with no Reject condition or other Hold gap, a designated risk owner separate from the implementer approves an independently enforced, tested compensation. Record the missing outcome, affected shape and route, data class, expiry, and reassessment trigger.
- **Reject:** a prohibited action succeeds without independent containment. Do not release that route.

[[securing-agentic-coding|Securing Agentic Coding]] carries the broader enterprise implementation context.

## CMM traceability and sources

Selected controls trace to directly testable criteria in the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]. A domain result applies to the defined deployment shape and requires the model's evidence rules:

- [[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]: the release decision and conditional exception support D1-GATE and D1-ALLOCATE only when tied to the deployment's risk tier and accountable owner.
- [[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]: C01, C06 and L05 bear on D2-INVENTORY, D2-OWNER and D2-TRACE; a session label is not a downstream service identity.
- [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]: C03 and the negative mode test bear on D3-ALLOW and D3-APPROVE.
- [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]: C04, L02 and L04 bear on D4-SANDBOX and D4-SANDBOX-FIRST; the built-in shell sandbox alone cannot establish whole-run coverage.
- [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]: C04 and L03 bear on D5-ALLOW and D5-REACH across actual model, proxy and connector paths.
- [[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]: C05 and C06 bear on D6-EXTEND and D6-CLASSIFY.
- [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]: C07 and L05 bear on D7-LOG and D7-ATTRIBUTE only where the organization holds joined records.
- [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]]: C02's fleet and review records bear on D8-HARNESS and D8-HARNESS-REVIEW; C05's source and scan records bear on D8-ABILITY-SOURCE and D8-ABILITY-SCAN. The route inventory, supplier assessment, and release packet bear on D8-VERSION, D8-SUPPLIER, D8-DESIGN, D8-IMPLEMENT, D8-TEST, and D8-RELEASE only where the stated customer and supplier evidence exists. C06's supplier-held prompt check needs a scoped assertion for D8-PROMPT-SECRETS. A settings file or test pack alone does not establish a domain result.
- [[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]: C07's timeout path bears on D9-GUARD-RUNBOOK.

Anthropic's [feature-availability matrix](https://code.claude.com/docs/en/feature-availability) and the cited settings, permissions, sandbox, cloud, CI, monitoring, and data-usage pages are capability references, not deployment attestations. Version-specific behavior reflects the linked documentation on 2026-10-27; the installed build and provider route govern the final test.
