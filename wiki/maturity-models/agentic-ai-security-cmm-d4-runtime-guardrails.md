---
type: maturity-model
title: "CMM D4: Runtime and Guardrails"
address: c-000126
created: 2026-05-25
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - guardrails
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[gemini-cli-workspace-trust-rce|Gemini CLI Workspace-Trust RCE]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[prompt-injection]]"
  - "[[lethal-trifecta]]"
  - "[[gke-agent-sandbox]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[owasp-agentic-ai-threats-mitigations]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[anthropic-sandbox-runtime]]"
  - "[[securing-agentic-coding]]"
  - "[[cmm-known-limitations]]"
  - "[[security-guidance-plugin]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[owasp-ai-exchange]]"
  - "[[agent-escape]]"
  - "[[agent-sandbox-isolation-landscape]]"
  - "[[inference-exposure]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[oversight-layer]]"
  - "[[least-agency-principle]]"
  - "[[agentic-ai-security-cmm-d1-governance]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[falcon-guardian]]"
  - "[[agent-runtime-protection-canvass-2026-09]]"
  - "[[chain-of-thought-monitorability]]"
  - "[[claude-cowork]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
sources:
  - https://owaspai.org/go/agentsandboxing/
  - https://owaspai.org/go/oversight/
  - https://github.com/OWASP/www-project-ai-security-and-privacy-guide/blob/main/content/ai_exchange/content/docs/1_general_controls.md
  - https://owaspai.org/go/leastmodelprivilege/
  - https://owaspai.org/go/promptinjectioniohandling/
  - https://owaspai.org/go/sensitiveoutputhandling/
  - https://owaspai.org/go/testingpromptinjection/
  - https://owaspai.org/go/discrete/
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Targeted alignment and risk-state check against live OWASP guidance; no archived source opened. Other source claims remain outside this pass."
---

# CMM D4: Runtime and Guardrails

## Domain decision and boundary

D4 assesses controls on model input and output, proposed tool effects, and agent-directed execution for one defined deployment shape. The assessor inventories the agents in scope, their model calls, untrusted-content routes, output destinations, tools, and execution modes. A control counts only on the paths it actually covers. The [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]] records an applicable path that cannot be inspected, tested, or supported by a scoped supplier statement as unanswerable.

[[agentic-ai-security-cmm-d3-control-least-agency|D3]] owns action authorization and human approval. D4 checks content, proposed impact, and execution confinement before an authorized action runs. [[agentic-ai-security-cmm-d5-egress-network|D5]] owns outbound and inter-agent routes, [[agentic-ai-security-cmm-d6-data-rag|D6]] owns retrieved and stored data, and [[agentic-ai-security-cmm-d8-supply-chain|D8]] owns pre-release checks of code and assembled agents. A shared gateway may provide evidence for more than one domain when its test record shows each required enforcement point.

A *prompt* here is a human request sent to an agent's model. *Untrusted content* includes retrieved documents, external pages, inbound messages, and third-party tool results. A *response* includes content shown to a person and model-generated content passed to a tool. A *sandbox* contains code or commands the agent chooses to run, including processes started from an untrusted workspace. A *high-impact call* can perform an action the D3 policy classifies as destructive.

## Failure paths

- A detector screens the chat field but misses an instruction in a retrieved item or tool result. The assessor presents the attack through the insertion route, with production-equivalent filtering in path.[^aix-testing]
- Shell commands run in a sandbox while a hook, local server, file tool, or retry acts on the host without a separate enforced boundary. The assessor tests process scope, autonomy modes, and startup order.
- An agent proposes a call within its broad grant that would exceed the current task or permitted impact. D4 evaluates the call before execution; a clean result cannot override a D3 denial or required approval.

## L1–L5 progression

| Level | Observable D4 outcome |
|---|---|
| L1 | Runtime protection is ad hoc or held only in instructions. |
| L2 | Human prompts and model responses pass configured safety screens. |
| L3 | Injection screens cover the actual input routes, and agent-directed execution is confined. |
| L4 | External checks assess proposed actions and sourced answers before their effects occur. |
| L5 | The deployment measures misses, treats bypasses, enforces guardrail budgets, and applies controls across its agents. |

## Criterion catalogue

Each bold name defines one determination. L2 through L5 are cumulative. A criterion is not applicable only under its stated condition; supplier operation alone is not such a condition. A missing route or failed facet is not met. Evidence identifies the running revision and the route or execution mode tested. A supplier document can establish a named feature, while a route-specific result or test establishes that the assessed path reaches it. These levels are the CMM's synthesis of the [OWASP AI Exchange I/O](https://owaspai.org/go/promptinjectioniohandling/), [oversight](https://owaspai.org/go/oversight/), and [sandbox](https://owaspai.org/go/agentsandboxing/) guidance.

