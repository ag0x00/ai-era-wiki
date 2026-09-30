---
type: maturity-model
title: "CMM D1: Governance and Accountability"
address: c-000136
created: 2026-05-24
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - governance
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[aiuc-1-critical-evaluation]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[decision-rights]]"
  - "[[shadow-automation]]"
  - "[[owasp-state-of-agentic-ai-security-governance]]"
  - "[[owasp-ai-exchange]]"
  - "[[microsoft-zt4ai]]"
  - "[[microsoft-rai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[standards-review-microsoft-rai-agent-365-2026-Q2]]"
  - "[[standards-review-eu-ai-act-2026-Q2]]"
  - "[[standards-review-saif-cosai-2026-Q2]]"
  - "[[standards-review-iso-42001-27090-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[securing-agentic-coding]]"
  - "[[microsoft-cli-coding-agent-adoption-study]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[claude-cowork]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[aiuc-1]]"
  - "[[iso-iec-42001]]"
  - "[[owasp-aivss]]"
  - "[[red-teaming-capability-framework]]"
  - "[[claude-code-control-sheet]]"
  - "[[cmm-known-limitations]]"
sources:
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[aiuc-1-critical-evaluation]]"
  - "[[iso-iec-42001]]"
  - "[[nist-ai-rmf]]"
verified: 2026-09-29
verified_against:
  - ".raw/papers/owasp-ai-exchange-general-controls-2026-08-19.md"
verified_findings: 0
verified_note: "Read the archived OWASP AI Exchange general controls, current NIST and AIUC assurance references, and prior CMM review; repaired third-party assurance scope and certification link."
---

# Agentic AI Security CMM — D1 Governance and Accountability

## Domain decision and boundary

D1 determines whether a named authority governs each agent deployment, decides its risk, and keeps an auditable record of the decision. Its organization criteria apply to every deployment in the assessment scope through the same policy, risk body, and reporting chain. Deployment criteria require evidence for the particular agents assessed. One shared governance program can therefore support many deployments, but one deployment's approval cannot substitute for another's. [[agentic-ai-security-cmm-2026|The CMM]] defines the five cumulative levels; [[agentic-ai-security-cmm-measurement-protocol|the measurement protocol]] records each determination and its evidence.

An agent is one deployed configuration of instructions, tools, and grants. Replicas of that configuration count as one agent. Materially different configurations count separately. The AI register describes the deployment and its accountable person. [[agentic-ai-security-cmm-d2-identity|D2]] inventories the agents and identities. [[agentic-ai-security-cmm-d8-supply-chain|D8]] inventories components and owns coding-harness configuration assurance. D1's approval records do not establish that tool calls are correctly mediated, which [[agentic-ai-security-cmm-d3-control-least-agency|D3]] assesses. A vendor-operated component remains inside the deployment's responsibility boundary. The organization may rely on scoped supplier evidence, yet its own approval, allocation, and residual-risk decisions still need records.

D1's technical-detail review records the security disclosure decision before the organization publishes or authorizes publication. [[agentic-ai-security-cmm-d9-operations|D9]] assesses what a user-facing notice covers and whether the required properties were addressed. One reviewed publication may provide evidence for both domains when it contains both records. The governance approval still belongs to D1, while D8 determines whether the assembled agent passed its pre-release security checks. The records have different owners and failure tests even when the same release packet holds them.

Three terms fix the scope of the criteria:

- A **supplying party** is an external party under the organization's agreement or an internal team that supplies a deployment component, including data or a model. The organization records its own remaining duty even when a supplier performs the control.
- **Technical detail** covers the model type and implementation information the organization holds, plus deployment descriptions it publishes or authorizes another party to publish. An undisclosed vendor implementation creates no inventory entry for information the organization does not possess.
- A **board attestation** is an on-record statement by a director or authorized board committee that a report is complete and accurate. Minutes that merely record receipt do not attest it.

## Failure paths

