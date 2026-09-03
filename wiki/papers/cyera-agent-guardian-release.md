---
type: paper
title: "Cyera Agent Guardian Release"
address: c-000328
created: 2026-08-31
updated: 2026-08-31
tags:
  - papers
  - dspm
  - guardian-agent
  - agent-security
  - cyera
status: summarized
scope_axis: [sec-of-ai, ai-in-sec-defense]
origin: aggregated
venue: "Cyera blog"
source_url: "https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store"
archived_copy: ".raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md"
key_claim: "Cyera frames agentic AI security as a data security problem first, on the premise that an agent inherits the full entitlement set of the human who launched it and can exercise all of it from its first task, and packages Agent Guardian's four-phase lifecycle (Discover, Govern, Protect, Validate) with endpoint coverage, an incident-materiality service, and expanded data-store integrations around that premise."
related:
  - "[[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]]"
  - "[[agentic-ai-security-cmm-2026|Agentic AI Security CMM 2026]]"
  - "[[cyera|Cyera]]"
  - "[[guardian-agent|Guardian Agent]]"
  - "[[guardian-agents-market-guide|Guardian Agents Market Guide]]"
  - "[[oversharing-controls|Oversharing Controls for AI Search]]"
  - "[[shadow-ai|Shadow AI]]"
  - "[[varonis|Varonis]]"
  - "[[non-human-identity|Non-Human Identity (NHI)]]"
  - "[[agent-catalog|AI Agent Catalog]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2|Standards Review — Microsoft RAI / Agent 365 (2026-Q2)]]"
sources:
  - "https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store"
  - "[[.raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md]]"
---

# Cyera Agent Guardian Release

