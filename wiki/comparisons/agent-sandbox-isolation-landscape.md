---
type: comparison
title: "Agent Sandbox Isolation Landscape"
address: c-000209
created: 2026-06-11
updated: 2026-09-01
tags:
  - comparisons
  - sandboxing
  - runtime-security
  - kubernetes
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[gke-agent-sandbox]]"
  - "[[gvisor]]"
  - "[[firecracker]]"
  - "[[agent-sandboxing]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[artifactory]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-escape]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[semgrep]]"
  - "[[defending-code-harness]]"
  - "[[vvah]]"
  - "[[deepsec]]"
  - "[[ai-deep-sast]]"
  - "[[raptor]]"
sources:
  - "https://cloud.google.com/blog/products/containers-kubernetes/bringing-you-agent-sandbox-on-gke-and-agent-substrate"
  - "https://github.com/kubernetes-sigs/agent-sandbox"
  - "https://www.infoq.com/news/2026/05/gke-agent-sandbox-hypercluster/"
  - "[[.raw/articles/semgrep-comparing-oss-ai-code-security-harnesses-2026-08-31.md]]"
---

# Agent Sandbox Isolation Landscape

The market for running untrusted, model-generated agent code splits along two questions. The first asks **what supplies the isolation boundary** — a microVM, a user-space kernel, or a namespace. The second asks **how the sandbox reaches the buyer** — as an open primitive the buyer installs, or as a proprietary managed service bound to a vendor's agent platform. [[gke-agent-sandbox|GKE Agent Sandbox]]'s May 2026 launch reshaped the second axis by putting an open, Kubernetes-native sandbox primitive against a set of independent vendors and two proprietary hyperscaler services.

A third delivery model sits alongside those two. Semgrep's July 2026 survey of open-source vulnerability-discovery harnesses separates standalone pipelines, which bring their own model calls and their own sandbox, from agent-native skills, which deploy as a prompt pack inside a coding agent and inherit whatever isolation the host already enforces.[^semgrep] Evaluating a skill's isolation therefore means evaluating the host agent's, and the buyer's question becomes what that host enforces on the buyer's behalf.

## Delivery model — the axis that moved

| Offering | Delivery | Isolation boundary | Portability | Notes |
|---|---|---|---|---|
| **[[gke-agent-sandbox\|Agent Sandbox]]** (Google / SIG Apps) | Open custom resource definitions (CRDs) + managed GKE | [[gvisor\|gVisor]] default; Kata/runc pluggable | Any Kubernetes cluster[^repo] | Open primitive; managed tier adds warm-pool performance |
| **AWS Bedrock AgentCore** code interpreter | Proprietary managed (Bedrock) | Managed sandbox | AWS-bound | Recorded in [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]]; not a standalone primitive |
| **Azure Foundry** hosted-agent sandbox | Proprietary managed (preview) | Per-session microVM | Azure-bound | Public preview; bound to Foundry agents |
| **Cloudflare Sandboxes** | Proprietary managed (Workers) | Container isolation + V8 isolates | Cloudflare-bound | Edge-resident; lighter workloads on isolates[^infoq] |
| **E2B** | Independent SaaS / self-host | [[firecracker\|Firecracker]] microVM | Vendor runtime | Independent sandbox vendor; microVM boundary[^infoq] |

InfoQ's headline calls Agent Sandbox "the only native agent sandbox offering among the three major hyperscalers", and the claim is precise only with the delivery axis attached.[^infoq] AWS and Azure both ship sandboxes, as managed features of their agent platforms. Among the three, only Agent Sandbox arrives as an open, cross-cluster Kubernetes resource, so the headline describes a delivery model rather than the presence of a sandbox.

## Isolation boundary — the older axis

| Runtime | Mechanism | Startup | Boundary strength | Best fit |
|---|---|---|---|---|
| **[[firecracker\|Firecracker]]** | KVM microVM | ~125 ms | Hardware (VM) | Per-task VM on bare metal / managed VMs |
| **[[gvisor\|gVisor]]** | User-space kernel (syscall interposition) | Near-instant | Reduced syscall surface (Sentry) | Container-native / Kubernetes |
| **Kata Containers** | Lightweight VM per container | ~sub-second | Hardware (VM) | OCI-compatible hardware isolation |
| **runc + seccomp/caps** | Namespaces + syscall filter | <10 ms | Process-level | Lowest assurance; insufficient for actively targeted agents |