### L2 detail

- **D4-INPUT.** A configured safety filter screens each human prompt before an agent's model reads it, on every model-call path that receives such prompts. It blocks, redacts, or flags matched content as the deployment's policy specifies. A bypass path fails. Not applicable where no agent receives a human prompt. *Evidence:* running filter configuration and path map, or a product-scoped supplier statement and observed result.
- **D4-OUTPUT.** A content-safety classifier screens each model response before delivery, including user-visible replies and model-generated content passed to tools. It blocks or redacts a matched response. Screening only the user interface fails when tool-directed content bypasses it. *Evidence:* running output configuration or scoped supplier statement, with tests or decision records for both destinations.

### L3 detail

- **D4-INJECT-CANON.** On every applicable direct or indirect text route, the in-path injection detector resists attacks disguised by Unicode normalization differences, invisible characters, case changes, and confusable or mixed-script characters.[^aix-piioh] A syntactic matcher normalizes or detects these forms before matching. A semantic classifier can read the original text if route tests show it blocks the attack variants. Tests include a benign control to expose indiscriminate blocking. For image, audio, or file routes, use presentation and encoding variants relevant to the accepted modality. Not applicable only when both D4 injection-screen criteria are not applicable. *Evidence:* route inventory, detector configuration, variant results, and benign-control result on the running revision.
- **D4-INJECT-DIRECT.** An in-path detector screens each human prompt for a direct prompt attack before the model acts, and it blocks or holds a detection. Not applicable where no agent receives a human prompt. *Evidence:* detector placement and configuration, with a blocked attack or a product-scoped detection record.
- **D4-INJECT-INDIRECT.** Each route that carries untrusted content into the model's context has an in-path injection detector that blocks or holds a detection before the model reads it. The assessor tests through each insertion route or inspects a detection recorded on content from that route; a generic feature statement alone does not establish route coverage.[^aix-testing] D5's inter-agent screen supplies the route check for messages between agents, which take no second D4 determination. Not applicable where no other route carries untrusted content to a model. *Evidence:* route inventory and, for each assessed route, its test or detection record.
- **D4-OUTPUT-SCOPE.** A dated record identifies the output classifier's safety categories and separately identifies each exposure-restricted data class reachable by the model, including system prompts. It states whether the classifier covers each class and other technical system detail.[^aix-discrete] The record agrees with the running configuration or scoped supplier statement. Omitting a reachable restricted class fails.[^aix-soh] *Evidence:* dated scope record and classifier configuration or supplier category statement.
- **D4-SANDBOX.** Every agent-chosen command and generated-code execution runs in a sandbox. Each process the agent's run starts, including hooks and local servers, and each agent-directed file or network effect stays in that sandbox or a separately enforced, narrower service boundary throughout the run and in every permitted autonomy mode. A denied operation cannot retry outside confinement. A trusted credential broker can remain outside if it does not execute agent-supplied code or workspace configuration. Not applicable where no agent executes code or commands. *Evidence:* process and tool map against each isolation profile, permitted modes, and refusal and retry tests.
- **D4-SANDBOX-CLEAN.** One task uses each sandbox. Normal completion and forced stop destroy its transient state, cached data, and any credential material left inside it before another task can run.[^aix-sandbox] Not applicable where no agent executes code or commands. *Evidence:* lifecycle configuration and observed teardown in both conditions.
- **D4-SANDBOX-CONFINE.** The sandbox refuses process escape, access to host files outside the authorized workspace and necessary read-only system files, agent-configuration changes, and interaction with host devices or other processes.[^aix-sandbox] Failure of any refusal fails the criterion. Not applicable where no agent executes code or commands. *Evidence:* isolation profile and a test of each refusal.
- **D4-SANDBOX-CREDS.** The code inside the sandbox cannot read any credential used by the run, including short-lived model, tool, MCP, code-host, or cloud tokens. An external store or broker attaches credentials to authorized calls.[^aix-sandbox] Not applicable where no agent executes code or commands. *Evidence:* sandbox environment and mounts, credential storage design, and a credential-read refusal.
- **D4-SANDBOX-FIRST.** Confinement begins before workspace-controlled settings can start a process or set its environment, or the run reads no such settings. A hook, MCP definition, or version-control configuration that executes before confinement fails. Not applicable if D4-SANDBOX is not applicable or the run reads no workspace-controlled executable configuration. *Evidence:* documented startup order or a workspace-supplied test hook that records where it ran.
- **D4-SANDBOX-LIMITS.** The platform enforces CPU, memory, and wall-clock limits for each sandbox. An agent-maintained turn or recursion count is not a platform limit.[^aix-sandbox] Not applicable where no agent executes code or commands. *Evidence:* live limit settings, enforcement component, and a limit-breach record.
- **D4-SANDBOX-MAC.** An unchangeable kernel or hypervisor access-control profile confines sandbox processes, which retain only required capabilities and privilege paths.[^aix-sandbox] Not applicable where no agent executes code or commands. *Evidence:* active profile and capability set from a running sandbox.

