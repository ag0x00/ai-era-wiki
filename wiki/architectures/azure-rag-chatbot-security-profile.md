---
type: architecture
title: "Azure-Native RAG Chatbot Security Profile (Copilot Studio)"
address: c-000131
origin: produced
created: 2026-05-25
updated: 2026-09-29
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
  - "[[google-cloud-agentic-security-profile]]"
  - "[[cmm-vocabulary-and-notation]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
sources:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Current Microsoft identity, billing and profile sources checked; no archived primary document is listed for this produced page."
---

# Azure-Native RAG Chatbot Security Profile (Copilot Studio)

This page applies the trust boundaries in the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] and the [[agentic-ai-security-cmm-2026|nine-domain CMM]] to one deployment shape: an **employee-facing, closed-corpus RAG chatbot built on Microsoft Copilot Studio** in an E5 + Copilot tenant. It identifies candidate Microsoft controls and target ranges; the [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]] determines observed levels from deployment evidence. A customer-facing bot needs a separate identity and entitlement analysis. The FFIEC/GLBA and Canadian-finance crosswalks sit on separate pages.

## The deployment shape

| Attribute | This profile |
|---|---|
| Host | Microsoft Copilot Studio agent (Power Platform) |
| Model | Azure OpenAI under Copilot Studio orchestration |
| Knowledge | SharePoint / OneDrive, Dataverse, Graph connectors over internal data (closed corpus) |
| Users | Employees authenticated in the tenant with per-user access to the knowledge sources |
| Tools | Retrieval and answer only — no external write actions, no MCP tool reach |
| Licensing | Microsoft 365 E5 + Copilot; Power Platform managed environment |

The bot reads private data but has no external write tool. Its operator must still establish the actual connector, model-provider, logging, and data paths before treating the [[lethal-trifecta|lethal trifecta]] as contained. A risk-selected target can be lower in domains whose high-impact action criteria do not apply; the assessor records each applicability decision rather than waiving an entire domain.

## The control profile

The ranges below are planning targets, not observed scores. Product status reflects Microsoft documentation read in September 2026. The assessor checks every applicable criterion through the target level and records supplier-held evidence gaps. [[google-cloud-agentic-security-profile|The Google Cloud Agentic Security Profile]] describes another deployment shape.

| Domain | Candidate target | Microsoft control | Status | Assessment focus |
|---|---|---|---|---|
| **Identity ([[agentic-ai-security-cmm-d2-identity\|D2]])** | L3 | Entra Agent ID for a new Copilot Studio agent[^agentid]; end users authenticate with Entra[^auth] | New agents receive Agent IDs; legacy identity migration remains in transition | Resolve the agent and accountable human in downstream decisions and traces; missing identity evidence blocks those particular D5 or D7 tests |
| **Control / Least-Agency ([[agentic-ai-security-cmm-d3-control-least-agency\|D3]])** | L2 → L3 | Copilot Studio action configuration and Power Platform DLP connector classification[^ppdlp] | GA | DLP governs connectors, tools, channels, and the authentication requirement; response-content screening is a separate check |
| **Runtime / Guardrails ([[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]])** | L3 | Azure AI Content Safety and Prompt Shields with default High moderation[^content] | GA | Test the configured prompt and response routes, including what the maker can change |
| **Egress / Network ([[agentic-ai-security-cmm-d5-egress-network\|D5]])** | L2 → L3 | Copilot Studio's hosted route and configured connectors; if the operator adds external tools, Azure API Management AI Gateway / Entra Internet Access[^apim] | Product dependent | Map every reachable endpoint, including supplier-held model and connector paths; inter-agent criteria apply only if such traffic exists |
| **Data / Memory / RAG ([[agentic-ai-security-cmm-d6-data-rag\|D6]])** | **L3 → L4 (priority)** | Entra answer-time permission trimming with Purview DSPM oversharing assessment[^auth][^dspm] | GA | Test entitlements on every reachable corpus and remediate excess SharePoint access |
| **Observability ([[agentic-ai-security-cmm-d7-observability\|D7]])** | L3 | Copilot Studio analytics with Purview audit and Sentinel ingestion[^dspm][^defender] | GA | Reconstruct sampled answers and decisions from records the bank can search or export; budget for ingestion and retention |
| **Governance ([[agentic-ai-security-cmm-d1-governance\|D1]])** | L2 → L3 | Power Platform admin center and Agent 365 inventory | GA | Verify the named owner, risk tier, approval route, and residual-risk decision |
| **Engineering and Supply Assurance ([[agentic-ai-security-cmm-d8-supply-chain\|D8]])** | L2 → L3 | Agent version and connector inventory; change-linked design review, configuration checks, supplier evidence, and release AI-BOM where the bank publishes a version | Product dependent | A hosted model can make producer criteria inapplicable, but customer-controlled changes and supplier-held release steps remain in scope |
| **Operations ([[agentic-ai-security-cmm-d9-operations\|D9]])** | L2 → L3 | Copilot Studio agent decommission, an AI incident runbook, and a system-prompt canary | Product dependent | Evidence for the owner-departure and incident paths of this agent |

