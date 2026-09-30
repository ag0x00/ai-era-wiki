---
type: thesis
title: "Productivity Assistant Deployment Shape"
created: 2026-09-30
updated: 2026-09-30
tags:
  - thesis
  - productivity-assistant
  - deployment-shape
status: developing
scope_axis:
  - sec-of-ai
origin: produced
question: "How should an organization bound and secure an employee productivity assistant that can read work content and optionally act in business systems?"
current_position: "Assess the route through work content, retained state, output, and actions rather than the product name or existing user access. Source permissions constrain retrieval but do not settle need-to-know, instruction provenance, rendered-output egress, or delegated writes. Investment rises when sensitive corpora, attacker-contributed content, retained sharing, unattended execution, or external actions meet in one route."
last_revised: 2026-09-30
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[oversharing-controls]]"
  - "[[echoleak-copilot-zero-click]]"
  - "[[geminijack-gemini-enterprise-injection]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[gemini-enterprise-control-sheet]]"
  - "[[gemini-workspace-control-sheet]]"
  - "[[claude-cowork]]"
sources:
  - "https://learn.microsoft.com/en-us/microsoft-365/copilot/microsoft-365-copilot-architecture"
  - "https://learn.microsoft.com/en-us/microsoft-365/copilot/copilot-controls/security-governance"
  - "https://support.google.com/a/users/answer/17010577"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/security-overview"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps-and-data-stores"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/connectors/introduction-to-connectors-and-data-stores"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features"
  - "https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview"
  - "https://support.claude.com/en/articles/13854387-schedule-recurring-tasks-in-cowork"
  - "https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities"
  - "https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans"
  - "https://arxiv.org/html/2509.10540"
  - "https://noma.security/blog/geminijack-google-gemini-zero-click-vulnerability/"
  - ".raw/talks/2026-03-04_Nicolas-Lidzborski_Securing-Workspace-GenAI-at-Google_transcript.md"
verified: 2026-09-30
verified_against:
  - ".raw/talks/2026-03-04_Nicolas-Lidzborski_Securing-Workspace-GenAI-at-Google_transcript.md"
verified_findings: 0
verified_note: "Whole-page source and adversarial read against archived Google talk, live official supplier documentation, and original incident research."
---

# Productivity Assistant Deployment Shape

## Question

An employee productivity assistant reads work content to search, summarize, draft, or plan, and may carry out a task through a tool. The question is which boundaries protect that work when the assistant combines mail, files, messages, calendar entries, local folders, or connected business systems. The unit of assessment is one **route**: a user population, reachable sources, execution placement, retained state, output channels, and actions, with the parties that enforce and record each crossing. A read-only answer and a scheduled workflow that sends mail are different routes even when employees enter both through the same interface. The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] supplies the boundary model; the [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol]] grades observed capability for the defined route.

## Current Position

**The security boundary for a productivity assistant is the route from work content to an answer or effect, not the assistant's user interface.** The same employee entitlement can support a safe search, an inappropriate synthesis of separately readable records, or a send action the employee never intended. Source permissions establish which individual items the assistant may retrieve. They do not establish that an answer meets the business need-to-know rule, that retrieved text remains data rather than instruction, or that the final output stays within the source's handling boundary. [[oversharing-controls|Oversharing Controls for AI Search]] and [[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]] distinguish those tests.

The organization should first narrow the route through existing source grants, connector and feature choices, and a documented owner. More expensive controls become justified when a sensitive corpus is available to a large population, external parties can place content in the corpus, responses can be shared or retained, or a model can initiate a write. For those routes, the investment moves toward corpus-wide access repair, source-aware output handling, target-side action policy, independent evidence export, and adversarial exercises. A supplier-held guardrail supports that choice only to the extent that a scoped supplier record or a route-matched test establishes its operation. No universal CMM target or fixed product list follows from the shape.

## Supporting Evidence

### Shape variants and trust boundaries

