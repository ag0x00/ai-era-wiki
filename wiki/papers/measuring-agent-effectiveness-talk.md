---
type: talk
title: "Measuring Security-Agent Effectiveness"
address: c-000168
created: 2026-06-02
updated: 2026-06-02
tags:
  - papers
  - talks
  - agentic-soc
  - evaluation
  - meta
status: summarized
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
origin: aggregated
year: 2026
authors: ["Joshua Saxe"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026 (Day 1 / Stage 1)"
key_claim: "Building a security agent is the easy part; the hard part is measuring it. Cybersecurity has no clean ground-truth oracle, so classical metrics (precision, recall, F-score, ROC, AUC-PR) hit a noise ceiling. Evaluation must keep those metrics but bake in their noise and add a multi-dimensional, interview-style rubric that scores the agent's reasoning, evidence gathering, and behaviour — graded at scale by a calibrated LLM judge — and the resulting signal can drive an automated improvement loop."
methodology: "Practitioner conference talk; transcript and slide deck now captured to `.raw/talks/`."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[rethinking-security-agent-evaluation-talk|Rethinking Security-Agent Evaluation]]"
  - "[[trajectory-aware-post-training-talk|Trajectory-Aware Post-Training]]"
  - "[[evaluating-ai-soc-agents|Evaluating AI SOC Agents]]"
  - "[[defensebench|DefenseBench]]"
  - "[[mechanistic-interpretability-for-defense|Mechanistic Interpretability for Defense]]"
  - "[[evidence-centered-benchmark-design|Evidence-Centered Benchmark Design]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[meta|Meta]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
  - .raw/talks/2026-03-03_Joshua-Saxe_The-Hard-Part-Isnt-Building-the-Agent_transcript.md
  - .raw/talks/2026-03-03_Joshua-Saxe_The-Hard-Part-Isnt-Building-the-Agent_slides.pdf
---

# Measuring Security-Agent Effectiveness

A practitioner talk by [[joshua-saxe|Joshua Saxe]] at the March 2026 [[unprompted-conference-march-2026|Unprompted Conference]]. Saxe spent roughly four years tech-leading AI security work at [[meta|Meta]] — applying AI across Meta's security problems and building prompt-injection defenses against AI-native attacks — and co-founded a startup the week before the talk.

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#)

## The Setup: Why Autonomous Defenders Are Coming

The talk opens from the threat side: as sophisticated attacks get cheaper, attackers shift from manual laborers to managers of AI agents that run attacks. Defenders, already understaffed, will need autonomous counterparts. The example case is a hospital with two security staff that cannot cover its threat surface and so must field a team of agents able to quarantine a compromised CTO account, patch a production codebase, or shut down compromised hosts on their own authority.

Trusting an agent with those actions raises a verification problem. The agent is a neural network with tens of billions of parameters that no one fully understands. Two paths exist to verify it will act correctly: [[mechanistic-interpretability-for-defense|mechanistic interpretability]] — reading the weight matrices directly — or statistical and behavioral evaluation. Saxe treats interpretability as impractical at deploy time and behavioral evaluation as the realistic path, comparing the needed shift to the safety-validation culture of autonomous driving, where a large share of effort goes to proving the system is safe. Evaluation is named as the field's central blocker, applying across autonomous code remediation, alert triage and closure, access management, and cloud posture management.

## The Bad News: There Is No Oracle

Classical machine-learning metrics assume an *oracle* — a labeler who, shown an image, returns a true label of cat, airplane, or banana. Precision, recall, F-score, ROC, and area under precision-recall all inherit that assumption: ground truth is transparent and ascertainable. Cybersecurity violates it. The oracle's process breaks down on security artifacts.

The argument runs on two fronts:

- **Theoretical limits.** Knowing whether a proposed patch works, or whether a binary is malware, runs into the halting problem and state-space explosion. It is not decidable whether a program is bug-free or whether a patch will crash production. Practitioners fall back on heuristics such as passing unit tests. Many binaries are also genuinely dual-use, so "malicious or not" is partly an ontological question, not a fact to be looked up.
- **Human disagreement.** Where labels come from people, they disagree at a double-digit rate. SOC analysts disagree on which alerts were worth investigating; access investigators disagree on who should hold access to a sensitive table. Labels carry noise — one analyst who had a bad lunch marks an alert not worth investigating — yet the metrics treat them as clean.

A simulation makes the cost concrete. Take a hypothetically perfect classifier that predicts whether a SOC alert is a false positive, then flip true-positive and false-positive labels in the evaluation set at rates from 0.5% to 3%. Measured accuracy plummets as noise rises and reaches a **noise ceiling** — a level above which real improvement in the system can no longer be measured. At roughly a 1% flip rate the effect is already severe. The uncertainty is built into the environment and will not yield to more effort.

