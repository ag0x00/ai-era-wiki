---
type: concept
title: "Agent Escape"
address: c-000298
created: 2026-08-18
updated: 2026-08-18
tags:
  - concepts
  - agentic-ai
  - runtime-security
  - sandboxing
status: active
scope_axis:
  - sec-of-ai
origin: aggregated
complexity: intermediate
domain: "agentic AI runtime security"
aliases: []
related:
  - "[[agent-sandboxing|Agent Sandboxing]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[least-agency-principle|Least Agency Principle]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
sources:
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - ".raw/papers/owasp-ai-exchange-runtime-appsec-threats-2026-08-18.md"
---

# Agent Escape

## Definition

Agent escape is an autonomous agent operating outside its defined security boundary: invoking tools it is not authorized to use, reaching systems outside its assigned scope, or taking actions beyond its assigned task.[^escape-def] The threat classifies as a runtime conventional security threat specific to agentic systems, distinct from the input-side threats (evasion, prompt injection) and the development-time threats covered elsewhere in the Exchange.[^escape-def]

An attacker who achieves agent escape converts a successful compromise — jailbreak, injection, or agent malfunction — into capability beyond the agent's intended scope: unauthorized data access, invocation of tools the agent holds no authorization for, or configuration changes to the agent or its environment.[^escape-def] These are unauthorized side effects that occur despite, or after, alignment holds. Infrastructure-layer enforcement can block an escape attempt even where jailbreak has already succeeded at the reasoning layer.[^escape-def]

## Aliases / Variants

No established variant terms. The Exchange fixes "agent escape" as a name distinct from jailbreak, with its own permalink and control set (below).

## Layer distinction from jailbreak

The [[owasp-ai-exchange|OWASP AI Exchange]] draws agent escape and jailbreak at different layers of the same system, and treats conflating them as a design error.[^escape-def] Jailbreak overrides a model's safety constraints while the agent remains inside its operational boundary; the source's example is an agent producing harmful content it was instructed to refuse. Jailbreak is a reasoning-layer failure: the model's own policy adherence gives way. Agent escape is a policy-enforcement failure at the infrastructure layer: the boundary around what the agent can reach gives way, independent of whether its reasoning stayed aligned.[^escape-def]

Conflating the two produces controls that are only partially effective against each.[^escape-def] A defense tuned against jailbroken output does not stop an aligned agent from reaching an unauthorized tool, and a capability boundary does not stop a jailbroken agent from misusing tools already inside its authorized scope. Control selection follows the layer the failure occurs at: reasoning-layer defenses — session-level drift monitoring, refusal behavior — address jailbreak. Infrastructure-layer defenses — capability-scoped access control, execution sandboxing — address escape.[^escape-controls]

A worked example illustrates the split. A code-execution agent is jailbroken across eight conversational turns through incremental reframing that normalizes exploratory system commands; by turn eight it attempts file-system enumeration it would have refused at turn one. A sandbox restriction confining it to a designated directory blocks the command from producing output: jailbreak succeeds at the reasoning layer, escape is blocked at the infrastructure layer. Session drift monitoring separately flags the progressive relaxation at turn five for human review.[^escape-example] Single-turn jailbreak testing underestimates this risk shape; red-team scope needs multi-turn and multi-session paths alongside single-turn testing.[^escape-controls]

The distinction has a bound. An agent with very broad authorized scope can cause harm through jailbreak alone, without technically escaping that scope, because the boundary itself was drawn wide enough to contain the harmful action.[^escape-limits] Capability enforcement retrofitted onto a system where tool access was historically managed through prompts alone is correspondingly difficult to add once that pattern is established.[^escape-limits]

## Controls against escape

The source pairs four controls with escape prevention:

- **Capability-based access control** at the backend, restricting tool sets, data sources, and action space independently of the model's own reasoning. An agent that infrastructure denies a tool cannot escape into that tool regardless of jailbreak success.
- **Role and scope boundary verification** at every tool invocation: a valid call from an agent performing an out-of-scope task is an escape event even when the tool itself is individually authorized.
- **Session-level oversight** for progressive-drift detection, paired alongside escape prevention: infrastructure enforcement blocks the specific out-of-scope action, and session monitoring catches alignment bypass accumulating gradually across turns, which infrastructure enforcement alone does not detect.
- **Multi-turn, multi-session red-team scope** for both jailbreak and escape scenarios, since single-turn jailbreak testing underestimates production agentic risk.[^escape-controls]

## Isolation and containment

