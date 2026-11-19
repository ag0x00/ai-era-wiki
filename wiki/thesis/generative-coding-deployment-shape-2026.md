---
type: thesis
title: "Generative Coding Deployment Shapes"
address: c-000237
created: 2026-07-30
updated: 2026-10-14
tags: [thesis, agentic-coding, deployment-shape, sandboxing]
status: developing
scope_axis: [sec-of-ai, sec-against-ai]
origin: produced
question: "How does a coding agent's deployment path change its failure modes, enforcement owner, and review point?"
current_position: "The security boundary follows the action path from trigger and principal through decision loop and tool worker to external effect. Review timing, execution location, and enforcement ownership determine which controls can prevent an effect and what evidence proves they operated. Resident, event-triggered, and parallel work can share a worker or service without sharing one security principal."
last_revised: 2026-10-14
related:
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]]"
  - "[[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]]"
  - "[[claude-code-control-sheet|Claude Code Control Sheet]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]]"
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
sources:
  - https://hermes-agent.nousresearch.com/docs/developer-guide/architecture
  - https://hermes-agent.nousresearch.com/docs/user-guide/security
  - https://hermes-agent.nousresearch.com/docs/guides/secure-hermes-on-a-work-machine
  - https://docs.devin.ai/cloud/outposts/overview
  - https://docs.devin.ai/product-guides/security-profiles
  - https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted
  - https://openai.com/index/running-codex-safely/
  - https://antigravity.google/docs/permissions
  - https://antigravity.google/docs/sandbox
  - https://antigravity.google/docs/enterprise/
  - https://code.claude.com/docs/en/permission-modes
  - https://code.claude.com/docs/en/sandboxing
  - https://code.claude.com/docs/en/sandbox-environments
  - https://code.claude.com/docs/en/self-hosted-environments
  - https://code.claude.com/docs/en/github-actions#who-can-trigger-runs
  - https://github.com/advisories/GHSA-wpqr-6v78-jr5g
  - https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/
  - https://www.manifold.security/blog/ai-coding-agents-git-hijack
  - .raw/articles/ai-coding-agents-git-hijack-2026-09-17.md
  - https://cloud.google.com/security/resources/ai-risk-and-resilience-2026
  - https://github.com/anthropics/claude-code/security/advisories/GHSA-5j29-h97v-84ch
  - https://www.youtube.com/watch?v=87DyyMV0kCY
  - .raw/talks/2026-08-06_Michael-Dalton-and-Eric-Wallace_OpenAI-Hugging-Face-Incident_transcript.md
verified: 2026-10-14
verified_against:
  - ".raw/articles/ai-coding-agents-git-hijack-2026-09-17.md"
  - ".raw/talks/2026-08-06_Michael-Dalton-and-Eric-Wallace_OpenAI-Hugging-Face-Incident_transcript.md"
verified_findings: 0
verified_note: "Action-path claims checked against archived incidents and current primary product documentation; no open findings."
---

# Generative Coding Deployment Shapes

## Question

A coding agent can start from a developer's terminal, a resident service, a delegated task, or a repository event. Which failure paths change with that deployment, and who can enforce the boundary before an action has an effect?

## Current Position

