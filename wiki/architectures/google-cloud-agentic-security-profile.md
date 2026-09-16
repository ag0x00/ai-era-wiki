---
type: architecture
title: "Google Cloud Agentic Security Profile"
address: c-629917
origin: produced
created: 2026-09-16
updated: 2026-09-16
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
sources:
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "https://docs.cloud.google.com/model-armor/feature-availability-by-region"
  - "https://docs.cloud.google.com/model-armor/data-residency"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency"
  - "https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap"
  - "https://support.google.com/a/users/answer/17010577"
---

# Google Cloud Agentic Security Profile

This page reads the [[agentic-ai-security-reference-architecture|six-plane reference architecture]] and the [[agentic-ai-security-cmm-2026|nine-domain CMM]] against Google Cloud alone, plane by plane and domain by domain, and names the planes where Google ships no native control. It answers a closure condition the May regulated-FI stress test left open: "a **single-stack reading** of the RA per major platform (Microsoft-only, AWS-only, GCP-only) that names where the platform has no native control and an off-stack component is unavoidable" ([[agentic-cmm-regulated-fi-stress-test|Agentic AI CMM: Regulated-FI Stress Test]]). The Microsoft-only and AWS-only readings stay open. The worked example is the productivity-assistant shape, an assistant holding tools over a whole tenant's mail, files and calendar, running on Gemini for Google Workspace. Product status is a 2026-09-16 documentation snapshot, and the pages cited here name Google's generative-AI surface Gemini Enterprise Agent Platform, which Google previously called Vertex AI.

## The deployment shape

| Attribute | This profile |
|---|---|
| Host | Gemini in the Workspace side panels and the Gemini app with Workspace extensions; agents built in-house run on Agent Runtime behind Agent Gateway |
| Model | Gemini, served by Gemini Enterprise Agent Platform |
| Knowledge | The employee's own Gmail, Drive, Docs, Calendar and Chat — per-user reach across the whole tenant corpus |
| Users | Every employee, through their own account |
| Tools | Read, write and share across mail, files and calendar |
| Licensing | Google Workspace with Gemini; Cloud Identity federated to the organization's provider |

The assistant reads private data, ingests untrusted content on every summarization of an inbox or a calendar, and writes and shares, so all three legs of the [[lethal-trifecta|lethal trifecta]] are present. The vault's other deployment-shape profile, [[azure-rag-chatbot-security-profile|the Azure RAG chatbot profile]], covers a closed-corpus bot whose broken trifecta lowers the required level across five domains; that reduction does not transfer here, and no plane falls out of scope.

## The control profile per plane and domain

