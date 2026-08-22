---
type: product
title: "GKE Agent Sandbox"
address: c-000208
created: 2026-06-11
updated: 2026-06-11
tags:
  - products
  - sandboxing
  - runtime-security
  - kubernetes
  - oss
status: developing
vendor: "Google Cloud (managed) / Kubernetes SIG Apps (open source)"
license: "Apache 2.0"
org: "[[google|Google]]"
scope_axis:
  - sec-of-ai
related:
  - "[[gvisor]]"
  - "[[firecracker]]"
  - "[[agent-sandboxing]]"
  - "[[agent-sandbox-isolation-landscape]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[google]]"
sources:
  - "https://cloud.google.com/blog/products/containers-kubernetes/bringing-you-agent-sandbox-on-gke-and-agent-substrate"
  - "https://github.com/kubernetes-sigs/agent-sandbox"
  - "https://docs.cloud.google.com/kubernetes-engine/docs/concepts/machine-learning/agent-sandbox"
  - "https://www.infoq.com/news/2026/05/gke-agent-sandbox-hypercluster/"
---

# GKE Agent Sandbox

**Agent Sandbox** is an open-source Kubernetes controller that runs isolated, stateful, single-replica workloads — the shape an AI agent's code-execution runtime takes when it must run untrusted, model-generated code without reaching the host. It is a [Kubernetes SIG Apps](https://github.com/kubernetes-sigs/agent-sandbox) subproject (Apache 2.0), announced at KubeCon NA 2025 and given a managed GKE delivery at Google Cloud Next '26 in May 2026.[^infoq] The project's claim is structural: that Kubernetes itself should be the agent runtime, with kernel isolation supplied as an open primitive rather than a proprietary platform feature.

## Contribution to Kubernetes

Agent Sandbox introduces custom resources that model a sandbox as a first-class workload. Coverage typically names three, but the repository defines four CRDs:[^repo]

| CRD | Role |
|---|---|
| **Sandbox** | The core resource: a declarative API for a single stateful pod with stable identity and persistent storage |
| **SandboxTemplate** | A reusable blueprint (image, resources, runtime class, network policy) — the security baseline applied to every sandbox spawned from it |
| **SandboxClaim** | A transactional request that draws a ready sandbox from a warm pool, hiding the underlying configuration from the caller (an ADK or LangChain orchestrator, for example) |
| **SandboxWarmPool** | A pool of pre-warmed sandboxes for fast allocation — the mechanism behind sub-second provisioning, omitted from the headline "three primitives" framing |

The `SandboxTemplate` / `SandboxClaim` split maps onto how platform teams already separate a policy blueprint from a request for an instance of it: the template is authored once by the platform owner, and application code claims environments against it without re-declaring the pod spec or its isolation settings.

## Isolation and performance

