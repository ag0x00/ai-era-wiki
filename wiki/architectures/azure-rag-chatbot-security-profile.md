---
type: architecture
title: "Azure-Native RAG Chatbot Security Profile (Copilot Studio)"
address: c-000131
origin: produced
created: 2026-05-25
updated: 2026-06-01
tags:
  - architectures
  - reference-implementation
  - copilot-studio
  - rag
  - azure
  - sec-of-ai
status: developing
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[inference-exposure]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
sources:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
---

# Azure-Native RAG Chatbot Security Profile (Copilot Studio)

This page projects the recalibrated [[agentic-ai-security-reference-architecture|six-plane RA]] and [[agentic-ai-security-cmm-2026|nine-domain CMM]] onto one common deployment: a **closed-corpus, member- or customer-facing RAG chatbot built on Microsoft Copilot Studio** in an E5 + Copilot tenant. Each plane and domain maps to a specific Microsoft control with its GA status and a realistic target level. The profile carries **no regulatory crosswalk** by design, so it applies to any organization running this shape. The FFIEC/GLBA and Canadian-finance crosswalks are separate pages.

> [!gap] Scope and grounding
> This profile covers the **closed-corpus chatbot shape**: it grounds on internal SharePoint/Dataverse/Graph content, answers users, and holds no external write tools. It does not cover coding copilots, MCP providers, or multi-agent meshes. Control mappings draw on the [[agentic-ai-security-cmm-d2-identity|D2]]–[[agentic-ai-security-cmm-d9-operations|D9]] deep dives plus verified Copilot Studio documentation. Tooling status is a May 2026 snapshot.

## The deployment shape

| Attribute | This profile |
|---|---|
| Host | Microsoft Copilot Studio agent (Power Platform) |
| Model | Azure OpenAI under Copilot Studio orchestration |
| Knowledge | SharePoint / OneDrive, Dataverse, Graph connectors over internal data (closed corpus) |
| Users | Authenticated members / customers or employees |
| Tools | Retrieval and answer only — no external write actions, no MCP tool reach |
| Licensing | Microsoft 365 E5 + Copilot; Power Platform managed environment |

The bot reads private data but has no external-communications or write path, so **the [[lethal-trifecta|lethal trifecta]] is broken by architecture**. That fact lowers the required level across D3, D4, D5, D7, and D9. The required controls narrow to a short list, and most of the full RA falls out of scope here.

## The control profile (plane / domain → Microsoft control)