| Plane / Domain | Evidenceable → needed | Google control | Status as documented | The one thing that matters |
|---|---|---|---|---|
| **Identity ([[agentic-ai-security-cmm-d2-identity\|D2]])** | L2 → L3 | Agent Identity: a [[spiffe\|SPIFFE]] ID per agent, a 24-hour X.509 certificate, and access tokens bound to it[^agentid] | No launch stage stated[^agentid] | An agent on Agent Runtime gets a first-class principal; the Workspace assistant acts as the employee and has none |
| **Control ([[agentic-ai-security-cmm-d3-control-least-agency\|D3]])** | L1 to L2 → L3 to L4 | IAM Unified Access Policies at Agent Gateway, evaluated by Identity-Aware Proxy, with CEL conditions and a DRY_RUN mode before ENFORCE[^uap] | No launch stage stated[^uap] | A declarative decision point exists and excludes the egress perimeter (below) |
| **Runtime ([[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]])** | L2 → L3 to L4 | Model Armor templates or floor settings; the check grounding API for a 0-to-1 support score at inference time[^armor][^grounding] | GA per named feature and integration; none stated for the core screening service[^armor][^armorrn] | Model Armor documents no Gmail, Docs or Drive integration, so this shape's screening is the vendor's own |
| **Egress ([[agentic-ai-security-cmm-d5-egress-network\|D5]])** | L1 to L2 → L3 | VPC Service Controls perimeters; Model Armor inline at Agent Gateway, Apigee or a Service Extension; Private Service Connect to the Model Armor API[^vpcsc][^integrations] | GA for the Model Armor, Agent Runtime, Secure Web Proxy and Sensitive Data Protection integrations[^vpcsc] | The customer holds no in-path position between a Workspace assistant and its tools |
| **Data ([[agentic-ai-security-cmm-d6-data-rag\|D6]])** | L2, L3 in part → L3 to L4 | Sensitive Data Protection inside Model Armor at no extra charge; CMEK on Agent Platform datasets, models and feature stores; Workspace data regions[^armor][^dspm][^dataregions] | SDP integration GA; DSPM states no launch stage and shuts down 2027-02-01[^vpcsc][^dspm] | No answer-time entitlement control is documented, which is the load-bearing absence |
| **Observability ([[agentic-ai-security-cmm-d7-observability\|D7]])** | L2 → L3 | OpenTelemetry GenAI semantic conventions into Cloud Trace and Application Monitoring; Google SecOps; Agent Platform Threat Detection[^otel][^aptd] | OTel path states no launch stage; Agent Platform Threat Detection Preview[^otel][^aptd] | The threat detector reads host and control-plane compromise, and grades no agent behaviour |
| **Governance ([[agentic-ai-security-cmm-d1-governance\|D1]])** | L3 → L3 to L4 | Per-organizational-unit enablement in the Admin console; the AI control centre and its usage review[^gemou][^gemaicc] | Not graded — administrative console features | "Per agent type" has no referent when the agent population equals the headcount |
| **Supply chain ([[agentic-ai-security-cmm-d8-supply-chain\|D8]])** | L2 → L2 to L3 | Consumer-grade only: the vendor's model card is this shape's bill of materials; Assured Workloads bounds which products are in scope[^aw][^awprod] | Control packages documented; product membership per package[^awprod] | Producer-grade AI-BOM does not apply, and package membership decides what a regulated workload may use |
| **Operations ([[agentic-ai-security-cmm-d9-operations\|D9]])** | L2 to L3 → L3 | Google SecOps SIEM and SOAR for the runbook, both in scope for Canada Protected B; the Admin console usage review[^awprod][^gemaicc] | SecOps in the Protected B product list[^awprod] | The involvement measure D9 asks for is a vendor-side override rate the customer cannot read |

The first column carries the September scoring of this shape in [[cmm-stress-test-canadian-fi-google-2026-09|CMM Stress Test: Canadian FI on Google Cloud]], band for band, and this page re-scores nothing. That review also records that neither the CMM's shape table nor the reference architecture's carries a row for a productivity assistant, so the model sets no target of its own; the row the review proposes for the missing shape reads L3 across all nine domains with L4 in D6 and D9.

## The four controls the customer holds

[[google|Google]] documents four controls the customer holds, and the list ends there: per-organizational-unit enablement, data-loss-prevention rules bounding what the assistant reaches, an administrative control centre with a usage review, and a stated training restriction over customer data.[^gemou][^gemdlp][^gemaicc][^privacyhub] The vendor's own account describes four layers against indirect prompt injection — visible-content-only input, structural hierarchy in the prompt, deterministic orchestration sandboxing that restricts downstream capability by data origin, and output hardening that scrubs image tags and ungrounded links ([[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]]). Three of those four are unevidenceable from the customer's side, which holds D3, D4 and D5 at L1 to L2 for this shape. [[cmm-stress-test-canadian-fi-google-2026-09|The September stress test]] checked twelve control absences in the vault's own coverage of this shape and confirmed all twelve against a strong threat record, the sharpest being that Model Armor is documented against no Workspace traffic.

## Planes with no Google-native control