Isolation defaults to [[gvisor|gVisor]], the same user-space-kernel sandbox that isolates Gemini; the runtime class is pluggable, so [Kata Containers](https://katacontainers.io/) and other hardened runtimes can be substituted.[^blog] A default-deny Kubernetes network policy ships with the template baseline.[^blog]

The managed GKE delivery reports a per-cluster allocation rate of **300 sandboxes per second** at sub-second latency, with **90% of allocations completing within 200 milliseconds**, and claims up to **30% better price-performance** on Axion (Arm) processors against comparable hyperscaler instances.[^blog] Warm pools keep cold-start under one second; Pod Snapshots suspend idle sandboxes and resume them in seconds, and suspended-VM standby buffers back the cold pool at lower cost.[^blog] [Lovable](https://lovable.dev/) runs production workloads on Agent Sandbox at a reported **200,000+ new AI-generated projects per day**.[^infoq]

## Portability — the load-bearing claim

The controller runs on any conformant Kubernetes cluster, not only GKE; the repository states no GKE-specific requirement.[^repo] This is the difference that matters for adoption: a team on EKS or AKS can install the same CRDs and controller and get the primitive without migrating clouds. The managed GKE offering adds the warm-pool performance envelope, Axion price-performance, and Pod Snapshots; the isolation contract itself is portable.

## Position among sandbox offerings

Coverage describes Agent Sandbox as "the only native agent sandbox offering among the three major hyperscalers."[^infoq] That phrasing needs precision, because AWS and Azure both ship sandboxes:

- **AWS** exposes a code-interpreter sandbox inside Bedrock AgentCore, and **Azure** ships a per-session microVM sandbox in Foundry hosted agents (public preview) — both already recorded in [[agentic-ai-security-cmm-d4-runtime-guardrails|D4 of the CMM]].
- The distinction is that those are **proprietary managed-service features bound to a vendor agent platform**, not standalone infrastructure primitives. Agent Sandbox is the only one delivered as an **open, Kubernetes-native, cross-cluster-portable** resource.

So the accurate reading is "the only open Kubernetes-native sandbox primitive among the hyperscalers," not "the only sandbox." The independent sandbox vendors — [Cloudflare Sandboxes](https://blog.cloudflare.com/) (container isolation plus V8 isolates) and [E2B](https://e2b.dev/) ([[firecracker|Firecracker]] microVMs) — remain the comparison set; see [[agent-sandbox-isolation-landscape|the isolation landscape comparison]].

## Limits — isolation is necessary, not sufficient

The CRD manages the execution environment; it does not monitor what happens inside it.[^armo] gVisor stops container escapes and kernel exploits, but cannot tell a legitimate agent API call from a prompt-injection-driven one: a service account with `roles/bigquery.dataViewer` issues an identical token for a 500-row query and a 10-million-row exfiltration, and VPC Service Controls pass a staged, policy-compliant exfiltration along sanctioned paths.[^armo] Runtime isolation therefore sits underneath, not in place of, the identity, egress, observability, and behavioral-detection planes of the [[agentic-ai-security-reference-architecture|reference architecture]]. It is also distinct from the [[supply-chain-security-for-agents|supply-chain]] guarantees — provenance, hermetic builds, reproducibility — that a team running agents in CI/CD needs once the sandbox is in place.

## Role in the RA / CMM

- **[[agentic-ai-security-reference-architecture|RA]] Runtime plane** — Agent Sandbox is the Kubernetes-native reference implementation in the Sandbox / containment row, alongside [[firecracker|Firecracker]] (per-task VM) and [[gvisor|gVisor]] (the runtime it wraps).
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4 L3]]** — "per-task sandbox for high-risk-tier actions": a `SandboxClaim` against a network-default-deny `SandboxTemplate` is a clean evidence artifact, and the open CRDs make the control portable across clouds rather than entitlement-locked.

## See also

- [[agent-sandboxing|Agent Sandboxing]] — the practice page this product instantiates
- [[agent-sandbox-isolation-landscape|Agent Sandbox Isolation Landscape]] — hyperscaler vs. independent-vendor comparison
- [[gvisor|gVisor]] — the default isolation runtime
- [[firecracker|Firecracker]] — the microVM alternative

## Notes

[^blog]: [Google Cloud blog — Bringing you Agent Sandbox on GKE and Agent Substrate](https://cloud.google.com/blog/products/containers-kubernetes/bringing-you-agent-sandbox-on-gke-and-agent-substrate), May 2026. 300 sandboxes/sec per cluster; 90% of allocations under 200 ms; 30% better price-performance on Axion; gVisor native, Kata pluggable; default-deny network policy; warm pools; Pod Snapshots; standby-VM cold pool.
[^repo]: [GitHub — kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox), 2026. SIG Apps subproject; Apache 2.0; CRDs Sandbox, SandboxTemplate, SandboxClaim, SandboxWarmPool; runs on any Kubernetes cluster.
[^infoq]: [InfoQ — Google Announces GKE Agent Sandbox and Hypercluster at Next '26](https://www.infoq.com/news/2026/05/gke-agent-sandbox-hypercluster/), May 2026. Launched as a Kubernetes SIG Apps subproject at KubeCon NA 2025; Lovable 200,000+ AI-generated projects/day; "only native agent sandbox offering among the three major hyperscalers"; Cloudflare Sandboxes and E2B as the independent-vendor comparison.
[^armo]: [ARMO — Securing AI Agents on GKE: Where gVisor, Workload Identity, and VPC Service Controls Stop Working](https://www.armosec.io/blog/sandboxing-ai-agents-gke-workload-identity/), 2026. Isolation manages the environment, not in-sandbox behavior; identity tokens are intent-blind; VPC-SC misses sanctioned-path exfiltration; recommends eBPF runtime behavioral detection inside the sandbox.