Three variants share the work-content threat model but place enforcement in different systems:

| Variant | Sources and execution | Boundary the organization must establish |
| --- | --- | --- |
| **In-suite assistant** | Microsoft 365 Copilot grounds in Microsoft Graph under the signed-in user's permissions; Gemini in Workspace uses the user's Workspace access. | Tenant permissions, sharing and labels govern retrieval; the supplier operates grounding, model, and rendering. |
| **Separate connected app** | Gemini Enterprise connects selected Google or third-party sources through app data stores; sources may be queried live or copied into an index. | App and store access, source permissions, connector mode, query recipients, copied data, features, and action grants each need a route decision. |
| **Connected desktop agent** | Claude Cowork can reach selected local folders, browser, and individually authorized connectors. Cloud sessions run on Anthropic infrastructure; existing desktop deployments can run a local agent loop with code in a device virtual machine. | Device and folder scope, connector tools, browser, runtime placement, and exported records join tenant controls. Scheduled tasks may use remote account files or a local folder. |

[Microsoft states](https://learn.microsoft.com/en-us/microsoft-365/copilot/microsoft-365-copilot-architecture) that Copilot's grounding reads Microsoft Graph under the signed-in user's permissions, while [Google states](https://support.google.com/a/users/answer/17010577) that Gemini in Workspace can reach only Workspace items the user can reach. These are useful permission boundaries, but broadly shared content remains broadly retrievable. [Microsoft's Copilot security guidance](https://learn.microsoft.com/en-us/microsoft-365/copilot/copilot-controls/security-governance) consequently treats oversharing assessment and source-permission repair as deployment work, not as a model setting.

The separate app adds a second data plane. [Google's connector documentation](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/introduction-to-connectors-and-data-stores) distinguishes federation, which queries the source, from ingestion, which copies data into an index. It also warns that a federated query sent to an enabled third-party backend can contain search-request and conversation-history data linked to the user's identity. [Feature Management](https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features) separately governs session sharing, projects, memory, uploads, and cross-domain documents. An app-level access decision therefore leaves questions about each source's live grants, indexed copies, outbound query recipients, and later readers of retained material. The [[gemini-enterprise-control-sheet|Gemini Enterprise Control Sheet]] assigns controls and tests to these specific routes; the [[gemini-workspace-control-sheet|Gemini Workspace Control Sheet]] handles Workspace's own administration.

The desktop variant crosses a boundary outside either suite. [Anthropic describes](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview) cloud Cowork sessions as running the agent loop on its servers and reaching selected local files or the browser through Claude Desktop while the app is online. The same documentation describes local sessions in existing desktop deployments with the agent loop on the device and code execution in an isolated virtual machine. Its [scheduled-task guide](https://support.claude.com/en/articles/13854387-schedule-recurring-tasks-in-cowork) describes remote tasks that run while the computer is asleep with account files and connectors, and a manually configured local-folder option that runs only locally. The guide's general statement that scheduled tasks cannot use local folders conflicts with that option; establish the effective route in the deployed edition. These are distinct processing, persistence, and human-review routes. [[claude-cowork|Claude Cowork]] records version-specific administration and unresolved supplier statements.

### Threat model and control consequences

The assets are work records and their access context: customer and employee information, privileged business plans, message and file histories, classifications, source permissions, draft decisions, tool authority, and records needed to reconstruct a disclosure or action. The potential attacker may be an external sender or document collaborator with no tenant role, a compromised source account or connector, or an insider with limited access. The attacker needs only one surface that the assistant may later read. Inbound mail, calendar invitations, shared documents, web pages, local files, connector results, and reusable prompts are all plausible carriers.

| Crossing | Plausible failure | Enforcing consequence |
| --- | --- | --- |
| **Contributor to source** | An external contributor places instructions in an ordinary work item. | Preserve origin and classify the item before retrieval; test attacker-controlled items in the real corpus. |
| **Source to model** | Retrieved text is treated as authority, or a broad source grant returns context outside the intended task. | Check the asking user's current grant, assess oversharing and answer purpose, and hold lower-trust instructions as data. |
| **Model to output** | The assistant combines restricted facts or renders a resource that transmits them. | Apply channel and sensitivity policy to the completed answer and rendered content; test automatic fetches. |
| **Model to action** | A suggestion, injected instruction, or mistaken recipient becomes a send, share, edit, or workflow call under the user's credential. | Restrict actions before execution at the connector or target, bind approval to exact parameters, and inspect the target effect. |
| **Session to later use** | A conversation, project, memory, file, or scheduled task retains data after a source grant changes. | Inventory derived copies and sharing; test revocation, deletion, ownership, and later-reader access. |

The characteristic attack chain begins with a business item that an assistant is expected to read. A routine employee request causes retrieval. The model follows an instruction embedded in that lower-trust item, then uses the employee's broader corpus reach or an allowed tool to collect or move data. The final leg may be a rendered image request, a connector call, a shared answer, or a business action. The attacker never needs the employee's password; the employee's ordinary work supplies the trigger. Google's [[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]] account identifies this combination of untrusted productivity content, orchestration, and rendered output as a structural risk and recommends source treatment, deterministic action gates, and output handling rather than reliance on a classifier alone ([source transcript](https://drive.google.com/file/d/1OgJBaHE6NfJvnangzg2DwobcQQqB2Zq2/view)).

An action can cross the same boundary without an attacker. A name collision, stale source record, or ambiguous request may cause an agent to choose the wrong recipient or record while pursuing the employee's task. The source talk calls this an [[agency-gap|Agency Gap]]. Target-side recipient and object checks, exact-parameter review for consequential effects, and target audit are therefore needed even when retrieved content is benign.

This chain has two distinct confidentiality failures. First, an assistant can reveal a file the asking principal cannot open if a connector or index uses a broader service identity or stale source permissions. Second, each source item may be individually authorized while the answer exposes a sensitive combination or an inappropriate need-to-know inference. D6 tests both current entitlement and material cross-source inferences; the latter requires a specified business restriction and paired tests, because no system can infer every unstated need-to-know rule. A connected desktop folder adds records outside the central content system, and a scheduled task removes the person from the immediate execution loop.

### Evidence chains

[[echoleak-copilot-zero-click|EchoLeak Zero-Click Copilot Exfiltration]] and [[geminijack-gemini-enterprise-injection|GeminiJack Gemini Enterprise Zero-Click Injection]] demonstrate the source-to-output chain against different productivity assistants. In EchoLeak, researchers sent a crafted email that reached Microsoft 365 Copilot through normal retrieval and induced a reference-style image whose request was proxied through a first-party Teams endpoint ([case study](https://arxiv.org/html/2509.10540)). In GeminiJack, a shared document, invitation, or message could supply instructions that induced cross-source search and an image request ([Noma's disclosure](https://noma.security/blog/geminijack-google-gemini-zero-click-vulnerability/)). Both suppliers remediated the reported flaws before or around public disclosure. These cases establish feasible failure mechanisms in historical versions; they do not establish that the same payload works now or that every vendor route shares the defect.

The improvement case depends on the route's reachable harm and on the control's actual reach:

| Route condition | Investment warranted | Cost or assurance limit |
| --- | --- | --- |
| Broad employee search over sensitive work stores | Repair source sharing, review corpus reach, and test representative principals and prohibited inferences. | Permission repair needs data owners and ongoing review; a narrow read-only pilot can start with a smaller assessment. |
| External contributions enter the retrieval path | Add provenance-aware intake, adversarial retrieval and rendered-output tests, and obtain supplier evidence for in-path screening. | Filtering and testing consume operating effort; a supplier statement without route coverage cannot prove refusal. |
| Answers or derived artifacts can be shared, exported, or retained | Add label and recipient checks, lifecycle records, and revocation tests for retained copies. | Disabling a feature reduces utility; leaving it on creates a new data owner and evidence obligation. |
| The assistant can send, share, modify, or run unattended | Enforce action and target limits outside model text, require exact-parameter review where impact warrants it, and join approval to target effect. | Human review adds latency and queue work; low-impact reversible actions can use narrower deterministic policy. |
| Local folders, browser, connector tools, or third-party backends expand reach | Admit paths and connectors by purpose, test each egress channel, and export searchable events across device, supplier, and target. | A code-sandbox allowlist may leave browser, search, or connector egress outside it; separate controls and supplier evidence may be needed. |

[Anthropic's connector documentation](https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities) illustrates a narrower action route: organization owners can allow read tools while blocking connector writes, and source permissions still apply. Its [Cowork network guidance](https://support.claude.com/en/articles/13455879-use-claude-cowork-on-team-and-enterprise-plans) says code-execution egress settings exclude web fetch, web search, and Model Context Protocol (MCP) connectors, including browser integration. Thus a code-sandbox test alone cannot establish the desktop agent's whole egress boundary. [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]] locates input and output screening, while [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]] tests every effective send path. For any variant, [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]] asks whether the organization can search or export action and answer records and join them to an accountable person. [[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]] then needs a supplier-specific guardrail failure and incident path that local operators can execute.

## Counter-Evidence

Current products provide meaningful boundaries:

- Microsoft and Google state that their in-suite assistants respect the asking user's source access ([Microsoft architecture](https://learn.microsoft.com/en-us/microsoft-365/copilot/microsoft-365-copilot-architecture), [Google Workspace access](https://support.google.com/a/users/answer/17010577)).
- Google documents [app and store access controls](https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps-and-data-stores) for Gemini Enterprise.
- Anthropic documents selected [folder scope](https://support.claude.com/en/articles/14479288-claude-cowork-architecture-overview) and [connector action restrictions](https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities) for Cowork.

A deployment that exposes only a small approved corpus, keeps actions off, and blocks sharing has a smaller credible loss path than an estate-wide assistant with unattended writes. Control investment can therefore scale with the exposed route.

The historical exploit record is narrower than a measured present-day attack rate. EchoLeak was fixed server side before disclosure, and Noma reports Google changed the affected retrieval path for GeminiJack ([case study](https://arxiv.org/html/2509.10540), [Noma disclosure](https://noma.security/blog/geminijack-google-gemini-zero-click-vulnerability/)). Neither case proves that every current renderer or model accepts the same payload. Product documentation establishes available controls, while customer tests and scoped supplier evidence establish their effective coverage. A missing supplier trace remains an evidence limit, not proof that the supplier has no control.

[[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]] assigns residual supplier dependence and the release decision to the organization.

## Position history

- **2026-09-30:** The initial position separates in-suite, separate connected-app, and connected desktop routes. Historical EchoLeak and GeminiJack cases make source-to-render egress part of the shape threat model. Cowork adds local file, browser, MCP, and scheduled-task boundaries, so the desktop route cannot be graded from tenant permissions alone. The position selects controls by route exposure and evidence cost; it sets no fixed maturity target.

## Open Sub-Questions

- For a given supplier and edition, which retrieved-item provenance, pre-render decisions, and target action records can the organization search or export, and which are available only through supplier assurance?
- How quickly do connector indexes, shared conversations, memory, local copies, and scheduled tasks reflect a source-permission change or source deletion in each actual route?
- Which material combinations of individually permitted work records warrant a need-to-know rule, and who owns that rule when sources have different data owners?
- Where cloud and local desktop sessions differ, which browser, web, MCP, and device paths enforce the organization's egress policy? The [[claude-cowork|Claude Cowork]] page records a supplier-document conflict that requires route testing.
- See [[wiki/gaps/_index|Gaps and Open Questions]] for durable unanswered coverage questions.
