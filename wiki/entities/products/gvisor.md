---
type: entity
entity_type: product
title: "gVisor"
created: 2026-05-03
updated: 2026-09-01
tags:
  - products
  - sandboxing
  - runtime-security
  - oss
status: developing
vendor: "Google (open source)"
license: "Apache 2.0"
related:
  - "[[firecracker]]"
  - "[[agent-sandboxing]]"
  - "[[gke-agent-sandbox]]"
  - "[[camel-pattern]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[defending-code-harness|defending-code-harness]]"
  - "[[anthropic|Anthropic]]"
  - "[[semgrep|Semgrep]]"
sources:
  - "https://gvisor.dev/docs/"
  - ".raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md"
---

# gVisor

gVisor is a Google-developed, open-source (Apache 2.0) container sandbox that provides kernel-level isolation without requiring a full virtual machine. Rather than running containers directly on the host kernel, gVisor interposes a user-space kernel (called **Sentry**) between the container and the host OS, intercepting and mediating system calls. This limits the blast radius of a compromised container to the gVisor sandbox boundary, not the host kernel.

## Mechanism

gVisor implements the Linux system call interface in user space (Go). When a containerized process makes a syscall, it is handled by Sentry rather than forwarded to the host kernel. Sentry itself makes a minimal set of host syscalls through a restricted interface (Gofer for filesystem, Sentry for network). The attack surface exposed to a compromised container is the gVisor implementation.

Runtime options:
- **runsc** — the OCI-compatible runtime shim; drop-in replacement for runc in Docker/Kubernetes
- **KVM mode** — Sentry runs in a hardware-isolated VM using KVM for additional separation

## Comparison with Firecracker for agent sandboxing

Both [[firecracker|Firecracker]] and gVisor provide stronger isolation than standard Docker containers. The choice depends on the deployment context:

| Dimension | [[firecracker\|Firecracker]] | gVisor |
|---|---|---|
| Isolation mechanism | Hardware VM (KVM) | User-space kernel (syscall interception) |
| Boot time | ~125ms | Near-instant (process startup speed) |
| Memory overhead | ~5MB + guest OS | Lower; no guest OS |
| Compatibility | Requires Firecracker VMM; not OCI-native | OCI-native; drop-in for Docker/Kubernetes |
| Syscall surface to host | Near-zero (VM boundary) | Reduced (gVisor Sentry surface) |
| Best for | Per-task agent isolation in managed environments | Container-native environments; Kubernetes sidecars |

For agentic AI workloads: Firecracker is preferred when the deployment model is **per-task VM** (strong isolation, higher overhead); gVisor is preferred when the deployment model is **containerized agent per request** and Kubernetes is the orchestration layer.

## Default runtime of GKE Agent Sandbox

[[gke-agent-sandbox|Agent Sandbox]] (Kubernetes SIG Apps, 2026) makes gVisor's container-native fit concrete: it is the **default runtime class** for the sandbox CRDs, the same isolation Google uses for Gemini. A `SandboxTemplate` pins the runtime class, so gVisor is the baseline while [Kata Containers](https://katacontainers.io/) remains a one-field swap for a hardware-VM boundary on the highest-risk tiers. This is the deployment shape the Kubernetes-sidecar row above anticipated, now shipping as a first-class primitive rather than a hand-wired `runsc` runtime class.

### gVisor in an agentic vulnerability-discovery pipeline

gVisor is also the isolation boundary of an open-source vulnerability-discovery harness that executes attacker-crafted input by design. [[defending-code-harness|`defending-code-reference-harness`]], published by [[anthropic|Anthropic]] under Apache 2.0 and forked by [[semgrep|Semgrep]], pairs a gVisor sandbox with an egress allowlist, and counts a finding only where a crafted input crashes the target under AddressSanitizer and reproduces three times in an isolated container.[^semgrep] The deployment is a per-run agentic pipeline rather than a Kubernetes sidecar, which extends the container-native case above rather than illustrating it.

## Role in the RA Runtime plane

In the [[agentic-ai-security-reference-architecture|RA]] Runtime plane, gVisor appears alongside Firecracker in the **Sandbox / containment** capability row. Both are classified OSS / Mature. The choice of Firecracker vs. gVisor is a deployment-shape decision:

- **Multi-agent mesh on Kubernetes** uses gVisor (runsc runtime class; OCI-native)
- **Serverless / task-isolated agents on bare metal or managed VMs** uses Firecracker

The FOSS/small-team recommended stack in the RA lists "Firecracker or gVisor" as equivalent options for per-task sandboxing.

## Security posture for agents

gVisor provides **defense in depth** against agent-level compromises:

- A prompt injection that achieves arbitrary code execution in the agent process is confined to the gVisor sandbox — host filesystem, other containers, and the host kernel are not directly reachable
- Network egress from the sandboxed container still flows through the host network stack, so egress filtering (the Egress plane) must be applied separately
- gVisor does not prevent the agent from making malicious *tool calls* via legitimate interfaces — it limits the blast radius of *code execution*, not API abuse

> [!note]
> gVisor's Sentry is a large attack surface relative to Firecracker's hardware boundary. The security model assumes Sentry is correct — a bug in Sentry's syscall implementation could allow a container escape. For the highest-security agentic deployments, Firecracker's hardware VM boundary provides stronger assurance.

## Notes

[^semgrep]: [Semgrep — Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses), July 2026 (no day-level date exposed; author not named). The licence and unmaintained/forked status are human-written; the gVisor-plus-egress-allowlist pairing and the AddressSanitizer/three-reproduction finding definition are from Semgrep's LLM-generated repository summary.
