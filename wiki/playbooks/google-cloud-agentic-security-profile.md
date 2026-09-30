---
type: playbook
title: "Google Cloud Agentic Security Profile"
address: c-629917
origin: produced
created: 2026-09-16
updated: 2026-09-30
tags:
  - playbooks
  - reference-implementation
  - google-cloud
  - gemini
  - sec-of-ai
status: developing
audience: "Google Cloud agent owners, security architects, and financial-sector assessors"
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[azure-rag-chatbot-security-profile]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[agentic-ai-security-cmm-crosswalk-canada-fi]]"
  - "[[securing-agentic-coding]]"
  - "[[gemini-workspace-control-sheet]]"
  - "[[gemini-enterprise-control-sheet]]"
  - "[[oversharing-controls]]"
  - "[[google]]"
  - "[[securing-workspace-genai-at-google-talk]]"
  - "[[geminijack-gemini-enterprise-injection]]"
  - "[[echoleak-copilot-zero-click]]"
  - "[[google-saif]]"
  - "[[indirect-prompt-injection]]"
  - "[[prompt-injection]]"
  - "[[spiffe]]"
  - "[[lethal-trifecta]]"
  - "[[inference-exposure]]"
  - "[[cmm-vocabulary-and-notation]]"
sources:
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/setup"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-flash"
  - "https://docs.cloud.google.com/iam/docs/agent-identity-overview"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes"
  - "https://docs.cloud.google.com/model-armor/integrations"
  - "https://docs.cloud.google.com/model-armor/feature-availability-by-region"
  - "https://docs.cloud.google.com/model-armor/data-residency"
  - "https://docs.cloud.google.com/model-armor/locations"
  - "https://docs.cloud.google.com/model-armor/model-armor-agent-gateway-integration"
  - "https://docs.cloud.google.com/stackdriver/docs/instrumentation/ai-agent-overview"
  - "https://docs.cloud.google.com/security-command-center/docs/agent-platform-threat-detection-overview"
verified: 2026-09-30
verified_against: []
verified_findings: 0
verified_note: "Whole-page source read against live official Google documentation; no archived document opened."
---

# Google Cloud Agentic Security Profile

This playbook helps a Google Cloud agent owner and security architect choose, test, and fund controls for a **customer-built agent on Gemini Enterprise Agent Platform**. Record the running agent, model, memory, tool, gateway, and network routes before applying a control. The organization supplies configuration and test evidence; Google supplies evidence for service internals the customer cannot inspect. Product observations were checked against Google's documentation on 2026-09-30. A tenant's control result requires deployed evidence.

Gemini in Google Workspace is a separate in-suite service. Its employee access, retrieval, action, and investment decisions belong to [[gemini-workspace-control-sheet|Gemini Workspace Control Sheet]]. The [[gemini-enterprise-control-sheet|Gemini Enterprise Control Sheet]] covers the separate Enterprise app with its own apps, data stores, agents, and settings. Agent Platform controls in this playbook do not establish controls on either employee-assistant path. The [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] describes the logical components and trust boundaries; this page provides the Google Cloud operating steps. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] grades evidence after implementation.

## Define the running route