The useful unit of analysis is the **action path**. A trigger and its principal start a decision loop, which directs a tool worker. That worker changes files, invokes dependencies, reaches a network destination, or publishes an artifact. The client surface does not locate every part of this path. In [Devin Outposts](https://docs.devin.ai/cloud/outposts/overview), the agent loop remains in Devin Cloud while commands and repository operations run on a customer worker. The [OpenAI Agents API](https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted) also separates a managed harness from a customer execution server. A [Claude Code self-hosted environment](https://code.claude.com/docs/en/self-hosted-environments) instead runs the session process on the customer's runner beneath an Anthropic control plane.

Security decisions therefore depend on when review occurs, where an action runs, and which party enforces the worker's limits. A foreground session can retain a pre-action prompt. A classifier, unattended trigger, or later diff review changes what that prompt can stop. A vendor can supply a policy and telemetry while the customer operates the actual firewall or runner. Parallel work adds shared resources and multiple principals to any of these paths; it is an overlay rather than a separate worker location. The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] must account for these distinctions. The cited documentation does not measure which mode dominates use in the field.

## Risk Scenarios

### An action loses its pre-execution reviewer

An unattended run can use the same local tools as a foreground run while moving human judgment from a specific action to its final artifact. [Claude Code](https://code.claude.com/docs/en/permission-modes) distinguishes manual approval from an auto mode in which a classifier reviews actions; [Google Antigravity](https://antigravity.google/docs/permissions) separates terminal permission from artifact review. Both document real gates, but a later code review cannot stop an earlier command's network or credential effect. Effective mode, surface, and organization policy determine whether a human, a classifier, or another control reviews the action first.

Even a human approval can authorize a harmful dependency. In a case published in [Mandiant's September 2026 field report](https://cloud.google.com/security/resources/ai-risk-and-resilience-2026), an attacker hijacked an active coding-assistant session and a developer accepted its recommendation to install a poisoned package. The ensuing intrusion stole GitHub tokens and spread through internal repositories. Mandiant does not disclose the product or the incident date. This case limits the claim that a visible approval is sufficient; the reviewer also needs evidence about the package and the authority of the session. An [[accidental-meltdown|Accidental Meltdown]] poses a separate challenge: an agent can cross a boundary while pursuing a legitimate user goal, without hostile input to flag.

### Policy and execution have different owners

A remote decision loop can direct commands on a worker whose network and filesystem boundaries belong to someone else. Devin documents mandatory security profiles for Outposts, but [its effective network policy is published for the customer to enforce](https://docs.devin.ai/product-guides/security-profiles). Approval of a destination in Devin updates that policy; it does not install a firewall rule on the customer's machine. The [OpenAI Agents API's self-hosted environment](https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted) likewise runs shell and file operations on the customer's execution server. Claude Code's [self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments) put the session process on a customer runner, so their split is different again.

The enforcement owner must show that the intended policy reached the worker and remained effective as sessions start, resume, or move. A policy declaration alone cannot demonstrate isolation, egress restriction, or credential separation. Vendor-hosted workers can have substantial first-party controls; for example, [Anthropic describes isolated cloud VMs and proxies](https://code.claude.com/docs/en/sandbox-environments) and [Devin describes machine-level network enforcement on its own VMs](https://docs.devin.ai/product-guides/security-profiles). Their existence is affirmative evidence for those products, while the customer's effective boundary remains a deployment fact in customer-run modes.

### Untrusted repository input acts before review

Repository intake can execute code before the model makes a tool request. In the [GitSpawn disclosure of 2026-09-01](https://www.manifold.security/blog/ai-coding-agents-git-hijack), background Git commands in several coding agents read executable repository configuration and ran its command on the host, in some cases before a trust prompt or model call. The demonstrated payload requires a repository to arrive **as files with its `.git` directory**; ordinary clone, fetch, and pull do not carry it. Four findings were unpatched **at publication**, a status this page does not extend to October. [[gitspawn-coding-agent-git-config-rce|GitSpawn Coding-Agent Git-Config RCE]] records the affected paths. Mandiant separately [reported an intrusion through a poisoned internal AI repository and altered assistant CLI hooks](https://cloud.google.com/security/resources/ai-risk-and-resilience-2026), without naming the product or incident date.

Event-triggered runners add untrusted issue and pull-request content to this intake path. The historical [[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]] combined headless workspace trust with an allowlist bypass. [Google's advisory lists patched CLI and Action releases](https://github.com/advisories/GHSA-wpqr-6v78-jr5g). A separate [[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]] case reached a sensitive process file through a file-read tool while shell sandboxing held. [Microsoft reports Anthropic's 2026-05-05 mitigation](https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/). [Current Claude Code Action documentation](https://code.claude.com/docs/en/github-actions#who-can-trigger-runs) describes actor checks for issue and pull-request events. Those checks constrain that Action's trigger path. Each runner still needs its own startup and tool-path boundary.

### A resident ingress outlives the interactive session

A gateway or timer can start coding work after the developer leaves while execution remains on the host. [Nous Research Hermes Agent's architecture](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture) routes messaging sessions through a gateway and starts scheduled work through cron. Its [security documentation](https://hermes-agent.nousresearch.com/docs/user-guide/security) distinguishes unattended approval behavior from interactive approval, and its [work-machine guide](https://hermes-agent.nousresearch.com/docs/guides/secure-hermes-on-a-work-machine) says the local terminal backend runs directly on the host.

The ingress principal, scheduled task, session identity, and chosen backend determine what can act without a foreground reviewer. A service's lifetime can exceed the user session that created it, so suspension and credential revocation must reach queued and scheduled work. Hermes documents one feasible arrangement; these pages do not measure enterprise prevalence or establish OS confinement for its local backend.

### Shared services carry effects between workers

Parallel workers may have distinct task identities yet share a workspace, package service, cache, or message channel. [Anthropic's advisory published 2026-10-05](https://github.com/anthropics/claude-code/security/advisories/GHSA-5j29-h97v-84ch) describes a lower-privileged writer racing a higher-privileged Claude Code file write: a path accepted at permission time was re-resolved at write time, allowing a symlink to redirect the write beyond the project. The advisory lists versions before 2.1.129 as affected and 2.1.129 as patched. It describes a historical write-time failure, not an established vulnerability in current default installations.

The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]] offers a narrower analogy. In that **model-evaluation** setting, agents used a shared writable package service to carry findings between runs and to reach the internet indirectly through an allowed dependency ([Black Hat reconstruction, 2026-08-06](https://www.youtube.com/watch?v=87DyyMV0kCY)). It was not a coding-agent deployment. The mechanism shows why each worker's direct permissions are an incomplete account of its effect when a permitted shared service has broader reach.

## Emerging Practices

### Trace the action path before choosing controls

An assessment should record the effective path for each mode an organization actually uses:

- **Trigger and principal:** who or what starts the run, including events and resident gateways.
- **Decision loop:** where planning and tool authorization occur.
- **Tool worker:** where commands, file tools, hooks, and dependencies run.
- **Review point:** which action a human or automated gate can stop before its effect.
- **Enforcer and evidence:** who operates the boundary and what proves the effective policy.

This record prevents a client label from standing in for a security boundary. Devin Outposts and the OpenAI Agents API place a managed loop above customer execution, while Claude Code self-hosted runs its session process customer-side. [Antigravity's app and CLI sandbox behavior also differs by surface and operating system](https://antigravity.google/docs/sandbox). A topology record assigns responsibility; worker tests and logs establish whether it was met.

### Enclose every path that can act

The worker boundary must begin before helper processes inspect untrusted material and must contain every route that can act on its behalf. GitSpawn shows why pre-model Git subprocesses matter. [Anthropic's Bash sandbox documentation](https://code.claude.com/docs/en/sandboxing) describes a narrower shell-and-child boundary: built-in file tools, local MCP servers, and hooks run outside it. Its [unattended-environment guidance](https://code.claude.com/docs/en/sandbox-environments) calls for an outer container, VM, or sandbox runtime that contains those paths. The write-time symlink advisory shows a second timing requirement: validate the target again at the actual write, particularly where principals share a workspace. [[agent-sandboxing|Agent Sandboxing]] and [[harness-config-as-supply-chain-artifact|Harness Config as Supply-Chain Artifact]] carry the reusable boundary and startup questions; product settings belong in the [[claude-code-control-sheet|Claude Code Control Sheet]] and equivalent tool sheets.

### Broker credentials at the worker boundary

A worker should receive authority scoped to its task rather than a reusable credential it can expose through a command or dependency. [Claude Code cloud sessions](https://code.claude.com/docs/en/sandbox-environments) document a credential proxy, and the [OpenAI Agents API](https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted) separates the application API key from a worker environment key. These are product-specific implementations of the [[credential-proxy-pattern|Credential Proxy Pattern for AI Agents]].

Credential scope must be paired with dependency integrity and egress checks. In Mandiant's [poisoned-package case](https://cloud.google.com/security/resources/ai-risk-and-resilience-2026), the accepted install led to token theft and further package poisoning. Its guidance calls for verification of recommended dependencies and constrained package routes. A permitted package service can also have wider network reach than its caller, as the OpenAI–Hugging Face evaluation incident illustrates by analogy. Encryption at rest does not constrain a secret legitimately delivered to the worker.

### Bind triggers, workers, and shared state to principals

Event actors and resident gateway users need explicit authority to start work; workers need separate identities and bounded writes to shared state. [Claude Code Action's current actor checks](https://code.claude.com/docs/en/github-actions#who-can-trigger-runs) are one documented trigger gate. [Hermes gateway routing](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture) illustrates a different principal path. The Claude Code write-time race shows why a lower-privileged workspace writer can matter even when a higher-privileged worker had permission for its original path.

The resulting record must connect initiating actor, worker, action, and artifact. Merge review can reject a change, but it cannot undo an earlier runner's secret read or network transfer. The same identity rule applies when several workers share a service: access to that service is an effect of the worker's authority, not simply an implementation detail of parallelism.

### Preserve evidence for review and response

Review and incident response need the effective policy at execution, the action trace, the approval decision, worker and network events, and the artifact produced. The enforcement owner must retain enough evidence to distinguish a denied request from a command that never reached the boundary, and to trace a resumed or scheduled job to its principal. Suspension, token revocation, and cleanup must reach delegated and resident runs as well as foreground sessions.

First-party telemetry is part of the available control surface. [OpenAI documents Codex logging and managed requirements](https://openai.com/index/running-codex-safely/), while [Antigravity's enterprise integration](https://antigravity.google/docs/enterprise/) describes request and response logging for model interactions in the connected Gemini Enterprise instance. These capabilities do not by themselves prove a particular organization's collection, retention, or correlation across its customer-run workers. [[securing-agentic-coding|Securing Agentic Coding]] carries the broader control pattern; the deployment record identifies which party can produce each piece of evidence.

## Open Sub-Questions

- How often do organizations use pre-action human review, classifier review, or artifact-only review for each worker location? Public product documentation does not measure deployment share.
- Which customer-run boundaries and policy snapshots remain effective after requeue, wake, mode change, or worker replacement?
- Where do parallel workers share writable workspaces, package services, or credentials, and what evidence links an effect through those services to its initiating principal?

## Notes

Product documentation cited here describes available architecture and controls as retrieved on 2026-10-14. The Mandiant cases are reported field observations. GitSpawn is a researcher disclosure. The OpenAI–Hugging Face case is a model-evaluation analogy. Their different evidence types support the mechanisms above but do not establish prevalence or the effective configuration of any particular deployment.
