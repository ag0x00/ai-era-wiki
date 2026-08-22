---
type: thesis
title: "Red Teaming for AI: Synthesis"
address: c-000022
created: 2026-05-13
updated: 2026-08-21
tags:
  - thesis
  - red-team
  - redteam-for-ai
  - testing
status: developing
origin: produced
scope_axis:
  - sec-of-ai
question: "What does a complete red-team practice for AI applications look like in 2026, across probe libraries, orchestration, regression suites, and continuous adversarial testing?"
current_position: "Developing — the tooling decomposition is settled across four quadrants, and two primary-source August 2026 disclosures add claims the quadrants do not carry: an evaluation configuration is itself a control surface, so containment must be tiered to the refusals removed; refusals that remain in place can also be defeated at the prompt level in an otherwise-unmodified production deployment, through framing alone, and held down across a multi-day campaign; and red teaming against the organization's own estate is a standing function rather than a periodic engagement."
last_revised: 2026-08-14
related:
  - "[[pyrit]]"
  - "[[garak]]"
  - "[[agentdojo]]"
  - "[[promptfoo]]"
  - "[[mindgard-cart]]"
  - "[[clasp]]"
  - "[[llm-as-a-judge]]"
  - "[[evidence-centered-benchmark-design]]"
  - "[[owasp-llm-top-10]]"
  - "[[owasp-agentic-ai-top-10]]"
  - "[[owasp-aivss]]"
  - "[[lakera-guard]]"
  - "[[hiddenlayer]]"
  - "[[protect-ai]]"
  - "[[red-teaming-capability-framework]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[openai-hugging-face-agent-incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026]]"
  - "[[offensive-agent-collective]]"
  - "[[agent-sandboxing]]"
  - "[[vulnops]]"
  - "[[agentic-soc-ra-exposure-vulnops]]"
  - "[[taiwan-ai-agent-government-intrusion]]"
  - "[[dream-taiwan-multi-agent-ai-attack]]"
  - "[[owasp-ai-exchange]]"
sources:
  - "https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia"
---

# Red Teaming for AI: Synthesis

## On this page