An unregistered agent can escape the risk tier and production gate. A production approval without a named accountable person leaves conditions and residual risk ownerless. A responsibility matrix that says only “vendor” can hide gaps between what the supplier promises and what the organization must configure. Technical disclosures can expose implementation detail or omit material properties without a recorded review. These paths explain why D1 separately grades the register, the authority to approve, the decision, supplier allocation, disclosure review, and follow-through. The [OWASP AI Exchange governance program](https://owaspai.org/go/aiprogram/) provides an external control anchor; the level boundaries and acceptance tests below are this model's synthesis.

## L1–L5 progression

| Level | Observable governance outcome |
|---|---|
| L1 — Initial | People make local decisions about agents, with incomplete organization records. |
| L2 — Developing | An accountable role, AI policy, risk-tier rule, RACI, and deployment register establish a decision owner and scope. |
| L3 — Defined | A standing risk body, tiered production gate, boundary and supplier decisions, disclosure review, and shadow-agent handling operate on records. |
| L4 — Managed | The board receives substantive measures; the organization maintains its requirements crosswalk and independently tests readiness for assurance. |
| L5 — Optimizing | Current third-party assurance covers the deployment, while the body, crosswalk, and board attestations show sustained operation. |

A level is reached only when its applicable criteria and the lower levels' applicable criteria are met. A missing artifact is a finding; it is not permission to infer that a control works. The handbook defines the determination for a truly absent control instance and for a vendor-held instance the assessor cannot inspect. No aggregate score or universal target follows from this table.

## Criterion catalogue

The bold identifier is the canonical criterion definition. Each entry gives a pass condition, a failure discriminator, and minimum evidence. A supplier's certificate for its own service does not by itself assure the organization's governance program.

### L2 detail

- **D1-ACCOUNTABLE.** *Organization.* A document the organization approved, such as its AI policy or a charter, names one role as accountable for the organization's AI governance, and a person holds that role on the assessment date. A document that names a committee, or several roles jointly, fails the criterion. *Evidence:* the document naming the role, with the personnel record of the person who holds it.
- **D1-POLICY.** *Organization.* The organization approved an AI policy that applies to every AI system it builds, buys or uses, its agents included, and publishes it where every person it applies to can read it. A policy scoped to one class of use, such as employees' use of public AI tools, fails the criterion. *Evidence:* the policy with its approval and its scope statement, and the place it is published.
- **D1-RACI.** *Organization.* An approved RACI assigns one accountable party to each of the three responsibilities the [[owasp-ai-exchange|OWASP AI Exchange]] gives as examples, model accountability, data accountability and risk governance ([`/go/aiprogram/`](https://owaspai.org/go/aiprogram/)). A row over rating or validating a model assigns the first, a row over approving the data an AI system uses the second, and a row over approving an AI system for production or accepting its residual risk the third. A row can name its accountable party by risk tier, one party for each tier. *Evidence:* the RACI with its approval record, each responsibility matched to its row.
- **D1-REGISTER.** *Deployment.* The AI register holds an entry for the deployment that covers each of its agents and names the person accountable for the deployment, and the organization's personnel record shows that person as current. *Evidence:* the register entry, with the personnel record of the person it names.
- **D1-REGISTER-TIER.** *Deployment.* The deployment's register entry records the risk tier the scheme's rule assigns it, with the inputs the rule reads as scored for the deployment. A tier set without those inputs, such as a default applied to a platform, fails the criterion. *Evidence:* the entry's tier, with the record that scored the rule's inputs, such as the deployment's risk assessment.
- **D1-SCHEME.** *Organization.* An approved risk-tier scheme assigns each AI system, its agents included, one of a set of tiers by a stated rule, and the rule reads at least how far the system acts without a person reviewing its output. A scheme that leaves agents out of its scope, or assigns tiers by no stated rule, fails the criterion. *Evidence:* the scheme with its rule, and the policy or standard that applies it to AI systems.

### L3 detail

- **D1-ALLOCATE.** *Deployment.* A responsibility matrix allocates each threat the deployment's threat identification selects to the party that addresses it, the organization or a supplying party, and it names every supplying party of the deployment. The threat identification covers each component the deployment runs on the assessment date. A supplying party the matrix omits, or a component the identification leaves out, fails the criterion. *Evidence:* the matrix and the threat identification, set against the deployment's components and the agreements that make each supplying party one.
- **D1-ALLOCATE-RESIDUE.** *Deployment.* The residue of each threat the matrix allocates carries a recorded disposition, made by the party the organization's rules give that decision. A residual rating with no disposition, or an acceptance made by a party the rules do not give the decision, fails the criterion. *Evidence:* the disposition of each residue, with the party who decided it and the rule that gives the decision.
- **D1-BODY.** *Organization.* A standing AI risk body, chartered by the organization, counts among its members a person from each of four functions: security, legal, privacy and engineering, meaning a function that builds or runs one of the organization's AI systems. A function represented only by a person who attends at the chair's invitation holds no seat, so the body fails the criterion. *Evidence:* the charter's membership, each member matched to a function.
- **D1-BODY-CADENCE.** *Organization.* The body meets on a cadence its charter fixes, and its minutes record a meeting in each interval of that cadence over the twelve months before the assessment starts, or since its charter where that date is more recent. *Evidence:* the charter's cadence, with the dates of the minutes over the period.
- **D1-BOUNDARY.** *Deployment.* A dated record the organization keeps for each agent type states which of its agents' actions they take on their own, which wait for a human approval and whose, and which they never take, and every agent of the deployment falls under such a record. A record can state a default for the actions it does not list. A configuration that enforces the boundaries, such as an allowlist, is technical policy and serves as no boundary record. Not applicable where no agent of the deployment holds a tool. *Evidence:* the boundary record for each agent type, set against the deployment's agents.
- **D1-BOUNDARY-DATA.** *Deployment.* The boundary record states, for each agent type of the deployment, the classes of the organization's information classification its agents may read, and for each class where they may store it, with whom they may share it and when it may leave the organization. A general handling standard that names no agent type states no agent's data handling and fails the criterion. *Evidence:* the data-handling statement for each agent type, set against the classes its agents read.
- **D1-DETAIL.** *Deployment.* The organization's information-security asset inventory holds an entry, with a class under its classification scheme, for each technical detail of the deployment it holds: the record of the model type and the model implementation, and each item it has published, or approved for another party to publish, about the deployment. *Evidence:* the inventory entries with their classes, set against the deployment's design records and publications.
- **D1-DETAIL-REVIEW.** *Deployment.* Each item the organization published, or approved for another party to publish, about the deployment's technical detail passed a review before publication, and the review's record states what the item withholds and what it discloses, set against the disclosure `AI TRANSPARENCY` asks for. A review that records only whether an item describes security controls fails the criterion. Not applicable where the organization has published, and approved for publication, nothing about the deployment. *Evidence:* the review record of each item.
- **D1-DETAIL-RULE.** *Organization.* The organization's rule for external publication requires each item that describes an AI system's technical detail to pass, before publication, a review whose record states what the item withholds and what it discloses, set against the disclosure the Exchange's `AI TRANSPARENCY` control asks for. A publication rule that reviews security controls and names no AI system fails the criterion. *Evidence:* the publication rule, with the review it requires.
- **D1-GATE.** *Deployment.* The approver the gate rule names for the deployment's risk tier, or a body the rule places above that approver, approved the deployment for production before it entered production. An approval dated after the deployment entered production, or given by an approver the rule does not name for the tier, fails the criterion. *Evidence:* the approval record with its approver and its date, set against the deployment's tier and the date it entered production.
- **D1-GATE-CONDITION.** *Deployment.* Each condition the approval set was met by the date the condition states, or before production where it states none, or the approver recorded a decision to change the condition before that date. Not applicable where the approval set no condition. *Evidence:* each condition of the approval, with the record that met it or the approver's decision.
- **D1-GATE-RULE.** *Organization.* An approved rule names, for each risk tier, the approver of a deployment entering production, the risk body itself for the highest tier, and requires that approval before the deployment enters production. *Evidence:* the rule, with the approver it names for each tier.
- **D1-SHADOW.** *Organization.* Discovery runs on a schedule the organization sets and covers each place in the estate where an agent can run: the organization's endpoints, its software-as-a-service tenants, its cloud accounts and its code hosts. Each run is compared with the AI register, and each agent the register lacks is recorded as a shadow agent. A place no discovery source covers, or a comparison made once with no schedule, fails the criterion. *Evidence:* the discovery sources with their schedule and coverage, set against the organization's endpoints, tenants, cloud accounts and code hosts, and the latest comparison with the register.
- **D1-SHADOW-REAP.** *Organization.* The organization sets a time within which each shadow agent is registered or removed, and each shadow agent discovery recorded in the twelve months before the assessment starts was registered or removed within that time. A finding past its time, or a class of finding with no time set, fails the criterion. *Evidence:* the rule that sets the time, and the findings of the period with the date each was registered or removed.

### L4 detail

- **D1-CROSSWALK.** *Organization.* The organization keeps a crosswalk that maps each AI policy and standard it has in force to the frameworks and regulations it names, and the crosswalk's latest revision falls within the review cadence the organization sets for it. A policy or standard in force with no row fails the criterion. *Evidence:* the crosswalk's latest revision with its review rule, set against the AI policies and standards in force.
- **D1-METRICS.** *Organization.* The board receives a report for the most recent complete quarter that records AI incidents, decisions escalated to the risk body, and open security findings grouped by applicable standards-anchor identifiers, such as OWASP agentic threat categories. A count of systems and assessments alone fails. *Evidence:* the quarter's report, traceable finding identifiers, and minutes recording receipt.
- **D1-READINESS.** *Deployment.* The deployment lies within the scope of a readiness assessment against a recognized assurance scheme, completed by an independent assessor in the twelve months before the assessment starts, and the report lists each gap it found against a clause or a control of the scheme. An assessment whose scope omits the deployment, or a self-assessment by the program's own owners, fails the criterion. *Evidence:* the readiness report with its scheme, assessor, date, scope and gap list.

### L5 detail

- **D1-ASSURE.** *Deployment.* Current independent third-party assurance of the organization's AI governance program includes the deployment in scope. A vendor certificate covering only its service, a planned audit, or a lapsed certificate fails. Acceptable routes include an ISO/IEC 42001 certificate under its surveillance conditions, an AIUC-1 certificate with its required quarterly technical testing, or an outside party's assessment of the in-scope governance program against a documented crosswalk to a recognized framework. A review of the crosswalk alone fails. *Evidence:* the certificate or independent assessment report, its scope and effective date, and the record of continued validity.
- **D1-BODY-HISTORY.** *Organization.* The risk body's minutes span at least the twelve months before the assessment starts, from a minuted meeting held twelve months or more before it starts, and record each decision the body took in that period. A body whose first minuted meeting is more recent fails the criterion. *Evidence:* the minutes over the period, with each decision they record.
- **D1-CROSSWALK-REFRESH.** *Organization.* The crosswalk carries a revision dated in each of the last four quarters. *Evidence:* the crosswalk's revision history over the four quarters.
- **D1-METRICS-ATTEST.** *Organization.* The board attested the report D1-METRICS reads for each of the last four quarters. A report the minutes record only as received or noted fails the criterion. *Evidence:* the attestation for each of the four quarters.

## Prerequisites and blockers

The register and risk-tier scheme precede a meaningful tiered gate: without the scored inputs, an approval has no defensible route. The supplier allocation needs the deployment's component and supplying-party scope, which [[agentic-ai-security-cmm-d8-supply-chain|D8]] can evidence. The readiness and assurance criteria require a scope that includes this deployment. A certificate held solely by a supplier does not meet the organization's assurance criterion. The assessment records the failed criterion and remedy in the handbook rather than adjusting another domain's number.

## Deployment-shape differences

A read-only assistant still needs an accountable owner, risk tier, data boundary, and supplier allocation. The action-boundary criterion may be not applicable if its agents hold no tools. An in-suite assistant can inherit one approved record for a class of vendor-built agents, while a tenant-specific connector or enabled agent may need its own boundary and approval. A coding fleet adds endpoint and code-host discovery to the shadow-agent scope. Its harness configuration review is assessed in D8. The [[securing-agentic-coding|agentic coding control set]] does not map its common controls to D1, so the assessor collects the governance records separately. [[cmm-known-limitations|CMM Known Limitations]] records the gap. A multi-agent deployment needs an allocation that includes each materially different agent type and supplier-held component.

For a vendor suite, the assessor can compare the tenant's enabled agents and connectors with the AI register, then trace one release decision back to its scored tier and named approver. The supplier's product description can establish what the supplier operates; the organization's enablement and contract records establish what it chose to turn on and which risk it retained. For an internally built agent, the deploy record fixes the production date against which the approval and its conditions are checked. These are examples of evidence paths, not extra criteria.

## Implementation and effort drivers

The initial work establishes policy and decision ownership, then reconciles the register with the risk-tier inputs. Recurring effort concentrates in:

- risk-body meetings and minutes;
- production-approval condition tracking;
- shadow-agent discovery and disposition;
- publication review.

The L4 crosswalk needs revision ownership and an independent readiness assessor. L5 adds recurring assurance, board attestations, and a full year of operating history. The organization chooses its target from exposure, autonomy, and the cost of evidence. A low-action deployment can rationally target less than a write-capable external service, provided the investment report records the residual risk and accountable decision.

Governance evidence has to remain current as agent configurations change. A new connector may change the boundary and supplying-party allocation even when the deployment name stays the same. A change to an external publication can require a fresh technical-detail review without reopening the original production approval. The organization therefore needs a change route that identifies which decisions a release invalidates and sends those records to the proper owners. A readiness assessment is useful only to the extent that its scope and gap list reflect the deployment the assessor is grading.

## Sources and limits

The [NIST AI RMF Govern function](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF) and [OWASP AI program guidance](https://owaspai.org/go/aiprogram/) support role, policy, and governance-record design. [OWASP risk analysis](https://owaspai.org/go/riskanalysis/) and [AI transparency](https://owaspai.org/go/aitransparency/) inform the allocation and disclosure checks. [[iso-iec-42001|ISO/IEC 42001]] and [[aiuc-1|AIUC-1]] are possible assurance routes when their scope and continuing conditions meet the criterion. [AIUC-1's certification terms](https://www.aiuc-1.com/aiuc-1-certification) specify quarterly technical retesting and an annual audit. Neither scheme defines the CMM levels. Regulatory and scheme-specific suitability requires a separate crosswalk for the organization's jurisdiction. The criteria assess recorded governance operation, not whether every residual risk was wisely accepted.
