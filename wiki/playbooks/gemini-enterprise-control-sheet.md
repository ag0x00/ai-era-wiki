---
type: playbook
title: "Gemini Enterprise Control Sheet"
created: 2026-09-30
updated: 2026-09-30
tags:
  - playbooks
  - gemini-enterprise
  - control-mapping
status: developing
scope_axis:
  - sec-of-ai
origin: produced
audience: "Gemini Enterprise app owners, security architects, data owners, and financial-sector assessors"
length: "~5,000 words; 13 controls and route decisions"
related:
  - "[[google]]"
  - "[[productivity-assistant-deployment-shape]]"
  - "[[oversharing-controls]]"
  - "[[gemini-workspace-control-sheet]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[model-armor]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
sources:
  - "https://docs.cloud.google.com/gemini/enterprise/docs/security-overview"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/compliance-security-controls"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/locations"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps-and-data-stores"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/identity"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/configure-identity-provider"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/connectors/introduction-to-connectors-and-data-stores"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gdrive"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gdrive/set-up-data-store"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gmail"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/connectors/manage-actions"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/projects"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/share-conversations"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/configure-personalization"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/reference/rest/v1alpha/projects.locations.collections.engines.assistants"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/use-vpc-service-controls"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/cmek"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/enable-model-armor"
  - "https://docs.cloud.google.com/model-armor/integrations"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/set-up-usage-audit-logs"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/manage-observability-settings"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/correlate-model-armor-logs"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/workflows"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/use-connector-actions"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/use-hitl-steps"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/share-chat-agent"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/share-workflows"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/share-custom-agents"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/quotas-and-limits"
  - "https://knowledge.workspace.google.com/admin/gmail/advanced/restrict-email-messages-to-authorized-addresses-or-domains-only"
  - "https://knowledge.workspace.google.com/admin/gmail/advanced/set-up-email-quarantine"
  - "https://knowledge.workspace.google.com/admin/gmail/advanced/set-up-rules-for-advanced-email-content-filtering"
  - "https://www.wiz.io/solutions/ai-spm"
  - "https://www.wiz.io/platform/wiz-defend"
  - "https://cloud.google.com/security/securing-ai"
  - "https://arxiv.org/html/2509.10540"
  - "https://noma.security/blog/geminijack-google-gemini-zero-click-vulnerability/"
verified: 2026-09-30
verified_against: []
verified_findings: 0
verified_note: "Whole-page adversarial source read against live official Google, Workspace, and Wiz documentation; no archived document opened."
---

# Gemini Enterprise Control Sheet

This sheet supports a release decision for one employee route through the **Gemini Enterprise app**, formerly Agentspace, at a Canadian financial organization. A control is a proposed requirement until the named owner records the effective setting and a refusal at its enforcement point. Google's [app security overview](https://docs.cloud.google.com/gemini/enterprise/docs/security-overview) describes the app boundary.

The app, its data stores, Core Assistant, and app-reached agents form this sheet's scope. Gemini in Workspace and Workspace Studio use Workspace administration. Agent Platform, Agent Studio, and Google AI Studio are developer surfaces with different controls. Gemini Notebook Enterprise is a separate product, even when its entry appears in the app. [[google|Google]] maps these products; [[gemini-workspace-control-sheet|Gemini Workspace Control Sheet]] and [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] cover adjacent routes.

The [[productivity-assistant-deployment-shape|Productivity Assistant Deployment Shape]] defines the employee-assistant threat paths and boundary decisions shared across vendor placements. This sheet applies them to Gemini Enterprise's app, data-store, and action routes, with a separate enforcer and refusal test for each control.

## Scope and route record

Make one record for each material change of app location, source, identity, model, agent, action, or execution path. In particular, **a Canadian app location does not establish a Canadian Drive or Gmail connector route**: both connector overviews limit their stores to global, US, and EU. A proposed Canada-only Drive route is therefore **Hold at the connector gate**, before any control test can release it. Record the intended route and the supported alternative separately. [Drive limits](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gdrive) and [Gmail limits](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gmail).

- **Boundary:** record these route facts:
  - Edition, project, app and store IDs, location, and allowlist grant.
  - Source, target, data class, prohibited source combinations, human and connector identities, and connector mode.
  - Models, grounding, agent tools, enabled actions, sharing, memory, uploads, and output clients.
  - Network path, owners, and approved version.
