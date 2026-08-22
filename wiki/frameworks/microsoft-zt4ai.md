---
type: framework
title: "Microsoft ZT4AI: Zero Trust for AI"
address: c-000015
created: 2026-05-07
updated: 2026-08-21
tags:
  - frameworks
  - microsoft
  - zero-trust
  - agentic-ai
  - reference-architecture
status: developing
scope_axis:
  - sec-of-ai
authoring_organization: "[[microsoft|Microsoft]]"
publication_date: "Updated 2026-03-20 (pre-RSAC 2026)"
related:
  - "[[microsoft|Microsoft]]"
  - "[[microsoft-rai|Microsoft Responsible AI Standard]]"
  - "[[microsoft-entra-agent-id|Microsoft Entra Agent ID + Agent 365]]"
  - "[[microsoft-secure-agentic-ai-end-to-end|Secure Agentic AI End-to-End]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]"
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[network-layer-prompt-injection-containment|Network-Layer Prompt Injection Containment]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2|ZT4AI Standards Review (2026-Q2)]]"
sources:
  - "[[.raw/articles/microsoft-secure-agentic-ai-end-to-end-2026-05-07.md]]"
no_public_url: "Microsoft ZT4AI is published across multiple Microsoft Learn / Security Blog locations; no single canonical URL captured at ingest time"
coined_by:
  - "[[microsoft]]"
---

# Microsoft ZT4AI — Zero Trust for AI

Microsoft's adaptation of [[xacml|Zero Trust]] principles to AI systems and agentic deployments, branded **ZT4AI** (Zero Trust for AI). Originally introduced as a control set with ~700 controls; the framework received a **March 2026 update** delivering a refreshed reference architecture, workshop, assessment tool, and patterns-and-practices articles, announced in [[microsoft-secure-agentic-ai-end-to-end|Vasu Jakkal's pre-RSAC 2026 post]].

## Foundation: Zero Trust principles applied to AI

ZT4AI inherits the three Zero Trust principles and extends them to the AI lifecycle:

1. **Verify explicitly** — every model invocation, every tool call, every data retrieval, every cross-agent message is authenticated and authorized at the time of use; no transitive trust from prior decisions.
2. **Use least privilege** — agents receive the minimum capability required for the current task; capabilities are scoped, time-bounded, and revocable. Aligns with [[least-agency-principle|Least Agency Principle]] applied to NHIs.
3. **Assume breach** — the architecture operates as if any individual agent, model, tool, or credential is already compromised; controls focus on containment and rapid remediation.

The framework's **lifecycle scope** spans data ingestion, model training, deployment, and agent runtime behavior — explicitly broader than runtime-only zero-trust frameworks.

## Architectural pillars (Microsoft's structuring)

ZT4AI's reference architecture organizes controls into pillars consistent with Microsoft's broader Zero Trust portfolio:

| Pillar | What it covers | Microsoft product surface |
|---|---|---|
| **Identity** | Workload + agent identity; authN; authZ; conditional access | [[microsoft-entra-agent-id\|Entra Agent ID]] / Entra Suite / Conditional Access Optimization Agent |
| **Data** | Classification; DLP; provenance; oversharing prevention | Microsoft Purview (DLP for Copilot; Data Security Posture Agent) |
| **Devices** | Endpoint hygiene; device-of-origin attestation; AI-app inventory | Microsoft Intune (Enhanced AI App Inventory) |
| **Apps & Workloads** | Container security; binary drift; antimalware; agent runtime | Defender for Cloud (Container Security; Posture Management) |
| **Network / Egress** | Network-layer controls plus LLM-egress gateway: prompt-injection blocking; shadow-AI detection; token governance; inline content safety; MCP brokering | Entra Internet Access ([[network-layer-prompt-injection-containment\|PI Protection]] + Shadow AI Detection, GA Mar 31 2026); **Azure API Management AI Gateway** (token-limit / token-metric / semantic caching / inline Content Safety / MCP-server brokering with Entra-OAuth authorization, GA) |
| **Visibility, Automation, Orchestration** | SIEM + SOAR + agentic SOC | Microsoft Sentinel (Data Federation; Playbook Generator; MCP Entity Analyzer) |
| **Governance** | Policy lifecycle; compliance; assessments | [[microsoft-rai\|Responsible AI Standard]]; ZT4AI Assessment Tool |

## Named controls per pillar (May 2026)

Microsoft does not publish a maturity scale for these controls; status reflects Microsoft Learn / Security Blog document dates as of May 2026. The control-level crosswalk to the [[agentic-ai-security-cmm-2026|CMM]] domains is in [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]].

