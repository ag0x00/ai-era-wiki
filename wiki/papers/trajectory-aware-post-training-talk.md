---
type: talk
title: "Trajectory-Aware Post-Training for Security Agents"
address: c-000171
created: 2026-06-02
updated: 2026-06-02
tags:
  - papers
  - talks
  - agentic-soc
  - post-training
  - sft
  - grpo
  - open-weights
  - aws
status: summarized
scope_axis:
  - ai-in-sec-defense
origin: aggregated
year: 2026
authors: ["Aaron Brown", "Madhur Prashant"]
venue: "Unprompted — AI Security Practitioner Conference, San Francisco, March 3, 2026 (Day 1 / Stage 2)"
key_claim: "Specialized security agents can be built on open-weight models with a trajectory-aware post-training pipeline — SFT for tool-call format, group-relative online RL on perturbed agent trajectories, and test-time GEPA prompt evolution — released as the open-source Open Trajectory Gym. A dense Qwen 3.5 27B running the BoxPwner agent moved from ~12.5% to ~35% solve rate on CyBench in under a day on 285 distilled traces."
methodology: "Practitioner talk with on-stage demo; transcript and slide deck captured to .raw/talks/. The published abstract planned a GLM-4.7 Flash 30B fine-tune; Brown switched to a dense Qwen 3.5 27B two days before the talk (Qwen had just released, and no open-source weight-synchronization fix exists for MoE post-training during RL)."
source_url: "https://unpromptedcon.org/#"
related:
  - "[[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]]"
  - "[[defensebench|DefenseBench]]"
  - "[[measuring-agent-effectiveness-talk|Measuring Security-Agent Effectiveness]]"
  - "[[rethinking-security-agent-evaluation-talk|Rethinking Security-Agent Evaluation]]"
  - "[[jagged-frontier|Jagged Frontier]]"
  - "[[unprompted-conference-march-2026|Unprompted March 2026]]"
  - "[[aws|AWS]]"
  - "[[nvidia|NVIDIA]]"
sources:
  - "[[unprompted-conference-march-2026|Unprompted Conference (March 2026)]]"
  - https://unpromptedcon.org/#
  - .raw/talks/2026-03-03_Aaron-Brown_Trajectory-Aware-Post-Training_transcript.md
  - .raw/talks/2026-03-03_Aaron-Brown_Trajectory-Aware-Post-Training_slides.pdf
---

# Trajectory-Aware Post-Training for Security Agents

A practitioner talk by [[aaron-brown|Aaron Brown]] (with [[madhur-prashant|Madhur Prashant]], [[aws|AWS]]) at the March 2026 [[unprompted-conference-march-2026|Unprompted Conference]]. The talk introduces **Open Trajectory Gym**, an open-source post-training stack Brown released about an hour before the session, and walks through a worked tutorial: fine-tuning a small open-weight model into a specialized web-application pentest agent.