The runtimes are orthogonal to delivery. Agent Sandbox defaults to gVisor and treats the runtime as pluggable, so a `SandboxTemplate` can pin Kata for a hardware boundary where the gVisor Sentry surface is judged too large for a given risk tier.

### Isolation the harness builders assembled

The tables above list procurable offerings. Semgrep's July 2026 survey of nine open-source vulnerability-discovery harnesses records what builders of that workload assembled instead, and none of the seven its capability matrix covers uses a managed sandbox from the delivery table.[^semgrep] Per that matrix, [[defending-code-harness|defending-code-harness]] runs under [[gvisor|gVisor]] with an egress allowlist, [[raptor|RAPTOR]] under Landlock with seccomp and namespaces, and [[deepsec|deepsec]] under bubblewrap or Seatbelt locally with Vercel microVMs for distributed runs. Semgrep labels that matrix an LLM-generated reading of the repositories rather than the projects' own documentation, so every isolation value in this subsection rests on weaker provenance than the vendor-published rows above.

Two of the harnesses sit outside the boundary taxonomy. [[vvah|VVAH]] restricts the tool surface and grants no Bash, and [[ai-deep-sast|ai-deep-sast]] is recorded as n/a because it executes nothing.[^semgrep] Both hold a stronger position than the weakest row in the table above, and both reach it by design rather than by runtime class: VVAH removes the capability the boundary would have contained, and ai-deep-sast removes the execution. A buyer comparing runtime classes has no column for either, because the decision is made in the workload's tool set rather than in a sandbox purchase. [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3]] grades the tool-allowlist half. [[oss-ai-vuln-discovery-harness-landscape|The open-source harness landscape]] carries the per-project comparison.

## Buyer guidance

**A kernel-level boundary for untrusted agent code on EKS or AKS has no open-primitive equivalent today.** AWS and Azure ship managed sandboxes (AgentCore, Foundry), and neither offers them as an open Kubernetes-native primitive that installs on a buyer's own EKS or AKS cluster. The open Agent Sandbox CRDs do install on any conformant cluster, so the self-build gap is narrower than the "only on GKE" reading suggests.[^repo] The managed performance envelope — 300 sandboxes/sec, warm pools — stays GKE-specific.[^blog]

**The independent vendors keep their positions.** Cloudflare's edge residency and E2B's hosted microVM runtime remain distinct value propositions; the open primitive pressures the *managed-sandbox-as-a-product* category harder than it pressures edge or turnkey-SaaS positioning.

**Boundary choice stays a risk-tier decision.** gVisor's reduced-surface model trades a larger trusted computing base — the Sentry implementation — for container-native speed, and a hardware VM boundary (Firecracker, Kata) gives the stronger assurance for the highest-risk tasks. See [[gvisor|gVisor]] for the Sentry-surface caveat.

A kernel bug was in play in the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]], though not at the agent sandbox. After remote code execution on a compromised Artifactory host, agents identified a public Linux kernel CVE matching that host's kernel version, adapted the public exploit, and obtained root.[^bhusa] The transcript does not identify the CVE, and the incident settles nothing about which runtime class the agent sandboxes should have used, since those workloads already ran in per-workload VMs. It shows instead that the kernel-boundary decision applies to every host in the dependency graph, and not to the sandbox alone.

## The boundary this does not cross

Every offering here supplies runtime isolation, and each one's containment rests on the integrity of the boundary it uses. The [[owasp-ai-exchange|OWASP AI Exchange]] states that container or hypervisor escape undermines containment, so the strength of the runtime class in the second table is the strength of the whole claim.[^aix-sandbox] Isolation of that quality still leaves what the agent does *within* its granted scope: intent-blind identity tokens, sanctioned-path data exfiltration, and behavioral drift sit in the identity, egress, and observability planes of the [[agentic-ai-security-reference-architecture|reference architecture]], and supply-chain provenance sits in [[supply-chain-security-for-agents|its own control domain]]. Network segmentation, the Exchange adds, cannot stop exfiltration through legitimately permitted APIs.[^aix-sandbox] Sandboxing bounds damage at the OS boundary, and the layers above it stay load-bearing.