- **Identity (verify explicitly / least privilege).** Per-agent [[microsoft-entra-agent-id|Entra Agent ID]] minted from agent blueprints (GA); three access patterns — on-behalf-of (delegated), application-only (client-credentials), agent's-user-account (GA); attribute-driven and blueprint-level Conditional Access, including disabling an agent class in one operation (GA); ID Protection for agents — per-agent risk baseline, risk-based Conditional Access, automatic remediation, Managed Policies blocking high-risk agents (shipping); Privileged Identity Management — time-limited active role assignment for agent identities (auto-expiring; agents cannot be PIM-*eligible*, so no agent self-activation).
- **Data (least privilege / assume breach).** Purview DSPM for AI (oversharing assessments, bulk remediation, GA); answer-time entitlement — agents return only data the user holds VIEW and EXTRACT rights to (GA); sensitivity-label inheritance and DLP for AI / Copilot (GA); the Risky AI Usage insider-risk template (GA). Microsoft governs the underlying SharePoint/Graph corpus rather than a distinct RAG-index product.
- **Apps & Workloads / Runtime (assume breach).** Azure AI Content Safety — Prompt Shields direct + indirect (GA), Protected Material text (GA), Groundedness Detection with correction (preview, English-only), Task Adherence (preview); Defender for Cloud AI workload runtime threat protection (GA); Defender AI-agent runtime protection blocking unsafe tool calls before execution (preview; Agent 365 subscription required from 2026-07-01).
- **Devices / Supply chain (verify explicitly).** Defender for Cloud AI model scanning for embedded malware and secrets in CI/CD (preview, 2026-04); AI-SPM generative AI-BOM discovery across Azure, Amazon Bedrock, and Google Vertex (GA), now extended to MCP-server discovery and AI-model-provider catalog coverage in Defender for Cloud Apps; AI agent discovery into the AI-BOM (preview); Intune enhanced app inventory and the Shadow AI device dashboard (rollout 2026-05; runtime blocking preview 2026-06).
- **Network / Egress (assume breach / least privilege).** Entra Internet Access network-layer prompt-injection protection and Shadow AI detection (GA 2026-03-31, text only); Azure API Management AI Gateway — token-limit and quota policies, inline Content Safety, semantic caching, and MCP-server brokering with Entra/OAuth/JWT (GA core policies). MCP tool-integrity / rug-pull has **no single Azure service** — Microsoft's own guidance prescribes a composed pattern of registry, version-pinning, egress allowlists, and runtime monitoring (Source: [Microsoft — OWASP MCP Top 10 for Azure, MCP03 Tool Poisoning](https://microsoft.github.io/mcp-azure-security-guide/mcp/mcp03-tool-poisoning/)).
- **Visibility / Orchestration (assume breach).** Microsoft Sentinel data-lake MCP server, AI Entity Analyzer, data federation, and Playbook Generator (preview); Defender XDR near-real-time AI-agent detections and the `AIAgentsInfo` / `CloudAppEvents` Advanced Hunting tables (preview); Agent 365 lifecycle telemetry (GA). Cross-platform `gen_ai.*` OpenTelemetry conventions remain at Development status, not stable.
- **Governance (verify explicitly).** [[microsoft-rai|Responsible AI Standard]] (mandatory impact assessments, named accountable owners); Purview Compliance Manager AI templates (EU AI Act, NIST AI RMF, ISO/IEC 42001, ISO/IEC 23894, GA); Agent 365 registry and inventory governance (GA, with Bedrock/GCP sync in preview); Entra ID Governance sponsors with automatic manager-transfer and time-bound access packages (GA).

## March 2026 deliverables