**Source:** [Conference abstracts page](https://unpromptedcon.org/abstract-march2026/) · [Conference agenda](https://unpromptedcon.org/#)

## The problem

Frontier and general models are competent at **atomic** pentest tasks — spotting a local file inclusion or a cross-site scripting flaw, then exploiting that single issue. They are poor at **multi-stage** tasks that chain several vulnerabilities together to reach an objective. Brown cites the irregular GPT-5.2 scorecard and discussions at Offensive AI Con as evidence of this gap. The unevenness is a case of the [[jagged-frontier|jagged frontier]]: strong on isolated steps, weak on long-horizon composition.

Base models are optimized for everything from question-answering to software development, not for the long-horizon exploration-and-action loop that pentest, incident response, and similar security tasks require. That gap motivates task-specific post-training rather than reliance on a general model behind an API.

The talk also restates the field's evaluation gap, the same one named earlier the same day in [[measuring-agent-effectiveness-talk|Saxe's effectiveness talk]] and in [[rethinking-security-agent-evaluation-talk|Khurana's multi-stage evaluation talk]]: coding and math have established benchmarks and training frameworks; security largely does not. Brown names CyBench, CVE-Bench, and XBOW validation as the sparse existing set and treats benchmark and reward design as first-order problems.

## Definition of an agent

Brown frames the substrate precisely. An agent is a model invoked through an application layer, with:

- **Memory** — a long-term out-of-band store, plus a short-term conversation that is truncated, pruned, and rehydrated.
- **Tools** — functions with names, argument schemas, and docstrings that describe each tool in text.
- **Actions** — execution in an environment (local machine, remote container).

Models are stateless. Every turn resends the system prompt, the tool schemas, and the growing conversation history, then asks for the next action. Post-training pushes the tool-call format, terminology, and task reasoning into the weights so the model does not have to learn them from scratch at inference time.

## Open Trajectory Gym

Standing up a distributed RL post-training cluster normally means assembling 36-plus libraries and SDKs, with debugging that has to separate algorithm faults from infrastructure, environment, and data faults. Open Trajectory Gym packages that stack into a single loop at a machine-learning-engineering level rather than a PhD-lab level. It builds on:

- **TRL** — the low-level training base everything else sits on.
- **SkyRL** — the RL layer, from Berkeley's Sky Computing Lab.
- **vLLM** — the inference engine that serves the policy model.

The design is bring-your-own: benchmark, model, agent framework (Strands, AutoGen, LangChain), containers, and reward functions. The worked example uses:

- **Agent — BoxPwner**, an open-source LangChain agent with a shell tool and Python code generation, lightly optimized for security tasks.
- **Live environment — CyBench**, roughly 40 containerized web-application and static-code-analysis challenges in CTF style. Every RL run needs a verifiable reward at the end — a flag, a hash, or a specific CVE number. Brown argues most cyber tasks do carry such a reward, even if obtaining it is awkward.

A synthetic-data-generation module is included: define a world manifest (network topology and the end verifiable reward), then have a teacher model (an open-weight model such as GLM-5 or Kimi K2) distill training traces for the scenario.

## The three-stage pipeline

Trajectories are collected from stronger teacher open-weight models (GLM-5, Kimi K2) — full records of plans, tool calls, observations, and outcomes — and converted to a common **ChatML** format. Training then runs in three stages. Two change the weights; the third runs at test time.

| Stage | What it does | Touches weights |
|---|---|---|
| 1. SFT / instruction tuning | Teaches the tool-calling format and security terminology as the first alignment layer | Yes |
| 2. Online RL | Aligns behavior to expert trajectories via group-relative reinforcement | Yes |
| 3. GEPA prompt evolution | Evolves the system prompt at inference time using a teacher LLM | No |

**Stage 2 (online RL)** loads the policy into vLLM for inference and runs a separate trainer for the backward pass. For each task, **8 agents run the same challenge in perturbed parallel**. Each trace is scored by composite reward functions producing numbers between 0 and 1; the group mean is computed, the best trace is selected, and its knowledge is distilled into the weights. This is GRPO-style group-relative RL.

**Stage 3 (GEPA — Genetic-Pareto prompt evolution)** is an established external technique Brown applied here, not coined in this work. It does **not** change weights. After training, at test time, it evolves the otherwise-static system prompt using an out-of-band teacher LLM that reflects on past function calls and traces. The rationale: for a multi-stage objective, the system prompt should not stay fixed from step 0 to step 60.

## Reward design

Reward functions are the core of transferable training:

- **Binary** — did the agent get the flag or not. Simple, but a coarse measure of how well an objective was completed.
- **Composite** — signals such as temporal decay, information sparsity, and exploration/uniqueness, scoring the space between the start and end of a trajectory. A success that is noisy or non-unique does not transfer to production, so the reward has to discriminate *how* the objective was reached, not only whether.

The lesson Brown stresses is **progressive reward scaling** — easy, then medium, then hard — the way a human is trained up through CTF difficulty rather than starting at the expert level. In Q&A he refined this to: start with binary rewards, then work backwards toward progressive composite rewards.

## Results

Baseline BoxPwner on a dense **Qwen 3.5 27B** started at about **12.5%** solve rate on CyBench. After the two weight-changing stages (SFT, then SFT plus RL), it lifted to about **35%** solve rate, in under a day.

Brown is explicit that this is a recipe and framework demonstration, not a state-of-the-art model. It used only **285 open-source traces** distilled from stronger models, against the 20,000–30,000 traces typical for an SFT stage, with limited RL time on a side-project GPU budget. Next-size-up open-weight models such as MiniMax 2.5 or GLM-5 are 5x–10x larger than this 27B.

A case study: an AES-variant crypto challenge with a shuffled nonlinear layer required reverse-engineering a cipher from source, interacting with a network oracle, and writing a solver to recover the flag. The base Qwen model (released two days before the talk) failed after about 25 turns; the tuned model solved it in fewer turns.

## Lessons learned

- **Start with dense models.** The abstract had planned a GLM-4.7 Flash 30B fine-tune. Brown switched to Qwen 3.5 27B two days before the talk — partly because Qwen had just released and was more topical, but mainly because mixture-of-experts (MoE) models have **no open-source fix for weight synchronization across expert nodes during RL**. Dense is the safe starting point today.
- **Progressive reward scaling** beats starting hard.
- **Long horizons are expensive.** Large key-value caches and VRAM are needed not only to load a 27B model (about 60 GB at a low quant) but to give it context; loading the same model with 128K-token context at least doubles or triples that VRAM.
- **More data helps** — more SFT traces and more RL challenges generalize better.

Hardware was a home [[nvidia|NVIDIA]] DGX Spark for inference and local testing, plus cloud H200s for the Ray training cluster. Everything is open source, with a GitHub release and a Discord community.

For general ML tooling, Brown's framing: TRL is the low-level base everything builds on; Unsloth is NVIDIA-only and oriented to SFT; none of these solve MoE post-training today (TRL has open PRs blocked on vLLM backend issues).

## Placement

The talk is the open-weights complement to vendor agentic-SOC stacks such as Google's agentic SOC and Microsoft Security Copilot, described in the [[agentic-soc-state-of-the-field|Agentic SOC: State of the Field]] thesis. It argues a path that avoids frontier-API dependence and ships reproducible artifacts — a packaged training loop, reward functions, and a published recipe — that a third party can evaluate.

CyBench plays the role that [[defensebench|DefenseBench]] plays for defender agents: a shared, verifiable task surface against which specialized security agents can be compared. The atomic-versus-multi-stage gap Brown opens with is the same capability unevenness mapped in [[jagged-frontier|Jagged Frontier]], and his evaluation-gap framing converges with the eval-and-improvement-loop argument in [[measuring-agent-effectiveness-talk|Saxe's talk]] and the multi-stage-task evaluation problem in [[rethinking-security-agent-evaluation-talk|Khurana's talk]].

## Status

`summarized` — based on the transcript plus the slide deck captured to `.raw/talks/`. The released repository (Open Trajectory Gym on GitHub) has not been read in depth.
