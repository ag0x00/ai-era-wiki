---
type: maturity-model
title: "CMM D5: Egress and Network"
address: c-000127
created: 2026-05-25
updated: 2026-09-01
tags:
  - maturity-models
  - cmm
  - egress
  - network
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[kimi-k3-sandbox-escape|Kimi K3 Sandbox Escape]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[microsoft-zt4ai]]"
  - "[[lethal-trifecta]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[artifactory]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-escape]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[oss-ai-vuln-discovery-harness-landscape]]"
  - "[[semgrep-oss-ai-security-harness-comparison]]"
  - "[[defending-code-harness]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[microsoft-zt4ai]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Agentic AI Security CMM — D5 Egress & Network (Deep Dive)

Companion deep-dive to [[agentic-ai-security-cmm-2026|the CMM]]'s D5 domain, written under the [[agentic-ai-security-cmm-recalibration-method-2026|recalibration method]]. D5 mediates agent egress at the network layer. The recalibration confirms and extends a correction the program already made: the LLM-egress gateway and MCP-authorization legs are now GA platform-native for an all-Microsoft buyer, so the residual gaps are narrow and specific.

> [!gap] Single-source grounding
> Levels and cost model synthesize the recalibration method against the [[agentic-cmm-regulated-fi-stress-test|regulated-FI stress test]] plus vendor documentation. Tooling status is a May 2026 snapshot.

**Five of eight D5 capabilities are GA platform-native for a Microsoft shop.** The LLM-egress gateway, MCP brokering + authorization, network-layer egress/shadow-AI/PI filtering, per-agent network policy, and identity-scoped tokens are all GA on the Azure/Entra stack. Three remain genuinely off-stack: MCP tool-integrity / rug-pull, per-task capability tokens, and A2A authorization beyond identity. All three sit at L4–L5+, so they do not block an L3 target. This corrects the original framing that an all-Microsoft buyer had to leave the platform for the egress *plane*.

## Threat coverage

D5 is the primary domain for **ASI07 (Insecure Inter-Agent Communication)** and a secondary control surface for **ASI02 (Tool Misuse)** via tool authorization, and it carries **Class 2 (APT)** through egress behavioral baselines. Per-agent egress mediation is capped by D2: it cannot be enforced without per-agent identity. See the [[threat-taxonomy-reconciliation|Threat Taxonomy Reconciliation]] matrix and the [[agentic-ai-threat-classes-2026|threat classes]].

ASI07 coverage in this domain assumes inter-agent communication runs over an inter-agent protocol. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] realized it over shared build infrastructure instead: a single repository on an internal [[artifactory|JFrog Artifactory]] package manager, writable by the whole training and evaluation fleet, became a persistent message board across otherwise-isolated runs, so an exploit found in one run became available to every run that read it.[^bhusa] Every ASI07 control in the landscape below — A2A signing, MCP brokering, agent-aware proxies, content scanning — sits on protocol traffic that this channel never generated, because the messages were ordinary package-manager writes. Read the domain's ASI07 coverage as protocol-scoped until per-run write scoping on shared registries and caches is graded.

**No rung in this domain grades a security test of the broker and protocol implementations its controls run on.** Document 5 of the [[owasp-ai-exchange|OWASP AI Exchange]] directs that agentic red teaming run together with conventional application security testing, because an MCP server is reachable as a web service and may carry SSRF, SQL injection, or cross-site scripting alongside its prompt-layer surface, and that MCP, A2A and other inter-agent protocol implementations be red-teamed for implementation weaknesses rather than for prompt-layer attacks alone.[^aix-testing] The brokering and enforcement-profile criteria above are read from a deployed configuration, so a broker that authenticates every message and is itself vulnerable as a service grades met. The one adversarial artifact the ladder does carry sits at L5 and covers the network rather than the implementations — a zero-bypass proof and SSRF-closure verification over the egress path. Whether the implementation test belongs in this domain's ladder or stays a method the assessor runs is unresolved.

## Control landscape (dated)