On three planes Google ships an instrument that stops short of what the plane requires, and on each of the three the shortfall is in the control rather than in its maturity. [[google-saif|Google SAIF: Secure AI Framework]] closes none of the three, because it names control categories without specifying controls, thresholds or test procedures.

### Answer-time entitlement enforcement

Google documents permission inheritance and one wholesale admin lever, and nothing between them. "Gemini has the same access to Workspace data as you do", and an administrator can restrict access to Gemini entirely or to some or all Workspace data.[^gemaccess] Two narrowings exist and are incidental to their own features: a Drive file whose owner blocks download, copy and print stays out of reach, and delegated Gmail access does not carry over.[^gemaccess] That page documents no control that narrows an answer at the time the answer is composed, which is the Data plane's load-bearing requirement and the control the Azure profile meets with Entra-authenticated per-user trimming, with Restricted SharePoint Search as its stopgap. [[oversharing-controls|Oversharing Controls for AI Search]] owns the failure mode, and the assistant surfaces it as [[inference-exposure|inference exposure]]: an answer composed from files the employee may open and never would.

Google Cloud DSPM does not close the gap. It lists supported asset types per control: BigQuery datasets and tables, Cloud Storage buckets, and Agent Platform models, datasets, feature stores and metadata stores, with Cloud Storage objects added at Preview. `Drive` does not appear on the page. The page states no launch stage of its own, three of its controls carry Preview markers, and the service shuts down on 2027-02-01.[^dspm] A buyer who needs answer-time narrowing over Workspace content buys it off-stack or accepts corpus permissions as the only control.

### Runtime screening of Workspace traffic

Model Armor names six supported integrations — Agent Gateway, Apigee, Gemini Enterprise, Google and Google Cloud MCP servers, Service Extensions, and Gemini Enterprise Agent Platform — and only the Gemini Enterprise integration screens documents, while every other integration scans text.[^integrations] Gmail, Google Docs, Google Drive and Google Workspace appear zero times on both the overview and the integrations page.[^armor][^integrations] An organization can therefore screen an agent it builds and cannot screen the assistant its whole workforce uses, which is the shape that carries the trifecta.

### Agent behavioral drift

Google's agent-observability guidance names drift as a risk — "AI agents can drift, hallucinate, and regress silently" — and documents no drift detector, anomaly rule or baseline to act on it.[^drift] Agent Platform Threat Detection detects the execution of malicious binaries or scripts, container escapes, reverse shells and attack tools in the agent's environment, plus control-plane activity from Event Threat Detection, and it carries a page-level Preview banner.[^aptd] Those detectors read host and control-plane compromise, and the page names data exfiltration attempts, excessive permission denials and suspicious token generation among the control-plane threats.[^aptd] None of them grades agent behaviour, so an agent talked into misusing the access it legitimately holds registers only where its actions surface as one of those events, and never as a departure from how that agent normally behaves.

## The agent decision point and the egress perimeter do not compose

IAM Unified Access Policies adjudicates agent-to-resource calls declaratively: policies bind to the project holding the Agent Gateway instances, each rule carries both an allow effect and a deny effect, conditions are written in CEL, a Principal Access Boundary can be set on the agent identity, and DRY_RUN logs disallowed agentic communications before ENFORCE blocks them.[^uap] The page opens with its own limit: "This feature does not support VPC Service Controls."[^uap] From the other side, Agent Gateway does not appear on the VPC Service Controls supported-products page, while Model Armor, Agent Runtime, Secure Web Proxy and Sensitive Data Protection each carry a GA integration there.[^vpcsc]

A single-stack buyer picks one. Adjudicating agent-to-resource calls at the gateway puts the decision point outside the service perimeter that protects the data plane around it; drawing the perimeter first leaves the agent-to-agent and agent-to-MCP paths to IAM allow and deny policies without the gateway's rule model. The RA's Control and Egress planes assume both controls run together, and on this platform they do not.