### L4 detail

- **D4-ALIGN.** A monitor outside the agent checks each proposed tool call against the authorized task before execution, using the stated plan where one is exposed. It blocks or holds a misaligned call. Deterministic task-to-parameter checks can establish this outcome; a semantic judge is an optional method where its tested behavior adds coverage. No model-family choice is required. An after-the-fact sample does not meet this gate. Not applicable where the agent has no tool. *Evidence:* call-path placement, monitor configuration, and a held call.
- **D4-CONTEXT.** For sessions exposed to untrusted content, the deployment defines which risk signals narrow tool actions, such as risky web material or a failed call judge.[^aix-oversight] An outside-model rule applies the narrower task policy.[^aix-least] It blocks or holds a side-effect or restricted-data egress request that trusted task inputs did not authorize, including one proposed by instructions in the untrusted content. An approved hosted-model inference call can continue under the data policy. Not applicable where no agent both receives untrusted content and can make a side-effect or restricted-data egress call. *Evidence:* risk-state trigger, external rule, refusal test for an injection-origin call, and permitted-call control test.
- **D4-GROUND.** A check compares each answer based on retrieved or supplied material with that material before delivery. An answer below the configured support threshold is held, corrected, or marked unsupported for the recipient. A post-delivery sample fails. Not applicable where no agent answers a person from supplied material. *Evidence:* threshold configuration and answer-level results.
- **D4-VALIDATE-DRYRUN.** For each high-impact tool that can preview its effect, the system computes and records the proposed state change before executing the call.[^aix-oversight] Not applicable where no accessible high-impact tool has a non-effecting preview. *Evidence:* call-linked dry-run records showing proposed changes before execution.
- **D4-VALIDATE-IMPACT.** Before a high-impact call runs, deterministic rules outside the model compare its parsed arguments or previewed change with configured per-call impact limits. A call above a limit is refused or held. D3 owns cumulative session limits.[^aix-oversight] Not applicable where the agent cannot make a high-impact call. *Evidence:* limits, parsed-impact decisions, and an excess-impact refusal.

### L5 detail

- **D4-BUDGET.** The component running each guardrail enforces a latency and cost budget for that guardrail. A breach stops the guardrail call or spending and passes any unscreened action to the failure mode tested under [[agentic-ai-security-cmm-d9-operations|D9]]. Monitoring a budget without enforcement fails. *Evidence:* budgets, enforcement settings, and a breach record showing the configured fail mode.
- **D4-INJECT-BYPASS.** Each injection detector's miss rate is measured and reported by bypass class against a current library of attacks. A suite without bypass classes or an expired library fails. Not applicable when both D4 injection-screen criteria are not applicable. *Evidence:* dated library revision and class-level evaluation results.
- **D4-INJECT-LANG.** Each injection detector's miss rate is measured and reported separately for the languages the deployment accepts on its prompt and untrusted-content routes.[^aix-piioh] An aggregate figure cannot stand for an untested language. Not applicable when both D4 injection-screen criteria are not applicable. *Evidence:* dated language-specific attack sets and results.
- **D4-INJECT-REFRESH.** The operator updates each injection detector against new attack techniques on a stated cadence and records the version and date.[^aix-piioh] A supplier-operated detector needs a scoped release record. Not applicable when both D4 injection-screen criteria are not applicable. *Evidence:* cadence and refresh receipts for the assessment period.
- **D4-INJECT-REMEDIATE.** For each relevant bypass class the current evaluation misses, an owner retests the affected route and deploys a mitigation or records an authorized residual-risk decision. Reporting to a supplier without treatment and retest fails. Supplier acknowledgement is not required. Not applicable if testing finds no missed class. *Evidence:* class result, accountable owner, treatment, route retest, and closure or risk acceptance.
- **D4-OUTPUT-LEAK.** An in-path output scan detects credentials before a model response leaves. Tests measure detection by literal and encoded form, including base64 and hexadecimal where relevant, and record the rate for each. Literal-only testing fails. Not applicable where no credential can reach a model's context. *Evidence:* scanner placement and dated results by form.
- **D4-PLATFORM.** Each applicable L4 gate covers every in-scope agent and run channel through enforcement that the agent owner cannot disable. An applicable agent opt-out fails. *Evidence:* coverage matrix, central enforcement settings, and a test of an owner-level bypass attempt.
- **D4-SHARED.** For a multi-agent deployment, the organization records each shared model, credential, and policy service and determines whether it partitions each agent's data and grants or forms an accepted cross-agent channel. A shared service absent from the record fails. Not applicable to a single-agent deployment. *Evidence:* service inventory, agent configurations, isolation test or accepted residual-risk record.