## The four controls that carry this profile

The four work packages below address the most consequential paths for this shape. They do not by themselves establish a CMM level; the assessor still checks all applicable criteria in each target domain.

1. **Force Entra authentication, and block the no-auth path.** Entra-authenticated knowledge sources trim every answer to what the *querying* user is permitted to see. This is answer-time entitlement enforcement, not a single service identity.[^auth] A maker can still publish a "No authentication" agent, which removes trimming. Close that path with a tenant **Power Platform data policy that blocks the *Chat without Microsoft Entra ID authentication* connector**[^ppdlp], the highest-priority control in the profile.
2. **Remediate oversharing on the reachable corpus.** Per-user trimming helps only if SharePoint permissions are correct. Run **Purview DSPM for AI** oversharing assessments and remediate before launch. Treat remediation as a multi-quarter project, not a switch.[^dspm] Uploaded files and public-website sources carry **no per-user permissions**. Treat them as a flat shared corpus.
3. **Turn off ungrounded responses and keep the default guardrails.** Set *Allow ungrounded responses* off so the agent declines when no knowledge source was used, and leave Content Safety + Prompt Shields at the default High moderation.[^content] Off is the closest "answer only from the corpus" lever, though it is not an absolute guarantee.
4. **Verify the agent identity and decommission path.** New Copilot Studio agents receive Entra Agent IDs. Older agents may still use app registrations pending migration. Confirm the principal in agent metadata and its permissions before relying on Conditional Access or audit attribution. Delete the agent in Copilot Studio before changing its identity. Copilot Studio removes the associated principal.[^agentid]

## Criteria and work that depend on the shape

The assessor records the relevant topology and target decision for each of these items:

- **Inter-agent authentication and signing** are not applicable where the deployment has no agent-to-agent traffic. A particular per-task capability-token format and a mesh sidecar are not scored requirements; the assessor tests any applicable task and network decisions on their actual paths.
- **Forward-pass or hidden-reasoning inspection** is not a scored D4 requirement. The assessor still tests applicable prompt and response guards for this bot's actual input paths.
- **Behavioral-drift detection and recurring adversarial evaluation** enter a D7 L4 target where their criterion conditions apply. A basic probe does not establish L4; a lower target must be justified by this bot's risk and recorded separately from its observed level.
- **Produced-model lineage, weight protection and exploitability statements** (D8-LINEAGE, D8-WEIGHTS, D8-VEX and D8-VEX-FEED): the bot calls no model the organization trains or fine-tunes and publishes no component outside the organization, so the four criteria are not applicable.
- **Decommission drills and independent assurance** belong to higher D9 and D1 targets than this profile proposes. A human-approval queue is absent for this read-only configuration, so queue-specific criteria use their own not-applicable rules.
- **Human approval tests** apply when the bot can propose an action in the `confirm` tier. The approval path is absent for this read-only configuration; the assessor proves that absence from its effective tools and policies.

## Cost signal