The framing failure is structural. An agent may generate 100,000 tokens of reasoning, tool calling, retrieval, and explanation to reach a well-justified decision, and the standard evaluation collapses all of it to a single bit of error against a label no one trusts. Hiring a security engineer on a binary multiple-choice test, never probing the ability to reason under uncertainty, would be absurd; that is the prevailing mode for agents. This is the same outcome-only-to-capability-centric critique that [[rethinking-security-agent-evaluation-talk|Khurana's Airbnb talk]] makes from the SOC side.

## The Thesis: Evaluate the Agent Like a Hired Engineer

The proposal keeps the classical metrics but bakes their noise into how they are read, and adds multi-dimensional evaluation of how the agent reasons and behaves. The mental model: hiring a security engineer means hiring someone to make well-validated decisions under extreme uncertainty, and evaluating an agent means assessing the same ability.

Operationally, this means an interview-style **rubric** that defines what good looks like in process, not only in outcome. For an access-management agent deciding whether to grant a contractor access to a table holding millions of user messages, the rubric scores:

- **Evidence gathering** — did the agent retrieve the right artifacts?
- **Policy understanding** — did it apply the relevant access policy?
- **First-principles reasoning** — did it reason correctly from the evidence rather than pattern-match?
- **Auditable logging** — did it record its reasoning in a reviewable form?
- **Decision accuracy** — and, alongside the above, was the final call correct?

Agent trajectories are graded against this rubric at scale. In Saxe's experience, agents are often right where human labelers disagree with them. Because human grading does not scale, a model is trained to automate trajectory review — an [[llm-as-a-judge|LLM-as-a-Judge]] grader. Calibrating that grader needs surprisingly few human samples; around 100 is often enough to start. The calibration runs on a Bayesian model: labels with high reviewer disagreement are down-weighted and carry more uncertainty, while a committee of reviewers in unanimous agreement on a dimension is weighted more heavily. Defining the rubric criteria is where deep domain expertise belongs; the underlying statistics are textbook. This is the same noisy-ground-truth, measure-the-right-thing problem that [[evidence-centered-benchmark-design|Evidence-Centered Benchmark Design]] addresses on the benchmark-construction side.

## Deployment and the Improvement Loop

The rubric scores give a team a hill to climb. The deployment process:

1. Define a bar across all rubric dimensions, aligned with leadership.
2. Hill-climb the criteria until the agent clears the bar.
3. Deploy, having aligned that it is safe to deploy.
4. Monitor in production for drift and off-the-rails behavior.

Saxe is blunt about the alternative: teams running with no evals at all operate on vibes — engineers tweaking agents and saying it works on their machine — and cannot ship with any rigor. An evals program is iterative; the team learns from running it, and keeping leadership aligned on the evaluation makes shipping straightforward once the bar is met. The cost is real: evaluation takes at least 50% of a team's time, but the team moves about 10× faster for it.

Good evals also unlock an automated improvement loop. With a reliable multi-dimensional evaluation in place, AI coding tools and genetic algorithms doing prompt optimization can hill-climb the metrics automatically — mutating and selecting agent variants — while guarding against overfitting. Saxe noted experimentation along these lines at both Meta and the new startup. The verifiable-reward and automated-improvement angle is the sibling theme in [[trajectory-aware-post-training-talk|Brown's Trajectory-Aware Post-Training]] talk.

## Placement

This is the practitioner-methodology layer of the wiki's evaluation argument, the operator entry under the continuous-evaluation domain of the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis. It sits alongside [[rethinking-security-agent-evaluation-talk|Khurana's Airbnb talk]], which makes the same outcome-to-capability move; Saxe adds the closed-loop angle, using the multi-dimensional scores to *drive* automated improvement and not only to *judge* readiness. It complements [[evaluating-ai-soc-agents|Gartner's evaluation criteria]] (the buyer-side framing) and [[defensebench|DefenseBench]] (the artifact-and-benchmark framing), and it shares the verifiable-reward eval gap with [[trajectory-aware-post-training-talk|Brown's Trajectory-Aware Post-Training]]. The cross-axis tagging follows from the content: scoring a security agent's behavior is both a defensive engineering activity (`ai-in-sec-defense`) and a securing-the-agent activity (`sec-of-ai`), because the same trajectory signals that measure effectiveness also surface unsafe tool use and reasoning failures. The deployment bar leaves room for, but does not require, [[hitl|human-in-the-loop]] review on the highest-stakes actions.

## Status

`summarized` — based on the transcript and slide deck captured to `.raw/talks/`.