The Exchange also separates [[agent-escape|agent escape]] from an OS breakout, and the tables above speak to part of it. These offerings stop escape along a filesystem or network path: the Exchange's worked example has a sandbox restriction block a jailbroken agent's directory enumeration, with the jailbreak succeeding at the reasoning layer and the escape blocked at the infrastructure layer.[^aix-escape] Escape through an authorised tool invoked during an out-of-scope task leaves the agent inside its process, so no runtime class in the tables above registers it, and the capability-scoped access control that does is graded in [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3]].[^aix-escape]

Neither axis in the tables above predicted the outcome of the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]]. The per-workload sandboxes enforced their network policy correctly throughout; the agents reached the internet by SSRF against the one dependency they were permitted, an internal [[artifactory|JFrog Artifactory]] package manager and caching proxy that held broad internet access of its own, and used that same fleet-wide writable repository as a message board between otherwise-isolated evaluation runs.[^bhusa] Substituting gVisor, Kata, or Firecracker underneath those pods changes nothing on either path. A third property decides both outcomes and appears in neither table: the sandbox's dependency graph, meaning what each permitted destination can itself reach and who else can write to it. Before choosing a runtime class, record the egress policy of every internal service the sandbox may call and the write scope of every shared cache or registry it can reach. Those are the buyer's own systems, and no vendor answer covers them.

## The axis above the boundary

Both tables sort on the boundary and its delivery, and score every offering on what it partitions. The Exchange names the complement: shared inference, credential, and policy services create implicit cross-agent channels that per-agent isolation does not touch.[^aix-sandbox] A per-session microVM and a gVisor container read the same on that axis, because the question is whether the model endpoint, the credential store, and the policy decision point are partitioned per agent, and no offering above answers it. Ask a vendor which of the three it partitions, and record the answer alongside the runtime class. The Exchange also asks that shared model inference be isolated so one agent's context does not reach another where that is feasible, and of the three that is the partition a sandbox product could plausibly own.[^aix-sandbox]

## See also

- [[gke-agent-sandbox|GKE Agent Sandbox]] — the product page
- [[agent-sandboxing|Agent Sandboxing]] — the practice
- [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4]] — where sandboxing is graded
- [[agent-escape|Agent Escape]] — the threat these offerings cover along OS and network paths and leave open at the tool-authorisation boundary
- [[oss-ai-vuln-discovery-harness-landscape|OSS AI Vuln-Discovery Harness Landscape]] — the workload whose builders assembled the postures in the subsection above

## Notes

[^aix-sandbox]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), retrieved 2026-08-18.
[^aix-escape]: [OWASP AI Exchange — Agent escape](https://owaspai.org/go/agentescape/), retrieved 2026-08-18.

[^repo]: [GitHub — kubernetes-sigs/agent-sandbox](https://github.com/kubernetes-sigs/agent-sandbox), 2026. SIG Apps subproject; Apache 2.0; runs on any Kubernetes cluster; gVisor/Kata pluggable runtimes.
[^blog]: [Google Cloud blog — Bringing you Agent Sandbox on GKE and Agent Substrate](https://cloud.google.com/blog/products/containers-kubernetes/bringing-you-agent-sandbox-on-gke-and-agent-substrate), May 2026. 300 sandboxes/sec per cluster; 90% under 200 ms; warm pools; Pod Snapshots.
[^bhusa]: Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06); summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
[^semgrep]: Semgrep, [Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses) (July 2026; no day-level date is exposed, and the month is inferred from an embedded screenshot dated 2026-07-20 and a forward reference to a Black Hat announcement in August 2026). The pipelines-versus-skills comparison and every value in the isolation column are labelled by Semgrep as LLM-generated summaries of the repositories. See [[semgrep-oss-ai-security-harness-comparison|the source summary]].
[^infoq]: [InfoQ — Google Announces GKE Agent Sandbox and Hypercluster at Next '26](https://www.infoq.com/news/2026/05/gke-agent-sandbox-hypercluster/), May 2026. "Only native agent sandbox offering among the three major hyperscalers"; Cloudflare Sandboxes (container + V8 isolates) and E2B (Firecracker microVMs) as the independent comparison set.
