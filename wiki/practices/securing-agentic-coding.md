---
type: practice
title: "Securing Agentic Coding"
address: c-000238
created: 2026-07-30
updated: 2026-08-16
tags:
  - practices
  - agentic-coding
  - claude-code
  - sandboxing
  - policy-enforcement
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: produced
maturity: emerging
addresses_threat: "Indirect prompt injection through repository content, credential exfiltration from agent process environments, unreviewed agent-authored change, and harness-configuration tampering"
related:
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]]"
  - "[[security-guidance-plugin|Security Guidance Plugin]]"
  - "[[endor-labs-ai-code-governance|Endor Labs AI Code Governance]]"
  - "[[agentshield|AgentShield]]"
  - "[[guard-canonicalization-gap|Guard Canonicalization Gap]]"
  - "[[agents-rule-of-two|Agents Rule of Two]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[credential-proxy-pattern|Credential Proxy Pattern]]"
  - "[[numbat|Numbat]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[mcp-security|MCP Security]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
sources:
  - https://code.claude.com/docs/en/security
  - https://code.claude.com/docs/en/sandboxing
  - https://code.claude.com/docs/en/settings
  - https://code.claude.com/docs/en/iam
  - https://code.claude.com/docs/en/analytics
  - https://code.claude.com/docs/en/monitoring-usage
  - https://code.claude.com/docs/en/sandbox-environments
  - https://code.claude.com/docs/en/third-party-integrations
  - https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/
  - https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/
  - https://github.com/advisories/GHSA-wpqr-6v78-jr5g
---

# Securing Agentic Coding

## Definition

The control set for the generative-coding deployment shape, mapped to the six planes of the [[agentic-ai-security-reference-architecture|AAI-S RA]] and the nine domains of the [[agentic-ai-security-cmm-2026|AAI-S CMM]].

**Every configuration key and control name on this page belongs to [[claude-code-security|Claude Code]].** The catalog is single-harness-deep by choice: Anthropic documents Claude Code's security model in public at a level its competitors do not match, so it is the only harness where a control can be named, graded, and checked rather than inferred. The *structure* transfers to Cursor, Codex, and the open-source harnesses; the keys do not, and an organization running one of those must locate each equivalent and will sometimes find it absent. Read a key below as an instance of its control, not as a portable instruction.

The organizing claim is a ranking. **Controls that constrain the process outrank controls that inspect a string, and both outrank controls that instruct the model.** The [[guardfall-shell-injection-audit|GuardFall audit]] is the empirical basis: ten of eleven surveyed harnesses had string-inspecting guards that could be walked past with shell syntax older than the tools themselves.

## Applicability

Whenever an agentic coding harness reads a repository. Which controls are load-bearing depends on which of the five shapes in [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] is in play; read that page first if the deployment is not obviously interactive.

## The Control Catalog

### Reading guide

**Control** states the security property in vendor-neutral terms, so the row survives a change of harness.

**Instrument** names the thing that implements it. Instruments are of four kinds, distinguished by how a practitioner touches them: a **settings key**, given in dotted JSON-path form and set in a settings file; a **session surface**, such as the `/sandbox` slash command; a **package or product**, installed or purchased; or a **practice** with no product behind it. Keys are written once in full and then by their last segment. Minimum Claude Code versions appear where a key has one.

**Grade** is availability as of July 2026.

**RA capability → CMM** anchors the row to the [[agentic-ai-security-reference-architecture|AAI-S RA]] capability it implements and the [[agentic-ai-security-cmm-2026|CMM]] domain that scores it. The rows in this catalog are not restatements of the RA's capability rows. The RA lists technology-neutral capabilities for agentic systems in general; this catalog lists controls for one deployment shape, at the granularity of the person configuring a harness. Some rows map onto an RA capability exactly, some map onto one at a coarser granularity, and nine map onto no RA row at all. Those nine carry `—` and are analyzed in [Controls with no RA capability](#controls-with-no-ra-capability).

### Settings-key locations

Every `settings.json` key below resolves through five scopes, highest wins: **managed**, then command-line arguments, then the repository's local override, then the repository's checked-in settings, then the user's own. Managed settings arrive two ways and do not merge with each other — a cached server-managed policy replaces the device-managed file outright:

| Delivery | Location | Reaches |
| --- | --- | --- |
| Device-managed, deployed by MDM | `/Library/Application Support/ClaudeCode/` (macOS), `/etc/claude-code/` (Linux and WSL), `C:\Program Files\ClaudeCode\` (Windows) — `managed-settings.json` plus `managed-settings.d/*.json` merged alphabetically | Any device the MDM enrolls, before first login |
| Server-managed, deployed from claude.ai | Delivered to the authenticated session | Only accounts already authenticated into the organization |

The distinction that decides whether a managed key is authoritative: **boolean keys take the managed value and ignore local settings; array keys merge entries from every scope.** So `sandbox.enabled` and `failIfUnavailable` cannot be relaxed locally, while `excludedCommands` and `allowRead` can be appended to by any developer. `allowManagedReadPathsOnly` and `allowManagedDomainsOnly` restore managed-only semantics for `allowRead` and for domains respectively. `excludedCommands` has no such lockdown.

### Identity plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Session pinned to a known organization | `forceLoginMethod` + `forceLoginOrgUUID` in managed settings | First-party, GA; full enforcement v2.1.212+ | — → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Federated identity behind that organization | SSO — Claude for Enterprise on the claude.ai side, or Claude Console SSO for API-billed organizations. Not available on Claude for Teams | COTS, GA, plan-gated | Agent identity & lifecycle → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Per-workflow credential scoping | One token per environment and workflow, minimum permission | Practice; no product | NHI governance → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Credential held outside the agent boundary | Vendor credential proxy in the delegated-cloud shape; [[credential-proxy-pattern\|credential proxy]] elsewhere | First-party for cloud; assembled elsewhere | Credential proxy → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Attribution of an action to an agent and a human | [[endor-labs-ai-code-governance\|Endor Labs AI Code Governance]] | COTS; [[endor-labs\|Endor Labs]] is the only sourced vendor | Action-to-identity tracing → [[agentic-ai-security-cmm-d2-identity\|D2]] |

**The organization pin covers fewer login paths than it appears to.** Terminal, IDE-extension, and SDK logins are held. The two token-minting commands check only the login *method*, not the organization, so both can produce a credential in a different tenant. Gateway sign-in never authenticates against an Anthropic organization at all, which makes the gateway's own identity provider the control. Bedrock, Vertex, and Foundry sessions authenticate against the cloud provider and are not blocked, so cloud IAM policy is the control there. Deploy the pin through device management: server-managed settings only reach accounts already inside the organization, and so cannot govern a first login.

The attribution row is the weakest link in the plane. An organization that cannot separate agent-authored from human-authored change cannot scope a review policy or trace a defect to the tool that introduced it, and no first-party feature covers it across harness vendors.

### Control plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Managed permission policy that local settings cannot override | `managed-settings.json` by MDM, or server-managed settings | First-party, GA | Policy engine, degenerate case → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Deny rules that survive autonomous modes | `permissions.deny`; explicit deny is honored even under sandbox auto-allow and `--dangerously-skip-permissions` | First-party, GA | Least-agency tier engine, block tier → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Content-scoped prompts that autonomy cannot suppress | `permissions.ask` with an argument pattern — `Bash(git push *)`, `Bash(dangerouslyDisableSandbox:true)` | First-party, GA | Least-agency tier engine, confirm tier → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Lock developers out of widening policy | `allowManagedReadPathsOnly`, `allowManagedDomainsOnly` | First-party, GA | — → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Deterministic per-action reference monitor | Cedar-routed lifecycle hooks — see [[hooking-coding-agents-with-cedar-talk\|Maisel]] | Practitioner prototype | Policy language / PDP engine → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Audit or block in-session settings changes | `ConfigChange` hooks | First-party, GA | — → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |

The `permissions.ask` row survives contact with an autonomous mode, and its behavior is specific enough to test. A bare `Bash` or `Bash(*)` ask rule is *skipped* for any command that runs sandboxed, so an organization that wrote one and then enabled the sandbox has no prompt left. An ask rule scoped to an argument pattern still fires. An ask rule on `Bash(dangerouslyDisableSandbox:true)` catches the escape hatch in flight.

The distinction that matters for scoring is the merge semantics described above: `excludedCommands` merges across scopes and has no managed-only lockdown, so a developer can always append entries that run commands outside the sandbox. Keep the managed list narrow and treat it as a reviewed artifact.

### Runtime plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| OS isolation of shell subprocesses | `sandbox.enabled`, configured through the `/sandbox` session surface — Seatbelt on macOS, bubblewrap plus `socat` on Linux and WSL2, optional seccomp for Unix-socket blocking | First-party, GA; no native Windows support | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| OS isolation of the whole session, including MCP servers and hooks | [[anthropic-sandbox-runtime\|`@anthropic-ai/sandbox-runtime`]] | FOSS, beta research preview | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Container isolation | Published dev container with default-deny iptables firewall; runs Claude Code as a non-root user | FOSS reference config | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Kernel-level isolation | VM, microVM ([[firecracker\|Firecracker]]), vendor-managed cloud VM | GA, varies by provider | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Write confinement inside the sandbox | `allowWrite`, `denyWrite`; default is the working directory and session temp directory only | First-party, GA | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Read confinement inside the sandbox | `denyRead`, re-opened selectively by `allowRead`; exact deny beats wider allow | First-party, GA | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Authoring-time dangerous-pattern warning | [[security-guidance-plugin\|Security Guidance plugin]] — pre-tool hook on Write, Edit, MultiEdit | First-party, free, warns only | Code-output static analysis → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Hard failure when isolation is unavailable | `failIfUnavailable`, `allowUnsandboxedCommands` | First-party, GA | — → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |

Four configuration facts an assessor should verify rather than assume.

Sandbox **read** access defaults to the whole filesystem minus explicit denies, so `~/.aws` and `~/.ssh` remain readable until `sandbox.credentials` or `denyRead` entries exist. The **escape hatch** is a tool parameter, `dangerouslyDisableSandbox`, which the model may set to retry a command that failed under isolation; setting `allowUnsandboxedCommands` makes the parameter ignored entirely, which the `/sandbox` panel labels strict sandbox mode. The built-in sandbox covers **Bash and its child processes only** — Read, Edit, and Write run under the permission system instead, which is how the [[claude-code-github-action-credential-exposure|Microsoft finding]] reached `/proc/self/environ` while the Bash boundary held.

The fourth is newer than the rest of this catalog and inverts a control an assessor may already have scored. `filesystem.disabled` (v2.1.216 and later) turns the filesystem layer off while leaving network isolation on. Setting it also disables the `sandbox.credentials.files` read protections, the `denyRead` rules, and the automatic write-deny on `settings.json` at every scope, while sandboxed commands stay auto-allowed. The vendor documentation flags that combination as self-escalating: a command can write a shell startup file or `settings.json` that widens its own access on the next run. The key is honored from user, managed, and `--settings` sources but not from project or local settings, and managed settings can reserve it exclusively by configuring `filesystem` or listing any `sandbox.credentials.files` entry. Verify the resolved value rather than the presence of `sandbox.enabled`.

One property of this plane is not a settings key at all, and so gets no row. **A sandbox is a control from the moment it exists, and the harness reads workspace configuration before that moment.** [[gemini-cli-workspace-trust-rce|GHSA-wpqr-6v78-jr5g]] is the case on the public record: headless Gemini CLI trusted the workspace folder for configuration and environment loading and executed from an attacker-supplied `.gemini/` tree before its sandbox initialized ([Novee Security, 2026-04-30](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/)). *Whether* workspace trust is granted can be a key — Gemini CLI's `GEMINI_TRUST_WORKSPACE` is one since 0.39.1. *When* the isolation boundary is established relative to that read is a property of the harness's startup sequence, which no vendor documentation states. Every row above is assessable by resolving a settings value; this one is an open question to put to a vendor rather than a control to score.

### Egress plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Domain allowlist for agent subprocesses | `allowedDomains`, `deniedDomains`; no domain is pre-allowed, and `WebFetch(domain:...)` allow rules pre-allow as well | First-party, GA | Egress destination filtering → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| Deny rather than prompt outside the allowlist | `strictAllowlist` (v2.1.219+, ignored from project and local settings); `allowManagedDomainsOnly` for the managed-only variant | First-party, GA | Egress destination filtering → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| TLS-terminating inspection | `tlsTerminate` (v2.1.199+); custom proxy via `httpProxyPort` with its CA installed inside the sandbox | Experimental first-party; custom proxy supported | — → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| Credential masking on outbound requests | `sandbox.credentials.envVars` with `mode: mask` and `injectHosts` (v2.1.199+); the command sees a per-session sentinel and the proxy substitutes the real value | First-party, GA; fails closed without TLS termination | Credential proxy, inverted → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| Corporate proxy for all traffic | `HTTPS_PROXY` / `HTTP_PROXY` | First-party, GA | MCP / A2A / LLM proxy → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |

Masking is the row most likely to be configured into a false sense of coverage. It is honored only from user, managed, and `--settings` sources — `mask` entries in a repository's `settings.json` are ignored — and every `injectHosts` entry must itself be covered by `allowedDomains`. Without `tlsTerminate` the sentinel reaches the server unchanged and authentication fails rather than leaking, which is the right failure direction but means the control is either working or visibly broken, never silently partial.

The default proxy makes its allow decision from the client-supplied hostname without inspecting TLS. The vendor documentation states the consequence directly: a broad allowlist entry such as `github.com` is reachable by domain fronting. **A hostname allowlist without TLS termination is a misconfiguration control, not an exfiltration control**, and it should be graded that way in [[agentic-ai-security-cmm-d5-egress-network|D5]].

### Data plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Credential file and environment-variable denial | `sandbox.credentials.files` and `sandbox.credentials.envVars` with `mode: deny` (v2.1.187+); files are blocked for reads, variables are unset before each sandboxed command | First-party, GA; sandboxed Bash only | — → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Subprocess credential scrubbing regardless of sandboxing | `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` — strips Anthropic and cloud-provider credentials from every subprocess, and forces filesystem isolation to stay on | First-party, GA | — → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Self-modification protection | Sandbox denies writes to `settings.json` at every scope and to the managed settings directory, resolving symlinks (v2.1.210+) | First-party, GA; lost if `filesystem.disabled` | Cognitive file integrity → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Instruction-file integrity | [[cognitive-file-integrity\|Cognitive file integrity]] over `CLAUDE.md`, skills, subagents | Practice; partial product coverage | Cognitive file integrity → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Harness-configuration audit | [[agentshield\|AgentShield]]; [[endor-labs-ai-code-governance\|Endor Labs]] | FOSS single instrument; COTS single vendor | Supply-chain scanning → [[agentic-ai-security-cmm-d8-supply-chain\|D8]] |
| Untrusted-input exclusion in CI | Disable fork-PR execution; exclude issue and comment bodies | Practice | — → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |

There is **no built-in credential deny list**. Only the paths and variables explicitly listed are restricted, which makes this the single most commonly skipped configuration step in the catalog. The two rows above it divide along a boundary worth stating: `sandbox.credentials` protects sandboxed Bash commands and nothing else, while `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` reaches every subprocess whether sandboxed or not. An organization that configured only the first has left MCP servers and hooks holding the credential.

### Observability plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Session telemetry to a SIEM | OpenTelemetry export — `CLAUDE_CODE_ENABLE_TELEMETRY=1` plus `OTEL_LOGS_EXPORTER`, emitting one `claude_code.*` event per prompt, tool decision, tool result, permission-mode change, MCP connection, and login, correlated by session and prompt ID | First-party, GA | SIEM / SOAR with agent playbooks → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Security-relevant content inside that telemetry | the four `OTEL_LOG_*` gates — tool details (shell command strings, MCP tool names, skill names), user prompts, assistant responses, raw API bodies | First-party, GA, **off by default** | OTel `gen_ai.*` SemConv → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Per-user usage and contribution analytics | Claude Enterprise Analytics API (`read:analytics` key, Enterprise only) or Claude Code Analytics API (Admin API key, Console organizations) | GA, plan-split; not available on Claude for Teams | — → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Fleet inventory of harnesses, MCP servers, skills, hooks | [[endor-labs-ai-code-governance\|Endor Labs AI Code Governance]] | COTS; [[endor-labs\|Endor Labs]] is the only sourced vendor | AI-SPM / runtime AI-BOM → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Pull-request security review as a gate | [`claude-code-security-review`](https://github.com/anthropics/claude-code-security-review) GitHub Action | FOSS, first-party | Code-output static analysis → [[agentic-ai-security-cmm-d8-supply-chain\|D8]] |
| Cross-harness session telemetry and retrospective forensics | [[numbat\|Numbat]] — filesystem session artifacts normalized to NDJSON plus a localhost OTLP receiver, spanning [[claude-code-security\|Claude Code]], [[codex-security\|Codex]], OpenCode, and Pi | FOSS; [[perplexity\|Perplexity]], vendor-announced; forensics half also implemented by [[adr-agentic-detection-system\|Uber ADR]] | SIEM / SOAR with agent playbooks → [[agentic-ai-security-cmm-d7-observability\|D7]] |

**Content redaction is the default, and it is the finding that separates the appearance of agent audit from the fact of it.** An organization that enables telemetry and stops there receives token counts, costs, durations, permission-mode transitions, and MCP connection status — and no shell command strings, no prompt text, no tool arguments. That stream cannot answer what an agent ran, which makes it a usage feed rather than a security feed. Turning it into a security feed means deciding to export prompt and tool content, with the data-handling consequences that decision carries.

The two analytics APIs are different products with different scopes, and neither substitutes for the telemetry stream. Contribution metrics are Teams and Enterprise only, in public beta, dependent on the GitHub app, unavailable to organizations running [Zero Data Retention](https://code.claude.com/docs/en/zero-data-retention), and documented as excluding Claude Console API usage and third-party integrations. The Console dashboard covers Console-billed API usage and shows spend rather than contribution. Sessions routed through Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry, or a self-hosted gateway fall outside both.

> [!check] Correcting an earlier reading of the gateway regression
> OpenTelemetry export is **not** disabled under Bedrock, Vertex, or Foundry — only the `claude_code.internal_error` event is suppressed there. The [[agentic-ai-security-cmm-d7-observability|D7]] regression an inference gateway causes is therefore narrower than a total loss of visibility: it removes the first-party analytics dashboards, and OpenTelemetry remains available as the rebuild path. Step 8 of the deployment order below depends on this.

### Controls with no RA capability

Nine rows anchor to a CMM domain but to no RA capability. They fall into three groups, and the groups have different implications.

**Properties of a control rather than controls.** `failIfUnavailable` and `allowUnsandboxedCommands`, `allowManagedReadPathsOnly` and `allowManagedDomainsOnly`, and `ConfigChange` auditing do not add a capability. They determine whether an existing capability fails closed, whether a local scope can widen it, and whether a change to it is visible. The RA's capability vocabulary has no slot for these, and the omission is not specific to generative coding: *every* plane has controls that can be silently disabled locally or that degrade to a warning when a dependency is missing.

> [!gap] The RA has no representation for fail-closed and lockdown properties
> A capability row records that a control exists, not that it holds when its dependency is absent or that a lower scope cannot relax it. This affects the whole RA rather than this shape. Resolving it means either a per-capability property column or a cross-cutting section, and the choice needs to be made against the RA as a whole, not decided here.

**Process-environment hygiene.** `sandbox.credentials` deny entries, `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`, and per-workflow token scoping cover the case where a credential is already inside the agent's process. The RA's Identity plane assumes the [[credential-proxy-pattern|credential-proxy]] pattern, where the credential never enters the process at all, and so has no row for containing one that has. For a local coding harness inheriting a developer's shell environment, the proxy assumption does not hold, and the hygiene controls are what remains.

**Genuinely shape-specific.** Pinning a session to a known Anthropic organization, excluding untrusted input from CI triggers, and the first-party analytics APIs are artifacts of how this particular product is licensed, triggered, and instrumented. They belong in a catalog and not in a reference architecture.

## Method

A deployment order that follows the control ranking rather than the plane order:

1. **Establish the isolation boundary before reducing prompts.** Enable the sandbox with `failIfUnavailable` and `allowUnsandboxedCommands` through managed settings, and only then allow autonomous modes. The reverse order — suppressing prompts first and hardening later — is the sequence that produces incidents. The same ordering holds one level down, inside a single run: ask each vendor whether its boundary is established before the harness reads workspace-supplied configuration, because [[gemini-cli-workspace-trust-rce|one harness answered no]] and no vendor documentation states the ordering.
2. **Close the read side.** Add `sandbox.credentials` deny entries for `~/.aws`, `~/.ssh`, and the secret environment variables in use. Nothing is denied by default.
3. **Match the boundary to the shape.** Bash-only sandboxing is insufficient for unattended runs; wrap the whole process ([[anthropic-sandbox-runtime|sandbox runtime]], container, or VM) wherever prompts are suppressed.
4. **Apply the [[agents-rule-of-two|Rule of Two]] to every non-interactive workflow.** For a CI-triggered agent, remove untrusted input, or remove secrets from the process environment, or remove push and egress. Choose one explicitly and record which.
5. **Lock the network before widening it.** `strictAllowlist` plus managed-only domains; terminate TLS if the threat model includes exfiltration rather than misconfiguration.
6. **Put the harness configuration under source control and scan it.** [[harness-config-as-supply-chain-artifact|The config tree is a supply-chain artifact]]; hooks, MCP manifests, and `CLAUDE.md` belong in review and in CI.
7. **Gate on the pull request, not on the keystroke.** The authoring-time plugin warns; only the CI review action can hold a merge.
8. **Export telemetry before the fleet grows.** Session-correlated OpenTelemetry to the SIEM, and an inventory mechanism that survives more than one harness vendor.

## Mechanism

Each step in the ordering removes a dependency on a weaker layer. OS enforcement holds regardless of how a command was spelled, which is the closure the [[guard-canonicalization-gap|guard canonicalization gap]] demands. Removing a Rule-of-Two property removes the precondition for the attack chain rather than detecting its execution. Scanning the configuration tree catches the persistence mechanism — an injected hook survives the session that installed it, which is what made CVE-2026-25725 a sandbox escape rather than a session compromise.

## Limits

**The single-harness scope stated at the top has a sharper edge than convenience.** GuardFall's finding suggests the open-source equivalents are weaker rather than merely different, so an organization that maps this catalog onto another harness should treat a missing key as a missing control until it has evidence otherwise, not as a naming difference. The reverse direction is the newer risk: another harness may hold a control surface this catalog has no row for, and a defect there is invisible to a reader working from these rows. Workspace trust is that surface — Gemini CLI grants or withholds it through `GEMINI_TRUST_WORKSPACE`, Claude Code has no equivalent key, and the control's absence from the catalog is a property of the catalog rather than a finding about either product.

**Cross-harness coverage now has one sourced instrument, and it covers only part of the catalog.** [[numbat|Numbat]] attaches to hooks, session artifacts, and OTLP across several harnesses behind one interface. It narrows the gap for observability and for blocking without closing it: the harness-native identity, sandboxing, and egress controls in the rows above have no cross-harness equivalent, so a second harness still inherits whatever isolation its own settings provide.

**Every control above is ordered against an adversary, and one relevant threat model has none.** The [[accidental-meltdown|accidental meltdown]] case — an agent crossing a boundary while pursuing the user's actual goal after an ordinary environmental error — passes through content filtering and injection classifiers untouched, because it never presents the malicious input they test for. The isolation and Rule-of-Two controls still hold, since they cap reach without reference to intent. The detection controls largely do not.

**Several load-bearing instruments are single-sourced.** Sandbox runtime is a beta research preview. The COTS control plane in the attribution, harness-audit, and fleet-inventory rows is [[endor-labs-ai-code-governance|Endor Labs AI Code Governance]] in all three cases — one vendor's product material, with no dated GA announcement and no independent evaluation. AgentShield is one open-source scanner. No competing implementation of the fleet-inventory capability is sourced here, so that row records a market gap as much as a control.

**Nothing here addresses code quality.** Every control in the catalog governs what the agent may *do*. Whether the code it writes is correct is a separate problem, addressed by review capacity that the [[microsoft-cli-coding-agent-adoption-study|throughput data]] suggests is already the binding constraint.

## Promotion Path

If a standards body publishes a control set for agentic software development — the natural candidates being an SSDF extension or an OWASP agentic-coding guide — this page becomes a crosswalk to it rather than a standalone catalog.
