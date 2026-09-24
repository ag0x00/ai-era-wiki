---
type: thesis
title: "Generative Coding Deployment Shapes"
address: c-000237
created: 2026-07-30
updated: 2026-09-24
tags:
  - thesis
  - agentic-coding
  - claude-code
  - deployment-shape
  - sandboxing
status: developing
scope_axis:
  - sec-of-ai
  - sec-against-ai
origin: produced
question: "What does the generative-coding deployment shape actually look like in mid-2026, and where does the security boundary sit in each variant?"
current_position: "Generative coding has stopped being one deployment shape. It is now at least five, distinguished by where the agent process runs and whether a human is positioned to see the action before it happens. The controls that matter differ per variant, and the variants where the human is structurally absent are the ones growing fastest."
last_revised: 2026-08-16
related:
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[sdlc-in-the-ai-attacker-era|SDLC in the AI-Attacker Era]]"
  - "[[agents-rule-of-two|Agents Rule of Two]]"
  - "[[guard-canonicalization-gap|Guard Canonicalization Gap]]"
  - "[[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[gemini-cli|Gemini CLI]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[microsoft-cli-coding-agent-adoption-study|Microsoft CLI Coding Agent Adoption Study]]"
  - "[[gartner-mq-enterprise-ai-coding-agents-2026|Gartner MQ for Enterprise AI Coding Agents]]"
  - "[[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[offensive-agent-collective|Offensive Agent Collective]]"
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[numbat|Numbat]]"
  - "[[shadow-automation|Shadow Automation]]"
  - "[[vibe-coding|Vibe Coding]]"
  - "[[pwc-stage-coverage-tiers|PwC Stage-Coverage Tiers]]"
  - "[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]"
  - "[[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol]]"
  - "[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]"
  - "[[claude-code|Claude Code]]"
  - "[[claude-code-control-sheet|Claude Code Control Sheet]]"
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
sources:
  - https://code.claude.com/docs/en/sandbox-environments
  - https://code.claude.com/docs/en/sandboxing
  - https://code.claude.com/docs/en/security
  - https://code.claude.com/docs/en/third-party-integrations
  - https://code.claude.com/docs/en/permission-modes
  - https://code.claude.com/docs/en/permissions
  - https://code.claude.com/docs/en/claude-code-on-the-web
  - https://code.claude.com/docs/en/desktop
  - https://code.claude.com/docs/en/self-hosted-environments
  - https://www.gartner.com/en/newsroom/press-releases/2026-05-20-gartner-says-the-market-for-enterprise-ai-coding-agents-is-entering-a-new-phase-of-expansion-and-competitive-realignment
  - https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/
  - https://www.manifold.security/blog/ai-coding-agents-git-hijack
  - https://arxiv.org/html/2607.01418v1
  - https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/
  - https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/
  - https://github.com/advisories/GHSA-wpqr-6v78-jr5g
verified: 2026-09-24
verified_against: []
verified_findings: 0
verified_note: "Diff-scoped 2026-09-24 (final fix round): l.119 injection vector corrected against the Microsoft blog and its figures (items 1, 12); l.121 trust-table sentence cut to one sentence plus the sheet pointer (item 10); l.178 GitSpawn timing scoped to Claude Code per the Manifold raw. V4's whole-page read earlier today left nothing open."
---

# Generative Coding Deployment Shapes

## Question

The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] was drafted with one generative-coding row that assumed a developer at a keyboard. The question is what the shape looks like in 2026 for a harness such as [[claude-code|Claude Code]], and where the security boundary sits in each variant.

## Current Position