[[agent-sandboxing|Agent Sandboxing]] is the runtime operational control the Exchange pairs with escape prevention. Each live agent runs in a bounded environment — compute, memory, storage, network, inter-process communication — so that compromise, malfunction, or prompt injection cannot spread beyond its designated boundary. The objective is blast-radius containment: a hijacked agent must not reach another agent's memory, credentials, or services, or exfiltrate data through an uncontrolled network path.[^sandbox-def]

### Execution environment isolation

Each agent runs in a dedicated container, microVM, or OS-enforced sandbox with separate PID, network, mount, and UID namespaces, on a read-only root filesystem with ephemeral writable layers discarded on termination. Mandatory access control (seccomp, AppArmor, SELinux) disables capabilities such as raw sockets and privilege escalation. Shared model inference is isolated where feasible, so one agent's context cannot leak into another's.[^sandbox-impl]

### Network segmentation

Egress defaults to deny, permitting only task-required endpoints, routed through a monitored proxy or service mesh that enforces allowlists and logging. An orchestration layer or authenticated message bus replaces direct agent-to-agent network paths. DNS resolution is restricted, and agents processing untrusted content are segmented away from sensitive internal services.[^sandbox-impl]

### Platform enforcement and termination

The platform restricts process spawning, filesystem access outside the agent's workspace, self-configuration changes, and host interaction, and monitors for escape indicators — unexpected syscalls, forbidden paths, non-permitted connections. Tool credentials for MCP servers and APIs sit outside the sandbox in a controlled credential store. Clean termination on task completion or forced stop destroys transient state, cached data, and any in-sandbox credentials.[^sandbox-impl]

### Resource quotas

CPU, memory, API volume, tool-invocation count, and wall-clock time are bounded per agent as platform-enforced limits, independent of the agent's own self-management — the same [[least-agency-principle|Least Agency Principle]] rate and resource controls applied at the sandbox boundary.[^sandbox-impl]

Sandboxing operates as one layer of a defense-in-depth stack, paired with escape prevention and capability-based least privilege.[^sandbox-impl]

## Limits

The source states its own containment control does not close every gap the layer distinction implies:

- **Container or hypervisor escape undermines containment outright** — a failure mode inherent to the isolation mechanism itself, independent of how carefully it is configured.
- **Network segmentation cannot stop exfiltration through APIs the agent is legitimately permitted to call** — an egress allowlist permits that traffic by design, so segmentation has nothing to block.
- **Shared inference, credential, and policy services create implicit channels between agents**, regardless of namespace isolation elsewhere in the stack.
- **Sandbox overhead scales with the number of concurrent agents**, and OS-specific behavior can weaken an isolation configuration that held on a different host.[^sandbox-limits]

These admitted gaps describe a failure category distinct from the escape/jailbreak split above: breakdowns internal to the infrastructure-layer control itself, occurring independent of whether an agent's reasoning ever attempted to cross the boundary.

> [!gap] Single-source extraction
> Every claim on this page traces to one document: the OWASP AI Exchange runtime
> application security threats deep dive. No second taxonomy the wiki tracks names agent
> escape as a category distinct from jailbreaking, so the layer distinction is recorded
> here on the Exchange's authority alone. Corroboration is a question for the remaining
> Exchange deep dives and for the next agentic threat taxonomy ingested.

## Occurrences

The [[owasp-ai-exchange|OWASP AI Exchange]] defines agent escape and agent sandboxing in its runtime application security threats deep dive, sections 4.8 and 4.9, as a paired threat and control.[^escape-def]

## Related Concepts

- [[agent-sandboxing|Agent Sandboxing]] — the runtime containment practice; this page's isolation and containment section summarizes the Exchange's control text, while the practice page carries the broader implementation landscape and incident record.
- [[least-agency-principle|Least Agency Principle]] — the capability-scoping principle that agent escape's backend access controls implement at the tool-invocation layer.
- [[owasp-ai-exchange|OWASP AI Exchange]] — source framework; hub page for the wider control catalogue.
- [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] — splits escape across two planes by the path it takes. An agent reaching a filesystem path or network destination outside its scope is stopped by the Runtime plane; an agent invoking an unauthorized tool stays inside its process and crosses a policy boundary the Control plane owns.

## Notes

[^escape-def]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/). Definition, layer distinction from jailbreak, impact, and conflation warning.
[^escape-controls]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/). Control list, oversight pairing, and red-team scope statement.
[^escape-example]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/). Worked example (eight-turn jailbreak, sandbox-blocked escape, turn-five drift flag).
[^escape-limits]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/). Limitations paragraph.
[^sandbox-def]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/). Definition and containment objective.
[^sandbox-impl]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/). Implementation text: execution environment isolation, network segmentation, platform enforcement, clean termination, resource quotas.
[^sandbox-limits]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/). Limitations paragraph.
