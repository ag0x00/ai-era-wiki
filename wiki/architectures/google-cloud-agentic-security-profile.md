---
type: architecture
title: "Google Cloud Agentic Security Profile"
address: c-629917
origin: produced
created: 2026-09-16
updated: 2026-09-29
tags:
  - architectures
  - reference-implementation
  - google-cloud
  - gemini
  - sec-of-ai
status: developing
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
  - "https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/google-workspace-with-gemini"
  - "https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services"
  - "https://support.google.com/a/users/answer/17010577"
  - "https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini"
  - "https://knowledge.workspace.google.com/admin/compliance/data-covered-by-data-regions"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/setup"
  - "https://docs.cloud.google.com/iam/docs/agent-identity-overview"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes"
  - "https://docs.cloud.google.com/model-armor/integrations"
  - "https://docs.cloud.google.com/model-armor/feature-availability-by-region"
  - "https://docs.cloud.google.com/model-armor/data-residency"
  - "https://docs.cloud.google.com/stackdriver/docs/instrumentation/ai-agent-overview"
  - "https://docs.cloud.google.com/security-command-center/docs/agent-platform-threat-detection-overview"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Read whole and checked current official Google documentation on 2026-09-29; clarified Montréal Memory Bank generation and D2 vendor-held identity condition. No archived source opened; no findings remain."
---

# Google Cloud Agentic Security Profile