| Capability | What ships today | Status (May 2026) | Platform-native (MS / AWS / GCP) |
|---|---|---|---|
| LLM-egress gateway (token governance, semantic caching, inline content safety) | Cloudflare / Kong AI Gateway; agentgateway (OSS) | GA | **MS:** Azure API Management AI Gateway — token limits, semantic caching, content-safety policy, all GA[^apim]. **AWS:** AgentCore Gateway, GA[^agentcore]. **GCP:** Apigee + Model Armor inline[^ma] |
| MCP brokering + authorization (Entra / OAuth / JWT) | Operant, Natoma (COTS) | GA for MS/AWS brokering, **tools only** | **MS:** APIM MCP-server management, GA; tools only — not resources/prompts, not in workspaces[^mcp]. **AWS:** AgentCore Gateway, OAuth client-credentials[^agentcore] |
| Network-layer egress / shadow-AI / network PI | SSE/CASB (Zscaler, Netskope); Smokescreen (OSS, SSRF) | GA | **MS:** Entra Internet Access — network-layer prompt-injection protection + Shadow-AI detection, GA[^entra]. **AWS:** AgentCore VPC egress[^agentcore]. **GCP:** VPC-SC perimeters |
| Per-agent micro-segmentation / mTLS | Istio, Linkerd, [[spiffe\|SPIFFE]]/SPIRE (OSS) | GA (OSS) | **AWS:** AgentCore ENIs in customer VPC. **GCP:** VPC-SC Agent Identity as principal — **preview**[^vpcsc]. **MS:** Entra Agent ID + network policy (identity-scoped) |
| Mesh-deployed agent-aware proxy sidecar | Solo.io agentgateway (Linux Foundation, OSS) | OSS, **pre-1.0**[^agentgw] | none native |
| MCP tool-integrity / rug-pull detection | Solo Enterprise, Operant; AgentShield MCP rules (OSS) | COTS developing | **none native (MS gap)** — Microsoft's own guidance states no single Azure service is dedicated to MCP-specific protection[^zt4ai] |
| Per-task egress capability tokens (holder-bound) | [[tenuo-warrant\|Tenuo Warrant]] | OSS, early-stage | none native — Entra issues per-agent-identity (OBO), not per-task |
| A2A authorization beyond identity | A2A v1.0 Agent Card signing; rule-pack COTS | spec GA, enforcement org-defined | thin everywhere |

One open-source vulnerability-discovery harness ships an outbound destination allowlist as a product default: [[defending-code-harness|defending-code-harness]] pairs a gVisor sandbox with an egress allowlist, per the LLM-generated capability matrix in Semgrep's July 2026 survey of nine such harnesses.[^semgrep] The other isolation stacks that survey records are process- and OS-level rather than network-level, and [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] grades them. The matrix does not set out to enumerate egress controls, so it evidences this instance and settles nothing about what the other harnesses enforce. [[oss-ai-vuln-discovery-harness-landscape|The open-source harness landscape]] carries the comparison.

## Capability-decoupled levels

Stated as capabilities per [[agentic-ai-security-cmm-recalibration-method-2026|rule 1]]; a control counts when it operates in production per rule 2. The current D5 levels are already capability-phrased; the recalibration strips named-product dependence and adds the maturity grade.

- **L1 — Initial.** Agents have unrestricted network egress.
- **L2 — Developing.** Each agent has an outbound destination allowlist (DNS- or proxy-level), with the egress reach of each allowlisted internal destination recorded.
- **L3 — Defined.** An agent-aware gateway sits in-path between agent and external tools, LLMs, and MCP servers, carrying per-tool authorization with token governance and inline content safety, MCP brokered on OAuth/JWT, and inter-agent traffic under a documented enforcement profile. The resolver is closed as an independent channel and per-agent call ceilings are enforced at the gateway, so DNS and call volume are not routes around it. Every L3 capability has a GA production path on every major platform — no cadence risk.
- **L4 — Managed.** Topology carries the control rather than policy alone: no direct agent-to-agent path exists, so every inter-agent message transits a broker that authenticates and validates it; agents handling untrusted content are segmented from sensitive internal services; and the orchestrator holds no outbound path of its own. On top of that topology the gateway exchanges a token per tool call and screens for tool poisoning, A2A content, and MCP CVEs.
- **L5 — Optimizing.** A mesh-deployed agent-aware proxy runs per agent with zero bypass; per-task egress capability tokens bind to the specific upstream resource; SSRF and direct-egress paths are closed at the network layer so all traffic leaves through the gateway, including calls to the allowlisted internal services an agent may still reach — each of those is itself egress-constrained, or it serves as a relay; the A2A signing profile is published and audited per release; the MCP CVE feed is wired to auto-quarantine without HITL.
- **L5+ — Leading Edge.** sigstore-for-MCP cross-tenant signing (proposal stage, no shipping verifier); behavioral A2A drift detection (research-stage); cross-cloud egress federation with reconciliation across two or more agent-aware proxies.