The March 2026 update delivered, per the announcement (Source: [Microsoft Security Blog, 2026-03-19](https://www.microsoft.com/en-us/security/blog/2026/03/19/new-tools-and-guidance-announcing-zero-trust-for-ai/)):

- A new **AI pillar in the Zero Trust Workshop** — a qualitative planning assessment of four objectives: secure AI access and agent identities, protect sensitive data used by and generated through AI, monitor AI usage and behavior, and govern AI responsibly.
- A **Zero Trust reference architecture for AI** showing how policy-driven access, continuous verification, monitoring, and governance combine.
- Five **patterns and practices**: threat modeling for AI, AI observability, securing agentic systems, robust safety engineering, and defense-in-depth for indirect prompt injection (XPIA).
- An update to the **Zero Trust Assessment tool** adding Data and Network pillars alongside Identity and Devices.

The automated Zero Trust Assessment does **not yet include an AI pillar**; the assessment site lists only Identity, Devices, Data, and Network. A Zero Trust Assessment for AI pillar is in development, expected **summer 2026** (Source: [Microsoft Security Blog, 2026-03-19](https://www.microsoft.com/en-us/security/blog/2026/03/19/new-tools-and-guidance-announcing-zero-trust-for-ai/)).

## Control set scale

ZT4AI was introduced as a **~700-control framework**. The March 2026 update did not publicly re-enumerate an AI-specific control count; the structure is now carried by the reference architecture, workshop, and assessment-tool deliverables rather than a flat control list.

> [!gap] The "700 / 116 groups / 33 swim lanes" figure describes the whole Workshop, not the AI pillar
> The Microsoft Security Blog states that the **entire Zero Trust Workshop 3.0** "covers 700 security controls across 116 logical groups and 33 functional swim lanes" (Source: [Microsoft Security Blog, 2026-03-19](https://www.microsoft.com/en-us/security/blog/2026/03/19/new-tools-and-guidance-announcing-zero-trust-for-ai/)) — all seven pillars, most of them not AI. Microsoft describes the AI pillar by what it *assesses*, with **no AI-specific control count**. Third-party write-ups (for example anoopcnair.com) conflated the whole-Workshop number with the AI pillar. Per [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], do not cite "700 / 116 / 33" as an AI-framework figure or treat ZT4AI as "the most control-rich AI security framework" on that basis.

## Predictive Shielding — adaptive policy contraction

The March 2026 update introduces **Defender Predictive Shielding** (preview), a primitive for **dynamically adjusting identity and access policies during active attacks**. Per [[standards-review-microsoft-zt4ai-2026-Q2|the 2026-Q2 ZT4AI review]], Predictive Shielding is a **Microsoft Defender XDR feature (Preview)** that operates on devices, users, and credentials through Defender for Endpoint; it is **not an agent-identity control**. The agent-specific equivalents of "contract access during an attack" are ID Protection auto-remediation (the Identity pillar) and the Agent Governance Toolkit trust-decay model. Sequence:

1. Detector signals an active attack (Defender for Cloud / Sentinel anomaly).
2. Predictive Shielding tightens applicable Conditional Access policies — e.g., elevates MFA strength, forces re-authentication, narrows permitted scopes, blocks specific tool calls.
3. After the attack signal recedes, policies revert to baseline automatically.

This is the **step-down** counterpart to step-up authentication: the policy contracts in response to detected threat rather than expanding for an authorization request. The wiki has previously treated step-up (D2 L4 in the [[agentic-ai-security-cmm-2026|CMM]]); step-down was implicit. The general primitive, automatic policy contraction triggered by anomaly signals, is broader than Microsoft's specific implementation and warrants separate tracking. See [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection for Agents]] for the detection side.

## Cross-walk to wiki frameworks

| ZT4AI Pillar | Wiki [[agentic-ai-security-cmm-2026\|CMM]] domain | Wiki [[agentic-ai-security-reference-architecture\|RA]] plane |
|---|---|---|
| Identity | D2 (Identity & Authorization) | Identity plane |
| Data | D6 (Data, Memory & RAG) | Data plane |
| Devices | D8 (Supply Chain & AI-BOM) | (Endpoint substrate; cross-cutting) |
| Apps & Workloads | D4 (Runtime & Guardrails) | Runtime plane |
| Network | D5 (Egress & Network) + D4 | Egress plane |
| Visibility / Orchestration | D7 (Observability and Detection) | Observability plane |
| Governance | D1 (Governance & Accountability) | (Cross-cutting) |

The mapping is approximate — ZT4AI is organized around Microsoft's product taxonomy while the CMM is organized around independent operational domains. A specific control may appear in different cells depending on which framing is applied.

## Adoption signals

- **Vendor lock-in** — ZT4AI is most directly actionable inside the Microsoft Security stack (Entra + Defender + Purview + Sentinel + Security Copilot). Other-vendor practitioners can take the framework's *principles* but the *implementation guide* assumes Microsoft tooling.
- **Wide reference base in the wiki** — ZT4AI was already referenced 12 times across the wiki at this page's creation, even without a dedicated framework page. The reference base reflects ZT4AI's strong mindshare among Microsoft-centric practitioners.
- **March 2026 update** — Microsoft's continuing investment is a positive signal for the framework's longevity; a pure marketing-only framework would not warrant a workshop + assessment-tool refresh.

## May 2026 status update and stack additions

A re-review on 2026-05-23 confirmed the page is structurally current but updated several status and coverage points:

- **Identity moved to GA.** [[microsoft-entra-agent-id|Entra Agent ID]] is GA (April 2026) and **Agent 365** is GA (May 1, 2026). The legacy Entra agent-registry API retires **June 15, 2026**; agents not re-registered through the new Graph API stop working. **Conditional Access for Agent Identities** is now a first-class feature with three access patterns (on-behalf-of, application-only, agent's-user-account) and attribute-driven targeting.
- **Egress gateway named.** Azure API Management's AI Gateway is the Microsoft LLM-egress chokepoint (token-limit, token-metric, semantic caching, inline Content Safety, backend load-balancing) and brokers MCP servers with Entra / OAuth 2.0 / JWT authorization. It was absent from the original Network pillar and is now added above.
- **Runtime guardrails detailed.** Azure AI Content Safety ships **Prompt Shields** (direct + indirect injection), **Groundedness Detection with correction** (the direct RAG-hallucination control), and **Protected Material** — all GA. **Defender AI-agent runtime protection** (preview) blocks unsafe tool actions (instruction exfiltration, data leak, credential leakage) and evaluates Copilot Studio prompts and responses.
- **Control plane.** Microsoft's **Agent Governance Toolkit** (OSS, MIT, April 2026) provides deterministic sub-millisecond policy enforcement bridged to Entra Agent ID — Microsoft's PDP-side primitive, alongside Conditional Access.
- **Data plane.** **Purview DSPM for AI** (new unified experience rolling to GA through late May 2026) is the specific oversharing / data-posture control for RAG and Copilot agents.
- **Copilot Studio** carries its own governance surface (Entra authentication default, Power Platform DLP / data policies, Purview audit logging and sensitivity-label inheritance, pre-publish security scan). It is the likely build surface for Azure-native RAG agents and warrants its own treatment.

## Limitations

- **Microsoft-centric.** Reference architecture and assessment tool assume the Microsoft Security stack; cross-cloud or non-Microsoft deployments need translation.
- **No agency-vs-autonomy distinction.** Uses both terms but does not formalize the split (now anchored from the [[aws-agentic-ai-security-scoping-matrix|AWS Scoping Matrix]]).
- **No formal threat enumeration per pillar.** Like [[maais-multilayer-agentic-ai-security|MAAIS]] (and unlike [[csa-maestro|MAESTRO]]), the ZT4AI pillars list controls without per-pillar threat models tying each control to a specific adversary action.
- **Control count not publicly maintained.** The "~700 controls" figure is from earlier ZT4AI documentation; the March 2026 update reorganized rather than re-enumerated; current control count is not publicly stated.
- **No specific ATLAS / OWASP ASI mapping.** Practitioners must do their own crosswalk to MITRE ATLAS techniques or OWASP ASI Top 10 categories.
- **Narrow residual agentic-egress gaps.** Even with APIM AI Gateway, Microsoft has no native **MCP tool-integrity / rug-pull defense** (Microsoft's own OWASP-MCP-for-Azure guidance states "there is no single Azure service dedicated to MCP-specific protection"), no **per-task capability tokens** (Entra tokens are scoped per agent identity, not per task), and only thin **agent-to-agent authorization** beyond identity. An all-Microsoft buyer fills these off-stack.

## Use cases

- **Microsoft-stack practitioners** — direct implementation guide for agentic-AI security inside the Microsoft Security ecosystem.
- **Reference-architecture comparison** — useful as a reference design when comparing alternative architectures (the [[agentic-ai-security-reference-architecture|wiki RA]], [[csa-maestro|CSA MAESTRO]], [[maais-multilayer-agentic-ai-security|MAAIS]], [[aws-agentic-ai-security-scoping-matrix|AWS Scoping Matrix]]).
- **Workshop / assessment** — Microsoft's ZT4AI Assessment Tool (March 2026 update) provides a self-evaluation surface for orgs already on the Microsoft stack.

## Provenance

The framework's foundational ZT4AI documentation predates the wiki. The March 2026 reference architecture, workshop, and assessment-tool refresh are anchored at [[microsoft-secure-agentic-ai-end-to-end|Vasu Jakkal's pre-RSAC 2026 post]]. The wiki's prior 12 inbound references are now consolidated through this canonical framework page.

<!-- sources:auto -->
## Sources

- [Secure Agentic AI End-to-End](https://www.microsoft.com/en-us/security/blog/2026/03/20/secure-agentic-ai-end-to-end/)
<!-- /sources -->