That choice exists only where the customer runs the agent. The Workspace assistant offers neither option, because the assistant, the tools it calls and the path between them all sit inside the vendor, so no perimeter, proxy or gateway the customer buys can take a position in that path. The closure condition this page answers asks where the platform ships no native control and an off-stack component is unavoidable, and on the Egress plane for this shape the answer runs the other way: [[cmm-stress-test-canadian-fi-google-2026-09|the September stress test]] holds D5 at L1 to L2 and records the domain among those the bank cannot remediate.

## The Canadian Agent Platform deployment spans two regions

### Service coverage across the two regions

A Canadian requirement reaches the Agent Platform half of this deployment and stops at the Workspace half. Workspace data regions offer the United States or Europe, so the whole-tenant assistant has no Canadian option at all, and what follows reads the agents an organization builds on Agent Runtime behind Agent Gateway.[^dataregions]

Google Cloud operates two Canadian regions, Montréal and Toronto, and three separate things are located per region without coinciding.[^agentloc] Each row below names the page that governs its claim, because Google's own pages disagree about what Toronto carries.

| What is located | Page that governs the claim | Montréal `northamerica-northeast1` | Toronto `northamerica-northeast2` |
|---|---|---|---|
| Gemini model serving | Deployments and endpoints[^locations] | Listed | Not listed |
| Agent runtime and Agent Gateway | Agent locations[^agentloc] | `v1` GA | `v1` GA |
| Model Armor | Feature availability by region[^featreg] | Not listed | Listed, limited support |

So the guardrail sits in Toronto beside the agent runtime it screens, and the model answering that agent is served from Montréal or from outside Canada. The split is a property of the platform's coverage rather than a misconfiguration a buyer can tidy away, and a Canadian Agent Platform deployment holds two regions from the first day.

Seven of the twenty-seven Google-model rows on the residency table carry a `Canada (northamerica-northeast1)` commitment: Gemini 3.5 Flash, Gemini 2.5 Flash at 128k and 1M, Gemini 2.5 Pro at 64k and 1M, and the two text-embedding models. No 3.x flagship above 3.5 Flash carries one, and neither does any tuning row.[^residency] Partner models sit on a separate table with no Canada column at all, so the coding shape described in [[securing-agentic-coding|Securing Agentic Coding]] has no Canadian residency option to configure.[^residency] Where ML processing happens follows the endpoint the caller uses,[^residency] and the locations page states that endpoints guarantee neither data residency nor in-region ML processing.[^locations]

### The guardrail trade

Toronto is a limited-support Model Armor region, and the reduction is in filters rather than in residency. A template with data-residency compliance enabled keeps the responsible-AI filters, Sensitive Data Protection, and [[prompt-injection|prompt injection]] and jailbreak detection, and loses malicious URL detection, multi-language detection, CSAM support, image support and antivirus scanning, "because they rely on services that might process data outside the jurisdiction of your chosen Model Armor region".[^featreg] That template's residency row reads `Yes / Yes / Yes` for data at rest, in use and in transit.[^maresidency]

Floor settings on the Agent Platform integration restore every filter and weaken the residency guarantee: Canada's floor-settings row reads `Yes / No / No`, and the page defines that `No` as "the data might be processed or transmitted outside of the jurisdiction".[^maresidency] At-rest residency survives either choice, so the trade is over processing and transit.

**Malicious URL detection is the filter a residency-enforced Toronto template gives up over the exfiltration route this shape's recorded attacks used.** [[geminijack-gemini-enterprise-injection|GeminiJack]] exfiltrated mail, calendar and document content through an auto-loading `<img>` tag in the rendered answer, and [[echoleak-copilot-zero-click|EchoLeak]] carried data out of another vendor's assistant by routing an image request through an allowlisted first-party link-preview endpoint. Each exfiltration leg is an outbound fetch the client performs as it renders markup, and malicious URL detection is the Model Armor filter that reads the URLs in a response. Model Armor's image screening does not sit on that route. It screens images supplied in prompts and responses by visual scanning and optical character recognition, so it inspects what an image carries and never a URL the client fetches.[^armor] Google's own pages then disagree about Toronto: the feature-availability table lists image support among the features a residency-enforced Toronto template loses, while the overview states that image screening is supported in the `us` and `eu` multi-regions only, which would leave that template no image screening to lose.[^featreg][^armor] EchoLeak also bounds the filter that does apply, because its egress URL belonged to the vendor's own allowlisted service and a reputation check clears such a URL.

