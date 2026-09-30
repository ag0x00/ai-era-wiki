---
type: maturity-model-companion
title: "CMM: Evidence Prerequisites and Dependencies"
address: c-000158
created: 2026-05-04
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - dependency-rules
  - 2026-proposal
status: developing
origin: produced
scope_axis:
  - sec-of-ai
target: "[[agentic-ai-security-cmm-2026]]"
rule_set_version: "2026-09-29 evidence prerequisites; numeric caps retired"
related:
  - "[[agentic-cmm-regulated-fi-stress-test|Regulated-FI Stress Test]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[cmm-calibration-stress-test-2026]]"
  - "[[lethal-trifecta]]"
  - "[[anti-patterns-and-failure-modes]]"
  - "[[wiki-novelty-and-counterarguments-2026]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-d5-egress-network]]"
  - "[[agentic-ai-security-cmm-d6-data-rag]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[agent-identity-architecture]]"
  - "[[owasp-ai-exchange]]"
  - "[[agentic-cmm-vs-standards-validation]]"
  - "[[cmm-vocabulary-and-notation]]"
  - "[[cmm-known-limitations]]"
sources:
  - "[[cmm-calibration-stress-test-2026]] §Part 2 (cumulative-floor stress test)"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Whole-page read against current core, Handbook, D2/D3/D5/D7/D8/D9 criteria and historical calibration text; repaired applicability and unanswerable/not-met distinction. No archived raw source opened; OWASP supply-chain guidance checked live."
---

# CMM: Evidence Prerequisites and Dependencies

The [[agentic-ai-security-cmm-2026|Agentic AI Security CMM]] assesses nine domains at five cumulative levels. A domain reaches a level when its applicable criteria at that level and below are met under the [[agentic-ai-security-cmm-measurement-protocol|measurement protocol]]. The report shows the nine domain results and the evidence gaps. It does not calculate a raw score, an effective score, a dependency cap, or a program-wide rating.

Cross-domain relationships still matter. A claimed control may depend on an identity, policy decision, or record produced elsewhere. The assessor checks that prerequisite in the action path being graded. A weak neighboring domain is a reason to inspect the path, not an arithmetic instruction to lower the first domain. This page records the principal checks and the history of the former cap rules.

## Evidence prerequisites

| Claim under examination | Prerequisite to inspect | Failure that defeats the claim |
|---|---|---|
| [[agentic-ai-security-cmm-d5-egress-network\|D5]] per-agent or per-task egress | [[agentic-ai-security-cmm-d2-identity\|D2]] identifies the caller at the gateway; [[agentic-ai-security-cmm-d3-control-least-agency\|D3]] binds the action to a task and permitted destination where the criterion requires it. | A shared credential or unbound route lets another agent use the same path without the claimed decision. |
| D5-TASK-EGRESS at L5 | D2-TASKBIND and D3-TASKSCOPE evidence names the same task and action. | A task label in a log without an enforced, scoped decision does not establish task-limited egress. |
| [[agentic-ai-security-cmm-d7-observability\|D7]] per-agent attribution and behavioral detection | D2 supplies a stable, distinct agent identity and the trace carries it through the relevant calls. | Fleet-only attribution cannot support a per-agent baseline. |
| [[agentic-ai-security-cmm-d4-runtime-guardrails\|D4]] action guardrail | D3 defines the authority decision; the runtime or gateway enforces it on every applicable route. | A guardrail can fire while a direct call bypasses the decision. |
| [[agentic-ai-security-cmm-d6-data-rag\|D6]] trusted retrieval or memory evidence | Source entitlement and write-provenance records identify the source, principal, task, and current decision where relevant. | A response trace alone cannot prove that the retrieved record was authorized or that a memory write was admitted. |
| [[agentic-ai-security-cmm-d8-supply-chain\|D8]] release assurance | D1 supplies risk authority for exceptions; D8 binds design, implementation, test, supplier evidence, and the release decision to the deployed version. | A passing component test or signed artifact without a release decision does not establish the assembled system's approval. |
| [[agentic-ai-security-cmm-d9-operations\|D9]] incident reconstruction | D7 preserves the execution and policy records; D9 runs the response, reconciliation, and continuity procedure against them. | A playbook cannot reconstruct an action when the required independent records are absent. |