**Source:** [Cyera blog — "New from Cyera: AI Security for Every Agent, Assistant, and Data Store"](https://www.cyera.com/blog/new-from-cyera-ai-security-for-every-agent-assistant-and-data-store). Local copy: `.raw/articles/cyera-ai-security-every-agent-assistant-data-store-2026-08-31.md`. The page carries no publication date and no author byline.

## Key claim

Cyera, a data security posture management vendor, states that every business already runs agents, sanctioned or not, and that each one inherits the access of the person who launched it the moment it goes live. Cyera states that this combination makes agentic AI security a data security challenge first: "The risk is not just that an agent exists; it is what that agent can reach, what it can do with the data it finds, and whether anyone can prove what happened after it acts."

Cyera argues the point by comparing an agent with a human hire. Cyera states: "A new hire typically grows into permissions over time and may never touch most of what they’re technically allowed to see. An agent is different. On day one, it can inherit broad entitlements and use them immediately whenever a task requires it." The comparison turns on onset, meaning how soon a holder exercises access it already holds, and Cyera publishes no measurement of either side. Onset joins scale, ephemerality, and autonomy as an amplifier of the [[non-human-identity|non-human identity]] problem.

## Agent Guardian

Cyera states that [Agent Guardian](https://www.cyera.com/blog/introducing-cyera-agent-guardian-secure-everything-ai-can-see-and-do) "makes every agent as visible and accountable as a human employee, with decisions grounded in data sensitivity and identity context." Cyera states the product combines automated data classification, identity access rights, behavioral baselines, and runtime analysis to evaluate both how an agent is designed and what it attempts to do, and secures the agent lifecycle in four named phases:

- **Discover.** Cyera states this phase builds a live inventory of every agent across cloud platforms, SaaS tools, endpoints, and Shadow AI, and maps the models, tool access, and data connections behind each one.
- **Govern.** Cyera states this phase watches for posture issues and intent drifts as they form, naming the combination of broad access, high autonomy, and privileged identity as what turns an ordinary agent into a risky one.
- **Protect.** Cyera states this phase acts during execution: it blocks a risky tool call, or strips sensitive fields out of a response before the response reaches somewhere it should not.
- **Validate.** Cyera states this phase runs continuous adversarial red teaming against prompt injection and jailbreak attempts, producing audit-ready evidence for regulatory frameworks.

### Cyera Endpoint

Cyera states that [Cyera Endpoint](https://www.cyera.com/blog/building-the-control-plane-to-secure-agentic-endpoints) extends Agent Guardian to employee laptops and covers sanctioned and unsanctioned agents alike. Cyera states the endpoint agent shows which agents are running, what data they can reach, and how they use local tools, naming `bash`, `run-code`, and MCP calls as the local tool classes it watches. Cyera names Claude Code, Cowork and ChatGPT Desktop among the desktop targets it covers, and writes the list as open.

## Additional release components

Three parts of the release extend Cyera's existing platform rather than the agent lifecycle directly. The source gives them less emphasis than Agent Guardian and Cyera Endpoint, and this page keeps that same weighting.

### Incident Materiality Assessment

Cyera's [Incident Materiality Assessment](https://www.cyera.com/blog/from-alert-to-insight-introducing-cyeras-incident-materiality-assessment) reached general availability in this release. Cyera states the service pairs targeted data scanning and enriched classification with expert-led analysis to produce a defensible inventory of exposed sensitive data by class, severity and materiality context for a disclosure decision, a timeline of who accessed what and when, and a report scoped for legal, board, and regulatory readers. Cyera frames the outcome as a move from a generic acknowledgment that a breach occurred to a defensible account of impact, stating this happens "in days instead of weeks" — a vendor comparative with neither baseline disclosed.

### Cy and Projects

Cyera states that Cy, its data and AI security assistant, expands across more of the platform in this release: it now answers DLP questions and returns project-scoped responses. Cyera adds Saved Prompts, which turn an investigation session into a reusable, parameterized workflow, and suggested next steps that continue an investigation after Cy returns an answer.

Cyera states Projects gives a single tenant scoped views for delegated teams, letting an administrator segment data access by assigning project users to designated datastores and alerts, focus triage so a project user sees only the alerts within their own responsibility, control identity visibility by choosing whether project users see every identity in the organization or only those touching their project's datastore scope, and scope API access so a project user's personal API token inherits the project's own scope and role permissions. A user assigned to a project sees only that slice of the tenant; a global user keeps organization-wide visibility.

### Access Trail, on-premises coverage, and integrations

Cyera extends Access Trail, its audit-event feed, to Microsoft Exchange Online mailboxes and NetApp, covering message access, attachment access, send activity, deletions, inbox rules, folder permissions, and delegated access; Cyera states the motivation is that Microsoft Copilot and other assistants already reach into Outlook and file shares. Cyera states on-premises database coverage grows to MySQL, MariaDB, clustered Oracle RAC, MSSQL 2025 on Windows or Linux, and Db2 for i on IBM Power Systems, justified by agents querying on-premises databases directly. Seven integrations carry Cyera's data context downstream: Microsoft Agent 365 (Cyera's risk signals feed the Agent Registry, showing each agent's permissions, tools, and access paths to sensitive data), Akamai (Guardicore VM inventory enriched with Cyera classification for micro-segmentation), Swimlane (Cyera states it "turns Cyera findings into playbooks that remediate exposed data, enforce policy, and close the loop with an audit trail"), Eve Security (classification labels enforce real-time agent guardrails), Nagomi (exposure investigations prioritized against Cyera classification), Panorays (third-party risk grounded in Cyera's discovered data footprint), and Native Security (cloud infrastructure controls across AWS, Azure, Google Cloud, and OCI).

## Tool-call-monitoring critique

Cyera positions Agent Guardian against tools it describes this way: "Most agent security solutions monitor prompts, outputs, or individual tool calls. They can see isolated actions, but they often miss the deeper context: what data an agent can access, whose permissions it inherits, and how that access could be used." The release's closing section is headed "AI security that reaches everywhere your data does", which states the coverage claim in the same terms.

That preference runs against a ranking [[agentic-ai-security-reference-architecture|the AAI-S reference architecture]] records and builds on. The RA carries the [[owasp-ai-exchange|OWASP AI Exchange]]'s ranking of three detection layers, which puts execution-level detection — "observing the tool calls and side effects an agent actually produces" — above text-level and model-level detection for reliability. Cyera treats individual-tool-call monitoring as insufficient; the Exchange ranks it the most reliable of the three layers it names, and the RA reads that ranking as agreeing with its own first design principle. The two positions disagree on the same question, and this page records the disagreement rather than resolving it.

## Strengths and weaknesses

The release states a coherent architectural position: an agent's risk is a function of data reach and inherited entitlement. Agent Guardian's four phases, Cyera Endpoint, Access Trail and the on-premises coverage each scope to some part of that data-and-identity surface. Cyera Endpoint also names Claude Code, Cowork and ChatGPT Desktop among the desktop harnesses it covers.

The release carries no third-party evaluation of any claim, no benchmark, no detection rate or false-positive rate, no coverage percentage, no customer count, and no pricing. It gives no technical detail on how discovery, blocking, field-stripping, or red teaming work, names no incident or CVE any component responded to, and leaves "intent drifts" undefined beyond the phrase itself. The phrase "in days instead of weeks" is the release's only comparative claim near a number, and it evidences neither side of the comparison.

## Mapping to the AAI-S RA and CMM

Cyera names neither the [[agentic-ai-security-reference-architecture|AAI-S reference architecture]] nor the [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]]. Each placement below reads a stated capability against a plane scope sentence and a ladder criterion. The table covers Cyera's own products and the two integrations it places on the wiki's artifacts, Swimlane alongside Cy and Projects and Microsoft Agent 365. Akamai and Eve Security are disposed of under Coverage shape, where the D5 finding needs them. Nagomi, Panorays and Native Security carry no placement on either artifact, and the table grades none of them individually. A vendor capability is an example of a control a ladder already names, and it is never evidence for a rung criterion; the release carries no efficacy measurement, so no placement here moves a grade.

### Capability placement

| Capability (as the source states it) | RA plane | CMM domain |
|---|---|---|
| **Discover** — a live inventory of every agent across cloud platforms, SaaS tools, endpoints and Shadow AI, mapping models, tool access and data connections | Identity; Observability | [[agentic-ai-security-cmm-d1-governance\|D1]], [[agentic-ai-security-cmm-d2-identity\|D2]]; [[agentic-ai-security-cmm-d7-observability\|D7]] for the map surface |
| **Govern** — watching for posture issues and intent drifts, including broad access combined with high autonomy and privileged identity | Observability | D7; D2 and [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] supply the objects watched |
| **Protect** — blocking a risky tool call during execution | Runtime | [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] |
| **Protect** — stripping sensitive fields out of a response | Data; Runtime | [[agentic-ai-security-cmm-d6-data-rag\|D6]]; D4 |
| **Validate** — continuous adversarial red teaming, producing audit-ready regulatory evidence | Observability | D7; D1 for the evidence artifact |
| **Cyera Endpoint** — sanctioned and unsanctioned agents on employee laptops, and their local `bash`, `run-code` and MCP calls | Identity; Observability; Runtime | D1, D2, D7; D4 |
| **Access Trail** — Exchange Online and NetApp events: message and attachment access, sends, deletions, inbox rules, folder permissions, delegated access | Observability; Data | D7; D6 |
| **Incident Materiality Assessment** — exposed records by data class, an access timeline, materiality for a disclosure decision, a report for legal, board and regulators | Data; Observability | [[agentic-ai-security-cmm-d9-operations\|D9]]; D6, D1 |
| **On-premises database coverage** — classification extended to MySQL, MariaDB, clustered Oracle RAC, MSSQL 2025 and Db2 for i | Data | D6 |
| **Cy, Projects, and the Swimlane integration** — an investigation assistant, tenant scoping, and findings turned into remediation playbooks | No agent-security plane | D9 |
| **Microsoft Agent 365 integration** — Cyera data context and risk signals in the Agent Registry, showing each agent's permissions, tools and access paths to sensitive data | Identity; Data | D2; D7, D6 |

Two rows depart from the plane a reader would infer from the capability's name.

**Govern sits on the Observability plane.** The Control plane "adjudicates policy and issues capability tokens **before** an agent's tool call reaches the runtime", and Cyera states that Govern watches for the broad-access, high-autonomy, privileged-identity combination as it forms. The Observability plane's AI-SPM and agent-behavioral-monitoring rows cover that detection, and Cyera's own input list for Agent Guardian names behavioral baselines.

**Response field-stripping sits on the Data plane.** The Egress plane "mediates an agent's reach to tools, MCP servers, peer agents, and the open internet" and carries no content-redaction control. Stripping sensitive fields from a response matches the Data plane's answer-time entitlement enforcement row and D6's L4 criterion, "label-aware DLP gating of responses". The other half of the Protect phase, blocking the tool call, is the Runtime enforcement act.

### Coverage shape

The release covers the data domains densely and thins out across agency, egress and supply chain. Its weight sits at D6; it reaches D4, D7 and D9 with named capabilities, adds an inventory input and evidence artifacts at D1 and D2, carries a detection signal at D3, and reaches neither D5 nor D8.

The release concentrates at D6. Classification, on-premises store coverage and response label-gating are examples of controls the L3 and L4 criteria already state, and Cyera states the same classification runs under the Incident Materiality Assessment.

At D7 the release supplies behavioral baselines, posture and drift watching, and a continuous red-team cadence, all examples of controls the L4 rung names. The release reaches no part of the L3 span criterion, which requires "OpenTelemetry `gen_ai.*` spans from every agent". Access Trail audits data-store events, so it supplies the data-store half of a reconstruction and no agent telemetry.

At D1 and D2 the release supplies an inventory input and evidence artifacts, and nothing beyond that. Every D1 L3 criterion "yields a document rather than a product", so a discovery product feeds the shadow-agent-inventory clause and satisfies no criterion on its own. The release also states no reaper or decommission action, which is the other half of that clause. At D2 the release touches the L5 shadow-agent-discovery clause and describes the human-delegated model L2 already grades, and it leaves the L3–L4 spine alone: attested per-agent identity, deploy-pipeline lifecycle, automated rotation, zero credentials in agent context, and per-hop delegation binding.

The Incident Materiality Assessment reaches D9 as the data-impact half of incident response. Cyera calls it expert-led analysis, so the assessment arrives as expert work and the domain's product landscape is unchanged. The domain's load-bearing center stays untouched: HITL-fatigue measurement, bus-factor continuity, runbook authoring and drilling, and decommission cadence.

[[agentic-ai-security-cmm-d5-egress-network|D5]] receives nothing that meets its rungs. Its L3 rung requires an in-path gateway: "An agent-aware gateway sits in-path between agent and external tools, LLMs, and MCP servers, carrying per-tool authorization with token governance and inline content safety." The release states no gateway, no MCP brokering and no inter-agent enforcement profile. Two integrations touch the plane and neither meets the rung. Akamai drives Guardicore VM micro-segmentation from Cyera's data classification, where the D5 tooling map grades per-agent micro-segmentation keyed on agent identity. Eve Security is stated to use Cyera's classification labels to "enforce real-time guardrails for AI agents, preventing them from reaching sensitive data before it's exposed", which is a claim about an agent's reach; the release describes it as a partner enforcing on Cyera's labels, and states no gateway position, no per-tool authorization and no token governance. D5 L3 grades those three. The release's single MCP mention is observational: Cyera Endpoint shows "how they’re using local tools like bash, run-code, and MCP calls".

### Supply-chain absence

[[agentic-ai-security-cmm-d8-supply-chain|D8]] receives nothing. The release states no model provenance, no AI-BOM, no artifact signing, no integrity verification at load, and no component-drift detection. The release's integration ecosystem is a partner roster.

Discover comes closest, and it stops five fields short. Cyera states the phase maps "the models, tool access, and data connections behind each one", and Cyera Endpoint reports how agents use "local tools like bash, run-code, and MCP calls". Models and MCP servers are two of the object classes D8's L2 rung enumerates, and that rung requires every entry to carry source, version, hash, maintainer and date. The release claims none of the five. [[standards-review-microsoft-rai-agent-365-2026-Q2|The 2026-Q2 review of Microsoft Agent 365]] settles the same distinction against the same rung: Agent 365's `observe` layer "inventories and visualizes but does not verify supply-chain integrity (AI-BOM)". A map of which models an agent uses populates an Identity-plane agent card and evidences no D8 criterion.

### Agency scoping

D3 receives a risk signal and an unspecified block decision, and no agency-scoping mechanism.

High autonomy appears in the release as a property to detect. Govern "watches for posture issues and intent drifts as they form, including the combinations of broad access, high autonomy, and privileged identity that turn an ordinary agent into a risky one." The verb is *watches*, and the output is a posture finding.

Protect's block is an enforcement act, which implies a decision behind it. The CMM assigns the decision to D3 and the enforcement to D4, whose deep dive states that D4 "is the Policy Enforcement Point at runtime: it enforces what D3 decides." The release describes the enforcement and describes no decision function: no policy language, no deny-by-default posture, no fail-closed behavior, and no action-risk tier. D3's L3 rung is graded off the deployed configuration of the policy decision point, and this release supplies none.

Nothing in the release reduces an agent's authority. D3's L4 rung requires the policy engine to decide on "an agent's current task scope, its session's cumulative activity, its position in a delegation chain"; the release names no permission reduction, no task-bounded credential and no capability attenuation.

### Plane precedence

Cyera argues that identity and data context should lead, because "access is usually invisible until it becomes a problem." The RA already holds the identity half of that position and reaches it from a different premise: "Per-agent identity is the prerequisite for per-agent egress and observability (the D2→D5 and D2→D7 caps), so build the Identity plane before investing in Egress or Observability." The RA argues from the dependency caps between domains, Cyera from data sensitivity and inherited entitlement. Two arguments reaching the same build order from different premises corroborate each other, and the sequencing rule stands as written.

The data half is Cyera's own. No RA text orders the Data plane against Runtime, and the RA's one statement of Data-plane precedence sits inside the plane: answer-time entitlement enforcement is the load-bearing control for a closed-corpus bot, ahead of corpus attestation. A vendor release carrying no benchmark does not license a new plane-ordering claim in the reference architecture.

Cyera's third argument, that watching individual tool calls misses the deeper context, disagrees with the OWASP AI Exchange ranking of detection layers that the RA carries; the Tool-call-monitoring critique above sets out both sides.

## Relations

- **Disagrees with** the [[owasp-ai-exchange|OWASP AI Exchange]] ranking that [[agentic-ai-security-reference-architecture|the AAI-S reference architecture]] carries, on the reliability of execution-level (tool-call) detection — see Tool-call-monitoring critique above.
- **Vendor profile:** [[cyera|Cyera]].