Workspace output hardening sits on that route, and it is not Model Armor. The vendor's account of it names markdown scrubbing of image tags and link protocols, Safe Browsing filtering of every URL, and removal of ungrounded links the model produced without a source in its input ([[securing-workspace-genai-at-google-talk|Securing Workspace GenAI at Google]]). No region gates that layer and no customer configures it: [[cmm-stress-test-canadian-fi-google-2026-09|the September stress test]] grades D4 for this shape on the vendor's four-layer architecture, sourced to a conference talk. A Canadian institution that enforces residency on the guardrail therefore gives up the response-side URL filter over the route [[indirect-prompt-injection|indirect prompt injection]] against this shape has used, and what still covers that route is a vendor-side layer the institution cannot evidence. Floor settings restore the filter and accept processing outside the jurisdiction. Both horns are configurations, and the choice belongs in the third-party-risk file the [[agentic-ai-security-cmm-crosswalk-canada-fi|Canadian regulated-finance crosswalk]] maps to OSFI Guideline B-10.

### Assured Workloads coverage

Four Canadian control packages exist. Canada Data Boundary is free and Canada Data Boundary and Support, Data Boundary for Canada Protected B, and Data Boundary for Canada Controlled Goods Program are premium; each sets data-location controls for Canada-only regions, and the three premium packages add a support-access control: Canada Data Boundary and Support limits first-level and second-level support access to personnel legally eligible to work in Canada and physically located in Canada, and the Protected B and Controlled Goods packages require Canadian support personnel who have completed the matching screening.[^aw] Model Armor is in scope for Canada Data Boundary and for Canada Data Boundary and Support, which carry 112 products each, and out of scope for Data Boundary for Canada Protected B, which carries 109.[^awprod] A Protected B workload therefore runs with no Assured Workloads coverage for the runtime-guardrail plane, while Google SecOps SIEM and SOAR, Cloud Logging, Apigee and Sensitive Data Protection remain in scope for it.[^awprod]

## Cost signal

Model Armor's screening is priced per token. A Security Command Center Enterprise or Premium subscription includes 3 billion tokens per month at no cost, project-level and organization-level Premium activation and standalone use include 2 million, and further screening costs \$0.10 per million tokens on the same four-characters-per-token definition the Agent Platform uses; Sensitive Data Protection inside Model Armor adds no charge.[^sccprice] A Security Command Center subscription costs a minimum of \$15,000 a year at Premium and at Enterprise.[^sccprice] The Enterprise tier shuts down on 2027-05-21, and organizations on it move to Premium.[^scctiers]

The Data plane carries the labor. With no answer-time narrowing control documented, an answer is bounded by the permissions on the reachable corpus, which makes Drive and Gmail permission remediation a standing program rather than a purchase.

## Status watch

- **Model Armor states no launch stage for its core screening service.** Google announces GA for named features and integrations — streaming sanitization for text on 2026-07-10, Model Armor on Agent Gateway on 2026-06-24, the Agent Platform integration on 2025-12-03 — and the VPC Service Controls table's `GA` scopes to "this product integration".[^armorrn][^vpcsc] Image screening carries a Preview banner.[^armor] A buyer states a support requirement against the integration it uses.
- **Two withdrawal dates sit inside a normal procurement cycle.** Google Cloud DSPM shuts down on 2027-02-01 and the Security Command Center Enterprise tier on 2027-05-21.[^dspm][^scctiers] A Canadian institution running a 12-to-18-month procurement cycle chooses a product that outlives the choice.
- **Workspace data regions offer the United States or Europe.** Gemini prompts and responses are covered data at rest and during processing, the processing half varies by edition, and Canada is not one of the locations offered.[^dataregions] Retention for Workspace prompts and responses runs 90 days to indefinite as determined by admins.[^privacyhub]
- **Agent Platform Threat Detection carries a page-level Preview banner**, and a buyer who needs runtime detection against a deployed agent runs a Preview service to get it.[^aptd]