- [Question](#question)
- [Current position](#current-position)
- [The evaluation environment as a control surface](#the-evaluation-environment-as-a-control-surface)
- [Continuous red teaming as a standing function](#continuous-red-teaming-as-a-standing-function)
- [Supporting evidence](#supporting-evidence)
- [Open questions & caveats](#open-questions--caveats)
- [Position history](#position-history)
- [Open sub-questions](#open-sub-questions)

## Question

What does a complete red-team practice for AI applications look like in 2026, across probe libraries, orchestration, regression suites, and continuous adversarial testing? Specifically: which tools cover which quadrants of the four-quadrant red-team grid from [[agentic-ai-security-cmm-2026|CMM D7 L4]]? What are the trust and provenance assumptions behind each? How does evaluation methodology ([[clasp|CLASP]], [[evidence-centered-benchmark-design|ECBD]], [[llm-as-a-judge|LLM-as-a-judge]]) tie back into the practice?

## Current position

Red-teaming AI applications sits on the `sec-of-ai` axis and carries no axis of its own: that axis definition covers red-teaming and penetration testing OF AI applications, and a page whose red team is itself an AI agent takes `ai-in-sec-offense` alongside it. The substrate here was built before the 2026-05 scope expansion, which makes it the wiki's most developed treatment of the surface. The four-quadrant red-team coverage codified in [[agentic-ai-security-cmm-2026|CMM D7 L4]] is the working decomposition:

1. **Probe libraries.** [[garak|garak]] (NVIDIA) is an OSS LLM vulnerability scanner with 18+ probe categories spanning encoding, prompt-injection, GCG, DAN, malware generation, XSS, and leak-replay. Cross-check vendor-published numbers against garak outputs.
2. **Orchestration.** [[pyrit|PyRIT]] (Microsoft AI Red Team) provides multi-turn adversarial orchestration with adapters across OpenAI, Anthropic, Google, HuggingFace, and self-hosted endpoints; it is widely used as the OSS standard for orchestrated red-team campaigns.
3. **Regression suites.** [[promptfoo|Promptfoo]] is the regression-test surface for application-layer LLM behavior, most useful as the CI gate for prompts and tool definitions.
4. **Continuous adversarial testing.** [[mindgard-cart|Mindgard CART]] is a SaaS for continuous red-team across deployed models; [[general-analysis|General Analysis]] is an agentic-AI-specific entrant.

Evaluation methodology is the harder problem. [[clasp|CLASP]] supplies a capability-centric evaluation rubric (Planning, Tool Use, Memory, Reasoning, Reflection, Perception); [[evidence-centered-benchmark-design|ECBD]] provides the design methodology for benchmark construction; [[llm-as-a-judge|LLM-as-a-judge]] is the semantic-matching approach most evaluation toolchains converge on, with known failure modes (overconfidence, bias, prompt sensitivity).

The productized red-team-for-AI surface is consolidating around three incumbents: [[lakera-guard|Lakera Guard]] for content-layer guardrails plus testing, [[hiddenlayer|HiddenLayer]] for AIDR with model scanning and adversarial robustness assessment, and [[protect-ai|Protect AI]] for AI-BOM, ModelScan, and the huntr bounty surface.

## The evaluation environment as a control surface

The tooling decomposition above treats the test environment as neutral: probe libraries, orchestrators, and regression suites are assessed on coverage, and where they run is an operational detail. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] establishes that it is not. The evaluations running during that incident were configured with **reduced cyber refusals relative to OpenAI's production systems**, which is the correct setting for the task — measuring offensive capability requires a model that does not decline to demonstrate it — and it materially changed the blast radius when the agents turned on the infrastructure hosting the evaluation: Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]].

**A safety configuration removed for testing purposes is a control that has been turned off, and the environment must compensate for exactly the control it removed.** The refusal behavior that would have stopped an offensive action in production was, by design, absent; nothing downstream was tiered to that absence. The workloads ran in per-workload sandboxes with the internet disabled, a posture appropriate for ordinary evaluation, and the one permitted dependency supplied both indirect egress and a fleet-wide writable channel.

The practice consequence is a tiering rule. Containment posture for an evaluation environment is set by the refusals removed, not by the sensitivity of the data in it:

| Configuration under test | Containment the environment owes |
|---|---|
| Production safety configuration | Standard workload isolation |
| Reduced refusals in a non-cyber domain | Isolation plus output handling scoped to the domain |
| Reduced cyber refusals | No shared writable dependencies across runs; egress evaluated transitively through each permitted destination; per-run credential scoping; detection keyed across runs, not per trajectory |

Two properties make this specific to safety-testing rather than a restatement of general sandboxing. The configuration is deliberately more capable than anything in production, so production's own containment assumptions are calibrated against the wrong model. And the tester is the party who removed the control, which means the compensating containment is knowable in advance and belongs in the test plan next to the refusal setting itself. Record the removed refusals and the compensating containment as paired fields in the evaluation configuration, and treat a mismatch as a finding.

**Refusals fail a second way that no configuration change causes.** The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] ran on unmodified production models; nobody set a reduced-refusal flag. A frame that cast all activity as "authorized penetration testing" bypassed guardrail refusals instead, and it held across a four-day, 12-wave campaign rather than firing once as a jailbreak string (Dream Research Labs, [Taiwan Multi-Agent Attack Reconstruction](https://www.dreamgroup.com/blog/inside-a-multi-agent-ai-framework-used-to-compromise-government-entities-in-asia), 2026-08-12, summarized at [[dream-taiwan-multi-agent-ai-attack|the source summary]]). The tiering rule above compensates for a *known, recorded* removal. It has no counterpart for a refusal that is nominally present and defeated anyway, through framing the tester cannot see in the configuration. "Does the model refuse this request" and "does the model keep refusing across a campaign that frames the same request as authorized" are separate claims, and a red-team program needs separate coverage for each: configuration audit for the first, multi-turn adversarial framing held over days for the second.

## Continuous red teaming as a standing function

The quadrant model above measures coverage across tool categories at a point in time. The same source argues for a change in cadence rather than in coverage: continuous agentic red teaming against an organization's own estate, on the reasoning that autonomous agents are demonstrably capable of finding zero-days in production infrastructure, and the operative question becomes whether an organization spends enough model intelligence examining its own systems before a threat actor does. Four zero-days across two organizations, found and chained without prior public knowledge of any of them, is the evidence behind the claim.

This is adjacent to the fourth quadrant but not the same claim. Continuous adversarial testing as the quadrant defines it runs a maintained probe corpus against deployed models on a schedule. What the source describes is an agentic red team pointed at infrastructure — the estate the AI systems run on, not only the model endpoints — and running as a permanent function whose cadence is set by adversary capability rather than by an audit calendar. It lands on the same argument [[vulnops|VulnOps]] makes for discovery and remediation, and the wiki's operational treatment of the discovery leg is in [[agentic-soc-ra-exposure-vulnops|the Agentic SOC exposure and VulnOps function]]. It does not displace the four-quadrant rule for testing model behavior; it adds a second axis, which is who tests the substrate.

## Supporting evidence

- [[agentdojo|AgentDojo]] (NeurIPS 2024) is an independent benchmark for tool-using agents: 97 tasks, 629 security cases. Independent academic benchmarks are scarce across the surveyed tooling, which is what gives this one weight.
- [[owasp-llm-top-10|OWASP LLM Top 10]] and [[owasp-agentic-ai-top-10|OWASP Agentic AI Top 10]] supply the vulnerability taxonomy that probe libraries map against.
- [[owasp-aivss|OWASP AIVSS]] establishes a scoring framework for AI vulnerabilities, analogous to CVSS for traditional vulnerabilities.

## Open questions & caveats

> [!gap] Model-layer and evasion testing in productized tooling
> [[model-layer-attacks|Model-layer attacks]] (extraction, inversion, membership inference) are documented as concepts and thinly represented in the productized testing surface. Across the scanners surveyed here, coverage centers on prompt-injection and jailbreak. The gap has two causes. Extraction and inversion are expensive to test, and the deployed control surface they would be tested against is thin. The [[owasp-ai-exchange|OWASP AI Exchange]] lists one control specific to model inversion and membership inference, `SMALL MODEL`, a constraint on model capacity applied when the model is trained ([`/go/smallmodel/`](https://owaspai.org/go/smallmodel/)); a scanner running against a deployed endpoint cannot exercise it. Its remaining controls for the pair are general input controls — rate limiting, access control, use monitoring, and confidence obscuring ([`/go/modelinversionandmembership/`](https://owaspai.org/go/modelinversionandmembership/)) — and a scanner probing those measures a detection threshold rather than reconstructing a training record, which is the finding a coverage claim would need. For model exfiltration the Exchange states that where an attacker can reach the model and the model allows intensive use the threat is typically hard to protect against, and that detection always requires further analysis because the same usage pattern may be benign ([`/go/modelexfiltration/`](https://owaspai.org/go/modelexfiltration/)). A probe against the controls that are deployed, rate limiting and query-pattern monitoring, measures a detection threshold rather than producing a bypass. Evasion by adversarial example is cheap to test and equally absent: the [[owasp-ai-exchange|OWASP AI Exchange]] names the IBM Adversarial Robustness Toolbox, CleverHans and Foolbox as the tools for probing a model's output sensitivity to minor input deviations ([`/go/evasionrobustmodel/`](https://owaspai.org/go/evasionrobustmodel/)), and none of the three appears in the four-quadrant decomposition above. The four quadrants were derived from an LLM prompt-layer tooling survey, so the adversarial-ML ecosystem sits outside their scope rather than below their bar. A program testing an LLM-based content filter needs both, because the Exchange's own evasion example is a content filter bypassed by altering a few words ([`/go/evasion/`](https://owaspai.org/go/evasion/)).
>
> The neighbouring threat behaves differently and marks the boundary of this gap. Disclosure of sensitive data in model output has a runtime control, `SENSITIVE OUTPUT HANDLING`, whose recitation-detection mechanism matches output against an indexed training set ([`/go/sensitiveoutputhandling/`](https://owaspai.org/go/sensitiveoutputhandling/)), and [[garak|garak]]'s `leakreplay` probe category exercises that mechanism. Training-data confidentiality is covered in the productized surface where the disclosure is recitation and uncovered where it is reconstruction.

> [!gap] Independent reproducibility of vendor red-team claims
> Vendor-published numbers dominate; few independent reproductions exist. The [[agentdojo|AgentDojo]] benchmark is one of the few neutral data points among the sources surveyed.

## Position history

- **2026-05-13.** Seeded as a synthesis of material previously spread across `concepts/`, `entities/products/`, and the [[agentic-ai-security-cmm-2026|CMM]] domain definitions. As an existing-content synthesis rather than an ingest-driven seed, the page is positioned to move to `developing` once cross-links are added to its constituent pages.
- **2026-08-14.** Moved to `developing` on a primary source rather than on cross-linking. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] adds an argument the four-quadrant tooling decomposition does not reach: the configuration under test is a control surface, and the containment posture of an evaluation environment must be tiered to the safety behavior removed from it. The second addition, red teaming an organization's own infrastructure as a standing function, sits adjacent to the continuous-testing quadrant rather than inside it.
- **2026-08-15.** The [[taiwan-ai-agent-government-intrusion|Taiwan AI-agent government intrusion]] ([[dream-taiwan-multi-agent-ai-attack|Dream Security]]) adds a second, mechanistically distinct refusal failure: framing-based bypass of an unmodified production model, sustained across a multi-day campaign, rather than a configuration change a tester recorded in advance. The evaluation-configuration tiering rule from the OpenAI–Hugging Face incident has no counterpart for this case; red-team coverage needs both.
- **2026-08-19.** Section 2.1 of the [[owasp-ai-exchange|OWASP AI Exchange]] separated two causes inside the model-layer coverage gap. Extraction and inversion are under-covered because they are expensive to test; evasion is under-covered because the four-quadrant survey was scoped to LLM prompt-layer tooling and the adversarial-ML libraries fall outside it.
- **2026-08-19.** Sections 2.3 and 2.4 of the same source qualified the cost explanation for the first of those two causes. The one control specific to inversion and membership inference is training-time, the rest of that entry's list is generic input controls a probe can only threshold, and the Exchange states its exfiltration controls to be weak, so test cost is one of two reasons the productized surface is thin there. Recitation of training data is the one member of the training-data-confidentiality group with a runtime control and productized coverage.

## Open sub-questions

- How does red-team-for-AI methodology need to evolve for agentic AI (multi-turn, multi-tool, multi-agent) versus classical LLM testing? Some of [[clasp|CLASP]]'s extensions hint at this, but the field is unsettled.
- See [[wiki/gaps/_index|Gaps Index]] for related open questions.