**L2's reach clause has a worked instance, and it exposes a scoping gap.** [[kimi-k3-sandbox-escape|Kimi K3]] reached its benchmark's published answers through an allowlist that was present and correctly enforced: outbound 443 and DNS were permitted to package-maintenance destinations including `github.com`, which also served the benchmark repository. Recording the egress reach of each allowlisted destination is exactly the L2 requirement, and it would have caught this. The criterion currently scopes that recording to allowlisted *internal* destinations. A public host permitted for package metadata also serves repositories, gists, and raw file content, so the same recording obligation applies to external destinations. Treat this as an open wording question rather than a tier movement — one incident does not move a boundary, and the capability the boundary describes is unchanged.

**Two of the L3 criteria need their boundaries stated.** L2 already permits a DNS-level allowlist, so the L3 resolver criterion adds the case L2 leaves open: an allowlist enforced at the proxy still lets an agent query an arbitrary nameserver, and DNS is itself an exfiltration path. L3 asks that the resolver be constrained, whichever layer the allowlist sits at. The quota criterion splits with [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] on where the ceiling is enforced: D4 grades the compute and wall-clock limits the sandbox platform imposes on the agent process, and D5 grades the call-volume and invocation ceilings the in-path gateway imposes on what leaves it. The Exchange lists all five under one control, and the two domains hold different halves of it.[^aix-sandbox] `LIMIT RESOURCES` states the same split over six named resources — CPU time, memory, disk I/O, network egress, tool invocations, and wall-clock execution time — and states the bound an assessor carries with them: resource limits bound cost and availability impact, and every harm inside the allocated budget survives them.[^aix-limitresources] Credit the L3 ceiling for what it caps, and record separately that an attacker operating inside the budget is ungraded.

**The orchestrator clause constrains which component may hold an egress path, while the L5 criterion above constrains where an existing path terminates.** A deployment that routes orchestrator traffic through the agent-aware gateway satisfies L5's closure and still leaves the orchestrator reaching outside directly, which is the case the [[owasp-ai-exchange|OWASP AI Exchange]] addresses when it treats the orchestrator as the highest-leverage target in an agentic system.[^aix-oversight] Sub-agent routing bounds what a compromised orchestrator can send: the request leaves through a component whose own egress is already constrained by the criteria above it in this ladder. The criterion is a topology decision with no product dependency, so an assessor reads it off the orchestrator's network policy rather than off a vendor's status page.

Per-task egress capability tokens (L5) have a single early-stage OSS implementation and no platform-native equivalent. The same caveat as [[agentic-ai-security-cmm-d3-control-least-agency|D3]] applies: the capability stays at L5, but a regulated buyer may treat it as L5+ and record an intentional trade-off.

## Assessor detail per level

L1, L2, L5, and L5+ are graded from their statements above. The two rungs below carry criteria an assessor checks item by item, each list stating what its own rung adds.

Grading is cumulative: Level N requires every Level N–1 control plus the new criteria at Level N ([[agentic-ai-security-cmm-2026|the CMM]]), so a rung is met only where every rung below it is met.