This profile helps an architect select evidence and control points for a defined Google deployment. It applies the [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] and [[agentic-ai-security-cmm-2026|Agentic AI Security Capability Maturity Model]] to two distinct shapes: Gemini in Google Workspace and a customer-built agent on Gemini Enterprise Agent Platform. The [[agentic-ai-security-cmm-measurement-protocol|CMM: Measurement Protocol (Assessor's Handbook)]] determines the domain results from the deployed routes. Product observations below were checked against Google's documentation on 2026-09-29. A tenant assessment and maturity result require deployed evidence. The [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]] used an earlier CMM instrument, so its levels do not transfer.

## Select the deployment shape

Record the deployed route and its effective configuration before requesting evidence. Gemini Enterprise, Gemini in Workspace, and Agent Platform are separate products. A control documented for one needs its own proof on another.

| Shape | Boundary to assess | Customer decision and first evidence |
|---|---|---|
| **W — Gemini in Workspace** | Google-operated model and in-suite action path | Workspace configuration export and action inventory |
| **A — customer-built agent on Agent Platform** | Customer-configured agent and model route on Google-operated services | Running configuration and action trace |

For W, export the [feature-access configuration](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services) and test the [effective data-access rules](https://support.google.com/a/users/answer/17010577). Gemini uses the asking user's Workspace access, subject to product restrictions. Disabling Gemini in one service still permits Gemini in another service to read that service's data. For A, identify the exact model and tool routes in the running configuration. A coding workflow is one instance; a different route may require a separate assessment.

### Canadian location decision

| Route or feature | Published scope | Decision for a Canada-bound design |
|---|---|---|
| Workspace data regions | [Gemini prompts and responses](https://knowledge.workspace.google.com/admin/compliance/data-covered-by-data-regions): United States or Europe | Obtain a service-specific Canada commitment. |
| Agent Runtime and Gateway | [Supported locations](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations): Montréal and Toronto | Verify the deployed region. |
| Memory Bank location | [Supported locations](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations): Montréal listed, Toronto excluded | Verify availability before design. |
| Model endpoint | [Endpoint list](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations): Montréal for selected models | Verify the exact model and endpoint. |
| Model processing | [Processing matrix](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency): Canada column for Google models | Verify the exact model row. |
| Memory generation | A new Montréal Memory Bank using [default generation](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/setup) uses a global Gemini endpoint | Verify the generation route separately from memory storage. |
| Model Armor template | [Toronto residency-enforced template](https://docs.cloud.google.com/model-armor/feature-availability-by-region): prompt-injection, responsible-AI, and sensitive-data filters | Verify the selected filters and integration route. |
| Model Armor floor settings | [Agent Platform floor settings](https://docs.cloud.google.com/model-armor/data-residency): no enforced Canadian processing or transit | Verify the integration and processing commitment. |

Workspace processing coverage varies by edition. The Agent Platform endpoint list does not itself guarantee processing location. Its [processing matrix](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency) has no Canada column for partner models. Canadian processing for those routes requires separate model-route evidence. A coding architecture can use a Google model with an applicable Canada commitment or a separately evidenced supplier arrangement. The Toronto Model Armor template omits malicious-URL, image, and antivirus filters. Agent Platform floor settings carry a different residency commitment from a regional template.

## Locate the control boundary

The customer can configure [Workspace feature access](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/manage-access-to-gemini-features-in-workspace-services), source permissions, and [DLP for Gemini](https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini). DLP can block a matching **Drive** source from an answer and log the event; Google's stated restriction covers Drive data only. Source permissions still permit Gemini to combine material that a user can read, including material shared too widely. Test two users with different grants and a Drive DLP block. Review the other reachable corpora separately under [[oversharing-controls|Oversharing Controls for AI Search]].

Google's [Model Armor integration list](https://docs.cloud.google.com/model-armor/integrations) documents customer-configurable integrations for Agent Gateway, Agent Platform, and other named services. It names no in-path integration for Gemini in Workspace. Supplier-held or customer-owned screening may exist at other boundaries. For shape W, request scoped supplier evidence for the internal prompt, retrieval, action, and output path, and test customer-visible effects. An organization can also screen an input, connector, or client path it owns. Such an adjacent control covers only the traffic the test shows it sees.

For shape A, [Agent Identity](https://docs.cloud.google.com/iam/docs/agent-identity-overview) can identify an Agent Runtime principal, and [IAM Access policies](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap) can govern Agent Gateway calls. A dry-run records disallowed calls without blocking them; an enforced refusal is separate evidence. [Model Armor at Agent Gateway](https://docs.cloud.google.com/model-armor/integrations) can screen supported text traffic that actually traverses the gateway. The chosen model, direct calls, retrieved files, client rendering, and tool responses still need a route inventory and tests.

The gateway's network and policy claims require a joint test. Google's [2026-09-09 release note](https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes) limits Agent Gateway VPC Service Controls enforcement to deployments created after 2026-09-08 with an agent connectivity template in `ALL_TRAFFIC` egress mode. The current [IAM Access policies overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap) still says that feature does not support VPC Service Controls. Until Google clarifies the interaction, inspect the deployed route and test both the access-policy refusal and perimeter refusal; do not claim combined enforcement from either document alone.

## Use the nine-domain evidence map

Each domain points to the decision the CMM grades. The organization supplies configuration and test evidence; the supplier supplies evidence for inaccessible service internals. A supplier assertion must identify the deployed route and period. The Handbook records an applicable opaque step as **unanswerable** and a failed customer-visible test as **not met**. A generic vendor security statement establishes neither result.

- **[[agentic-ai-security-cmm-d1-governance|CMM D1: Governance and Accountability]].** W: enabled-service inventory and owner. A: agent inventory and risk decision.
- **[[agentic-ai-security-cmm-d2-identity|CMM D2: Identity and Authorization]].** W: asking-user identity and evidence for any vendor-held agent identity condition. A: effective agent grants and revocation test.
- **[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3: Control and Least-Agency]].** W: action-confirmation and denied-action tests. A: gateway bypass and refusal tests.
- **[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4: Runtime and Guardrails]].** W: scoped supplier screening evidence and tenant injection test. A: attack and benign tests on the in-path detector.
- **[[agentic-ai-security-cmm-d5-egress-network|CMM D5: Egress and Network]].** W: action destinations and supplier egress evidence. A: effective route and perimeter refusal test.
- **[[agentic-ai-security-cmm-d6-data-rag|CMM D6: Data, Memory and RAG]].** W: two-principal retrieval and Drive DLP block tests. A: corpus entitlement and memory-write tests.
- **[[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]].** W: action reconstruction from available events and supplier trace. A: reconstructed action and tested alert.
- **[[agentic-ai-security-cmm-d8-supply-chain|CMM D8: Engineering and Supply Assurance]].** W: supplier assessment and configuration promotion. A: versioned security test and release decision.
- **[[agentic-ai-security-cmm-d9-operations|CMM D9: Operations and Human Factors]].** W: approval and disablement exercise. A: guardrail runbook and recovery exercise.

For D7, [OpenTelemetry spans](https://docs.cloud.google.com/stackdriver/docs/instrumentation/ai-agent-overview) can support Agent Platform action reconstruction. [Agent Platform Threat Detection](https://docs.cloud.google.com/security-command-center/docs/agent-platform-threat-detection-overview) covers named execution and control-plane signals; test the rule needed for the assessed behavior. For D8, Google's [Workspace feature description](https://knowledge.workspace.google.com/admin/generative-ai/workspace-with-gemini/google-workspace-with-gemini) names no running model version for the in-suite route. If the supplier cannot provide one, D8-VERSION and D8-MODEL-CARD are unanswerable for that model. Customer configuration promotion remains assessable.

## Make the investment decision

For shape W, the principal decision is how much supplier-held action, screening, and telemetry assurance the organization requires for the data and actions it enables. The first work package is a bounded evidence request and tenant test. Where the evidence remains unavailable, reduce the enabled data or action scope, add a proven control on a customer-owned path, or record the residual exposure and decision owner. Drive DLP and permission repair address different gaps; neither establishes enforcement over every Workspace source.

For shape A, the first work package is a route inventory and an end-to-end refusal test across identity, gateway, model, data and egress. Subsequent work can close observed gaps in tool authorization, screening, trace reconstruction, release assurance, and operations. A region or product selection should be funded only after the exact feature, model, endpoint and processing commitment meet the deployment's requirement. The report compares each domain's current and target level, the exposure removed, implementation and operating effort, supplier dependencies, and the evidence required to recheck it. It does not aggregate the domain levels.

## Source and date limits

This is a documentation profile as of **2026-09-29**. Product availability, edition gates, model routes, and region commitments can change. Recheck the linked Google pages and the tenant's effective configuration before an assessment or procurement decision. Applicable regulatory obligations are selected separately in [[agentic-ai-security-cmm-crosswalk-canada-fi|CMM: Canadian Regulated-Finance Crosswalk]].