| Plane / Domain | Realistic target | Microsoft control | Status | The one thing that matters |
|---|---|---|---|---|
| **Identity (D2)** | L3 | Entra Agent ID for the Copilot Studio agent (auto-created per environment)[^agentid]; end users authenticate with Entra[^auth] | Agent ID **preview**; user auth GA | Per-agent identity is the prerequisite that unlocks the egress and observability scores (the D2→D5 and D2→D7 caps) |
| **Control / Least-Agency (D3)** | L2 → L3 | Copilot Studio topics + generative orchestration; **Power Platform DLP** classifying connectors Business/Non-Business/Blocked[^ppdlp]; managed environment | GA | Power Platform DLP governs connectors, tools, channels, and the auth requirement, not the generated text — a separate plane from response-content DLP |
| **Runtime / Guardrails (D4)** | L3 | Azure AI Content Safety + **Prompt Shields**, on by default and non-optional, dual-pass; moderation level defaults to High[^content] | GA | Enterprise-baseline guardrails the maker can tune but not disable; a no-tool bot needs no chain-of-thought auditing |
| **Egress / Network (D5)** | L2 → L3 | None needed for a no-tool bot beyond the platform boundary; if connectors reach out, Azure API Management AI Gateway / Entra Internet Access[^apim] | GA | Egress is mostly out of scope because the trifecta is broken, so a mesh gateway is unwarranted for one bot |
| **Data / Memory / RAG (D6)** | **L3 → L4 — the load-bearing plane** | Entra-authenticated knowledge sources enforce **per-user permission trimming at answer time**[^auth]; **Purview DSPM for AI** + oversharing assessments; sensitivity labels honored on the SharePoint source; **Restricted SharePoint Search** as a stopgap[^dspm][^rss] | GA | Oversharing / [[inference-exposure\|inference exposure]] is the live risk, and the remediation is a multi-quarter labor project, not a purchase |
| **Observability (D7)** | L3 | Copilot Studio analytics; **Purview DSPM for AI** sees the agent (Audit must be on); Sentinel ingestion; Defender **AIAgentsInfo** hunting table[^dspm][^defender] | GA; Defender AIAgentsInfo table newly released | Logging is near-zero licensing on E5; the variable is the SIEM ingestion run-rate, with low-fidelity logs tiered to a cheaper plane |
| **Governance (D1)** | L2 → L3 | Power Platform admin center; Purview Compliance Manager; Agent 365 inventory; a named owner/sponsor | GA | A chatbot needs an owner, a policy, and a risk-tier — not a certification near-term |
| **Supply Chain (D8)** | L2 → L3 | Consumer-grade only: knowledge-source and connector provenance; sensitivity-label hygiene; the agent is a **model consumer**, not a producer | GA | Producer-grade AI-BOM generation, training-data provenance, and ML-VEX do not apply to a model consumer |
| **Operations (D9)** | L2 → L3, narrow | Owner-departure decommission via Entra Agent ID delete (cascades child cleanup); CoSAI-derived IR runbook; canary-token / system-prompt trip-wire | GA | No HITL queue to fatigue for a read-only bot; the load-bearing items are a decommission runbook and a system-prompt trip-wire |

## The four controls that carry this profile

Everything above reduces to four load-bearing controls. With these in place the deployment meets its target levels; the remaining controls are right-sized out.

1. **Force Entra authentication, and block the no-auth path.** Entra-authenticated knowledge sources trim every answer to what the *querying* user is permitted to see. This is answer-time entitlement enforcement, not a single service identity.[^auth] A maker can still publish a "No authentication" agent, which removes trimming. Close that path with a tenant **Power Platform data policy that blocks the *Chat without Microsoft Entra ID authentication* connector**[^ppdlp], the highest-priority control in the profile.
2. **Remediate oversharing on the reachable corpus.** Per-user trimming helps only if SharePoint permissions are correct. Run **Purview DSPM for AI** oversharing assessments and remediate before launch; treat it as a multi-quarter project, not a switch.[^dspm] Uploaded files and public-website sources carry **no per-user permissions**; treat them as a flat shared corpus.
3. **Turn off ungrounded responses and keep the default guardrails.** Set *Allow ungrounded responses* off so the agent declines when no knowledge source was used, and leave Content Safety + Prompt Shields at the default High moderation.[^content] Off is the closest "answer only from the corpus" lever, though it is not an absolute guarantee.
4. **Give the agent a per-agent identity and a decommission path.** The Entra Agent ID makes the bot a first-class principal for Conditional Access, audit attribution, and clean teardown; deleting the agent deletes the identity.[^agentid] It is still preview, so track its move to GA.

## Controls this shape does not need

For this shape, the following controls add cost without reducing risk. They are recorded as intentional trade-offs, not deficiencies:

- **Per-task capability tokens, mesh gateway sidecars, A2A authorization** (D5/D3 L5): there are no tools and no agent-to-agent traffic.
- **Chain-of-thought / alignment auditing** (D4 L4): no tool-call surface to hijack.
- **Behavioral-drift detection and multi-tool red-team programs** (D7 L4): disproportionate for a single read-only bot; a basic eval (for example PyRIT) suffices.
- **Producer-grade AI-BOM, training-data provenance, ML-VEX** (D8 `[P]`): the bot is a model consumer.
- **Quarterly decommission drills, HITL-fatigue dashboards, certification** (D9 L4 / D1 L5): no HITL queue, and certification is premature near-term for a read-only, no-tool bot.