Budget for the work packages before assigning a target. Check the tenant's actual Copilot Studio, Entra, Purview, and Sentinel entitlements and the agent's expected usage against current price terms. Copilot Studio applies content moderation to generative requests ([generative answers FAQ](https://learn.microsoft.com/en-us/microsoft-copilot-studio/faqs-generative-answers), read 2026-09-29); its [billing rates](https://learn.microsoft.com/en-us/microsoft-copilot-studio/requirements-messages-management) do not list a separate moderation meter. Customer calls to a dedicated Azure AI Content Safety resource use its [own pricing](https://azure.microsoft.com/en-us/pricing/details/content-safety/). The E5 Sentinel data grant covers specified Microsoft 365 sources, so price agent trace ingestion separately ([E5 Sentinel offer](https://azure.microsoft.com/en-us/pricing/offers/sentinel-microsoft-365-offer), read 2026-09-29). The effort case should estimate corpus-permission remediation, identity and DLP rollout, logging volume and retention, and recurring review for this deployment. Do not infer that existing E5 licensing makes these changes cost-free.

## Caveats and preview-watch

- **Legacy identity migration remains a separate task.** Microsoft states that new Copilot Studio agents receive Entra Agent IDs and opt-out has ended; older agents may still have app-registration identities. Manual migration is documented as preview, so a regulated buyer should verify the supported migration route and the identity actually assigned to this bot.[^agentid]
- **Response-content DLP is narrow.** Sensitivity-label enforcement on the agent's answers applies **only to the SharePoint knowledge source**. Uploaded-file and website corpora get no label-based DLP, and the "block sensitive-info-types in prompts" control is an M365-Copilot-proper feature not confirmed for custom Copilot Studio agents.[^respdlp]
- **The regulatory crosswalk is omitted by design.** A regulated buyer maps this profile to its examiner's expectations through the separate FFIEC/GLBA and Canadian-finance crosswalk pages; nothing here is a compliance attestation.

## Notes

[^agentid]: [Microsoft Learn — Microsoft Entra Agent IDs for Copilot Studio agents](https://learn.microsoft.com/en-us/microsoft-copilot-studio/admin-use-entra-agent-identities), updated 2026-09-15, read 2026-09-29. New agents automatically receive an Agent ID; older agents may retain app registrations; opt-out has ended; deleting an agent in Copilot Studio removes its Agent ID. [Manual migration documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/govern-migrate-api-entra-agent-identity) labels that process preview.
[^auth]: [Microsoft Learn — Configure user authentication in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/configuration-end-user-authentication), 2026. Entra-authenticated agents surface only content the querying user can access (answer-time security trimming).
[^ppdlp]: [Microsoft Learn — Configure data policies for agents (Power Platform DLP)](https://learn.microsoft.com/en-us/microsoft-copilot-studio/admin-data-loss-prevention), 2026. Connector classification Business/Non-Business/Blocked; blocking the no-auth connector; governs connectors/tools/channels, not generated text.
[^content]: [Microsoft Learn — Knowledge and content moderation in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-copilot-studio), 2026. Content Safety + Prompt Shields on by default; moderation level (default High); Allow-ungrounded-responses toggle.
[^dspm]: [Microsoft Learn — Purview DSPM for AI and Copilot Studio](https://learn.microsoft.com/en-us/purview/ai-copilot-studio), 2026. DSPM for AI sees Copilot Studio agents (Audit required); oversharing assessment and sensitivity-label support.
[^defender]: [Microsoft — Securing AI agents end-to-end (Purview, Agent 365, Defender)](https://techcommunity.microsoft.com/blog/microsoft-security-blog/securing-ai-agents-end%E2%80%91to%E2%80%91end-connecting-purview-dspm-agent-365-and-the-ai-secur/4521155), 2026. Defender AIAgentsInfo hunting table; Sentinel ingestion of Copilot audit events.
[^apim]: [Microsoft Learn — AI gateway capabilities in Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/genai-gateway-capabilities), 2026. Token governance, content-safety policy, MCP brokering — relevant only if the bot gains external connector reach.
[^respdlp]: [Microsoft Learn — DLP for the Microsoft 365 Copilot location](https://learn.microsoft.com/en-us/purview/dlp-microsoft365-copilot-location-learn-about), 2026. Label-based response restriction is SharePoint-source-scoped; SIT-in-prompt blocking is M365-Copilot-proper.