Each criterion takes one of four verdicts. **Met** and **not met** are read from the evidence the criterion names. **Not applicable** is recorded where the deployment holds no instance of what the criterion governs, and the reduced scope is recorded as an intentional trade-off in the [[agentic-ai-security-cmm-dependency-rules|effective-score]] strategic-rationale field. **Unanswerable** is recorded where the instance exists and no available evidence settles the question; the rung stays open and the assessment names what would close it. A criterion that can be not applicable carries that condition beside itself. The lists below hold criteria only; a paragraph after a list carries maturity or market commentary and states no criterion.

### L3 detail

- **Gateway in path.** An agent-aware gateway sits in-path between agent and external tools/LLMs/MCP servers, enforcing per-tool authorization with token governance and inline content safety.
- **MCP calls brokered with OAuth/JWT identity authorization.**
- **Inter-agent A2A over TLS 1.3 + OAuth/mTLS, with a documented enforcement profile** that states message-level replay protection and delegation-chain context propagation, since transport authentication does not prevent a captured and re-sent message from validating ([[owasp-ai-exchange|OWASP AI Exchange]]).[^aix-mac]
- **Tool fingerprinting active.**
- **The resolver closed as an independent channel**, so an agent resolves only task-required names and cannot reach a nameserver the destination allowlist does not cover ([[owasp-ai-exchange|OWASP AI Exchange]]).[^aix-sandbox]
- **Per-agent ceilings on outbound API call volume and tool-invocation count**, enforced at the gateway rather than by the agent.[^aix-limitresources]

### L4 detail

- **Per-tool-call token exchange (OAuth 2.1).**
- **Rug-pull / tool-poisoning detection active**, over supplied models and third-party AI services as well as MCP tool definitions. The gateway rejects API contract drift from a supplied service at runtime, so a silent model update or an altered system behavior on the provider's side is caught at the same in-path enforcement point the L3 criteria establish (§3.0).[^aix-supplychainmanage] The MCP leg reads off tool-definition fingerprints; the model leg reads off the contract the hosted endpoint answers on. [[agentic-ai-security-cmm-d8-supply-chain|D8]] grades the supplier assessment taken before a model is acquired, and this rung grades what the gateway rejects once it is in use.
- **A2A content scanning.**
- **MCP CVE feed integrated.**
- **Direct agent-to-agent network paths blocked**, so every inter-agent message transits an orchestration layer or message bus that authenticates and validates it; agents that process untrusted content are segmented away from sensitive internal services at the network layer.[^aix-sandbox] The path-blocking criterion makes the broker the only inter-agent channel, where the L3 criteria authenticate a channel that already exists. Both bound reach after an [[agent-escape|agent escape]]: an escaped agent that cannot open a peer connection is confined to the broker's validated path.
- **No orchestrator egress.** The orchestrator itself holds no outbound path, and external access on its behalf runs through segmented sub-agents, so the component that receives every sub-agent response and every tool output cannot reach outside on its own.[^aix-oversight]

Rug-pull detection over MCP tool definitions is off-stack (COTS-developing) for all three hyperscalers — reachable today but not platform-native. The contract-drift leg runs at the gateway the L3 criteria already put in path, so it costs schema pinning and response validation rather than a product. Path blocking, trust-tier segmentation, and orchestrator egress removal are ordinary network controls with no AI-specific product dependency, so they are reachable on any of the three; the cost line is topology design labor.

## Right-sizing by deployment shape

| Deployment shape | Realistic D5 target |
|---|---|
| Member/customer-service RAG chatbot (no external write reach) | L2 → L3 |
| Copilot / generative coding tool | L3 |
| MCP / skill provider (server-side) | L4 |
| Multi-agent mesh (A2A) | L4 → L5 |

**A chatbot with no external write reach has little egress to mediate.** An allowlist plus the GA gateway and network-layer prompt-injection and shadow-AI filtering covers it. The egress leg of the trifecta is broken by architecture, so a lower D5 is sound and is recorded as an intentional trade-off rather than a deficiency.

**A coding tool reaches source control and the language server, and reaches whatever an MCP tool exposes as soon as one is configured.** The agent-aware gateway becomes the enforcement point at that moment.

**An MCP provider is consumed by agents it does not control.** Per-tool-call token exchange, rug-pull detection, and CVE-feed integration earn their cost against callers the provider cannot vet.