The checks are criterion-specific. An assistant with no outbound tool or inter-agent path has no task egress to grade. A vendor-managed suite may hold a prerequisite inside the supplier boundary. In the latter case, inspect the supplier's versioned evidence and the customer-visible crossing. A supplier-operated fact that cannot be inspected, tested, or attested is **unanswerable**. A required record known never to have been made is **not met**. A compensating design can satisfy an outcome when the criterion permits it and a rejection test demonstrates the boundary. The assessor records that design and its limits.

### Attack-path basis

The original dependency study identified three useful paths. A network gateway needs caller identity to apply a per-agent rule. A behavioral detector needs that identity to attribute actions. A runtime enforcement hook needs a decision rule and a route that cannot bypass it. [[agent-identity-architecture|AI Agent Identity Architecture]] traces the first two paths across identity and action layers. The [[lethal-trifecta|Lethal Trifecta]] explains the egress risk; the [[hooking-coding-agents-with-cedar-talk|Sondera Cedar harness]], [[agentcordon|AgentCordon]], and [[1-8m-prompts-30-alerts-talk|Salesforce Rittinghouse]] illustrate the others. These examples justify inspecting the linked evidence. They do not justify assigning the same maturity level to different domains.

Another path runs from acquired components to data integrity. A poisoned skill, model, or tool can change what a retrieval or memory system writes. The [[owasp-ai-exchange|OWASP AI Exchange]] separates the supplier's controls from the receiver's remaining data-poisoning and supply-chain controls ([supply-chain management](https://owaspai.org/go/supplychainmanage/)). A weak D8 finding therefore triggers a test of the affected D6 path. It does not automatically erase D6 controls the receiver can demonstrate. The [[clawhavoc|ClawHavoc]] incident remains a case to test, not a numeric cap.

## Historical rule register

From 2026-05-04 until the September redesign, this page defined a formula that took the minimum of a domain's raw level and upstream domains' raw levels. It reported median effective, weakest, and strongest levels. That formula, its three-number headline, and its program rating are **retired**. Dated stress tests and reviews that quote them describe the model in force when written; new assessments use the criterion evidence and nine-domain matrix.

[[cmm-known-limitations|CMM Known Limitations]] archives the former L5 gate and per-task-token level split, with their September resolution. Its historical paragraphs are not instructions for new assessments.

| Historical ID | Former numeric rule | Current use |
|---|---|---|
| DR-001 | D2 capped D5. | Inspect caller and task identity for the D5 criterion actually claimed. |
| DR-002 | D2 capped D7. | Inspect attribution for the D7 signal actually claimed. |
| DR-003 | D3 capped D4. | Inspect the decision and enforcement path for the D4 guardrail actually claimed. |
| DR-C001 to DR-C006 | Candidate caps joining D8→D6, D5→D7, D4→D5, D6→D4, D9→D2, and D1→all domains. | Retired as candidate score rules. Use their threat hypotheses to select tests where the deployment exposes those paths. |

The [[cmm-calibration-stress-test-2026|May calibration stress test]] compared five deployment archetypes under a single floor and then under dependency-resolved aggregation. Its conclusions about which controls interact remain useful. Its arithmetic and three-number comparisons do not describe the current assessment method. An architectural-containment choice, such as removing write tools or external communication, is recorded in the deployment boundary and applicable criteria. It is not converted into a bonus or a penalty elsewhere in the matrix.

The [[agentic-cmm-regulated-fi-stress-test|Agentic AI CMM: Regulated-FI Stress Test]] applied the former caps and three-number summary to a customer-service RAG bot. Its historical scores do not transfer to the current nine-domain evidence method.

## Revision and reporting

When new evidence reveals a dependency, name the affected action, the upstream record or control, the bypass or failure path, and the criterion whose outcome changes. Test the path in the deployment shape at issue. A paper or product diagram may motivate the test; a level claim requires assessable evidence from the implemented path. Add a prerequisite to this page only when it helps an assessor locate a real cross-domain record. The domain page remains the owner of its criterion and pass/fail rule.

The assessment report lists each domain result, unmet and unanswerable criteria, applicable exceptions, assurance class, and the evidence path for a cross-domain prerequisite. It may explain an intentional design trade-off, but does not calculate a median, minimum, maximum, or cap. See [[cmm-vocabulary-and-notation|CMM Vocabulary and Notation]] for the current terms.

## Relations

- Historical basis: [[cmm-calibration-stress-test-2026|CMM Calibration Stress Test]] and [[agentic-cmm-vs-standards-validation|the standards validation]].
- Assessment method: [[agentic-ai-security-cmm-measurement-protocol|CMM Measurement Protocol]].
- Architectural boundary: [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]].