- **Decision:** applicable control IDs, required outcomes, exact enforcing settings or target policies, owners, and supplier answers needed for a claim.
- **Proof:** effective configuration, permitted and refused canary calls, observed source or target effect, timestamps, and retrievable logs or screening verdicts.
- **Disposition:** Pass, Hold, Conditional exception, or Reject for this route. An exception names its independent gate, risk owner, expiry, and retest trigger.

## Common controls

### C01 — Bind app, store, and document access

**Requirement.** A user must reach only approved apps and stores and retrieve only source-authorized documents. The Cloud IAM and source data owners approve those distinct boundaries.

**Enforcer.** Google Cloud IAM enforces app and data-store access; Google Identity and source-system permissions enforce document access. Custom Cloud Storage or BigQuery sources also need supplied ACL metadata and an access-controlled store. [Identity setup](https://docs.cloud.google.com/gemini/enterprise/docs/identity).

**Mechanism and proof.** Bind a restricted project-level role and app-level Gemini Enterprise User. For distinct audiences within an app, enable resource access control and bind each user to both the app and permitted stores. The predefined restricted role can satisfy this pattern. A custom restricted role is an alternative if it includes the [granular guide's required read permissions](https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps-and-data-stores). A broad project User role can defeat a resource restriction. Keep effective IAM exports, group membership, source ACLs, and store ACL configuration. Check child store bindings where collection grants do not cascade. [App policy](https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps).

**Test.** As entitled and unentitled users, attempt the app and store, then a document denied by its source ACL. Remove a store grant and repeat. Keep IAM, ACL, result, and source audit evidence. An unauthorized retrieval rejects the route.

### C02 — Admit a supported connector and test revocation

**Requirement.** The connector must support the chosen location and meet the source owner's tolerance for stale entitlements or deleted content. The connector operator records its mode and the source owner sets that tolerance.

**Enforcer.** The published connector location limit is the admission gate. Source IAM and OAuth grants constrain live access; Gemini Enterprise store configuration and full or identity sync govern indexed copies.

**Mechanism and proof.** [Drive](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gdrive) and [Gmail](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gmail) stores support global, US, and EU only. A **ca** app with either store remains Hold even though the base app has Canadian residency support. Enterprise app data-residency and CMEK claims cover Google Cloud data, not Drive or Gmail source data. Apply Workspace's own controls to the source. For a supported Drive store, the [new-store setup](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gdrive/set-up-data-store) lacks administrator folder and shared-drive filters. Use source permissions and store IAM. If the intended subset remains readable to the user in Drive, Hold the subset claim.

For other connectors, [federation queries the source and ingestion indexes a copy](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/introduction-to-connectors-and-data-stores). Record the chosen mode, query recipient, OAuth scopes, and sync schedule. Federated backends can receive query and conversation context. Incremental sync omits deletions and identity data; full and identity syncs carry them. Store ACL and [identity-provider configuration](https://docs.cloud.google.com/gemini/enterprise/docs/configure-identity-provider) are creation choices that can require rebuilding a store. Shorten and monitor the appropriate sync or remove the source when measured revocation exceeds tolerance.

**Test.** First compare the exact location and connector to its published availability. On a supported store, change a canary file ACL, remove group access, and delete the file; query until retrieval and citation stop. Save source state and elapsed time. For a federated source, find a unique canary query in the backend audit.

### C03 — Keep processing within the approved geography

**Requirement.** For a Canada-only route, each model, feature, and agent step must meet Canadian processing and storage requirements. The residency owner approves the boundary; the app administrator applies it.

**Enforcer.** App location, web-app Feature Management, assistant grounding policy, and each agent node's model and tools control the selectable paths. The connector location gate in C02 is separate.

**Mechanism and proof.** For Standard and Plus, Google's [location table](https://docs.cloud.google.com/gemini/enterprise/docs/locations) lists **ca** as GA on allowlist with in-country base-app at-rest and machine-learning processing. Record the allowlist grant. The table supports Gemini 3.5 Flash and 2.5 Pro in Canada. It does not list Canadian support for 3.1 Pro; 3.6, 3.7, and 3.8 Flash use global availability from a Canadian app. Inspect effective model and node settings rather than inferring them from the app location. [Feature Management](https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features) can disable optional model selection, image and video generation, and the Notebook entry. Keep them off for a Canada-only route until each has its own approved location record. The [Notebook product](https://docs.cloud.google.com/gemini/enterprise/docs/locations) has feature-specific guarantees.

Disable Grounding with Google Search and Web Grounding for Enterprise on this route. The former is global, may temporarily log customer data, and is excluded from DRZ, CMEK, VPC Service Controls, and Access Transparency in Google's [control matrix](https://docs.cloud.google.com/gemini/enterprise/docs/compliance-security-controls). Set the Core Assistant's [web grounding policy to disabled](https://docs.cloud.google.com/gemini/enterprise/docs/reference/rest/v1alpha/projects.locations.collections.engines.assistants). Record each [workflow node's model and tools](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/workflows); assess a registered custom agent at its own runtime.

**Test.** In the live app, attempt an unapproved model, grounding, image, video, and Notebook path. Repeat for each permitted agent and inspect its effective node version. Keep settings and refusals. A feature without evidence of the required Canadian path remains Hold.

### C04 — Govern retained and external content

**Requirement.** A source ACL change must not be mistaken for deletion of previously generated, shared, or locally uploaded content. The data owner sets the retention and sharing rule; the app administrator configures it.

**Enforcer.** [Web-app Feature Management](https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features) controls session sharing, Projects, Memory and customization, Google Drive upload, Canvas, and Include cross-domain documents. Drive ACLs govern linked Drive files, while the app governs local Project uploads and session links.

**Mechanism and proof.** For confidential routes, disable session sharing, Projects, memory/customization, Google Drive upload, Canvas, and Include cross-domain documents unless the data owner approves and tests each path. [A shared conversation](https://docs.cloud.google.com/gemini/enterprise/docs/share-conversations) exposes content already in the session to link holders. [Project local uploads](https://docs.cloud.google.com/gemini/enterprise/docs/projects) can be viewed and downloaded by project members; linked source files retain source permissions. [Core memory defaults on](https://docs.cloud.google.com/gemini/enterprise/docs/configure-personalization). Record its effective setting and the deletion/opt-out path. Source ACL revocation must therefore be tested against those separate copies or memories. [Google Drive upload and Canvas](https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features) add direct upload and file-export paths. Include cross-domain documents lets a Drive store retrieve external-organization documents, which Google warns can carry prompt injection and manipulated results. Keep that toggle off unless external documents are an approved source with an injection test.

**Test.** Place a unique canary in a source document, session, local Project upload, and memory path, then revoke the source ACL. Try the old session link, project membership, and a new query. Attempt Drive upload and Canvas export when disabled, then test approved uses. With cross-domain access off, an external-owned Drive document must be denied. If approved on, use a benign injection canary. Save each exposure or refusal and the deletion result.

### C05 — Establish the perimeter before stores

**Requirement.** Approved identities and network paths must reach the app and Discovery Engine API; new connector egress requires a separate review. The Cloud perimeter owner acts before store creation.

**Enforcer.** VPC Service Controls and Access Context Manager govern project, API, and UI ingress. Managed organization constraints govern allowed sources and egress domains for new stores.

**Mechanism and proof.** Add the project to a perimeter, restrict discoveryengine.googleapis.com, define ingress, dry-run, then enforce. Google's [procedure](https://docs.cloud.google.com/gemini/enterprise/docs/use-vpc-service-controls) requires existing stores to be deleted and recreated to gain a new perimeter. After enforcement, set allowed source and egress-domain constraints before creating new stores. Existing stores can continue. Preserve policy and creation timestamps. Public third-party connector endpoints sit outside this perimeter under Google's [security overview](https://docs.cloud.google.com/gemini/enterprise/docs/security-overview). Review their recipients separately.

**Test.** Attempt UI and API access from denied and allowed contexts. Create a canary store under enforced constraints. For an older store, recreate it and retest ACL and retrieval before crediting perimeter protection.

### C06 — Make encryption claims resource-specific

**Requirement.** The Cloud KMS and data owners must distinguish default encryption from a contractual customer-managed-key requirement for each actual resource and source.

**Enforcer.** Cloud KMS and Gemini Enterprise CmekConfig govern supported app and connector resources when registered before creation. The contractual gate holds an unsupported route.

**Mechanism and proof.** Google's [CMEK guide](https://docs.cloud.google.com/gemini/enterprise/docs/cmek) documents US/EU multi-region apps and stores; keys must be ready before creation, and existing resources cannot be retrofitted. Its first-party connector exceptions cover import-once or periodic BigQuery and Cloud Storage, not Drive/Gmail source storage. The [location table](https://docs.cloud.google.com/gemini/enterprise/docs/locations) marks in-country CMEK support while pointing to US/EU. Treat the Canadian key claim as unresolved until Google confirms the exact app/store route and a live test supports it. Preserve key, registration time, resource creation time, connector type, and supplier response. A separate supported import can be considered only if its copied index and sync cost meet the source owner's requirement.

**Test.** On a documented supported route, inspect the resource key binding and disable a canary key to observe refusal. For a proposed **ca** route, require written location/connector confirmation plus an observed result. Default encryption does not prove CMEK.

### C07 — Screen covered app paths

**Requirement.** Approved input and output paths must receive the selected screening and failure behavior. The app security owner sets the policy; the Model Armor owner deploys it.

**Enforcer.** The [direct Gemini Enterprise–Model Armor integration](https://docs.cloud.google.com/gemini/enterprise/docs/enable-model-armor) binds prompt and response templates to the assistant policy. For a hard stop, select Inspect and block plus Block all user interactions on screening failure. A [direct REST call](https://docs.cloud.google.com/model-armor/integrations) only returns a verdict; the calling application must block.

**Mechanism and proof.** Record template names, enforcement types, failure mode, covered agent class, and verdict logs. The direct integration covers Core Assistant, Google-made agents, and employee-made Workflow Builder agents, but excludes registered ADK, A2A, and Dialogflow agents. Guard a custom agent at its own runtime and test that guard. The [integration matrix](https://docs.cloud.google.com/model-armor/integrations) says this integration screens intermediate grounding data and web-search responses, recording a blocked result as `SANITIZE_USER_PROMPT`; establish which enabled connector and agent paths actually reach that point. The [location table](https://docs.cloud.google.com/gemini/enterprise/docs/locations) and [enable guide](https://docs.cloud.google.com/gemini/enterprise/docs/enable-model-armor) give conflicting or incomplete Canadian template routing. Obtain a route-specific answer before claiming Canadian screening locality. The enable guide says images embedded in directly uploaded documents are screened. The integration matrix says embedded document images are not. Leave that modality uncredited until tested.

**Test.** Send harmless input and output canaries, an embedded-image document, and a simulated screening outage through each agent class. Put an instruction in an attacker-controlled document, invitation, or message that asks the assistant to reveal a separate canary or invoke a write. Observe the retrieved item, model result, target effect, and SanitizeUserPrompt/SanitizeModelResponse verdicts. Match the blocked grounding-data result claimed in the integration matrix to this connector, item, and agent path; an initial-prompt verdict alone does not establish intermediate coverage. Require scoped supplier evidence where the customer trace stops, or narrow the source and action route until the end-to-end injection test gives an acceptable result. A blocked assistant request does not prove custom-agent coverage.

### C08 — Join evidence to the actor and target effect

**Requirement.** Responders must reconstruct a session, screening verdict, agent step, and target action while limiting access to sensitive query and response logs. The detection and privacy owners define retention and access; the app owner maintains a stop path.

**Enforcer.** Gemini Enterprise [app and agent observability](https://docs.cloud.google.com/gemini/enterprise/docs/manage-observability-settings), [usage audit logs](https://docs.cloud.google.com/gemini/enterprise/docs/set-up-usage-audit-logs), Model Armor logs, Cloud Logging IAM, and target-system audit provide distinct records. IAM revocation, action disablement, and agent suspension stop future use.

**Mechanism and proof.** Enable app observability for Core Assistant and agent observability for Workflow Builder where applicable. Usage audit can include raw sensitive queries and responses. Restrict log readers, retention, and export. To identify a Model Armor ingress verdict, join the third segment of its client_correlation_id to StreamAssist response.assistToken after documented normalization, then take userIamPrincipal and trace from StreamAssist. The sanitization log alone has no end-user identity or trace. [Google's correlation procedure](https://docs.cloud.google.com/gemini/enterprise/docs/correlate-model-armor-logs). Join the agent trace and target audit to the operation and actor. Preserve the configured stop procedure.

**Test.** Run one permitted and one refused canary, correlate the sanitization verdict to the correct user and session, then find or refute the target effect. Try the same request after revoking the user or disabling the action. Retain queries used for the log join with access limited to responders.

### C09 — Disable unintended data-store actions

**Requirement.** A read-only route must refuse every mutative connector action through Core Assistant as well as through agents. The app administrator owns the store action table; the source owner accepts permitted reads.

**Enforcer.** The Gemini Enterprise console's **Data Stores > store > Actions** table enables or disables individual connector actions. [Action management](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/manage-actions). Source permissions supply a second refusal boundary.

**Mechanism and proof.** A connected [Drive store exposes natural-language actions to Core Assistant](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gdrive); turning off Workflow Builder alone leaves this path. On a supported-location read-only Drive store, save the effective action table below and source grants. Disable every mutative row and Download file content unless the source owner separately approves download. Recheck the full table after store or connector changes.

| Drive action | Read-only state |
| --- | --- |
| Search files | Enabled |
| Read file content | Enabled |
| Get file metadata | Enabled |
| Get file permissions | Enabled if approved |
| List recent files | Enabled if approved |
| Download file content | Disabled unless approved |
| Copy file | Disabled |
| Create file | Disabled |
| Share file | Disabled |
| Trash file | Disabled |
| Update file | Disabled |

**Test.** As an authorized user, search and read a canary. Ask Core Assistant and every available agent to copy, create, share, trash, and update it; attempt download when disabled. Confirm no source change in Drive audit, not just a verbal refusal. A **ca** + Drive route remains Hold under C02 even if this table passes in another location.

## Route overlays

### C10 — Govern a shared employee agent

**Requirement.** Shared agent access, uploaded knowledge, actions, and live revisions must stay within the approved audience and version. The agent owner and data owner approve the content; the app administrator governs sharing.

**Enforcer.** [Agent sharing settings](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/share-chat-agent), agent/resource IAM, node tool configuration, and a controlled workflow owner govern this boundary. Source ACLs govern linked files. Uploaded agent knowledge follows the agent's sharing boundary.

**Mechanism and proof.** Record the exact audience, source or uploaded Knowledge, node model, tools, and enabled actions. Restrict Drive Knowledge selection and leave the full Drive tool off on that node when a subset is required. The [node guide](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/workflows) says the full tool can bypass restricted Knowledge. [Share and obtain admin approval while the workflow is a non-runnable draft](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/share-workflows). Before first activation, the app admin [transfers ownership](https://docs.cloud.google.com/gemini/enterprise/docs/share-custom-agents) to a controlled publisher, then verifies the former owner has agentUser access and cannot edit. The publisher checks the action and tool diff against an approved change record and turns on the first live version. Later live revisions automatically reach users, so only that publisher may activate them after the same review. Hold shared write workflows if a prior owner or other editor can publish around this gate. [Schedules run only in multi-region and use delegated author credentials that expire after 14 days](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/quotas-and-limits); record trigger owner, renewal, and failure alert for an unattended route.

**Test.** Ask an out-of-audience user to open the draft and query uploaded and linked canaries. Confirm approved sharing leaves it non-runnable and inspect run history before transfer. Transfer ownership, attempt a prior-owner edit and first activation, then let the publisher approve and activate. Alter an action and try to publish a later version without publisher approval. Verify refusal or Hold and retest the released version. Any run during the handoff is a Hold.

### C11 — Bind a Gmail write workflow to target approval

**Requirement.** For an approved US/EU Gmail route, a workflow must not send a message before a human sees its final recipient and body, and the target must refuse any send path that evades that review. The workflow, Workspace mail, and business owners jointly own this gate.

**Enforcer.** Workflow Builder's explicit Approval step pauses its subsequent action. The [Gmail store Actions table](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/manage-actions) limits available connector actions. Workspace Gmail [Restrict delivery](https://knowledge.workspace.google.com/admin/gmail/advanced/restrict-email-messages-to-authorized-addresses-or-domains-only) can reject recipients outside approved addresses or domains by sender OU; [outgoing quarantine](https://knowledge.workspace.google.com/admin/gmail/advanced/set-up-email-quarantine) can hold eligible messages for independent release. Gmail target audit proves delivery or refusal.

**Mechanism and proof.** Use a deterministic [connector action step](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/use-connector-actions) after a displayed-payload [Approval step](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/use-hitl-steps). Keep every Agent node's **Add Or Update Data** off. The [node guide](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/workflows) makes it connector-wide for supported mutations, so a node before Approval can write. In the store Actions table, enable only the required Send message action and approved reads. Disable Reply, Forward, draft writes, and other mutations. This is store-level, not a native per-recipient rule. Each [shared workflow user authorizes the connector and can set a trigger](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/share-workflows). Apply Gmail sender-OU policy to every such user and review OAuth grants and trigger scope.

The [Gmail connector](https://docs.cloud.google.com/gemini/enterprise/docs/connectors/gmail) also lets Core Assistant send when Send message is enabled. Workflow Approval therefore cannot be the sole gate. For a requirement of human review before *every* delivery, configure target-side [outgoing quarantine rules](https://knowledge.workspace.google.com/admin/gmail/advanced/set-up-rules-for-advanced-email-content-filtering) for both **Outbound** and **Internal - Sending** messages and an independent reviewer for every permitted sender/recipient path, alongside Restrict delivery. Leave [Bypass this setting for internal messages](https://knowledge.workspace.google.com/admin/gmail/advanced/restrict-email-messages-to-authorized-addresses-or-domains-only) off when the recipient list must bind internal mail. Google excludes messages sent to Groups from [quarantine](https://knowledge.workspace.google.com/admin/gmail/advanced/set-up-email-quarantine), while Restrict delivery has a Groups bypass caveat. Prohibit and test Group recipients through an effective target rule or Hold the universal-approval claim. Google's [sharing guide](https://docs.cloud.google.com/gemini/enterprise/docs/workflow-builder/share-workflows) also exempts self-addressed email from native HITL; the independent target gate must catch that case. This route remains Hold until those bypass tests pass. A **ca** + Gmail route remains Hold for connector availability regardless of added controls.

**Test.** In a supported US/EU app, approve one canary workflow send and inspect the displayed recipient/body, quarantine decision, reviewer release, recipient mailbox, and Gmail audit. Deny Approval; try altered recipients, an unauthorized internal address, an external address, Group and self-send, a direct Core Assistant send, an Agent-node mutation, a different authorized shared user, and a changed live version. Record refusal at the action or target and absence of delivery. If any path delivers without required independent review, Reject that write route.

## Shape-specific disclosure gates

### C12 — Bound material cross-source inference

**Requirement.** The data owners must name material combinations of individually readable records that an assistant must not synthesize for the route's audience. A normal source ACL is insufficient when the same user may open each record separately.

**Enforcer.** Source ACLs and Gemini Enterprise app and data-store IAM can prevent the assistant from retrieving a prohibited combination in one route. The [app/store IAM guide](https://docs.cloud.google.com/gemini/enterprise/docs/iam-policy-for-apps-and-data-stores) and [Feature Management guide](https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features) document access and feature controls, but do not establish a customer-configurable, session-aware semantic withholding gate. Where the same user must reach both sources, the risk owner must Hold that combination or obtain and test a specific supplier or downstream output control.

**Mechanism and proof.** Record the prohibited inference, sources, affected audience, and the source owner who authorizes the restriction. [[oversharing-controls|Oversharing Controls for AI Search]] supplies the source-repair method. Where the business can separate the work, assign the sensitive stores to different app routes and restrict each app and store with C01. Keep uploads, Projects, memory, sharing, and cross-domain retrieval within C04's approved boundary. Save the effective IAM and source ACLs and an observed refusal to co-retrieve the paired canaries. Segmentation reduces automatic composition within the admitted app route. It cannot stop a person who is entitled to both sources from manually combining answers, and it is no claim of a native semantic output policy.

**Test.** Ask an entitled principal for each item separately, then ask for the prohibited inference in one session and after a source or store switch. Pair that denial with a permitted combination so a blanket refusal does not pass. Repeat with a shared agent, upload, and retained session if enabled. If the prohibited answer appears and no independently enforced output gate stops it, Hold the affected audience and corpus combination. Fund corpus separation or a proved output gate when the inference would disclose regulated information or cross business-unit need-to-know boundaries; count lost search utility and ongoing access review against the reduction in disclosure.

### C13 — Test rendered-output egress

**Requirement.** Confidential source content must not leave through an automatic render fetch or generated link outside the approved output channel. The app owner and data owner approve the destination boundary; Google operates the managed renderer.

**Enforcer.** C01–C04 narrow reachable content and C07 may block a matching response before rendering. The [Feature Management guide](https://docs.cloud.google.com/gemini/enterprise/docs/manage-web-app-features) and [security control matrix](https://docs.cloud.google.com/gemini/enterprise/docs/compliance-security-controls) do not establish a customer-managed renderer destination allowlist. The release gate therefore needs a route-matched refusal test and scoped supplier evidence of output handling before claiming this path blocked; a VPC Service Controls or Model Armor setting alone does not prove it.

**Mechanism and proof.** Use a controlled, externally hosted collector with a unique URL and canary value. Place an instruction to include the canary in a Markdown image or link in a lower-trust document or message that the route can read. The historical [EchoLeak](https://arxiv.org/html/2509.10540) and [GeminiJack](https://noma.security/blog/geminijack-google-gemini-zero-click-vulnerability/) cases show why the destination and fetch need inspection after the model produces text. Retain the source item, assistant response, browser or collector request, screening verdict, and any supplier assurance that names this rendering path. Use a narrower source grant or disable the exposed route while the renderer boundary remains unproved.

**Test.** Run a benign image and the canary-bearing response through every enabled client and sharing path. Check whether a fetch occurs before a user click, whether the canary reaches the collector, and whether a link preview, proxy, or redirect changes the destination. Repeat after a client or renderer change. Reject a route if a prohibited fetch carries the canary. Hold a route that depends on this egress boundary if the customer cannot observe the path or obtain scoped supplier evidence. Fund repeated render tests and supplier assurance when untrusted contributors and sensitive retrieval coexist. Those costs are lower for a corpus without attacker-contributed content.

## Validation and decision

Test the actual route at its most permissive allowed setting. Save resource IDs, policy versions, identities, models, connector and action tables, canaries, results, audit joins, assessor, and date. Repeat affected tests after a change to region, source entitlement, model, agent version, connector, action, sharing, or target policy. The investment decision compares an enforceable baseline to a stronger option; extra monitoring never makes an unsupported connector available.

| Route and risk trigger | Baseline and stronger mechanism | Setup and recurring burden | Exposure reduction and refusal test | Owner and disposition |
| --- | --- | --- | --- | --- |
| Canada-only Core Assistant over Drive: connector unavailable in **ca** | No supported **ca** Drive store; consider a separately assessed Canadian source/import only if Google documents that exact route | New source design, copy/sync, ACL mapping, rebuild and recurring revocation tests; broader copied-data blast radius | A Canadian app and action table cannot remove the connector limit; require documented Canadian connector or a distinct approved source and a successful ACL/action test | App and data owners: **Hold** for **ca** + Drive |
| US/EU Gmail write agent: Send enabled also reaches Core Assistant; shared users have own OAuth | Workflow Approval plus narrow store Actions; stronger Workspace Restrict delivery and outgoing quarantine for every sender, with Groups prohibited or refused | Mail OU rules, reviewer staffing, quarantine delay, audit and version review; rule errors can block legitimate mail across that OU | Denied workflow, direct assistant send, self-send, Group, changed recipient, and changed version must produce no unreviewed delivery | Workflow, mail, and risk owners: **Hold** until every target refusal test passes; then assess Pass or bounded exception |
| Material inference from individually readable sources | C12 records the prohibited combination and separates sources by app/store grant where feasible; a stronger session-aware output gate needs a specific supplier or downstream enforcement claim | App split reduces search utility; classification, audience review, and paired canary retests recur | Ask for each item and the prohibited combination, alongside a permitted combination. Hold if the combination remains answerable without an independent gate | Data and risk owners: **Hold** the affected audience and corpus until C12's denial is proved |
| Untrusted content plus sensitive retrieval and rendered output | C07 screens covered prompts and responses; C13 tests the managed renderer and requires route-scoped supplier evidence where customer logs stop | Collector tests, client-version retests, supplier assurance, and narrower corpus grants add effort; source removal can reduce utility | Seed a canary-bearing image/link, inspect automatic requests, redirects, and the destination. A prohibited fetch is a Reject; unobservable required boundary is a Hold | App, data, and supplier-risk owners: **Hold** or **Reject** under C13 |
| Observable custom-agent or connector drift after release | Native app, source and target audits. Optionally use existing [Wiz AI-SPM](https://www.wiz.io/solutions/ai-spm) inventory or [Wiz Defend](https://www.wiz.io/platform/wiz-defend) detection where the deployed integration sees that resource or runtime | License may already exist. Integration, coverage validation, triage, and false-positive work recur. Missed surfaces create false assurance | Inventory change or alert can shorten detection. Seed a known change and compare Wiz observation with app/source/target audit. It does not block in-app actions | Detection owner: optional after coverage test; never substitutes for C09/C11 |

- **Pass:** every applicable requirement has effective configuration, a permitted result, an unauthorized refusal, and retrievable evidence.
- **Hold:** required proof or a test is missing, including a published product limit or an unproven compensation.
- **Conditional exception:** the designated risk owner accepts an independently enforced and tested compensation for a named route, data class, duration, and retest condition, with no other Hold or Reject.
- **Reject:** prohibited access, disclosure, or action succeeds without independent containment.

## CMM traceability and sources

These controls and refusal tests provide selected evidence for the [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]]. For formal domain grading, see the [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]]. This sheet assigns no grade.

- [[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]]: the route record and C02–C03, C06, C09–C11 decisions bear on **D1-BOUNDARY** and **D1-GATE**. Production tier, accountable approver, and dated approval remain necessary.
- [[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]]: C01 ACL, C10 ownership, and C11 OAuth tests bear on **D2-DELEGATE**. C08's joined records bear on **D2-TRACE** only if agent and human identities survive downstream. App IAM does not prove per-agent identity.
- [[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]]: C09 action refusals and C11 alternate-path tests bear on **D3-ALLOW**, **D3-APPROVE**, and **D3-APPROVE-GATE**. Credit requires every callable action and bypass path tested.
- [[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]]: C07 canaries bear on **D4-INPUT** and **D4-OUTPUT** for covered app routes. C04 and C07 indirect-source tests bear on **D4-INJECT-INDIRECT** when an intermediate screening decision and end-to-end refusal are proved for the actual source and agent path. C13 checks a later render path outside a prompt/response verdict. Custom-agent and supplier execution need separate evidence. This sheet supplies no sandbox result.
- [[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]]: C02 connector recipients, C05 perimeter, C11 target denial, and C13 renderer test bear on **D5-REACH** with downstream forwarding evidence. They do not prove **D5-ALLOW** for every agent connection or managed render path. Obtain supplier route evidence or mark it unanswerable on the managed path.
- [[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]]: the route's data-class record bears on **D6-CLASSIFY**; C01–C02 ACL/revocation tests bear on **D6-ENTITLE**. C04 copy tests expose retained content. C12's paired canaries identify a **D6-ENTITLE-INFER** target, but segmentation alone does not prove the criterion's session-aware withholding. Canary samples do not establish **D6-REACH** across every corpus or supplier-held index.
- [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]]: C08's joined records bear on **D7-LOG** and **D7-ATTRIBUTE** only with answer/tool coverage across agents. Optional Wiz testing bears on **D7-POSTURE** only with complete inventory, findings, and dispositions.
- [[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]]: C03 model records and C10 version diffs bear on **D8-INVENTORY** and **D8-VERSION**. Supplier components need provider evidence; current settings do not establish version history.
- [[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]]: C07 outage and C08 stop tests bear on **D9-GUARD-FAILMODE** and **D9-IR-CONTAIN**. C10 transfer and C11 review also need runbooks for **D9-OFFBOARD** and **D9-QUEUE-RUNBOOK**.

Google's linked app, connector, and control guides and the cited Workspace mail pages are capability references as of **2026-09-30**, not deployment attestations. The installed edition, location, connector, policies, and observed behavior govern each route decision.
