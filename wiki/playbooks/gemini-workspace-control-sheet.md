---
type: playbook
title: "Gemini Workspace Control Sheet"
created: 2026-09-30
updated: 2026-09-30
tags:
  - playbooks
  - gemini
  - google-workspace
  - control-mapping
status: developing
scope_axis:
  - sec-of-ai
origin: produced
audience: "Workspace and security administrators, data owners, and financial-sector assessors"
related:
  - "[[claude-code-control-sheet]]"
  - "[[productivity-assistant-deployment-shape]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[gemini-enterprise-control-sheet]]"
  - "[[model-armor]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[oversharing-controls]]"
  - "[[wiz-ai-spm]]"
sources:
  - "https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services"
  - "https://knowledge.workspace.google.com/admin/generative-ai/workspace-intelligence/control-workspace-intelligence"
  - "https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-google-apps-in-gemini-on-or-off"
  - "https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini"
  - "https://knowledge.workspace.google.com/admin/generative-ai/explore-the-ai-control-center"
  - "https://knowledge.workspace.google.com/admin/reports/gemini-for-workspace-log-events"
  - "https://knowledge.workspace.google.com/admin/reports/workspace-studio-log-events"
  - "https://knowledge.workspace.google.com/admin/studio/get-started-workspace-studio-set-up-guide-for-admins"
  - "https://knowledge.workspace.google.com/admin/generative-ai/support-access-to-workspace-integrations"
  - "https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-conversation-sharing-on-or-off"
  - "https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-gem-sharing-on-or-off"
  - "https://knowledge.workspace.google.com/admin/drive/manage-external-sharing-for-your-organization"
  - "https://knowledge.workspace.google.com/admin/compliance/data-covered-by-data-regions"
  - "https://knowledge.workspace.google.com/admin/generative-ai/generative-ai-in-google-workspace-privacy-hub"
  - "https://www.wiz.io/blog/introducing-wiz-ai-app"
  - "https://www.wiz.io/blog/wiz-launches-support-for-google-workspace"
verified: 2026-09-30
verified_against: []
verified_findings: 0
verified_note: "Checked current Google and Wiz documentation; corrected evidence and investment instructions and Gem citation; no archived source opened."
---

# Gemini Workspace Control Sheet

This playbook helps a financial organization select and implement controls for Gemini in Google Workspace. Its operator records what each employee population can read and do, chooses a tested control for each material risk, and justifies stronger controls where the expected reduction in exposure warrants their cost. The result is a deployment plan and an evidence-backed improvement target, as well as an enablement decision. The control record describes proposed requirements; it does not attest to any customer's tenant.

The [[productivity-assistant-deployment-shape|Productivity Assistant Deployment Shape]] sets out the employee-assistant threat paths and boundary decisions. This sheet applies them to Gemini in Workspace and its enabled service routes. Use [[gemini-enterprise-control-sheet|Gemini Enterprise Control Sheet]] for the distinct employee app, [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] for customer-built agents on Agent Platform, and [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] for capability grading. The dated [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] supplies failure modes to test, not a score for this customer.

## Scope and service boundaries

