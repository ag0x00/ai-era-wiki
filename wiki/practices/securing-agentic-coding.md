---
type: practice
title: "Securing Agentic Coding"
address: c-000238
created: 2026-07-30
updated: 2026-09-24
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
  - "[[agentic-ai-security-cmm-d1-governance|CMM D1 — Governance & Accountability]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3 — Control & Least-Agency]]"
  - "[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol]]"
  - "[[agentic-ai-security-cmm-d9-operations|CMM D9 — Operations & Human Factors]]"
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
  - "[[injecting-security-context-vibe-coding-talk|Injecting Security Context During Vibe Coding]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison|OSS AI Security Harness Comparison]]"
  - "[[security-audit-skill|security-audit-skill]]"
  - "[[trail-of-bits-skills|Trail of Bits skills]]"
  - "[[cloudflare|Cloudflare]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]]"
  - "[[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]]"
  - "[[agent-runtime-protection-canvass-2026-09]]"
  - "[[claude-code|Claude Code]]"
  - "[[claude-code-control-sheet|Claude Code Control Sheet]]"
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
sources:
  - https://code.claude.com/docs/en/security
  - https://code.claude.com/docs/en/sandboxing
  - https://code.claude.com/docs/en/settings
  - https://code.claude.com/docs/en/managed-settings
  - https://code.claude.com/docs/en/managed-mcp
  - https://code.claude.com/docs/en/mcp
  - https://code.claude.com/docs/en/permissions
  - https://code.claude.com/docs/en/permission-modes
  - https://code.claude.com/docs/en/authentication
  - https://code.claude.com/docs/en/iam
  - https://code.claude.com/docs/en/google-vertex-ai
  - https://code.claude.com/docs/en/zero-data-retention
  - https://code.claude.com/docs/en/analytics
  - https://code.claude.com/docs/en/monitoring-usage
  - https://code.claude.com/docs/en/sandbox-environments
  - https://code.claude.com/docs/en/third-party-integrations
  - https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency
  - https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations
  - https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/
  - https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/
  - https://github.com/advisories/GHSA-wpqr-6v78-jr5g
  - https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/
  - https://www.manifold.security/blog/ai-coding-agents-git-hijack
  - https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
verified: 2026-09-24
verified_against: []
verified_findings: 1
verified_note: "Diff-scoped 2026-09-24: D3 records, org pin and TLS grading cut to claims with sheet pointers, facts the sheet lacks moved to [^ccpin] and [^ccd3] (item 10); MCP admission scoped for Desktop-delivered connectors (item 14); GitSpawn timing scoped to Claude Code per the Manifold raw. Open: the no-RA-capability groups leave the TLS row ungrouped."
---

# Securing Agentic Coding

## Definition

Securing Agentic Coding catalogs the controls for an agentic coding harness by the six planes of the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] (RA) and maps each control to the domain of the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] (CMM) that scores it.

