---
type: review
title: "ZT4AI Standards Review"
address: c-000150
origin: produced
created: 2026-05-26
updated: 2026-06-22
tags:
  - reviews
  - standards-review
  - microsoft
  - zero-trust
  - agentic-ai
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
methodology: "[[standards-validation-methodology-2026-05]]"
target: "[[agentic-ai-security-cmm-2026]]"
standard: "[[microsoft-zt4ai]]"
reviewer: "Claude (Opus 4.7)"
review_started: "2026-05-26"
review_completed: "2026-05-26"
adversarial_pass: "completed 2026-05-26"
related:
  - "[[microsoft-zt4ai]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[microsoft-entra-agent-id]]"
  - "[[microsoft-rai]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
sources:
  - "https://www.microsoft.com/en-us/security/blog/2026/03/19/new-tools-and-guidance-announcing-zero-trust-for-ai/"
  - "https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview"
  - "https://learn.microsoft.com/en-us/entra/identity/conditional-access/agent-id"
  - "https://learn.microsoft.com/en-us/entra/id-protection/concept-risky-agents"
  - "https://learn.microsoft.com/en-us/security/zero-trust/sfi/secure-agentic-systems"
  - "https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/"
---

# Standards Review — Microsoft ZT4AI (Zero Trust for AI), 2026-Q2

This review answers the open item carried in [[agentic-cmm-vs-standards-validation|the 2026-04-30 validation]] §2: that [[microsoft-zt4ai|ZT4AI]] is "the most control-rich AI security framework available" and that "the CMM does not crosswalk a single ZT4AI control," with the three Zero Trust pillars proposed as a way to ground [[agentic-ai-security-cmm-d2-identity|D2]] and [[agentic-ai-security-cmm-d3-control-least-agency|D3]]. It applies the [[standards-validation-methodology-2026-05|Standards Validation Methodology]]: primary-source citations, a control-level coverage view, and falsifiable absence claims with bounded scope.

The review produced one correction to the original gap statement and a control-level crosswalk spanning all nine CMM domains and the six [[agentic-ai-security-reference-architecture|RA]] planes. The crosswalk evidence **validates the 2026-05 CMM recalibration** rather than overturning it: the points where the recalibration graded a control preview or off-stack (D4 reasoning controls, D5 MCP tool-integrity, D7 behavioral detection, D9 human-factors tooling) match Microsoft's own documentation.

This review covers the **control catalogue** only. The [[microsoft-rai|Microsoft RAI Standard]] (a responsible-AI goals standard, seventeen goals) and [[microsoft-entra-agent-id|Agent 365]] (the agent-management control plane over these controls) are reviewed separately in [[standards-review-microsoft-rai-agent-365-2026-Q2|the 2026-Q2 RAI / Agent 365 review]]. Where Agent 365 surfaces a control catalogued here (Entra Agent ID, Purview, Defender), its catalogue membership stays in this review; the management-plane role stays in that one.