## Cost signal

Licensing is **near-zero** for an E5 + Copilot incumbent: Entra Agent ID, Purview DSPM for AI, DLP, Content Safety, Prompt Shields, and Sentinel ingestion all sit inside existing entitlements. The real spend lands in three places: the **oversharing-remediation labor project** (the dominant cost, one to two FTE-equivalent over two to four quarters, recurring as new content arrives), the **per-agent identity and DLP-policy rollout** (weeks of platform work), and the **SIEM ingestion run-rate** (agent logs run roughly 10–20× human volume per the [[agentic-ai-security-cmm-d7-observability|D7 observability deep dive]]; tier low-fidelity logs to a cheaper data-lake plane). Reaching the realistic targets requires no new product purchase.

## Caveats and preview-watch

- **Entra Agent ID for Copilot Studio is preview** (opt-out is temporary, and identities are slated to become required).[^agentid] A buyer who cannot run preview features should plan for its GA.
- **Response-content DLP is narrow.** Sensitivity-label enforcement on the agent's answers applies **only to the SharePoint knowledge source**. Uploaded-file and website corpora get no label-based DLP, and the "block sensitive-info-types in prompts" control is an M365-Copilot-proper feature not confirmed for custom Copilot Studio agents.[^respdlp]
- **The regulatory crosswalk is omitted by design.** A regulated buyer maps this profile to its examiner's expectations through the separate FFIEC/GLBA and Canadian-finance crosswalk pages; nothing here is a compliance attestation.

## Notes

[^agentid]: [Microsoft Learn — Automatically create Entra agent identities for Copilot Studio (preview)](https://learn.microsoft.com/en-us/microsoft-copilot-studio/admin-use-entra-agent-identities), 2026. Per-environment auto-creation from 18 Mar 2026; delete-agent deletes the identity.
[^auth]: [Microsoft Learn — Configure user authentication in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/configuration-end-user-authentication), 2026. Entra-authenticated agents surface only content the querying user can access (answer-time security trimming).
[^ppdlp]: [Microsoft Learn — Configure data policies for agents (Power Platform DLP)](https://learn.microsoft.com/en-us/microsoft-copilot-studio/admin-data-loss-prevention), 2026. Connector classification Business/Non-Business/Blocked; blocking the no-auth connector; governs connectors/tools/channels, not generated text.
[^content]: [Microsoft Learn — Knowledge and content moderation in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-copilot-studio), 2026. Content Safety + Prompt Shields on by default; moderation level (default High); Allow-ungrounded-responses toggle.
[^dspm]: [Microsoft Learn — Purview DSPM for AI and Copilot Studio](https://learn.microsoft.com/en-us/purview/ai-copilot-studio), 2026. DSPM for AI sees Copilot Studio agents (Audit required); oversharing assessment and sensitivity-label support.
[^rss]: [Microsoft Learn — Restricted SharePoint Search](https://learn.microsoft.com/en-us/sharepoint/restricted-sharepoint-search), 2026. Site-capped stopgap for oversharing while remediation proceeds.
[^defender]: [Microsoft — Securing AI agents end-to-end (Purview, Agent 365, Defender)](https://techcommunity.microsoft.com/blog/microsoft-security-blog/securing-ai-agents-end%E2%80%91to%E2%80%91end-connecting-purview-dspm-agent-365-and-the-ai-secur/4521155), 2026. Defender AIAgentsInfo hunting table; Sentinel ingestion of Copilot audit events.
[^apim]: [Microsoft Learn — AI gateway capabilities in Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities), 2026. Token governance, content-safety policy, MCP brokering — relevant only if the bot gains external connector reach.
[^respdlp]: [Microsoft Learn — DLP for the Microsoft 365 Copilot location](https://learn.microsoft.com/en-us/purview/dlp-microsoft365-copilot-location-learn-about), 2026. Label-based response restriction is SharePoint-source-scoped; SIT-in-prompt blocking is M365-Copilot-proper.