## Notes

[^agentid]: [Google Cloud — Agent Identity overview](https://docs.cloud.google.com/iam/docs/agent-identity-overview), fetched 2026-09-16. SPIFFE-based per-agent identity with 24-hour X.509 certificates and certificate-bound access tokens; the page states no launch stage.
[^uap]: [Google Cloud — IAM Access policies overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/policies/iam-overview-uap), fetched 2026-09-16. Declarative agent-action authorization enforced at Agent Gateway through Identity-Aware Proxy; the page's opening note states "This feature does not support VPC Service Controls" and no launch stage.
[^armor]: [Google Cloud — Model Armor overview](https://docs.cloud.google.com/model-armor/overview), fetched 2026-09-16. Filter categories, enforcement modes and the Private Service Connect requirement; states a launch stage for image screening (Preview) and none for the core screening service. On image screening: "Model Armor screens images provided in the prompts and responses", by "Visual scanning" and "Optical character recognition (OCR)", and "Image screening is supported only in the us and eu multi-regions."
[^armorrn]: [Google Cloud — Model Armor release notes](https://docs.cloud.google.com/model-armor/release-notes), fetched 2026-09-16. Dated entries announcing GA for individual Model Armor features and integrations, with no entry for the service itself.
[^integrations]: [Google Cloud — Integrate Model Armor with services or frameworks](https://docs.cloud.google.com/model-armor/integrations), fetched 2026-09-16. Names the six supported integrations and records that only the Gemini Enterprise integration screens documents.
[^grounding]: [Google Cloud — Check grounding with RAG](https://docs.cloud.google.com/generative-ai-app-builder/docs/check-grounding), fetched 2026-09-16. Returns a 0-to-1 support score with claim-level citations for filtering at inference time; the page states no launch stage.
[^vpcsc]: [Google Cloud — VPC Service Controls supported products](https://docs.cloud.google.com/vpc-service-controls/docs/supported-products), fetched 2026-09-16. GA integration status for Model Armor, Agent Runtime, Secure Web Proxy and Sensitive Data Protection; Agent Gateway does not appear on the page.
[^dspm]: [Google Cloud — Data Security Posture Management overview](https://docs.cloud.google.com/security-command-center/docs/dspm-data-security), fetched 2026-09-16. Google-Cloud-scoped asset types, three Preview-stage controls, no page-level launch stage, and shutdown on 2027-02-01.
[^aptd]: [Google Cloud — Agent Platform Threat Detection overview](https://docs.cloud.google.com/security-command-center/docs/agent-platform-threat-detection-overview), fetched 2026-09-16. Page-level Preview banner; detects malicious binaries, container escapes, reverse shells, attack tools and control-plane activity.
[^otel]: [Google Cloud — Observability for AI agent developers](https://docs.cloud.google.com/stackdriver/docs/instrumentation/ai-agent-overview), fetched 2026-09-16. Cloud Trace extracts events from generative-AI spans that conform to the OpenTelemetry semantic conventions; the page states no launch stage.
[^drift]: [Google Cloud — Agent observability](https://docs.cloud.google.com/stackdriver/docs/observability/agent-observability), fetched 2026-09-16. Names agent drift as a risk and documents no drift detector, anomaly rule or baseline.
[^locations]: [Google Cloud — Gemini Enterprise Agent Platform deployments and endpoints](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/locations), fetched 2026-09-16. Model-serving locations, listing Montréal and not Toronto, and stating that endpoints guarantee neither data residency nor in-region ML processing.
[^agentloc]: [Google Cloud — Agent locations](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/agent-locations), fetched 2026-09-16. Both Canadian regions carry "v1 is supported for GA features. v1beta1 is supported for Preview features."
[^featreg]: [Google Cloud — Model Armor feature availability for templates by region](https://docs.cloud.google.com/model-armor/feature-availability-by-region), fetched 2026-09-16. Lists Toronto as a limited-support region and names the filters a residency-enforced template keeps and loses.
[^maresidency]: [Google Cloud — Model Armor data residency and endpoints](https://docs.cloud.google.com/model-armor/data-residency), fetched 2026-09-16. Canada's template row reads `Yes / Yes / Yes` and its floor-settings row for the Agent Platform integration reads `Yes / No / No`, with the page defining that `No`.
[^residency]: [Google Cloud — Gemini Enterprise Agent Platform data residency](https://docs.cloud.google.com/gemini-enterprise-agent-platform/resources/data-residency), fetched 2026-09-16. Per-model ML-processing table carrying one Canadian column, `Canada (northamerica-northeast1)`, and a separate partner-model table with no Canada column.
[^aw]: [Google Cloud — Assured Workloads control packages](https://docs.cloud.google.com/assured-workloads/docs/control-packages), fetched 2026-09-16. The four Canadian control packages, their tiers and the personnel screening each applies.
[^awprod]: [Google Cloud — Assured Workloads supported products](https://docs.cloud.google.com/assured-workloads/docs/supported-products), fetched 2026-09-16. Product membership per package: 112 products for Canada Data Boundary, 112 for Canada Data Boundary and Support, 109 for Data Boundary for Canada Protected B, with Model Armor absent from the last.
[^sccprice]: [Google Cloud — Security Command Center pricing](https://docs.cloud.google.com/security-command-center/pricing), fetched 2026-09-16. Model Armor token allocations and the \$0.10 per million rate, and the \$15,000 minimum annual subscription at Premium and Enterprise. The page carries no shutdown date.
[^scctiers]: [Google Cloud — Security Command Center service tiers](https://docs.cloud.google.com/security-command-center/docs/service-tiers), fetched 2026-09-16. Carries the banner stating that the Enterprise service tier shuts down on 2027-05-21 and that organizations using it move automatically to Premium; the same banner appears on the Data Security Posture Management overview.
[^dataregions]: [Google Workspace — Data covered by data regions](https://support.google.com/a/answer/9223653), fetched 2026-09-16. Gemini prompts and responses covered at rest and during processing, the processing column gated by edition, and the United States or Europe as the only locations.
[^gemaccess]: [Google Workspace — What controls Gemini's access to Workspace data](https://support.google.com/a/users/answer/17010577), fetched 2026-09-16. Permission inheritance, the administrator's wholesale lever, and the Drive-download and delegated-Gmail narrowings.
[^privacyhub]: [Google Workspace — Generative AI privacy hub](https://support.google.com/a/answer/15706919), fetched 2026-09-16. Training-use commitments with their qualifiers, and retention of 90 days to indefinite for Workspace prompts and responses.
[^gemou]: [Google Workspace — Manage access to Gemini features in Workspace services](https://support.google.com/a/answer/15698295), fetched 2026-09-15. Per-organizational-unit enablement of Gemini features.
[^gemdlp]: [Google Workspace — About DLP for Gemini](https://knowledge.workspace.google.com/admin/security/about-dlp-for-gemini), fetched 2026-09-15. Data-loss-prevention rules bounding the data sources the assistant reaches.
[^gemaicc]: [Google Workspace — Explore the AI control center](https://knowledge.workspace.google.com/admin/gemini/explore-the-ai-control-center), fetched 2026-09-15. The administrative control centre and its usage review.