Every settings key, hook and session surface named belongs to [[claude-code|Claude Code]], whose [documentation](https://code.claude.com/docs/en/security) is detailed enough to name, grade and check a control instead of inferring it. The structure transfers to Cursor, Codex and the open-source harnesses and the keys do not, so an organization running one of those locates each equivalent and sometimes finds it absent. [[claude-code-control-sheet|Claude Code Control Sheet]] grades the same harness against the CMM criterion by criterion.

The organizing claim is a ranking. **Controls that constrain the process outrank controls that inspect a string, and both outrank controls that instruct the model.** The [[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]] is the empirical basis: ten of eleven surveyed harnesses had string-inspecting guards that shell syntax older than the tools walked past ([Adversa AI](https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/)).

## Applicability

The practice applies whenever an agentic coding harness reads a repository. Which controls carry the load depends on the deployment shape, and [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] defines the five shapes for a deployment that is not plainly interactive.

## The Control Catalog

### Reading guide

Each plane's table carries four columns:

- **Control** states the security property in harness-neutral terms, so the row survives a change of harness.
- **Instrument** names what implements it, of four kinds: a **settings key** in dotted JSON-path form, a **session surface** such as `/sandbox`, a **package or product**, or a **practice** with no product behind it. A key is written once in full and then by its last segment, with its minimum Claude Code version where it has one.
- **Grade** is availability as of July 2026, and as of 2026-09-24 for the first-party rows.
- **`RA capability → CMM`** names the RA capability the row implements and the CMM domain that scores it. The catalog works at the grain of the person configuring a harness, so some rows map onto a capability exactly, some at a coarser grain, and nine onto none, which carry a dash and are analyzed under [Controls with no RA capability](#controls-with-no-ra-capability).

### Settings-key locations

A managed value holds only where no lower scope can relax it. In Claude Code a boolean key takes the managed value, so `sandbox.enabled` and `failIfUnavailable` cannot be relaxed locally, and an array key merges entries from every scope, so any developer can append to `excludedCommands` or `allowRead` ([widening the policy](https://code.claude.com/docs/en/sandboxing#keep-developers-from-widening-the-policy)). `allowManagedReadPathsOnly` and `allowManagedDomainsOnly` restore managed-only semantics for `allowRead` and for domains. `excludedCommands`, whose entries run outside the sandbox, has no such lock, so an organization keeps its managed list narrow and reviews it as an artifact.

[[claude-code-control-sheet|Claude Code Control Sheet]] ranks the five settings scopes and the four managed sources. Under `managedSourcesBehavior`'s default, `"first-wins"`, the highest-ranked source that delivers a policy key applies and the others are ignored, apart from the keys read from every admin source, such as the env block, merged per variable, and `forceRemoteSettingsRefresh`. The `"merge"` value, from v2.1.242, composes every admin source instead.[^ccmanaged] Device-deployed policy reaches an enrolled device before its first login, and server-managed settings reach only accounts already authenticated into the organization.

### Identity plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Session pinned to a known organization | `forceLoginMethod` + `forceLoginOrgUUID` in managed settings | First-party, GA; full enforcement v2.1.212+ | — → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Federated identity behind that organization | SSO: Claude for Teams or Enterprise on the claude.ai side, or Claude Console SSO for API-billed organizations; not on Pro or Max | COTS, GA, plan-gated | Agent identity & lifecycle → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Per-workflow credential scoping | One token per environment and workflow, minimum permission | Practice; no product | NHI governance → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Credential held outside the agent boundary | Vendor credential proxy in the delegated-cloud shape; [[credential-proxy-pattern\|Credential Proxy Pattern for AI Agents]] elsewhere | First-party for cloud; assembled elsewhere | Credential proxy → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Inference routed to a cloud the organization already contracts | `CLAUDE_CODE_USE_VERTEX=1` on Google Cloud's Agent Platform, formerly Vertex AI | First-party, GA | Agent identity & lifecycle → [[agentic-ai-security-cmm-d2-identity\|D2]] |
| Attribution of an action to an agent and a human | [[endor-labs-ai-code-governance\|Endor Labs AI Code Governance]] | COTS; [[endor-labs\|Endor Labs]] is the only sourced vendor | Action-to-identity tracing → [[agentic-ai-security-cmm-d2-identity\|D2]] |

**The organization pin covers fewer login paths than it appears to.** It holds only on the login paths the harness checks, and a route that authenticates elsewhere, through a gateway or a cloud provider, moves the control to that route's identity system. Only device-deployed settings reach a first login, so the pin belongs there.[^ccpin] [[claude-code-control-sheet|Claude Code Control Sheet]] lists under D1 which Claude Code login paths check it.

**Cloud routing moves the session's authorization to cloud IAM.** `CLOUD_ML_REGION` and `ANTHROPIC_VERTEX_PROJECT_ID` join `CLAUDE_CODE_USE_VERTEX=1`, under the Google Cloud IAM role `roles/aiplatform.user`, and a multi-user rollout needs a pinned model version. The documented `CLOUD_ML_REGION` examples are global, eu, us and `us-east5`, with `us-east5` the unset default, and the page states nothing about retention or residency. No Canadian region exists to select: Google's residency table for partner models has no Canada column ([Google Cloud, fetched 2026-09-16](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency)), and the setup example's global endpoint leaves the caller unable to control or know which region receives a request ([Google Cloud, fetched 2026-09-16](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations)). [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]] reads that position as OSFI B-10 third-party-risk evidence, and [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] sets it beside the Canadian commitments Google's own models carry.

Claude Code's contribution metrics attribute merged pull-request lines to Claude Code and label those pull requests in GitHub ([PR attribution](https://code.claude.com/docs/en/analytics#pr-attribution)), and no first-party feature attributes a change to the agent or the human that produced it across harness vendors. An organization that cannot separate agent-authored from human-authored change cannot scope a review policy or trace a defect to the tool that introduced it.

### Control plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Managed permission policy that local settings cannot override | A managed source: the managed-settings file, MDM policy, or server-managed settings | First-party, GA | Policy engine, degenerate case → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Deny rules that survive autonomous modes | `permissions.deny`, honored even under sandbox auto-allow and `--dangerously-skip-permissions` | First-party, GA | Least-agency tier engine, block tier → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Content-scoped prompts that autonomy cannot suppress | `permissions.ask` with an argument pattern, such as `Bash(git push *)` or `Bash(dangerouslyDisableSandbox:true)` | First-party, GA | Least-agency tier engine, confirm tier → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Lock developers out of widening policy | `allowManagedReadPathsOnly`, `allowManagedDomainsOnly` | First-party, GA | — → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Pin which MCP servers may run, and lock the customization sources | Eight managed-settings keys and one deployed managed-MCP file, named below | First-party, GA | Tool registry / allowlist → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Deterministic per-action reference monitor | Cedar-routed lifecycle hooks, per [[hooking-coding-agents-with-cedar-talk\|Hooking Coding Agents with Cedar]] | Practitioner prototype | Policy language / PDP engine → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |
| Audit or block in-session settings changes | `ConfigChange` hooks | First-party, GA | — → [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] |

Under the sandbox's auto-allow mode the `permissions.ask` row keeps firing only when scoped. A bare `Bash` or `Bash(*)` ask rule is skipped for any command that runs sandboxed, so an organization that wrote one and then enabled the sandbox has no prompt left for sandboxed commands, while an ask rule on an argument pattern still fires, and one on `Bash(dangerouslyDisableSandbox:true)` catches the escape hatch in flight.

**Eight managed-settings keys and one deployed file admit MCP servers and close the customization escapes.** Four keys and the file govern admission ([managed MCP](https://code.claude.com/docs/en/managed-mcp#policy-based-control-with-allowlists-and-denylists)):

- `allowedMcpServers`, the allowlist, merges from every scope, so a user can broaden it, and a malformed managed value is enforced as an **empty** allowlist.
- `deniedMcpServers`, the denylist, merges from every scope.
- `allowManagedMcpServersOnly` keeps the managed allowlist alone, from the highest-ranked admin source that sets one.
- A deployed `managed-mcp.json` admits only its own servers, the servers `managedMcpServers` provides and the in-process servers the launching app registers ([exclusive control](https://code.claude.com/docs/en/managed-mcp#exclusive-control-with-managed-mcp-json)).

No MCP setting or managed file reaches the claude.ai connectors the Desktop app delivers in-process to its local and SSH sessions, so an organization governs those through its `blocked` connector tool controls or by turning Claude Code off in the Desktop app ([how connectors reach Claude Code](https://code.claude.com/docs/en/mcp#how-connectors-reach-claude-code)).

The other four keys lock the customization sources:

- `allowManagedPermissionRulesOnly` extends managed-only semantics to permission rules.
- `disableBypassPermissionsMode` disables the permissions-bypass mode.
- `disableSideloadFlags` rejects the plugin-directory, plugin-URL, agents and MCP-config startup flags.
- `strictPluginOnlyCustomization`, the strict-customization lock, blocks skills, agents, hooks and MCP servers from user or project sources and leaves plugin delivery as the only route.

**These rows are this shape's policy decision point, and [[agentic-ai-security-cmm-d3-control-least-agency|D3]] L3 grades them as one.** The harness returns permit or deny before the tool call runs, and write protection over the policy's own files holds the decision against an injected instruction. An external decision point is tested by invoking its interface, and this in-process one through four substitute artifacts. [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] states the two limits on that substitution and the circularity it records: vendor code interprets the customer's policy file. [[claude-code-control-sheet|Claude Code Control Sheet]] names the Claude Code record behind each artifact under D3.[^ccd3]

### Runtime plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| OS isolation of shell subprocesses | `sandbox.enabled`, configured through `/sandbox`: Seatbelt on macOS, bubblewrap plus `socat` on Linux and WSL2, optional seccomp for Unix-socket blocking | First-party, GA; no native Windows support | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| OS isolation of the whole session, including MCP servers and hooks | [[anthropic-sandbox-runtime\|Anthropic Sandbox Runtime]] (`@anthropic-ai/sandbox-runtime`) | FOSS, beta research preview | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Container isolation | Published dev container with a default-deny iptables firewall, running Claude Code as a non-root user | FOSS reference config | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Kernel-level isolation | VM, microVM ([[firecracker\|Firecracker]]), vendor-managed cloud VM | GA, varies by provider | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Write confinement inside the sandbox | `allowWrite`, `denyWrite`; default is the working directory, added directories and the session temp directory | First-party, GA | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Read confinement inside the sandbox | `denyRead`, re-opened selectively by `allowRead`; exact deny beats wider allow | First-party, GA | Sandbox / containment → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Authoring-time dangerous-pattern warning | [[security-guidance-plugin\|Security Guidance Plugin]]: per-edit pattern match, end-of-turn diff review, commit and push review | First-party, all plans; warns only | Code-output static analysis → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| Hard failure when isolation is unavailable | `failIfUnavailable`, `allowUnsandboxedCommands` | First-party, GA | — → [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |

An assessor verifies the sandbox's reach instead of assuming it, against three defaults:

- The default read policy leaves credential files readable.
- The model can retry a failed command outside the sandbox through the regular permission flow, a prompt in Manual mode and a classifier decision in auto mode, unless `allowUnsandboxedCommands` is set to false, which the `/sandbox` panel labels strict sandbox mode.
- The sandbox covers shell commands only, so the file tools that carried the exfiltration in [[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]] run outside it.

[[claude-code-control-sheet|Claude Code Control Sheet]] records each default under its D4 criteria.

One key inverts a control an assessor may already have scored. `filesystem.disabled`, from v2.1.216, turns the filesystem layer off and leaves network isolation on, dropping the `sandbox.credentials.files` deny protection, the `denyRead` rules and the write protection over `settings.json` while sandboxed commands stay auto-allowed, a combination the vendor flags as self-escalating: a command can write a shell startup file or `settings.json` that widens its own access on the next run. The key is honored from user, managed and `--settings` sources, and managed settings reserve it by configuring `sandbox.filesystem` or listing a deny entry in `sandbox.credentials.files` ([disable filesystem isolation](https://code.claude.com/docs/en/sandboxing#disable-filesystem-isolation)). The presence of `sandbox.enabled` says nothing about its resolved value.

**A sandbox constrains only what runs after it initializes, and a harness can read workspace configuration before that point.** [[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]] is one case on the public record: headless Gemini CLI trusted the workspace folder for configuration and environment loading and executed from an attacker-supplied `.gemini/` tree before its sandbox initialized ([Novee Security, 2026-04-30](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/)). [[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]] is another: seven coding agents, Claude Code among them, spawn a git subprocess to gather repository context, and the subprocess ran a command named in the repository's own git configuration on the host, outside the sandbox and unseen by the permission model, in Claude Code before the workspace-trust prompt was accepted ([Manifold Security, 2026-09-01](https://www.manifold.security/blog/ai-coding-agents-git-hijack)). *Whether* workspace trust is granted can be a key, as Gemini CLI's `GEMINI_TRUST_WORKSPACE` has been since 0.39.1. *When* isolation begins relative to that read is a property of the startup sequence, with no key and no row. Claude Code documents it for the configuration it reads from a repository: a headless run in an untrusted folder uses the repository's hooks and MCP servers, and they run on the host outside the per-command sandbox ([what runs before you trust a folder](https://code.claude.com/docs/en/permissions#what-runs-before-you-trust-a-folder)), which [[claude-code-control-sheet|Claude Code Control Sheet]] pairs with the flags that keep that content out. For the other harnesses the wiki tracks, no vendor documentation it holds states the ordering, so it stays a question for the vendor.

A September 2026 canvass of twenty-one agent-runtime-protection vendors found one product inspecting a coding agent's generated commands before execution, Operant AI's CodeInjectionGuard, launched 2026-04-21, and none covering reasoning-trace auditing or generated-code static analysis ([[agent-runtime-protection-canvass-2026-09|Agent Runtime Protection Market Canvass]]). An assessor grades the inspector, a different mechanism from the authoring-time warning row, on its own production record.

### Egress plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Domain allowlist for agent subprocesses | `allowedDomains`, `deniedDomains`; no domain is pre-allowed, and `WebFetch(domain:...)` allow rules pre-allow as well | First-party, GA | Egress destination filtering → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| Deny rather than prompt outside the allowlist | `strictAllowlist` (v2.1.219+, ignored from project and local settings); `allowManagedDomainsOnly` for the managed-only variant | First-party, GA | Egress destination filtering → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| TLS-terminating inspection | `tlsTerminate` (v2.1.199+); custom proxy via `httpProxyPort` with its CA installed inside the sandbox | Experimental first-party; custom proxy supported | — → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| Credential masking on outbound requests | `sandbox.credentials.envVars` with `mode: mask` and `injectHosts` (v2.1.199+); the command sees a per-session sentinel and the proxy substitutes the real value | First-party, GA; fails closed without TLS termination | Credential proxy, inverted → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |
| Corporate proxy for all traffic | `HTTPS_PROXY` / `HTTP_PROXY` | First-party, GA | MCP / A2A / LLM proxy → [[agentic-ai-security-cmm-d5-egress-network\|D5]] |

Masking is honored only from user, managed and `--settings` sources, so masking entries in a repository's `settings.json` are ignored, and every `injectHosts` entry must itself be covered by `allowedDomains`. Without `tlsTerminate` the sentinel reaches the server unchanged and authentication fails instead of leaking, so the control is either working or visibly broken.

**A hostname allowlist without TLS termination grades as a misconfiguration control**, because a proxy that decides from the client-supplied hostname admits domain fronting past a broad entry.[^ccsandbox] An exfiltration claim rests on the TLS-terminating row, and [[claude-code-control-sheet|Claude Code Control Sheet]] places Claude Code's proxy on the levels of [[agentic-ai-security-cmm-d5-egress-network|D5]].

### Data plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Credential file and environment-variable denial | `sandbox.credentials.files` and `sandbox.credentials.envVars` with `mode: deny` (v2.1.187+); files are blocked for reads, variables are unset before each sandboxed command | First-party, GA; sandboxed shell commands only | — → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Subprocess credential scrubbing regardless of sandboxing | `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`, which strips Anthropic and cloud-provider credentials from every subprocess and keeps filesystem isolation on | First-party, GA | — → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Self-modification protection | Sandbox protected paths over the harness's own settings and configuration files, a planted symlink's target included | First-party, GA; lost if `filesystem.disabled` | Cognitive file integrity → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Instruction-file integrity | [[cognitive-file-integrity\|Cognitive File Integrity (CFI)]] over `CLAUDE.md`, skills, subagents | Practice; partial product coverage | Cognitive file integrity → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Retrieval bounded by the asking developer's repository grants | Repository access scoped per shape, as the paragraph below sets out | Practice locally and in CI; first-party in delegated cloud | Answer-time entitlement enforcement → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |
| Harness-configuration audit | [[agentshield\|AgentShield]]; [[endor-labs-ai-code-governance\|Endor Labs AI Code Governance]] | FOSS single instrument; COTS single vendor | Supply-chain scanning → [[agentic-ai-security-cmm-d8-supply-chain\|D8]] |
| Untrusted-input exclusion in CI | Disable fork-PR execution; exclude issue and comment bodies | Practice | — → [[agentic-ai-security-cmm-d6-data-rag\|D6]] |

Claude Code has **no built-in credential deny list**: only the listed paths and variables are restricted, and nothing reports an absent list. `sandbox.credentials` protects sandboxed shell commands and nothing else, while `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` reaches every subprocess, so an organization that configured only the first has left MCP servers and hooks holding the credential.

[[agentic-ai-security-cmm-d6-data-rag|D6]] L3 grades the retrieval row against the asking developer's repository and branch grants, scoped to the paths they cover, and grades a run with no asking principal, such as a CI job, on the scope binding its identity to the task. A local session reads the developer's own clone under those grants, a delegated cloud session's GitHub proxy lets API requests reach only the repositories attached to it, and a CI run reads what its workflow token reaches, which the per-workflow scoping row keeps to the task's repositories.

### Observability plane

| Control | Instrument | Grade | RA capability → CMM |
| --- | --- | --- | --- |
| Session telemetry to a SIEM | OpenTelemetry export: `CLAUDE_CODE_ENABLE_TELEMETRY=1` with `OTEL_LOGS_EXPORTER`, one `claude_code.*` event per prompt, tool decision, tool result, mode change, MCP connection and login, correlated by session and prompt ID | First-party, GA | SIEM / SOAR with agent playbooks → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Security-relevant content inside that telemetry | The `OTEL_LOG_*` content gates: tool details (shell command strings, MCP tool names, skill names), tool content in trace spans, user prompts, assistant responses, raw API bodies | First-party, GA, **off by default** | OTel `gen_ai.*` SemConv → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Per-user usage and contribution analytics | Claude Enterprise Analytics API (`read:analytics` key, Enterprise only) or Claude Code Analytics API (Admin API key, Console organizations) | GA, plan-split; no API on Claude for Teams | — → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Fleet inventory of harnesses, MCP servers, skills, hooks | [[endor-labs-ai-code-governance\|Endor Labs AI Code Governance]] | COTS; Endor Labs is the only sourced vendor | AI-SPM / runtime AI-BOM → [[agentic-ai-security-cmm-d7-observability\|D7]] |
| Pull-request security review as a gate | [`claude-code-security-review`](https://github.com/anthropics/claude-code-security-review) GitHub Action | FOSS, first-party | Code-output static analysis → [[agentic-ai-security-cmm-d8-supply-chain\|D8]] |
| Cross-harness session telemetry and retrospective forensics | [[numbat\|Numbat]]: filesystem session artifacts normalized to NDJSON plus a localhost OTLP receiver, spanning Claude Code, Codex, OpenCode and Pi | FOSS from [[perplexity\|Perplexity]], vendor-announced, with the forensics half also implemented by [[adr-agentic-detection-system\|ADR: Agentic Detection for Enterprise AI]] | SIEM / SOAR with agent playbooks → [[agentic-ai-security-cmm-d7-observability\|D7]] |

**Telemetry ships with content redaction on by default, so an organization that enables it and stops there holds the appearance of agent audit without its substance.** The default stream carries token counts, costs, durations, permission-mode transitions and MCP connection status, and no command strings, prompts or tool arguments, so it cannot answer what an agent ran until the organization decides to export that content, with its data-handling consequences.

Neither analytics API substitutes for that stream. Contribution metrics are Teams and Enterprise only, in public beta, dependent on the GitHub app, unavailable under [Zero Data Retention](https://code.claude.com/docs/en/zero-data-retention), and exclude Claude Console API usage and third-party integrations, and the Console dashboard shows Console-billed spend. Sessions routed through Bedrock, Google Cloud's Agent Platform, Foundry or a self-hosted gateway fall outside both, while OpenTelemetry export keeps working under the three cloud providers with only the `claude_code.internal_error` event suppressed. The [[agentic-ai-security-cmm-d7-observability|D7]] regression a gateway causes is the loss of the first-party dashboards, with OpenTelemetry as the rebuild path step 8 of the [Method](#method) depends on.

### Controls with no RA capability

Nine rows anchor to a CMM domain and to no RA capability, in three groups that place the gap differently.

**Properties of a control.** `failIfUnavailable` and `allowUnsandboxedCommands`, `allowManagedReadPathsOnly` and `allowManagedDomainsOnly`, and `ConfigChange` auditing decide whether an existing capability fails closed, whether a local scope can widen it, and whether a change to it is visible. The RA has no slot for such properties, and the omission reaches every plane, since each has controls that can be disabled locally or that degrade to a warning when a dependency is missing.

> [!gap] The RA has no representation for fail-closed and lockdown properties
> A capability row records that a control exists, not that it holds when its dependency is absent or that a lower scope cannot relax it. The omission reaches every plane of the RA, the coding shape's included. Resolving it means either a per-capability property column or a cross-cutting section, a choice made against the RA as a whole.

**Process-environment hygiene.** `sandbox.credentials` deny entries, `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` and per-workflow token scoping contain a credential already inside the agent's process. The RA's Identity plane assumes the credential proxy pattern, where the credential never enters the process, and has no row for one that has. A local harness inherits the developer's shell environment, so the hygiene controls carry the load there.

**Shape-specific artifacts.** Pinning a session to a known Anthropic organization, excluding untrusted input from CI triggers, and the first-party analytics APIs follow from how this product is licensed, triggered and instrumented, so they belong in a catalog and stay out of a reference architecture.

## Method

The deployment order follows the control ranking instead of the plane order.

1. **Establish the isolation boundary before reducing prompts.** Enable the sandbox with `failIfUnavailable`, and with `allowUnsandboxedCommands` set to false, through managed settings, and only then allow autonomous modes, because suppressing prompts first and hardening later is the sequence that produces incidents. The same order holds inside one run, so each vendor is asked whether its boundary exists before the harness reads workspace-supplied configuration.
2. **Close the read side.** Add `sandbox.credentials` deny entries for `~/.aws`, `~/.ssh` and the secret environment variables in use, since nothing is denied by default.
3. **Match the boundary to the shape.** Shell-only sandboxing is insufficient for unattended runs, so wrap the whole process, in [[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]], a container or a VM, wherever prompts are suppressed.
4. **Apply the [[agents-rule-of-two|Agents Rule of Two]] to every non-interactive workflow.** For a CI-triggered agent, remove untrusted input, or secrets from the process environment, or push and egress, and record which one.
5. **Lock the network before widening it.** Set `strictAllowlist` with managed-only domains, and terminate TLS where the threat model includes exfiltration as well as misconfiguration.
6. **Put the harness configuration under source control and scan it.** Per [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]], hooks, MCP manifests, acquired third-party skill packs and `CLAUDE.md` belong in review and in CI.
7. **Gate at the pull request.** The authoring-time plugin warns, and only the CI review action can hold a merge.
8. **Export telemetry before the fleet grows.** Send session-correlated OpenTelemetry to the SIEM, and run an inventory mechanism that survives more than one harness vendor.

## Mechanism

Each step removes a dependency on a weaker layer. OS enforcement holds however a command was spelled, which is the closure the [[guard-canonicalization-gap|Guard Canonicalization Gap]] demands. Removing a Rule-of-Two property removes the precondition for the attack chain instead of detecting its execution. Scanning the configuration tree catches the persistence mechanism: an injected hook survives the session that installed it, which turned the Claude Code flaw CVE-2026-25725 from a session compromise into a sandbox escape.

## Limits

**A key absent from another harness is a missing control until evidence says otherwise.** GuardFall's finding suggests the open-source equivalents are weaker as well as different, so an organization mapping this catalog across harnesses treats each absence as a gap and confirms it before recording a naming difference. The reverse risk is a control surface another harness holds and this catalog has no row for. Workspace trust is that surface: Gemini CLI grants or withholds it through `GEMINI_TRUST_WORKSPACE`, Claude Code records it per folder and documents what a headless run uses without it, and the missing row is a property of the catalog.

**A security skill installed into the harness is an acquired instruction pack that runs under the boundary this catalog configures.** Semgrep's survey of open-source AI code-security harnesses records four such sets distributed for Claude Code and Codex: [[trail-of-bits-skills|trailofbits/skills]], [[cloudflare|Cloudflare]]'s [[security-audit-skill|security-audit-skill]], Capital One's `vulnhunter` and Google's `mantis` ([Semgrep, July 2026](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses)). Per the survey's LLM-generated summaries, two of them build, run or compile code against the repository under audit, so the Runtime-plane and Egress-plane rows bound the run while step 6 covers the acquisition. The security team installs the packs, so the function that scores the harness-audit row also acquires what it would score, and [[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]] carries the per-project comparison.

**Every control above is ordered against an adversary, and one relevant threat model has none.** In an [[accidental-meltdown|Accidental Meltdown]] an agent crosses a boundary while pursuing the user's actual goal after an ordinary environmental error, and it passes content filtering and injection classifiers untouched because it never presents the malicious input they test for. The isolation and Rule-of-Two controls still hold, since they cap reach without reference to intent, and the detection controls largely do not.

**Several load-bearing instruments are single-sourced.** The sandbox runtime is a beta research preview. The COTS control plane in the attribution, harness-audit and fleet-inventory rows is Endor Labs AI Code Governance in all three, one vendor's product material with no dated GA announcement and no independent evaluation, and AgentShield is one open-source scanner. No competing fleet-inventory implementation is sourced here, so that row records a market gap as much as a control.

**The catalog's rows carry CMM coordinates for D2 through D8 and none for D1 or D9, and one D1 criterion reads its evidence off them.** [[agentic-ai-security-cmm-d1-governance|D1]] L3 grades harness configuration held under managed policy that local configuration cannot override or, where the harness offers a lock, extend, with every change to the configuration tree reviewed. The managed-policy and lock rows in the [Control plane](#control-plane) supply the first half, and step 6 of the [Method](#method) the second, with the strict-customization lock covering the user- and project-sourced configuration that source control does not reach. [[claude-code-control-sheet|Claude Code Control Sheet]] maps four D1 and eleven [[agentic-ai-security-cmm-d9-operations|D9]] criteria against Claude Code, and [[cmm-known-limitations|CMM Known Limitations (current state)]] item 10 tracks the D1, D9 and fleet-inventory coordinates that [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] found open against this catalog.

**The catalog states no retention or training position for the cloud-routed shape, because the vendor documentation for that route states none.** Anthropic's page for Google Cloud's Agent Platform carries no retention, logging or training statement, and its zero-data-retention page scopes ZDR to Anthropic's own platform and refers Bedrock, Google Cloud and Foundry deployments to the provider's retention policy ([Anthropic, fetched 2026-09-16](https://code.claude.com/docs/en/zero-data-retention)). The position an organization needs is Google's, in the Agent Platform's own zero-data-retention terms, and no row grades it.

**Code quality sits outside the catalog.** Every control above governs what the agent may *do*, and whether its code is correct is bounded by review capacity, which the [[microsoft-cli-coding-agent-adoption-study|Microsoft CLI Coding Agent Adoption Study]] suggests is already the binding constraint. One practitioner pattern governs what the agent is told before it writes: the MCP server in [[injecting-security-context-vibe-coding-talk|Injecting Security Context During Vibe Coding]] retrieves the ticket, the architecture document, the applicable OWASP cheat sheets and the organization's standards into the prompt, then verifies the generated code against the same requirements. It belongs to no plane because it constrains generation, and it carries that placement's enforcement weakness: the agent calls the server because the tool description persuaded it to, which the speaker, Gupta, states does not happen every time.

**Two instruments transfer across harnesses, and each covers part of the catalog.** [[numbat|Numbat]] attaches to hooks, session artifacts and OTLP across several harnesses behind one interface, which narrows the gap for observability and blocking, while the harness-native identity, sandboxing and egress rows have no cross-harness equivalent. A control delivered over MCP, such as the pattern above, is portable because every product in this market ships MCP support as a condition of entry. For a fleet whose majority runs a harness whose keys the catalog does not carry, the catalog specifies the harness the organization can configure, and the two portable instruments give partial cover for the rest, each weaker than the harness-native rows it stands in for.

## Promotion Path

If a standards body publishes a control set for agentic software development, with an SSDF extension or an OWASP agentic-coding guide the natural candidates, the catalog becomes a crosswalk to it.

## Notes

[^ccsandbox]: [Anthropic — Claude Code sandboxing, "Security limitations"](https://code.claude.com/docs/en/sandboxing#security-limitations), fetched 2026-09-18. The hostname-based allow decision without TLS inspection, domain fronting past a broad entry, stronger TLS-aware isolation named an active area of development, the experimental `tlsTerminate` (v2.1.199+) terminating TLS for masked-credential substitution with no content filtering, and a custom TLS-inspecting proxy for stronger guarantees.
[^ccmanaged]: [Anthropic — Deploy managed settings, "How Claude Code combines managed sources"](https://code.claude.com/docs/en/managed-settings#how-claude-code-combines-managed-sources), fetched 2026-09-24. The four managed sources and their rank, `"first-wins"` and `"merge"`, and the keys read from every admin source. Under "Where each mechanism stores the policy" the same page places the file source at /Library/Application Support/ClaudeCode/ on macOS, /etc/claude-code/ on Linux and WSL, and C:\Program Files\ClaudeCode\ on Windows, as managed-settings.json plus a managed-settings.d directory merged alphabetically.
[^ccpin]: [Anthropic — Authentication, "Restrict login to your organization"](https://code.claude.com/docs/en/authentication#restrict-login-to-your-organization), fetched 2026-09-24. For a Console login, `forceLoginOrgUUID` naming a single Console organization pre-selects it on the sign-in page, and Claude Code does not check which organization the resulting credential belongs to. `claude setup-token` and `/install-github-app` enforce only `forceLoginMethod` and can mint a token in a different organization. Gateway sign-in authenticates against no Anthropic organization, so the gateway's identity provider restricts access, and cloud-provider sessions are restricted through cloud IAM. Server-managed settings reach only accounts already authenticated into the organization and cannot redirect a first login.
[^ccd3]: [Anthropic — Choose a permission mode, "Protected paths"](https://code.claude.com/docs/en/permission-modes#protected-paths), fetched 2026-09-24. The file-editing tools meet the permission system's protected paths, which prompt for a write in default and accept-edits modes, route it to the classifier in auto mode, deny it in don't-ask mode and allow it in bypass-permissions mode, where the managed-only locks keep a written file from carrying a permission rule or hook the harness honors. Two further records sit beside the four artifacts: the resolved `filesystem.disabled`, which removes the sandbox half of that write protection ([disable filesystem isolation](https://code.claude.com/docs/en/sandboxing#disable-filesystem-isolation)), and the empty allowlist a malformed managed `allowedMcpServers` produces ([Deploy managed settings, "Find entries Claude Code dropped"](https://code.claude.com/docs/en/managed-settings#find-entries-claude-code-dropped)).