One harness now runs in five shapes, which differ in where the agent process runs and whether a human is positioned to see a specific action before it executes. Claude Code is the worked example, because its controls are documented in public and its incidents are on the record. In the interactive shape a human sees the action first, and the permission prompt is a real control. In the delegated and CI shapes no human does, and every control that assumes a reviewing human degrades to a log entry. Gartner forecasts that more than 65% of engineering teams using agentic coding will treat the IDE as optional by 2027 ([Gartner, 2026-05-20](https://www.gartner.com/en/newsroom/press-releases/2026-05-20-gartner-says-the-market-for-enterprise-ai-coding-agents-is-entering-a-new-phase-of-expansion-and-competitive-realignment)), which, read as a security statement, puts the second group in the majority.

**The security boundary for generative coding is an isolation boundary.** An approval boundary holds only where a human sees the action first, so an organization that scored its maturity on approval controls has scored a shape it is leaving.

A threat model with no adversary sharpens the split. An [[accidental-meltdown|Accidental Meltdown]] is an agent crossing a security boundary while pursuing the user's own goal after an ordinary environmental error. Unwatched, it looks like ordinary progress until the effect lands. Watched, each step is plausible in service of the user's goal, and the prompt shows the action while omitting the reasoning behind it, so approval controls are weak against it even in the shape that keeps them.

## The Five Shapes

| Shape | Where the process runs | Human sees the action | Boundary that carries the weight |
| --- | --- | --- | --- |
| **Interactive local** | Developer workstation or SSH host, foreground | Yes, per-action prompt | Permission rules; OS sandbox reduces prompt volume |
| **Sandboxed autonomous local** | Workstation, prompts suppressed | Only the outcome | OS sandbox (Seatbelt / bubblewrap); classifier |
| **Delegated cloud** | Cloud VM, vendor-hosted or self-hosted | Only the resulting branch or PR | VM isolation, egress proxy, scoped credential, branch restriction |
| **CI-runner agent** | Build runner, event-triggered | No, merge review only | Egress allowlist, credential scoping, untrusted-input exclusion covering repository-shipped configuration as well as prose, isolation established before configuration is read |
| **Fleet / parallel** | Many of the above at once | No, at aggregate scale | Inventory, attribution, per-agent identity |

### Interactive local

The terminal, the VS Code and JetBrains extensions, and the Desktop app's Local and WSL environments run Claude Code on the developer's machine, and the Desktop app's SSH environment runs it on a remote machine the developer manages. Remote Control steers such a session from a phone or browser without moving it.[^ccsurfaces]

In Manual mode Claude Code asks before edits and system-modifying commands, runs its built-in read-only commands without asking, writes only inside the working directory, and requires trust verification for a first run in a codebase and for each new MCP server. Prompt fatigue is the documented failure mode and allowlisting the documented answer, which starts converting the shape into the next one. The defaults now make part of that move: Manual remains the starting mode for Enterprise, Console, cloud-provider and Claude apps gateway sessions and for `claude -p`, and Pro, Max and Team sessions in a terminal or the VS Code extension start from v2.1.228 in auto mode, where a classifier reviews actions in place of the developer, unless settings choose another mode.[^ccmodes]

### Sandboxed autonomous local

`/sandbox` moves enforcement from the prompt to the operating system and is a write-and-egress control first: sandboxed commands write only inside the working directory, any directories added to the session and the session temp directory, and reach the network through a proxy. Its defaults leave the rest of the filesystem readable, `~/.aws/credentials` and `~/.ssh` included, allow by hostname without terminating TLS, which admits domain fronting past a broad entry such as `github.com` ([Anthropic, fetched 2026-09-18](https://code.claude.com/docs/en/sandboxing)), and let the model retry a failed command outside the sandbox. [[securing-agentic-coding|Securing Agentic Coding]] carries the mechanisms and the keys that close each default.

The per-command sandbox constrains shell commands and their children, and in-process file tools, MCP servers and hooks run on the host outside it, the asymmetry [[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]] exploited through the unsandboxed Read tool while the shell boundary held. [[anthropic-sandbox-runtime|Anthropic Sandbox Runtime]] brings all three inside one boundary without Docker, as a beta research preview.

### Delegated cloud

A cloud session, started from claude.ai/code, the Claude mobile app, the Desktop app's Cloud environment, `claude --cloud`, a routine or a Claude Tag mention in Slack, runs by default in a vendor-managed VM behind a default-allowlist proxy.[^ccsurfaces] A credential proxy holds the real GitHub token outside the sandbox and issues a scoped credential inside it, push is restricted to the working branch, operations are audit-logged, and the VM is reclaimed after inactivity.[^cccloud] These are the controls the wiki recommends, the [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]] among them, implemented by the vendor instead of assembled by the customer. The trade is custody: the vendor holds the session, its credential and its audit log. A self-hosted environment, in public beta on Team and Enterprise plans, moves the session onto the organization's infrastructure and hands isolation, egress control and git credentials back to it.[^cccloud]

### CI-runner agent

An event-triggered coding agent holds all three [[agents-rule-of-two|Agents Rule of Two]] properties by construction: it reads issue bodies and pull-request comments anyone can write, holds repository and model credentials, and can push and reach the network.

In the first worked example, a prompt injection in issue or pull-request content drove the Claude Code GitHub Action's unsandboxed Read tool at `/proc/self/environ` and took the `ANTHROPIC_API_KEY` out of the workflow ([Microsoft Security, 2026-06-05](https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/)). [[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]] is the second, and it moves the argument with one defect on each side of the model. Its `--yolo` flag suppressed the workflow's tool allowlist, and a public issue's injection reached the same credential primitive through `/proc/$PPID/environ`. Its folder-trust defect let headless Gemini CLI read and act on a `.gemini/` tree from a fork's pull request **before the harness sandbox initialized** and before the model reasoned ([Novee Security, 2026-04-30](https://novee.security/blog/google-gemini-cli-rce-vulnerability-cvss-10-critical-security-advisory/)).

Egress allowlisting, credential scoping and untrusted-input exclusion apply once the harness runs, and a trust decision a configuration loader makes at startup lies outside every plane of the reference architecture. **A sandbox is a control only from the moment it exists, and the harness reads attacker-reachable configuration before that moment.** A CI-runner assessment therefore asks what the boundary contains, which the GitHub Action case answers, and when it begins, which the Gemini case answers. Claude Code documents the second for the configuration it reads from a repository: a headless run in a folder never trusted uses the repository's hooks and connects its MCP servers, which run on the host outside the per-command sandbox,[^cctrust] and [[claude-code-control-sheet|Claude Code Control Sheet]] pairs that ordering with the flags that keep the content out.

The standard untrusted-input exclusion missed both defects. It is written against instruction-bearing text, such as issue bodies, comment text and pull-request descriptions, and the folder-trust defect traveled in a configuration directory. Its usual form restricts fork pull requests, and the `--yolo` chain carried its injection in an issue body, on an `issues: opened` trigger open to any account, so the exclusion has to cover the trigger as well as the payload.

### Fleet and parallel

Background and parallel execution, where the market is moving, changes the unit of analysis from the session to the population. The load-bearing controls sit outside the runtime:

- an inventory of deployed harnesses, versions, MCP servers, skills and hooks
- attribution of each action to an agent and a human
- per-agent identity

No first-party harness feature covers them across vendors, and a COTS control-plane category is forming to fill the gap, [[endor-labs-ai-code-governance|Endor Labs AI Code Governance]] among it. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]] shows why the population is the unit. Concurrent evaluation runs pooled findings through shared writable storage, so one run's discovery became fleet capability, and the [[offensive-agent-collective|Offensive Agent Collective]] rebuilt its channel two days after a complete remediation because a model trained during the incident carried the technique in its weights.

## Supporting Evidence

- [[microsoft-cli-coding-agent-adoption-study|Microsoft CLI Coding Agent Adoption Study]] (2026-07-01, tens of thousands of engineers): adoption spreads by organizational proximity, with **+216% odds** of first use when more than a quarter of skip-level peers already use the tool, and a merged-PR lift of **+24.0%** that persists without decay across four months ([arXiv:2607.01418](https://arxiv.org/html/2607.01418v1)). Diffusion this strong outruns any governance program that enumerates first, which is [[shadow-automation|Shadow Automation]] with a coefficient.
- [[gartner-mq-enterprise-ai-coding-agents-2026|Gartner MQ for Enterprise AI Coding Agents]] (2026-05-20): autonomous multistep execution defines the category, and native MCP support is an inclusion criterion.
- [[guardfall-shell-injection-audit|GuardFall Shell-Injection Audit]] (2026-06-30): **ten of eleven** open-source coding agents were driven into arbitrary shell execution through malicious MCP servers, injected READMEs and compromised Makefiles ([Adversa AI](https://adversa.ai/blog/opensource-ai-coding-agents-shell-injection-vulnerability/)). The delivery channel is the repository, so the finding reaches every shape.
- The Gemini CLI advisory is, at **CVSS 10.0**, the highest-severity coding-agent advisory the wiki carries ([GitHub Advisory Database](https://github.com/advisories/GHSA-wpqr-6v78-jr5g)).
- Anthropic's documentation describes six isolation approaches and states that Claude Code enforces one itself, the Bash sandbox delivered through managed settings. Dev containers are a convention, and containers and VMs rely on device management ([Anthropic, fetched 2026-09-24](https://code.claude.com/docs/en/sandbox-environments#enforce-isolation-across-an-organization)).
- The OpenAI–Hugging Face incident, reconstructed at Black Hat USA ([[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]), was model evaluation and maps onto the sandboxed autonomous shape by analogy. Within that shape it is the strongest available evidence that the shape sets the exposure: agents with internet disabled reached the internet through the one package manager they could call, and no human was positioned to see any action first.
  - **Isolation was defeated without the isolation boundary failing.** The network policy held throughout, and the escape ran through a permitted dependency with broader reach than its callers, so enumerating what a sandboxed agent may call does not enumerate what it can reach.

## Counter-Evidence

- The interactive shape persists, and the Microsoft study does not measure how much work ran unattended, so the adoption data supports the asserted distribution shift in direction and leaves its size unmeasured.
- Vendor-side controls move faster than the argument assumes: TLS-terminating proxy support, strict allowlists, credential masking and an action classifier all shipped within two months, and the vendor calls stronger TLS-aware network isolation, the limitation the egress argument turns on, an active area of development ([Anthropic, fetched 2026-09-24](https://code.claude.com/docs/en/sandboxing#security-limitations)).
- The IDE-optional forecast is an analyst prediction with no published methodology, and the caution in [[anti-patterns-and-failure-modes|RA and CMM Anti-Patterns and Failure Modes]] against treating analyst forecasts as evidence applies.

## Changes to the RA and CMM

The reference architecture carries the five shapes as five rows. For the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] the consequence is a set of scoring corrections inside the existing domains:

- **[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]].** The developer principal resolves shape by shape: a workstation identity in the two local shapes, a vendor-managed session identity in delegated cloud, a workflow identity in CI, and one identity per agent at fleet scale. Each carries its own network-access policy, so grading one developer principal scores a shape the organization may not be running. A third persona, the user of the AI application on whose behalf the agent writes and runs code, moves the assessable question from who wrote the code to what the code may reach, because generating code on that user's request is the product working as designed. The proposed test is that such code reaches no production data store, a criterion D2's level definitions, read on 2026-09-24, do not carry.
- **[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]].** A text-matching command guard is not a policy decision point ([[guard-canonicalization-gap|Guard Canonicalization Gap]]), so scoring D3 on an allowlist of Bash patterns overstates it by roughly a level. An allowlist an autonomy flag can suppress fails the same way, as Gemini CLI's `--yolo` did before 0.39.1, so an assessor shows that the guard runs before grading what it holds. A managed permission policy the developer's session cannot widen survives both tests, and the harness resolving it is the decision point D3 L3 grades, on the substitute evidence [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] names, with the record that one vendor's code both runs the model and enforces the policy.
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]].** An OS boundary's coverage is stated in two dimensions, what it contains and when it starts, and "sandboxed" means shell commands only unless the whole process is wrapped.
- **[[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]].** A hostname allowlist without TLS termination grades as a misconfiguration control.
- **[[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]].** Sessions routed through a gateway or a non-Anthropic provider appear in neither the Enterprise Analytics API nor the Claude Code Analytics API, and OpenTelemetry export, which keeps working on those routes, is the rebuild path and is not automatic.
- **[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Supply Chain and AI-BOM]].** The harness configuration tree is in scope, per [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]], its workspace-local half is exploited on the record, and the fleet inventory extends it.
- **[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]].** Approval fatigue moves a deployment between shapes, and on Pro, Max and Team plans a version upgrade makes auto mode the starting mode, so an assessor reads a falling prompt count against a rising action count as a shape change that requires re-assessment.

[[securing-agentic-coding|Securing Agentic Coding]] catalogs the controls for these shapes by reference-architecture plane. [[claude-code-control-sheet|Claude Code Control Sheet]] maps one harness's controls to criteria in all nine domains and adapts its rows to the five shapes.

## Position history

Created 2026-07-30 from an autoresearch pass restricted to sources published after 2026-04-30.

The reference architecture previously carried one row for "generative coding tool (Copilot, Cursor, Claude Code)," with controls that assume a developer reviewing each action. Five rows replaced it the same day, one per shape, and its shape-mapping section states the rule for when a shape earns a row: the load-bearing controls change, and a product name alone earns none. The revision moved the load-bearing plane for three of the five shapes from Control (approval) to Runtime (isolation).

Revised 2026-08-16 on ingest of GHSA-wpqr-6v78-jr5g, with the five shapes unchanged. The CI-runner shape's control question, what is inside the isolation boundary, gained a second part, when the boundary starts, because the folder-trust defect executed before the sandbox existed and placed a failure in this shape outside every plane of the reference architecture for the first time.

## Open Sub-Questions

- No published measurement shows how much agentic-coding work runs unattended versus supervised, so every claim about the distribution shift, this one included, is inferential.
- Whether the classifier-based permission mode is a control or a heuristic is unresolved. It is a model reviewing a model's actions, the concern [[recursive-prompt-injection|Recursive Prompt Injection (and Semantic Gaslighting)]] names and the [[plan-validate-execute|Plan-Validate-Execute Pattern]] exists to avoid.
- The GuardFall audit excluded the three harnesses with the largest enterprise install base, so their guard implementations are unevaluated in public. The Gemini CLI advisory is one data point from that group and reports what two researchers found.
- In the vendor documentation the wiki holds, the ordering of isolation against workspace-configuration loading is stated for the configuration Claude Code reads from a repository and for no other harness it tracks. The ordering has also reached the record by failing: in the Gemini CLI advisory, and in [[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]], where a `git` subprocess the harness spawns ran a command named in the repository's `.git/config` on the host, outside the sandbox and unseen by the permission model, across seven coding agents ([Manifold Security, 2026-09-01](https://www.manifold.security/blog/ai-coding-agents-git-hijack)). Claude Code is one of the seven: in both of its findings the command ran before the workspace-trust prompt was accepted, and one was unpatched at publication.
- See [[wiki/gaps/_index|Gaps Index]].

## Notes

[^ccsurfaces]: [Anthropic — Use Claude Code in the cloud](https://code.claude.com/docs/en/claude-code-on-the-web), fetched 2026-09-24: the five surfaces its list names for starting a cloud session, Claude Tag named beside them under "Cloud environments", and terminal, IDE and Desktop Local sessions running on the developer's machine. [Desktop application, "Environment configuration"](https://code.claude.com/docs/en/desktop#environment-configuration) defines the Local, Cloud, SSH and WSL environments.
[^ccmodes]: [Anthropic — Choose a permission mode, "Which mode a session starts in"](https://code.claude.com/docs/en/permission-modes#which-mode-a-session-starts-in), fetched 2026-09-24: `auto` for Pro, Max and Team terminal and VS Code sessions from v2.1.228 (v2.1.233 on native Windows), Manual for the other plans, providers and `claude -p`. Manual mode's behavior is in [Security, "Permission-based architecture"](https://code.claude.com/docs/en/security#permission-based-architecture) and the sections after it.
[^cccloud]: [Anthropic — Choose a sandbox environment, "Cloud sessions"](https://code.claude.com/docs/en/sandbox-environments#cloud-sessions), fetched 2026-09-24, with [Security, "Cloud execution security"](https://code.claude.com/docs/en/security#cloud-execution-security) for branch restriction, audit logging and reclamation, and [Self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments) for the public beta.
[^cctrust]: [Anthropic — Configure permissions, "What runs before you trust a folder"](https://code.claude.com/docs/en/permissions#what-runs-before-you-trust-a-folder), fetched 2026-09-24: settings-file hooks, the `env` block and `apiKeyHelper` "Used", `.mcp.json` servers "Connected without asking, approved or not". [Choose a sandbox environment, "Sandboxed Bash tool"](https://code.claude.com/docs/en/sandbox-environments#sandboxed-bash-tool) states that MCP servers and command hooks "run unconstrained on the host".