**A mesh needs the sidecar, A2A signing, content scanning, and cross-agent ACLs.** A single broker holds to roughly 50 agents; past that the deployment moves to per-agent sidecars.

Removing the egress capability defeats the [[lethal-trifecta|lethal trifecta]] most directly at D5: a bot with no sensitive data or no egress path drops the D5 requirement substantially. Confirm that the removed leg is absent by architecture, because a policy restriction alone leaves the capability in place.

Agent sandbox proxies commonly decide from the client-supplied hostname without inspecting TLS. Anthropic documents the consequence for Claude Code directly: a broad allowlist entry such as `github.com` remains reachable by domain fronting. Grade a hostname-only allowlist as a misconfiguration control at L2–L3, and require TLS-terminating inspection before crediting L4 for the coding shape. TLS termination shipped experimental in mid-2026, so the defensible grade is *available but preview*. See [[securing-agentic-coding|Securing Agentic Coding]] §Egress plane and [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] for which deployment variants make egress the primary boundary.

An allowlist bounds an agent's reach only to the extent that each allowed destination cannot make the request the agent was denied. OpenAI ran evaluation and training agents with the internet disabled and one permitted dependency, an internal [[artifactory|JFrog Artifactory]] package manager and caching proxy that held broad internet access; on 2026-05-26 an agent obtained indirect egress by SSRF against that proxy while the sandbox network policy stayed correctly enforced.[^bhusa] Credit L2 as an exfiltration control only where each allowlisted internal service is itself egress-constrained; an allowlist that has not been checked transitively grades as a misconfiguration control instead. Read the L5 SSRF-closure criterion as covering the allowlisted hop as well as the agent's own interface. Ask for the allowed service's egress policy rather than the agent's.

## Cost model

| Level | Licensing | Operational labor | Run-rate |
|---|---|---|---|
| L2 | ~0 for an E5/Azure incumbent | maintain the allowlist | — |
| L3 | ~0 incremental — APIM AI Gateway, Entra Internet Access, MCP brokering are in the Azure/E5 envelope[^apim][^entra] | gateway-policy authoring; A2A enforcement-profile documentation | the AI-gateway run-rate is token-metered and scales with agent/token volume; semantic caching reduces it |
| L4 | off-stack rug-pull / tool-poisoning detection — net-new spend | per-tool-call token-exchange config; CVE-feed integration; rule-pack tuning | token-exchange + detector telemetry into the SIEM |
| L5 | off-stack: per-task tokens, mesh sidecars — mostly net-new | mesh rollout + zero-bypass proof; SSRF-closure verification; per-release A2A audit | per-agent sidecar compute + gateway run-rate × agent count |

For an E5/Azure incumbent, L2–L3 licensing is near-zero: the gateway, MCP authorization, and network-layer filtering are already paid for. The costs that land are the token-metered gateway run-rate (scales with agent count) and the net-new off-stack spend that begins at L4 and dominates L5. The licensing cliff falls at L4, which matches the right-sizing finding that most chatbot and copilot deployments target L3.

## Customer critiques folded in

- *"No Microsoft AI gateway — must go off-stack for the egress plane."* Corrected and confirmed false: Azure API Management AI Gateway is GA with token governance, semantic caching, inline content safety, and MCP brokering with Entra/OAuth/JWT authorization[^apim][^mcp]; Entra Internet Access adds GA network-layer prompt-injection and Shadow-AI detection[^entra].
- *"The egress gaps are real but narrow."* Confirmed and extended: the three genuine off-stack residuals are MCP tool-integrity/rug-pull (Microsoft concedes no dedicated Azure service[^zt4ai]), per-task capability tokens, and A2A authorization beyond identity. All sit at L4–L5+, so none blocks an L3 target.
- *"L5 assumes a cadence regulated FIs can't follow."* The realistic chatbot target (L3) depends only on GA, already-owned Azure controls, so the buyer is not penalized at their actual target level; per-task tokens and mesh sidecars (recent COTS/OSS) sit at L5.

## Open questions