Inventory the enabled surface before applying a control. Google [separates service feature access](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services), [Workspace Intelligence sources](https://knowledge.workspace.google.com/admin/generative-ai/workspace-intelligence/control-workspace-intelligence), and the [Gemini app's Workspace connections](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-google-apps-in-gemini-on-or-off). Each has its own administrative setting and effective population. Turning off the side panel in Drive does not prevent a user from asking Gemini in Gmail about a Drive file. Turning off a Workspace Intelligence source does not necessarily remove the currently open document from the side panel's context. Test the effect, not the label on the setting.

| Surface | Boundary to record | First test |
| --- | --- | --- |
| Gemini in Workspace | Enabled service and side panel | Ask in each enabled app |
| Workspace Intelligence | Searchable Gmail, Drive/Docs, Calendar, and Chat sources | Retrieve a canary per source |
| Gemini app | Workspace apps, other Google apps, and third-party apps | Attempt each connection |
| Workspace Studio | Flow owner, AI steps, service steps, and integrations | Run an approved and a prohibited flow |
| Gems and shared material | Instruction owner, file sharing, and recipient | Share a canary Gem |

The service inventory also records edition, configuration group, organizational unit, smart-feature choice, beta enrollment, devices, and any third-party integration. Google documents default-on settings for [Workspace features](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services), [Workspace Intelligence sources](https://knowledge.workspace.google.com/admin/generative-ai/workspace-intelligence/control-workspace-intelligence), and [Workspace apps in the Gemini app](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-google-apps-in-gemini-on-or-off). An enterprise rollout therefore starts by recording the settings and testing their effect on users. Gemini Enterprise, Gemini in Chrome, Gemini CLI, and customer-built agents on Agent Platform are separate deployments.

In the Admin console, the main settings sit under **Generative AI → Gemini for Workspace → Feature access / Workspace Intelligence**, **Generative AI → Gemini app → Apps / Sharing**, and **Apps → Google Workspace → Workspace Studio**. DLP rules sit under **Security → Access and data control → Data protection**; Gemini and Studio events sit under **Reporting → Audit and investigation**. Record each setting by organizational unit and configuration group, then test its effect on the intended users. [Workspace feature access](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services), [Studio setup](https://knowledge.workspace.google.com/admin/studio/get-started-workspace-studio-set-up-guide-for-admins), [Drive DLP rule creation](https://knowledge.workspace.google.com/admin/security/create-dlp-for-drive-rules-and-custom-content-detectors), and [Gemini event search](https://knowledge.workspace.google.com/admin/reports/gemini-for-workspace-log-events) document those locations.

## Control record

For each employee population whose data or action rights differ materially, keep one row per applicable control. A high-trust administrator, a payments operator, and a general employee can share a product yet need different controls. Record:

- **Boundary:** population, edition, surface, connected sources, data class, permitted actions and recipients, and accountable business and technical owners.
- **Decision:** control ID, desired result, enforcer, and the setting or policy applied to the population.
- **Proof:** recorded configuration, test identities and canary data, applicable allowed and refused results, timestamped logs, and supplier evidence for service internals.
- **Investment:** current failure path and consequence, narrower-scope and stronger-control options, incremental license, implementation, operating, and workflow costs, expected exposure reduction, and the test that will show it.
- **Disposition:** implemented, planned with owner and date, exception with expiry, or not applicable with reason. A missing observable result is unverified, not implemented.

Start with the least sensitive representative population, then repeat the tests for privileged and regulated-data populations. The security architect selects a target by exposure and dependency rather than averaging CMM levels or claiming a product feature proves an operating control.

## Control recommendations

### G01 — Grant the intended population

**Requirement and owner.** The Workspace administrator must restrict each Gemini surface to the approved employee population. Identity administrators own account assurance and privileged-group membership; the Gemini Settings administrator owns feature access. A population-wide grant is appropriate only after the organization accepts the same source and action scope for that population.

**Mechanism and proof.** Record service settings by organizational unit and configuration group. Check which settings the selected edition provides before naming an enforcer. Google states that [group settings override organizational-unit settings](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services). Include the Gemini app, Workspace Intelligence, Studio, and beta features in the record. Restrict who can change those settings, and record an admin change in the normal change process. Use the [AI control center](https://knowledge.workspace.google.com/admin/generative-ai/explore-the-ai-control-center) to review enabled use and related security settings; it is a management view, not an enforcement point by itself.

**Test and investment trigger.** Sign in as an approved employee, a denied employee, and a newly transferred employee. Try each surface after the setting propagation window and retain the result. Invest in automated entitlement review and change alerts when the population is large, role moves are frequent, or one mistaken group grants access to regulated content. The benefit is reduced grant drift; the recurring cost is review and alert triage.

### G02 — Bound retrieval and repair sharing

**Requirement and owner.** Data owners must decide which corpora Gemini may search and repair broad sharing before they rely on asking-user permissions. Google states that [Workspace Intelligence respects the user's content access](https://knowledge.workspace.google.com/admin/generative-ai/workspace-intelligence/control-workspace-intelligence). A document that was shared too broadly remains available to a user who can read it, and Gemini can make the exposure easier to discover.

**Mechanism and proof.** Record Workspace Intelligence source settings and Gemini app connections separately. Review sensitive Drive shares, shared drives, mail delegation, calendar visibility, and Chat membership with the relevant data owners. Use [[oversharing-controls|Oversharing Controls for AI Search]] for the repair method. The [Workspace Intelligence setting](https://knowledge.workspace.google.com/admin/generative-ai/workspace-intelligence/control-workspace-intelligence) controls active search sources for in-suite features; Google documents exceptions for currently active content and for other Gemini services. Record those exceptions in the source map.

**Test and investment trigger.** Place canary content in a restricted item and in an intentionally over-shared item. Ask two users with different grants to retrieve each item through every enabled route. The restricted item must remain unavailable to the unauthorized user. The over-shared item demonstrates the repair queue. Invest in continuous permission analysis and owner-led cleanup when broad shares cross business units or sensitive repositories are large. Source disablement reduces reach quickly but may also remove the productivity use case. Permission repair keeps approved collaboration available.

### G03 — Protect regulated content

**Requirement and owner.** The data-protection owner must state which classes may enter prompts, retrieval, generated content, and external messages. Legal and privacy owners set retention, hold, supplier terms, and location requirements before the corresponding population is enabled.

**Mechanism and proof.** Configure [DLP for Gemini](https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini) to block access to matching Drive sources where the class requires it and the tenant's edition supports it. If the control is unavailable, compare a narrower enabled population or repaired sharing with an edition change, and record the residual exposure. Google's current documentation limits that rule's source restriction to Drive. An audit-only action logs the match and still allows use. Ordinary [Workspace DLP](https://knowledge.workspace.google.com/admin/security/about-dlp) and sharing policies govern Gmail, Drive, Chat, Calendar, and outgoing effects under their own scopes. Keep their rules and evidence distinct. Record Gemini app and in-suite [retention settings](https://knowledge.workspace.google.com/admin/generative-ai/generative-ai-in-google-workspace-privacy-hub), and whether Google Vault rules or holds prevail. For a Canada-bound design, Google's [Workspace data-regions table](https://knowledge.workspace.google.com/admin/compliance/data-covered-by-data-regions) lists United States or Europe for covered Gemini prompts and responses. Seek a service-specific Canada commitment or make an explicit exception decision.

**Test and investment trigger.** Put a labeled or detector-matching canary in Drive, mail, and Chat. Verify the Drive source block, the ordinary outbound DLP result, and the logs independently. Test retention and a legal hold with legal staff. Invest in stronger classification, DLP engineering, and ongoing false-positive review when regulated data is pervasive or users can export generated content to external recipients. A Drive block alone cannot carry a whole-Workspace confidentiality claim.

### G04 — Bound writes and external recipients

**Requirement and owner.** The service owner must approve each action Gemini can initiate or assist: a draft, a sent message, a calendar change, a shared file, a Studio step, or an integration call. The target service owner controls recipients and writes. A model's text saying it will ask permission is not an action gate.

**Mechanism and proof.** Build an action inventory from the enabled surfaces and test the actual confirmation and target-service checks. Workspace Studio can run multi-step flows; Google's [admin setup guide](https://knowledge.workspace.google.com/admin/studio/get-started-workspace-studio-set-up-guide-for-admins) distinguishes AI steps, service steps, and integrations. Its [send-reply step](https://support.google.com/workspace-studio/answer/18107630) documents approval when a run includes external people, but that rule does not establish approval for every Studio action. Apply Gmail recipient restrictions, Drive sharing policy, Calendar sharing rules, and integration-side permissions at the target system. Keep a separate record for an unattended flow because its effect can occur after the user leaves the screen.

**Test and investment trigger.** Try an internal permitted draft and an external prohibited send or share, including a dynamic recipient in a flow. Observe the actual target-system denial or human confirmation and the resulting event. Fund a managed flow catalog, narrower service steps, and approval or outbound policy for payments, customer communications, privileged administration, and other high-impact actions. The added review cost is justified where a wrong recipient or write creates an external obligation or hard-to-reverse effect.

### G05 — Treat retrieved content as untrusted

**Requirement and owner.** Security testing must treat mail, documents, calendar invitations, Chat, web results, and integration responses as untrusted instructions even when the asking user can read them. Product and target-system owners must keep the intended action boundary in place when retrieved text asks for a different action.

**Mechanism and proof.** Seed a controlled message or document with an instruction to reveal a canary or change an approved task. Observe the answer, any tool action, destination, and logs. Request from Google evidence scoped to the enabled Workspace path for prompt handling, retrieved-content screening, action mediation, and incident response. Google's published [[model-armor|Model Armor]] [integrations](https://docs.cloud.google.com/model-armor/integrations) include the separate Gemini Enterprise app and Agent Platform; they do not establish a customer-configurable in-path guard for Gemini in Workspace. Adjacent screening earns credit only for the traffic the test proves it sees. The [[geminijack-gemini-enterprise-injection|GeminiJack Gemini Enterprise Zero-Click Injection]] case is a useful attack pattern, though Gemini Enterprise is a different product.

**Test and investment trigger.** Run benign and adversarial retrieval cases through each enabled source, including a legitimate question on the poisoned item. A verbal refusal is insufficient when the recipient or file changed. Invest in recurring attack tests, tighter source or action scope, and stronger supplier assurance when the assistant can read sensitive data and produce outward effects in one workflow. These measures cost test engineering and may constrain useful automation; they address a path that ordinary content classification does not test.

### G06 — Approve extensions and automation

**Requirement and owner.** The Workspace platform owner must approve each new data source, extension, shared instruction, and unattended flow before it expands the assistant's reach. Third-party service owners retain control of connector credentials and permissions.

**Mechanism and proof.** Govern Gemini app Workspace and other Google app connections separately. [Workspace Integrations](https://knowledge.workspace.google.com/admin/generative-ai/support-access-to-workspace-integrations) can read or act in third-party systems from Gemini Apps and Studio. Google directs admins to Marketplace allowlists and states that changing an integration from Trusted to Blocked in API controls does not restrict its use. Test the Marketplace path and the third-party grant. Review Studio service steps, AI steps, sharing, and flow ownership. [Shared Gems use Drive sharing](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-gem-sharing-on-or-off). Google's [Drive guidance](https://knowledge.workspace.google.com/admin/drive/manage-external-sharing-for-your-organization) says that Google files included in a Gem are shared with anyone who has access to it. Disabling Gem sharing does not revoke an already shared Gem in Drive. The Gemini app also permits [conversation sharing](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-conversation-sharing-on-or-off): public links are off by default, Drive sharing is on by default, and old public links remain reachable until deleted. Treat beta enrollment as a separate change because [beta access covers the available beta feature set](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/turn-access-to-google-workspace-with-gemini-beta-on-or-off).

**Test and investment trigger.** Attempt an unapproved integration, access to a previously shared Gem, sharing a conversation, and an unauthorized Studio step with a test account. Check the helper app and third-party permissions as well as Workspace settings. A small, fixed connector set can be reviewed manually. Invest in a connector register, periodic recertification, and flow-owner review when many business units add integrations or flows; this lowers credential and data-path drift at a continuing administrative cost.

### G07 — Reconstruct activity and contain incidents

**Requirement and owner.** The security operations owner must reconstruct a material Gemini-assisted action and stop further effects. The record needs actor, surface, time, source where available, action, target, recipient, and policy outcome. A usage total alone cannot support incident reconstruction.

**Mechanism and proof.** Collect [Gemini for Workspace log events](https://knowledge.workspace.google.com/admin/reports/gemini-for-workspace-log-events), DLP events, and the relevant Gmail, Drive, Calendar, or Chat target-service logs. Google lists actor, application, action, event category, and feature source for Gemini events, while warning that not every attribute appears on every event. [Studio log events](https://knowledge.workspace.google.com/admin/reports/workspace-studio-log-events) add flow, run, and step identifiers. The [AI control center](https://knowledge.workspace.google.com/admin/generative-ai/explore-the-ai-control-center) supports use review. Export required logs to the organization's store under its retention rule, and request supplier traces for opaque steps when the customer logs cannot establish the result.

**Test and investment trigger.** Perform an allowed action, a DLP block, and a prohibited Studio step. Reconstruct each from the stored events and fire an alert for the harmful pattern. Exercise account revocation and the surface-level disablement. Google documents that stopping one [Studio flow](https://knowledge.workspace.google.com/admin/studio/stop-a-workspace-studio-flow-as-an-admin) requires Support, while moving its owner to a Studio-disabled organizational unit stops all that user's flows. Invest in automated correlation and a practiced containment runbook when use spans the workforce or the bank must meet short investigation and reporting windows. The cost is ingestion, retention, tuning, and on-call response.

### G08 — Review supplier and configuration change

**Requirement and owner.** Third-party risk, legal, privacy, and platform owners must approve the exact Google Workspace edition, service route, data terms, processing region, retention, and supplier evidence. The platform owner must recheck controls when Google changes a feature, default, integration, or model route.

**Mechanism and proof.** Keep the applicable [Workspace privacy terms](https://knowledge.workspace.google.com/admin/generative-ai/generative-ai-in-google-workspace-privacy-hub) and [data-regions scope](https://knowledge.workspace.google.com/admin/compliance/data-covered-by-data-regions) beside the recorded tenant configuration. Separate the Gemini app as a Workspace core service from access as an additional Google service, because Google describes different [data-use conditions](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/turn-the-gemini-app-on-or-off). Request a route-specific model/version inventory, change notice, testing evidence, and incident-assistance commitment from Google where the customer cannot observe the internal path. Google's [Workspace feature description](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/google-workspace-with-gemini) does not identify the running model version for every in-suite interaction; record the unanswered item rather than assigning a version from another Gemini product.

**Test and investment trigger.** Re-run the canary suite after an edition change, source toggle, new connector, new action, or material supplier notice. A basic annual review can fit a small read-only rollout. A bank-wide deployment with sensitive retrieval and automated effects warrants more frequent change review, supplier evidence, and contract work. That investment buys assurance over the service boundary the bank cannot instrument itself.

## Wiz contribution and limits

Use the Wiz investment where its connector sees the relevant asset and its finding changes a decision. Wiz [documents Google Workspace identity modeling](https://www.wiz.io/blog/wiz-launches-support-for-google-workspace) for users, groups, super administrators, and their Google Cloud entitlements. That can strengthen G01's privileged-access review and the adjacent GCP risk picture. Wiz [AI Application Protection](https://www.wiz.io/blog/introducing-wiz-ai-app) covers customer-built AI applications across cloud assets and runtime paths; the product description does not establish inspection or enforcement inside Google's Gemini in Workspace service. Do not count a Wiz AI asset finding as a refused in-suite prompt or tool action.

For each proposed Wiz use, capture the connected account, granted read scope, discovered asset or event, finding, and remediation owner. Test whether it includes the specific Workspace group, GCP entitlement, connector, or cloud-hosted application at issue. Keep recorded Workspace settings, DLP events, Gemini logs, and supplier evidence as the sources of proof for in-suite behavior. A broader Wiz license may lower inventory and prioritization effort where the bank already uses Wiz across GCP, but its incremental value is measured by findings the team can act on, not by treating every available Wiz product as a Gemini control.

## Investment sequence and risk tradeoffs

Implement the control baseline before broad access: an accurate surface inventory, accountable grants, source and sharing review, data-class rules, action and recipient limits, collected logs, and a tested disablement path. Each item has a direct failure case in G01–G08 and can be evidenced in the tenant. An inexpensive setting is not a cheap control if nobody owns its exceptions or checks its effective result.

Select additional investment by the consequence of a failed boundary:

| Exposure | Stronger measure | Cost and decision |
| --- | --- | --- |
| Broadly shared regulated content | Continuous permission analysis and owner-led cleanup | Operations and data-owner time; preserves useful search |
| Sensitive Drive sources | Classification and blocking Gemini DLP | Rule engineering and false-positive handling; narrows retrieval |
| External sends or unattended writes | Target-service restrictions, managed flow catalog, and approvals | Workflow friction and review capacity; limits irreversible effects |
| Untrusted mail and document retrieval | Repeated injection tests and supplier assurance | Test engineering and procurement effort; probes hidden action paths |
| Workforce-wide or high-impact use | Log export, correlation, alerts, and containment exercise | Storage, analyst time, and on-call cost; shortens investigation |
| Opaque Google service internals | Scoped evidence request and contractual commitment | Supplier dependency; may leave a residual exception |

A stronger measure is warranted when the affected data or action is consequential, the path is reachable by many employees, and the proposed control can demonstrably interrupt or detect it. Prefer a narrower source or action grant when its business cost is low. Where a required boundary remains supplier-held and unevidenced, the decision owner can limit the population or action, secure a supplier commitment, or accept a time-bound exception. Record the productivity loss, implementation effort, operating burden, and expected risk reduction for each option. No Wiz or Google license, CMM level, or generic supplier statement substitutes for that comparison.

For example, a customer-service group may need Gemini to summarize mail and draft responses. A read-and-draft pilot can preserve that benefit while the customer tests retrieval and outbound policy. If the group can read regulated records and an unattended Studio flow can send generated replies, the failure can cross both a data and recipient boundary before a person reviews it. The stronger target is then a limited flow catalog, tested external-recipient approval or denial, Gemini and target-service event correlation, and repeated injected-mail tests. Fund those controls before enabling unattended sends; if they cannot be demonstrated, keep the read-and-draft route and record the work saved and automation deferred. This is a risk and cost argument tied to an observable effect, not a claim that all employees need the same control tier.

## Validation and customer decision

Run the tests in a tenant or controlled pilot with representative identities and canaries. Use the same edition and feature settings planned for production:

1. Record the feature, source, app-connection, Studio, integration, DLP, sharing, retention, and logging settings by population, organizational unit, and configuration group. Test their effective result with representative identities.
2. Test approved and denied access to every surface, then retrieve restricted and over-shared canaries through each enabled source.
3. Test a Drive DLP block and separate outbound mail or sharing rules; collect their different outcomes.
4. Perform allowed and prohibited writes, including an unattended flow and a dynamic external recipient.
5. Place an indirect-injection instruction in controlled mail or a document. Observe target effects and logs, not just the answer.
6. Reconstruct the events, alert, revoke the user or connector, and stop the affected flow or surface. Repeat after a material configuration or supplier change.

Record one of four implementation results for each test:

- **Pass:** the named enforcer produces the required result, and the evidence can be retrieved.
- **Fail:** the enforcer does not produce the required result.
- **Unverified:** the tenant or supplier cannot prove the result.
- **Not applicable:** the route has no instance of the tested action or data path, with a specific reason.

The final record names the current capability, risk-selected target, funded control changes, owners, completion evidence, residual exposure, and retest date. These are control implementation results. Use [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] for formal maturity verdicts and levels. A Canadian regulated institution can map its evidence through [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]]. Another jurisdiction needs its own obligations map.