The agent owner records its deployed revision, Google Cloud project, user population, model and endpoint, Agent Runtime, Agent Identity, Agent Gateway, tools, MCP servers, connected corpora, Memory Bank, egress destinations, and external effects. If the agent was built in [Agent Studio](https://docs.cloud.google.com/gemini-enterprise-agent-platform/agent-studio), record the prompt and model settings promoted from that workspace into the deployed revision. Mark direct calls that bypass the gateway. Capture the applicable edition, region, and supplier terms for each service. A route diagram and one trace of a representative action establish where the customer can enforce policy and where Google must supply evidence.

The first decision is the action boundary. A summarizer that reads an approved corpus and returns a draft needs source entitlement and output tests. An agent that can send mail, update records, or invoke an external MCP tool also needs a target-system authorization and an observed refusal. Use a separate record for each materially different action route; a single product setting does not prove that every route traverses it.

## Choose and test controls

### Identity and tool authorization

**Owner and mechanism.** The identity owner binds users and the running agent to the intended principals. Google's [Agent Identity](https://docs.cloud.google.com/iam/docs/agent-identity-overview) can identify an Agent Runtime principal. The platform owner configures [Agent Gateway IAM Access policies](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap) for calls through the gateway and restricts the target service's credentials and permissions. A dry run records a proposed refusal; enforced policy and a denied target call prove a boundary.

**Test and investment.** Attempt an allowed tool call, a disallowed call, and a direct route around the gateway. Retain principal, policy, trace, target outcome, and revocation evidence. Fund per-agent identity, narrower grants, and gateway routing where agents cross data owners or can make consequential writes. Their added operational cost is justified by the ability to revoke one agent without disrupting unrelated work and to refuse an unauthorized action at the actual target.

### Prompt and response screening

**Owner and mechanism.** The agent security owner places [[model-armor|Model Armor]] at a documented integration point. The [integration matrix](https://docs.cloud.google.com/model-armor/integrations) distinguishes Agent Gateway templates from Agent Platform floor settings and model integrations. Gateway screening covers supported text that traverses it; a direct model call, retrieved file, tool response, or client rendering needs its own route check. Record detectors, thresholds, inspect-or-block mode, processing-failure behavior, and regional template. Source permissions and target authorization remain separate controls.

**Test and investment.** Put a controlled indirect-injection instruction and a sensitive-data canary in a reachable source. Run them through a supported route, an unsupported payload through the gateway, and a direct route that bypasses it. Google's [Agent Gateway integration guide](https://docs.cloud.google.com/model-armor/model-armor-agent-gateway-integration) limits ingress screening to ADK `streamQuery` and names egress payloads it allows without sanitization. Observe the model answer, tool effects, Model Armor result, and logs for benign and attack cases. Fund additional integration work or supplier assurance when untrusted content and privileged actions meet on a route that the configured screen does not see. Estimate latency, false blocks, and response cost alongside the reduction in harmful effects.

### Data, memory, and location

**Owner and mechanism.** The data owner approves each corpus, memory use, retention rule, and processing location. Test source authorization with two principals and a document each may or may not read. Test memory writes and later retrieval for cross-user leakage. The platform owner records the effective service and model locations separately; a regional endpoint does not establish the location of every processing step.

| Route | Published scope | Required check |
|---|---|---|
| Agent Runtime and Gateway | [Montréal and Toronto](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations) | Running deployment region |
| Memory Bank | [Montréal listed; Toronto excluded](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations) | Availability and storage route |
| Model endpoint and processing | [Model-specific endpoint list](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations) and [processing matrix](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency) | Exact model, endpoint, and commitment |
| Memory generation | [Default-generation routing](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/setup) | Generation and embedding routes apart from storage |
| Model Armor | [Regional template availability](https://docs.cloud.google.com/model-armor/feature-availability-by-region) and [integration residency terms](https://docs.cloud.google.com/model-armor/data-residency) | Selected filters, region, and integration |

The named Canadian components cannot be combined in one region on the documented route. [Memory Bank supports Montréal but not Toronto](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations), while [Model Armor lists Toronto but not Montréal](https://docs.cloud.google.com/model-armor/locations). [Agent Gateway requires its Model Armor template in the same region](https://docs.cloud.google.com/model-armor/model-armor-agent-gateway-integration). The [Gemini 3.5 Flash model page](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/gemini/3-5-flash) lists a Montréal endpoint and processing region, but [Memory Bank default generation](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/setup) sends instances outside the US and EU regions to the global endpoint. The [processing matrix](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency) has no Canada column for partner models. A Canada-bound design therefore needs a diagram of each cross-region call, its processing commitment, and any documented exception before the combined control is credited. The Toronto Model Armor template omits some filters, and Agent Platform floor settings have a different residency commitment from the regional template.

**Test and investment.** Retain configuration, a request trace, the supplier commitment, and a two-principal retrieval and memory test. Fund a different region, model, memory pattern, or supplier arrangement when the required data class cannot use the observed route. Compare migration and operating cost with the value of keeping the action or memory feature.

### Egress and perimeter

**Owner and mechanism.** The network owner lists every permitted tool and egress destination, then configures network and gateway restrictions for the deployed route. Google's [2026-09-09 release note](https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes) limits Agent Gateway VPC Service Controls enforcement to deployments created after 2026-09-08 with an agent connectivity template in `ALL_TRAFFIC` egress mode. The [IAM Access policies overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap) still states that its feature does not support VPC Service Controls. These statements address different control surfaces and do not establish combined enforcement on a particular deployment.

**Test and investment.** Attempt a prohibited destination through the gateway and through a direct route. Collect the gateway denial, perimeter denial, and target outcome separately. Fund network redesign or narrower tool registration where an unmediated destination can carry regulated data or create an external effect. Recheck the documented interaction with Google before crediting combined policy enforcement.

### Telemetry and response

**Owner and mechanism.** The security operations owner collects user, agent, model, tool, gateway, and target-service events with enough identifiers to reconstruct a material action. [OpenTelemetry spans](https://docs.cloud.google.com/stackdriver/docs/instrumentation/ai-agent-overview) support Agent Platform tracing. The [Agent Platform Security tab](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/view-security-findings) draws on configured Security Command Center features; its findings are useful for triage but do not prove an action was refused. Keep the application trace and target event as the action record.

**Test and investment.** Execute an allowed and a prohibited action, reconstruct both, fire the intended alert, and exercise principal revocation and agent rollback. Fund broader correlation and on-call response when many agents or high-impact actions make manual reconstruction too slow. Record the storage, tuning, and response effort as part of the target control cost.

## Make the investment decision

Start with a route inventory and an end-to-end refusal test. Then compare each observed gap with a narrower data or action scope, a customer-owned enforcer, a supplier commitment, or an accepted exception. Record the exposure removed, implementation effort, recurring operating cost, effect on agent utility, owner, and evidence needed to retest. For a bounded read-only agent, source entitlement and trace reconstruction may carry most of the risk reduction. An agent with external writes needs proven tool authorization, egress control, injection tests, and a practiced stop path before those writes are enabled. Use [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] and its measurement protocol for formal domain results; do not treat this playbook's tests as maturity grades.

## Source and date limits

Product availability, model routes, and location commitments change. Recheck the linked Google documentation and effective tenant configuration before procurement or assessment. The dated [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] used an earlier CMM instrument and assessed Gemini in Workspace, so its levels do not transfer to this Agent Platform route. A Canadian regulated institution can map its control evidence through [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]].