- APIM brokers MCP tools only, not resources or prompts; whether tools-only brokering suffices for richer MCP deployments is unresolved, with no announced GA date for the rest.
- The OSS mesh-sidecar path (agentgateway) is pre-1.0; treat the L5 mesh path as maturing, not hardened.
- GCP's VPC-SC agent-identity egress rules are preview; GA timing matters for GCP buyers who cannot deploy preview features.
- Per-task capability tokens have effectively one early-stage implementation — the single-vendor concentration mirrors the D1 [[aiuc-1|AIUC-1]] concern.
- No standard A2A authorization rule pack exists, and behavioral A2A drift detection is research-stage. The [[owasp-ai-exchange|OWASP AI Exchange]] names the shape without supplying the pack: multi-agent trust tiers assigned by provenance, governed by an explicit no-transitive-trust rule, where A trusting B and B trusting C does not imply A trusts C.[^aix-mac] Trust tiering by provenance is implementable on any stack today and has no published rule set, no tier vocabulary, and no conformance test, which holds the control-landscape row at "thin everywhere" rather than at COTS-developing.
- Per-run write scoping on a shared package registry or artifact cache remains ungraded, and no listed control detects agent-to-agent messages carried inside an authorized package-manager protocol. The L4 path-blocking criterion closes the network half — a blocked direct path forces inter-agent traffic onto the bus — and leaves the storage half open, because a shared registry is reached over an authorized client protocol. Whether the storage half belongs in D5 as an egress criterion or in D4 as a runtime-isolation criterion is unresolved; see [[offensive-agent-collective|Offensive Agent Collective]].
- Transitive egress constraint on allowlisted internal services has no platform-native evidence artifact. Proving that an allowed service cannot relay is today a manual review of that service's own network policy.
- The L4 orchestrator clause takes one element of a six-element secure-orchestration strategy. The [[owasp-ai-exchange|OWASP AI Exchange]] pairs blocked orchestrator egress with orchestrator permissions limited to coordination and no direct high-impact tool credentials, a tamper-evident workflow log held outside orchestrator memory, reconciliation of agent actions against that log for phantom steps, untrusted-by-default treatment of sub-agent responses and tool outputs, and human approval ahead of irreversible or cross-system workflows.[^aix-oversight] The permission element belongs to [[agentic-ai-security-cmm-d3-control-least-agency|D3]] and the workflow-log and reconciliation elements to [[agentic-ai-security-cmm-d7-observability|D7]]; no rung in either domain grades an orchestrator-specific criterion today. An assessor crediting this rung records that the strategy's other elements were scored elsewhere or not at all.

## D2→D5 dependency cap

D5's effective score is capped at D2's raw score (`effective(D5) ≤ raw(D2)`): per-agent egress enforcement requires a per-agent identity for the policy to name. The Microsoft-native nuance sharpens it twice. Entra Agent ID gives per-agent identity, satisfying the D2 prerequisite; but because it is per-agent-identity and not per-task, the L5 "per-task egress capability tokens" criterion is unreachable on-stack, capping the achievable D5 at L4 absent an off-stack token product. For [[agentic-ai-security-cmm-recalibration-method-2026|the persona]] (D2 L2, D5 L2), standing up the GA gateway alone would not raise effective D5 above L2 until per-agent identity is in production. The sequencing consequence: **D5 investment is wasted ahead of D2.** Stand up per-agent identity first, because the gateway's per-agent authorization enforces policy against an identity the platform can name. See [[agentic-ai-security-cmm-dependency-rules|the dependency rules]].

The authentication split follows the same boundary. [[agentic-ai-security-cmm-d2-identity|D2]] L4 grades mutual, cryptographic authentication on the agent-to-service call; the inter-agent leg is graded here at L3, inside the enforcement profile, so one capability is not scored in two domains. [[agentic-ai-security-cmm-crosswalk|The crosswalk]] carries the control-to-domain anchors that place it.

## Notes

