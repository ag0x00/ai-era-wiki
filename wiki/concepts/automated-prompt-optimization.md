---
type: concept
title: "Automated Prompt Optimization"
address: c-000179
created: 2026-06-02
updated: 2026-06-02
tags:
  - concepts
  - evaluation
  - agent-improvement
  - post-training
  - ai-in-sec-defense
status: seed
no_public_url: "wiki synthesis across three non-published talks; underlying techniques (GEPA, DSPy) have their own paper pages"
origin: aggregated
scope_axis:
  - ai-in-sec-defense
  - ai-in-sec-offense
related:
  - "[[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]]"
  - "[[trajectory-aware-post-training-talk|Trajectory-Aware Post-Training for Security Agents]]"
  - "[[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph for Threat Intel]]"
  - "[[evidence-centered-benchmark-design|Evidence-Centered Benchmark Design]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[adversarial-reflexion|Adversarial Reflexion]]"
sources:
  - "[[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness (Unprompted March 2026)]]"
  - "[[trajectory-aware-post-training-talk|Trajectory-Aware Post-Training (Unprompted March 2026)]]"
  - "[[osint-to-knowledge-graph-talk|OSINT to Knowledge Graph (Unprompted March 2026)]]"
---

# Automated Prompt Optimization

Automated prompt optimization is the practice of using an automated search process — a genetic algorithm, an out-of-band reflective LLM, or a reinforcement-learning loop — to evolve an agent's prompts or policies against an evaluation signal, rather than hand-tuning them. It is the optimization half of the build–evaluate–improve loop: once an agent's behavior can be scored, the score becomes an objective function that a machine can hill-climb.

The technique is generic to machine learning. Its security relevance is that three independent practitioner teams at [[unprompted-conference-march-2026|Unprompted March 2026]] converged on it as the way to close the loop between evaluating a security agent and improving it, across both defensive and offensive agents.

## The convergence across three talks

- **Genetic prompt optimization driven by an eval.** [[measuring-agent-effectiveness-talk|Joshua Saxe]] argues that once an agent's reasoning is scored on stable multi-dimensional rubrics, genetic algorithms can mutate prompt, plan, and tool-use variants while AI coding tools implement the code-level changes, and the evaluation harness selects survivors. The eval is what makes the loop tractable; overfitting is the named risk.
- **GEPA at test time, on top of trajectory RL.** [[trajectory-aware-post-training-talk|Aaron Brown]]'s pipeline applies GEPA (Genetic-Pareto prompt evolution) after supervised fine-tuning and online reinforcement learning. GEPA changes no model weights; it evolves the system prompt using an out-of-band teacher LLM that reflects on past traces, which matters when the original objective is multi-stage and a fixed system prompt does not carry the agent through every step.
- **Constrained automatic prompt optimization for extraction.** [[osint-to-knowledge-graph-talk|Dongdong Sun]] reports that a naive automatic-prompt-optimization loop fails on threat-report extraction: the optimizer fixes one false positive and introduces another, never converging. The fix is to split the prompt into an LLM-editable section and a human-editable section, which stops the optimizer from thrashing the parts a human must control.

## Dependence on evaluation

Automated optimization is only as good as the signal it optimizes against. All three accounts pair it with a serious evaluation investment: Saxe estimates evaluation consumes at least half a team's time, and Sun reports more than half the project's time spent building the evaluation dataset. A weak or noisy signal produces an optimizer that hill-climbs the metric without improving the agent — the [[evidence-centered-benchmark-design|measurement]] problem and the [[llm-as-a-judge|judge-calibration]] problem are upstream of any optimization gain. Sun's non-convergence and Brown's overfitting caveat are two faces of the same dependency.

## Related techniques

- [[adversarial-reflexion|Adversarial Reflexion]] is a sibling reflective-LLM technique aimed at verification rather than optimization: it constrains an attacker persona to suppress false-positive exploit findings, where automated prompt optimization tunes an agent toward a higher score.
- Reinforcement learning on trajectories (Brown's stage 2) optimizes the model weights against a verifiable reward; prompt optimization optimizes the prompt against the same signal without touching weights. The two compose, as the Open Trajectory Gym pipeline shows.