**The "700 controls / 116 groups / 33 swim lanes" figure describes the whole Workshop, not ZT4AI.** The Microsoft Security Blog states that the **entire Zero Trust Workshop 3.0** "covers 700 security controls across 116 logical groups and 33 functional swim lanes" (Source: [Microsoft Security Blog, 2026-03-19](https://www.microsoft.com/en-us/security/blog/2026/03/19/new-tools-and-guidance-announcing-zero-trust-for-ai/)) — all seven pillars, most of them not AI. Microsoft describes the AI pillar by what it *assesses* (AI access and agent identities, data protection, AI monitoring, responsible governance), with **no AI-specific control count**. Third-party write-ups (for example [anoopcnair.com, 2026-03-20](https://www.anoopcnair.com/zero-trust-workshop-3-0-updated-with-ai-pillar/)) conflated the whole-Workshop number with the AI pillar. The original gap statement repeated that conflation, so ZT4AI is not "the most control-rich AI security framework" on that basis. The automated Zero Trust Assessment for AI pillar is in development, expected summer 2026.

## Primary documents reviewed

ZT4AI has no single specification document. The review anchors on the March 2026 announcement plus the Microsoft Learn pages that document the named controls. None is paywalled; none has a local archived copy (HTML sources).

| Document | URL | Date / `ms.date` | Scope used in this review |
|---|---|---|---|
| Zero Trust for AI announcement | [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/03/19/new-tools-and-guidance-announcing-zero-trust-for-ai/) | 2026-03-19 | March 2026 deliverables; AI pillar; Workshop control figure |
| Microsoft Entra security for AI overview | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview) | 2026-05-08 | Agent identity; access packages (D2) |
| Conditional Access for Agent Identities | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/identity/conditional-access/agent-id) | 2026-05-18 | Three access patterns; attribute targeting (D2) |
| ID Protection for agents (risky agents) | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/id-protection/concept-risky-agents) | current | Agent risk; auto-remediation (D2) |
| Governing Agent Identities (ID Governance) | [Microsoft Learn](https://learn.microsoft.com/en-us/entra/id-governance/agent-id-governance-overview) | 2026-05-01 | Sponsors; manager-transfer; lifecycle (D9) |
| Secure autonomous agentic AI systems | [Microsoft Learn](https://learn.microsoft.com/en-us/security/zero-trust/sfi/secure-agentic-systems) | 2026-03-19 | Deny-by-default least-action design (D3) |
| Agent Governance Toolkit | [Microsoft Open Source Blog](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/) | 2026-04-02 | Runtime PDP; trust decay (D3) |
| Purview — secure and govern AI | [Microsoft Learn](https://learn.microsoft.com/en-us/purview/ai-microsoft-purview) | 2026-05-01 | Answer-time entitlement; labels; DLP (D6) |
| Purview DSPM for AI — oversharing | [Microsoft Learn](https://learn.microsoft.com/en-us/purview/data-security-posture-management-oversharing) | current | Oversharing assessment + remediation (D6) |
| Azure AI Content Safety — what's new | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/whats-new) | current | Prompt Shields; Groundedness; Task Adherence (D4) |
| Defender for Cloud — AI model security | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-model-security) | 2026-04-01 | CI/CD model scanning (D8) |
| Defender for Cloud — AI-SPM / AI-BOM | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-security-posture) | 2026-05-18 | AI-BOM discovery; agent discovery (D8) |
| OWASP MCP Top 10 for Azure — Tool Poisoning | [Microsoft (GitHub Pages)](https://microsoft.github.io/mcp-azure-security-guide/mcp/mcp03-tool-poisoning/) | current | MCP tool-integrity guidance (D5) |
| APIM AI Gateway capabilities | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities) | current | Token limits; content safety; MCP brokering (D5) |
| Defender XDR — AI agent detection | [Microsoft Learn](https://learn.microsoft.com/en-us/defender-xdr/security-for-ai/ai-agent-detection-protection) | 2026-04-28 | Agent detections; Advanced Hunting (D7) |
| Sentinel data lake MCP server | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/sentinel/datalake/sentinel-mcp-overview) | 2026-05-07 | Agentic-SOC tooling (D7) |
| Security for AI — govern | [Microsoft Learn](https://learn.microsoft.com/en-us/security/security-for-ai/govern) | current | Purview Compliance Manager AI templates (D1) |

The March 2026 update delivered a new AI pillar in the Zero Trust Workshop, updated Data and Network pillars in the Zero Trust Assessment tool, a Zero Trust reference architecture for AI, and five patterns-and-practices (threat modeling for AI, AI observability, securing agentic systems, robust safety engineering, defense-in-depth for indirect prompt injection). None of these re-enumerates an AI-specific control count or introduces an AI-specific "logical group / swim lane" structure.

## Pillar-to-domain grounding

The three Zero Trust principles ground specific CMM rungs once mapped to named, shipping controls rather than to the pillar label alone:

- **Verify explicitly → D1, D2, D7, D8.** A distinct directory-resident identity per agent with authorization re-evaluated at use (D2); named accountable owners and compliance assessment (D1); agent telemetry and detection (D7); AI-BOM discovery and provenance (D8).
- **Use least privilege → D2, D3, D5, D6, D9.** Scoped, time-bound, revocable grants (D2 credential lifecycle, D9 access packages); deny-by-default action authorization at a policy decision point outside the model (D3); identity-scoped egress and token limits (D5); answer-time entitlement against oversharing (D6).
- **Assume breach → D4, D5, D6, D7, D9.** Runtime guardrails and tool-call blocking (D4); network-layer prompt-injection containment (D5); oversharing remediation and risky-AI-usage detection (D6); behavioral detection and agentic-SOC response (D7); automatic capability contraction when risk or policy-violation signals rise (the adaptive controls at L4/L5).

## Control-level coverage matrix (CMM × ZT4AI control)

Each row cites a named Microsoft control, its Zero Trust pillar, its shipping status, and the CMM rung it grounds. Status reflects the May 2026 document dates.

| CMM rung | ZT4AI / Microsoft control | Pillar | Status | Source |
|---|---|---|---|---|
| D1 L3 (named owners, impact assessment) | [[microsoft-rai\|Responsible AI Standard]] — mandatory impact assessments; named accountable owners; RAI embedded in the dev lifecycle | Verify explicitly | Guidance | [RAI Standard](https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/microsoft/final/en-us/microsoft-brand/documents/Microsoft-Responsible-AI-Standard-General-Requirements.pdf) |
| D1 L3–L4 (compliance assessment) | Purview Compliance Manager AI templates — EU AI Act, NIST AI RMF, ISO/IEC 42001, ISO/IEC 23894; syncs Foundry eval results | Verify explicitly | GA | [Learn — govern](https://learn.microsoft.com/en-us/security/security-for-ai/govern) |
| D1 L3 (agent inventory) | Agent 365 registry — discovers and inventories agents (incl. Bedrock / GCP sync) for lifecycle governance | Least privilege | GA (sync preview) | [Agent 365 GA](https://www.microsoft.com/en-us/security/blog/2026/05/01/microsoft-agent-365-now-generally-available-expands-capabilities-and-integrations/) |
| D2 L3 (verifiable per-agent identity) | [[microsoft-entra-agent-id\|Entra Agent ID]] — per-agent directory identity minted from agent blueprints | Verify explicitly | GA | [Learn — security for AI](https://learn.microsoft.com/en-us/entra/agent-id/security-for-ai-overview) |
| D2 L3–L4 (delegation, scoped authz) | Three access patterns: on-behalf-of (delegated), application-only (client-credentials), agent's-user-account | Verify + least privilege | GA | [Learn — CA for agent identities](https://learn.microsoft.com/en-us/entra/identity/conditional-access/agent-id) |
| D2 L4 (policy at scale) | Attribute-driven + blueprint-level Conditional Access — custom security attributes auto-apply one rule to a class; disable an entire agent class in one operation | Verify + least privilege | GA | [Learn — CA for agent identities](https://learn.microsoft.com/en-us/entra/identity/conditional-access/agent-id) |
| D2 L4–L5 (behavioral baseline → adaptive block) | ID Protection for agents — per-agent risk baseline; risk-based CA; automatic remediation; Confirm-compromise / Disable; Managed Policies block high-risk agents | Assume breach | GA-direction | [Learn — risky agents](https://learn.microsoft.com/en-us/entra/id-protection/concept-risky-agents) |
| D2 L4 (time-bound role grants) | Entra PIM — time-limited active role assignment for agent identities (auto-expiring at duration end). **Caveat:** agents cannot be PIM-*eligible*, so there is no agent self-activated JIT — the assignment is admin-granted active with an expiry, not eligible-then-activate | Least privilege | GA | [Learn — Assign roles in PIM](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-how-to-add-role-to-user) |
| D3 L3–L4 (deny-by-default authz) | Least-action design — "start with no permitted actions by default"; agents as microservices; explicit action schemas; deterministic HITL in orchestrator logic | Least privilege | Guidance | [Learn — secure agentic systems](https://learn.microsoft.com/en-us/security/zero-trust/sfi/secure-agentic-systems) |
| D3 L4 (inline PDP) | Agent Governance Toolkit — stateless policy engine intercepting each action sub-millisecond; OPA Rego / Cedar / YAML; execution rings with resource limits | Least privilege | OSS (MIT) | [Open Source Blog](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/) |
| D3 L5 (adaptive contraction) | Agent Governance Toolkit — trust decay + automated kill switch; SLO/error-budget model auto-restricts capability when policy-violation rate exceeds threshold until the agent recovers | Assume breach | OSS (MIT) | [Open Source Blog](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/) |
| D4 L2–L3 (input/output safety) | Azure AI Content Safety — Prompt Shields (direct + indirect); Protected Material (text) | Assume breach | GA | [Learn — Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/whats-new) |
| D4 L4 (reasoning-layer controls) | Groundedness Detection with correction (English-only); Task Adherence — detects tool-call / output divergence from the task | Verify explicitly | Preview | [Learn — Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/whats-new) |
| D4 L4–L5 (runtime tool-call blocking) | Defender AI-agent runtime protection — inspects tool invocations, blocks unsafe calls before execution | Assume breach | Preview (Agent 365 sub from 2026-07-01) | [Learn — runtime protection](https://learn.microsoft.com/en-us/defender-cloud-apps/real-time-agent-protection-during-runtime) |
| D5 L3 (network-layer egress) | Entra Internet Access — network-layer prompt-injection protection + Shadow AI detection (text only) | Assume breach | GA (2026-03-31) | [Entra RSAC 2026](https://techcommunity.microsoft.com/blog/microsoft-entra-blog/microsoft-entra-innovations-announced-at-rsac-2026/4502146) |
| D5 L3–L4 (LLM/MCP gateway) | Azure API Management AI Gateway — token-limit and quota policies; inline Content Safety; MCP-server brokering with Entra / OAuth / JWT | Least privilege | GA (core policies) | [Learn — AI gateway](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities) |
| D5 L4–L5+ (MCP tool-integrity) | No single Azure service for MCP tool-poisoning / rug-pull; composed pattern only — registry, version-pinning, egress allowlists, runtime monitoring | Assume breach | Guidance (no product) | [Microsoft — MCP03 Tool Poisoning](https://microsoft.github.io/mcp-azure-security-guide/mcp/mcp03-tool-poisoning/) |
| D6 L3 (answer-time entitlement) | Purview — agents return only data the user holds VIEW + EXTRACT rights to; DSPM for AI oversharing assessment + bulk remediation | Least privilege | GA | [Learn — Purview for AI](https://learn.microsoft.com/en-us/purview/ai-microsoft-purview) |
| D6 L3–L4 (label-aware DLP) | Sensitivity-label inheritance into AI interactions; DLP for AI / Copilot; data classification in prompts and responses | Least privilege | GA | [Learn — Purview for AI](https://learn.microsoft.com/en-us/purview/ai-microsoft-purview) |
| D6 L4 (risky-usage detection) | Insider Risk — Risky AI Usage template (prompt-injection attempts, protected-material access) feeding Defender XDR | Assume breach | GA | [Learn — oversharing](https://learn.microsoft.com/en-us/purview/data-security-posture-management-oversharing) |
| D7 L3–L4 (agent detection) | Defender XDR — near-real-time AI-agent detections; `AIAgentsInfo` / `CloudAppEvents` Advanced Hunting tables | Assume breach | Preview | [Learn — AI agent detection](https://learn.microsoft.com/en-us/defender-xdr/security-for-ai/ai-agent-detection-protection) |
| D7 L4–L5 (agentic SOC) | Sentinel data-lake MCP server; AI Entity Analyzer; data federation; Playbook Generator | Assume breach | Preview | [Learn — Sentinel MCP](https://learn.microsoft.com/en-us/azure/sentinel/datalake/sentinel-mcp-overview) |
| D8 L3 (AI-BOM discovery) | Defender for Cloud AI-SPM — generative AI-BOM discovery across Azure, Amazon Bedrock, Google Vertex; dependency-vuln scan; attack-path analysis. Extended in Defender for Cloud Apps to MCP-server and AI-model-provider catalog coverage ([Discover risks in AI model providers and MCP servers](https://techcommunity.microsoft.com/blog/microsoftthreatprotectionblog/discover-risks-in-ai-model-providers-and-mcp-servers-with-microsoft-defender/4440050)). Distinct from MCP tool-integrity verification (D5 absence claim) | Verify explicitly | GA | [Learn — AI-SPM](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-security-posture) |
| D8 L3–L4 (artifact scanning) | Defender for Cloud AI model scanning in CI/CD — malware, unsafe operators, exposed secrets in model artifacts | Verify + assume breach | Preview (2026-04) | [Learn — model security](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-model-security) |
| D9 L3 (lifecycle accountability) | Entra ID Governance — human sponsor per agent with automatic manager-transfer; Lifecycle Workflows | Verify explicitly | GA | [Learn — agent governance](https://learn.microsoft.com/en-us/entra/id-governance/agent-id-governance-overview) |
| D9 L4 (time-bound grants, decommission) | Access packages with expiry + sponsor notification; disable / decommission via My Account | Least privilege | GA | [Learn — agent governance](https://learn.microsoft.com/en-us/entra/id-governance/agent-id-governance-overview) |

The CMM's `Maps to:` lines now carry control-level backing rather than a pillar label. The matrix mirrors the dependency caps already in the model: the adaptive D2 controls (ID Protection auto-remediation) and the D3 policy decision point (Agent Governance Toolkit) lift D2/D3 from defined to managed/optimizing. Most of the adaptive D4/D7/D9 layer is **preview**, consistent with the recalibration's grading.

### RA-plane mapping

The six [[agentic-ai-security-reference-architecture|RA]] planes map onto the same controls, since the planes correspond one-to-one to D2–D7:

| RA plane | Anchor ZT4AI controls |
|---|---|
| Identity | Entra Agent ID; three access patterns; attribute/blueprint Conditional Access; ID Protection for agents |
| Control | Least-action design; Agent Governance Toolkit (OPA/Cedar/YAML PDP, trust decay) |
| Runtime | Prompt Shields; Groundedness + Task Adherence (preview); Defender AI-agent runtime protection (preview) |
| Egress | Entra Internet Access PI protection; APIM AI Gateway; MCP brokering (tool-integrity is guidance-only) |
| Data | Purview answer-time entitlement; DSPM oversharing; label-aware DLP |
| Observability | Defender XDR agent detections; Sentinel agentic-SOC tooling; Agent 365 telemetry |

> [!contradiction] Predictive Shielding is not an agent-identity control
> ZT4AI commentary sometimes cites "Predictive Shielding" as an adaptive identity control. Per primary source it is a **Microsoft Defender XDR feature in Preview**, operating on devices, users, and credentials through Defender for Endpoint — not on agent identities (Source: [Microsoft Learn — Predictive Shielding, updated 2026-05-03](https://learn.microsoft.com/en-us/defender-xdr/shield-predict-threats)). The agent-specific equivalent of "contract access during an attack" is ID Protection auto-remediation (D2) and the Agent Governance Toolkit trust-decay model (D3). The [[microsoft-zt4ai|ZT4AI page]] describes Predictive Shielding as a step-down primitive; that description should carry the Defender-XDR-Preview scope.

## Falsifiable absence claims found

What ZT4AI, as documented in the scope above, does not provide that the CMM scores. Each claim is bounded to the searched documents and reversible by the stated refuting evidence.

1. **No per-task capability tokens.** ZT4AI scopes authorization to the agent identity (or its blueprint), not to a single task. *Searched:* the Entra security-for-AI overview, CA-for-agent-identities, and secure-agentic-systems pages. *Terms:* "per-task", "capability token", "task-scoped token", "warrant". *Verdict:* not addressed. *Refuting evidence:* any Microsoft control issuing a credential bound to one task rather than to the agent identity. *Reviewed 2026-05-26.* This is the CMM `D2 L5+` / `D3 L5` residual (see [[tenuo-warrant|Tenuo Warrant]] as the only shipping primitive).

2. **No single MCP tool-integrity / rug-pull product.** Microsoft's own guidance states there is no Azure service dedicated to MCP-specific protection and prescribes a composed pattern (internal tool registry, version-pinning, egress allowlists, runtime monitoring). *Searched:* the MCP03 Tool Poisoning guide, APIM AI Gateway docs, the ZT4AI announcement. *Terms:* "tool integrity", "rug pull", "tool poisoning", "MCP server verification". *Verdict:* covered only as guidance, no shipping product. *Refuting evidence:* a Microsoft service that verifies MCP tool integrity across versions automatically. *Reviewed 2026-05-26.* Matches CMM `D5 L4–L5+` (off-stack).

3. **No formal agency-vs-autonomy promotion model.** ZT4AI uses "least privilege" and "deterministic human-in-the-loop" but does not define progressive-autonomy tiers with promotion criteria. *Searched:* secure-agentic-systems, the announcement. *Terms:* "autonomy level", "promotion", "progressive autonomy", "tier". *Verdict:* not addressed. *Refuting evidence:* a Microsoft control defining staged autonomy tiers with promotion gates. *Reviewed 2026-05-26.* The CMM `D3 L4` fills this from the [[csa-maestro|CSA Agentic Trust Framework]], not from ZT4AI.

4. **No human-factors / HITL-fatigue tooling.** Microsoft ships approval and oversight mechanisms (sponsor approvals, access-package cycles, Defender block-then-alert) but nothing that measures approval/alert fatigue, oversight load, or rubber-stamp rate. *Searched:* Entra Agent ID, Agent 365, Defender security-for-AI, the ZT4AI blogs. *Terms:* "fatigue", "human factors", "oversight load", "approval rate". *Verdict:* not addressed. *Refuting evidence:* a Microsoft control measuring HITL fatigue or oversight quality. *Reviewed 2026-05-26.* Matches CMM `D9` (the recalibration already grades HITL-fatigue measurement as having no product on any stack).

## Scope exclusions

- **The automated Zero Trust Assessment for AI.** It is in development (expected summer 2026); only the qualitative Workshop AI pillar exists today, and its question bank was not enumerated.
- **Per-control GA-vs-preview precision inside multi-feature services.** APIM MCP sub-features and some Defender capabilities carry tier- or date-specific preview notes; the matrix records the load-bearing status, not every sub-feature flag.
- **Production effectiveness.** This is a document-versus-document review per the methodology, not a deployment audit.

## Adversarial-pass log

`adversarial_pass: completed 2026-05-26`. A second reviewer (separate agent run) attempted a counter-example for each of the four absence claims against current Microsoft sources through May 2026. **All four claims survived; no counter-example met the bar of a shipping Microsoft control.**

1. **Per-task capability tokens — survives.** Entra Agent ID tokens are scoped to the agent identity or its blueprint in all three flows, with no per-invocation or single-action binding ([Tokens in the Microsoft agent identity platform](https://learn.microsoft.com/en-us/entra/agent-id/agent-tokens)). The closest adjacent control, Privileged Identity Management — which now accepts agent identities as assignees — activates a standing role for a time window, not a single task ([Activate roles in PIM](https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/pim-how-to-activate-role)).
2. **MCP tool-integrity / rug-pull — survives.** Microsoft's MCP guidance still prescribes a composed pattern ([Protecting against indirect prompt injection in MCP](https://developer.microsoft.com/blog/protecting-against-indirect-injection-attacks-mcp)). Defender's MCP coverage discovers and inventories servers and assesses risk posture but does not verify tool-definition integrity across versions ([Discover risks in AI model providers and MCP servers](https://techcommunity.microsoft.com/blog/microsoftthreatprotectionblog/discover-risks-in-ai-model-providers-and-mcp-servers-with-microsoft-defender/4440050)).
3. **Agency-vs-autonomy promotion model — survives, with a near-miss.** Copilot Studio autonomous-agent guidance advises incremental capability expansion without named tiers or gates ([Design autonomous agent capabilities](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/autonomous-agents)). The Agentic AI maturity model classifies agents by autonomy level and uses a tiering concept, but it is an organizational governance-capability maturity model, not a per-agent autonomy-promotion ladder with gates ([Agentic AI maturity model](https://learn.microsoft.com/en-us/agents/adoption-maturity-model/maturity-model-security-governance)).
4. **HITL-fatigue tooling — survives.** Defender XDR alert tuning and aggregation reduce alert volume but do not measure approver fatigue, rubber-stamp rate, or oversight quality ([Alert policies in the Defender portal](https://learn.microsoft.com/en-us/defender-xdr/alert-policies)); Copilot Studio analytics and Purview Communication Compliance review interactions but do not quantify oversight load.

The two adjacent controls surfaced by this pass have been folded into the crosswalk above: Entra PIM time-limited active role assignment for agent identities is now anchored under D2 (with the explicit caveat that agents cannot be PIM-*eligible*, so the model is admin-granted active with expiry, not eligible-then-self-activate); Defender MCP-server and AI-model-provider catalog discovery is captured under D8 as an extension of the AI-SPM AI-BOM discovery anchor, and remains distinct from MCP tool-integrity verification (which stays a documented gap at D5).

## Effect on existing wiki pages

- **[[agentic-cmm-vs-standards-validation|2026-04-30 validation]] §2 (ZT4AI row):** the "700 controls / 116 groups / 33 swim lanes … most control-rich AI security framework" claim is corrected — it is a whole-Workshop figure (stated in the Microsoft Security Blog for all seven pillars), not an AI-pillar count. A correction marker points here.
- **[[microsoft-zt4ai|ZT4AI framework page]]:** deepened with a per-pillar named-controls section and the March 2026 deliverables; the control-scale note now attributes the figure to the whole Workshop; the Predictive Shielding description carries its Defender-XDR-Preview scope.
- **[[agentic-ai-security-cmm-2026|Canonical CMM]]:** the `Maps to:` lines for D1, D3, D5–D9 gain control-level ZT4AI anchors (D2 and D4 already cited Microsoft controls); the crosswalk evidence validated the recalibration, so no level criteria changed.
- **[[agentic-ai-security-cmm-crosswalk|Standards crosswalk]]:** the ZT4AI column is rewritten from pillar labels to named controls per domain.
- **[[agentic-ai-security-reference-architecture|RA]]:** the stale "700+ controls across 116 groups" line in the Prior-Work table is corrected, and the six planes gain control-level ZT4AI anchors.