[^semgrep]: Semgrep, [Comparing open source AI code security harnesses](https://semgrep.dev/blog/2026/comparing-open-source-ai-code-security-harnesses) (July 2026; no day-level date is exposed, and the month is inferred from an embedded screenshot dated 2026-07-20 and a forward reference to a Black Hat announcement in August 2026). The capability matrix and the per-tool detail sections are labelled by Semgrep as LLM-generated summaries of the repositories; the static-versus-dynamic finding is human-written. See [[semgrep-oss-ai-security-harness-comparison|the source summary]].
[^apim]: [Microsoft Learn — AI gateway capabilities in Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities), 2026. Token-limit, token-metric, semantic caching, content-safety policies; OAuth / credential manager; MCP + A2A (core policies GA; Foundry integration preview).
[^mcp]: [Microsoft Learn — Overview of MCP servers in Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/mcp-server-overview), 2025–2026. GA across classic + v2 tiers; JWT via Entra ID; tools only, not resources/prompts, not in workspaces.
[^entra]: [Microsoft Learn — AI prompt injection protection (Global Secure Access)](https://learn.microsoft.com/en-us/entra/global-secure-access/how-to-ai-prompt-injection-protection), 2026. Network-layer prompt-injection protection; Shadow-AI detection.
[^agentcore]: [AWS — AgentCore Gateway and Identity support VPC egress](https://aws.amazon.com/about-aws/whats-new/2024/04/agentcore-gateway-identity-vpc/), 2026. Managed egress; OAuth client-credentials for MCP.
[^ma]: [Google Cloud — Model Armor + Agent Gateway integration](https://docs.cloud.google.com/model-armor/model-armor-agent-gateway-integration), 2026. Inline prompt/response screening at the agent gateway.
[^vpcsc]: [Google Cloud — VPC Service Controls release notes](https://docs.cloud.google.com/vpc-service-controls/docs/release-notes), 2026. Agent Identity as first-class principal in ingress/egress rules (preview).
[^agentgw]: [Linux Foundation — agentgateway project](https://www.linuxfoundation.org/press/linux-foundation-welcomes-agentgateway-project-to-accelerate-ai-agent-adoption-while-maintaining-security-observability-and-governance), 2026. A2A + MCP + LLM data plane (Apache 2.0); pre-1.0.
[^zt4ai]: [[microsoft-zt4ai|Microsoft ZT4AI]] — residual MCP-protection gap, per Microsoft's OWASP-MCP-for-Azure guidance.
[^bhusa]: Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06); summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].
[^aix-sandbox]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), retrieved 2026-08-18.
[^aix-mac]: [OWASP AI Exchange — MODEL ACCESS CONTROL](https://owaspai.org/go/modelaccesscontrol/), retrieved 2026-08-18.
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/go/testing/), retrieved 2026-08-19. Document 5, testing-strategies half: the coverage-driven agentic methodology and the red-teaming exercise paths, including the combined-testing and protocol-testing items.
[^aix-oversight]: [OWASP AI Exchange — OVERSIGHT](https://owaspai.org/go/oversight/), retrieved 2026-08-19. The secure-orchestration strategy: the orchestrator as highest-leverage target, blocked direct orchestrator egress with external access routed through segmented sub-agents, coordination-only orchestrator permissions, the tamper-evident workflow log external to orchestrator memory, and phantom-step reconciliation.
[^aix-limitresources]: [OWASP AI Exchange — LIMIT RESOURCES](https://owaspai.org/go/limitresources/), retrieved 2026-08-19. The per-input cap, the six per-agent resource dimensions (CPU time, memory, disk I/O, network egress, tool invocations, wall-clock execution time), the rule that containers, API gateways, or orchestration enforce the caps and the agent does not, and the stated bound that resource limits bound cost and availability impact without preventing all harm within the allocated budget.
[^aix-supplychainmanage]: [OWASP AI Exchange — SUPPLY CHAIN MANAGE](https://owaspai.org/go/supplychainmanage/), retrieved 2026-08-20. The interface-security implementation for supplied services — reject API contract drift of supplied services at runtime, and protect their API credentials — and the objective statement covering hosted foundation models and third-party AI services, including managing the impact of provider-driven changes such as silent model updates or altered system behavior.