## Prerequisites and blockers

D3 supplies the task, action tier, and decision path for D4's call checks. D4-CONTEXT also needs a trustworthy risk-state signal from content ingress. A model-authored claim cannot set or clear that state. The [[least-agency-principle|Least Agency Principle]] describes broader risk-based permission narrowing. D4-CONTEXT tests the risk-state action decision defined here. D5 owns the outbound route and D6 the data classification used in a restricted-data egress decision. A clean alignment or impact result cannot replace D3 approval. [[agentic-ai-security-cmm-d7-observability|D7]] receives detection and escape events, including evidence of sandbox escapes on command-execution paths, while D9 tests the failure mode when a D4 guardrail cannot return. The Handbook records a failed criterion and its prerequisite owner as a target blocker rather than changing another domain's observed level.

## Deployment-shape differences

| Shape | Material D4 question |
|---|---|
| Read-only assistant | Which prompt and retrieval routes reach its classifiers? Sandbox criteria are not applicable if it executes no code or commands. |
| Coding agent | Do shell, file tools, hooks, local servers, and startup settings stay behind an enforced boundary in each autonomy mode? |
| Vendor-managed suite | Can tenant tests or scoped supplier records establish hidden controls on every applicable route? |
| Multi-agent workflow | Do each agent's routes and shared services preserve isolation and per-agent coverage? |

## Implementation and effort drivers

First inventory routes and execution modes. Then place injection screens, call checks, and isolation where agent-controlled instructions cannot turn them off. A deployment may use separate components for prompt screening, tool-result detection, grounding, and call validation; the assessor tests the assembled path. The principal effort grows with sandbox concurrency, route-specific tests, false-positive review, bypass-library upkeep, and the latency of gates before high-impact calls. L5 adds ongoing evaluation, refresh receipts, bypass treatment, budget enforcement, and coverage checks across the agent fleet.

## Sources and material limits

The [OWASP AI Exchange prompt-injection guidance](https://owaspai.org/go/promptinjectioniohandling/) describes normalization as a syntactic matching aid and semantic detection as another method. The [testing procedure](https://owaspai.org/go/testingpromptinjection/) tests the actual insertion route and benign behavior. The [least-privilege control](https://owaspai.org/go/leastmodelprivilege/) describes risk-based permission reduction, and the [oversight source](https://github.com/OWASP/www-project-ai-security-and-privacy-guide/blob/main/content/ai_exchange/content/docs/1_general_controls.md) makes post-risk read-only restriction optional; D4-CONTEXT grades a defined, testable risk-state decision. These source controls inform the design but do not assign D4 levels. Classifiers can miss attacks, and a sandbox cannot prevent harm through a legitimately permitted action. Pre-release analysis of agent-generated code belongs to D8.

[^aix-oversight]: [OWASP AI Exchange — Oversight source](https://github.com/OWASP/www-project-ai-security-and-privacy-guide/blob/main/content/ai_exchange/content/docs/1_general_controls.md), retrieved 2026-09-29, simulate-before-execute and optional read-only restriction after risky web access or judge failure.
[^aix-least]: [OWASP AI Exchange — Least model privilege](https://owaspai.org/go/leastmodelprivilege/), task-based minimization and risk-triggered reduction of tool rights after untrusted content.
[^aix-discrete]: [OWASP AI Exchange — Discrete](https://owaspai.org/go/discrete/), minimizing technical details in model output.
[^aix-piioh]: [OWASP AI Exchange — Prompt injection I/O handling](https://owaspai.org/go/promptinjectioniohandling/), character handling, semantic recognition, and per-language evaluation.
[^aix-sandbox]: [OWASP AI Exchange — Agent sandboxing and isolation](https://owaspai.org/go/agentsandboxing/), platform isolation, credential placement, teardown, and resource limits.
[^aix-soh]: [OWASP AI Exchange — Sensitive output handling](https://owaspai.org/go/sensitiveoutputhandling/), output-time treatment of restricted data.
[^aix-testing]: [OWASP AI Exchange — Testing against prompt injection](https://owaspai.org/go/testingpromptinjection/), steps 3–5 and benign controls.
