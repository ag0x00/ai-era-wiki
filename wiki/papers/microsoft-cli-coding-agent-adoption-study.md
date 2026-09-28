---
type: paper
title: "Microsoft CLI Coding Agent Adoption Study"
address: c-000241
created: 2026-07-30
updated: 2026-09-25
tags:
  - papers
  - agentic-coding
  - adoption
  - measurement
  - microsoft
status: summarized
scope_axis:
  - sec-of-ai
origin: aggregated
year: 2026
authors:
  - "Emerson Murphy-Hill"
  - "Jenna Butler"
  - "Alexandra Savelieva"
venue: "arXiv:2607.01418v1"
source_url: "https://arxiv.org/html/2607.01418v1"
key_claim: "Command-line coding agents spread through a team by organizational proximity, and their merged-pull-request lift persists over four months with no statistically distinguishable decay."
methodology: "Discrete-time logistic regression on an engineer-week panel for adoption; Bayesian structural time-series with synthetic control plus within-person fixed-effects Poisson regression for outcomes; tens of thousands of Microsoft engineers, 2026-01-05 to 2026-04-29."
related:
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[collaboration-paradox|The Collaboration Paradox]]"
  - "[[metr-rct-2025|METR RCT 2025]]"
  - "[[vibe-coding|Vibe Coding]]"
  - "[[pwc-stage-coverage-tiers|PwC Stage-Coverage Tiers]]"
  - "[[shadow-automation|Shadow Automation]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
sources:
  - https://arxiv.org/html/2607.01418v1
verified: 2026-09-25
verified_against: []
verified_findings: 0
verified_note: "Read whole against arXiv 2607.01418v1 (HTML, fetched 2026-09-25): 4 fixed (dose-response +15.0% is at three days, the survey supplies themes not covariates, the Meta figure is Fortune's estimate of more than $1.4M, key_claim's tool-merit contrast unsupported); none open"
---

# Microsoft CLI Coding Agent Adoption Study

**Source:** [arXiv:2607.01418v1 — *Adoption and Impact of Command-Line AI Coding Agents: A Study of Microsoft's Early 2026 Rollout of Claude Code and GitHub Copilot CLI*](https://arxiv.org/html/2607.01418v1) (2026-07-01), Emerson Murphy-Hill, Jenna Butler, and Alexandra Savelieva, Microsoft. CC BY 4.0.

## Key Claim

Two findings sit side by side. Who starts using a command-line coding agent is predicted best by who around them already does, and the merged-pull-request lift the tools produce does not fade across a four-month observation window.

## Methodology

The observation window runs 2026-01-05 to 2026-04-29 over a 13-week pre-period and a roughly four-month post-period. The adoption half fits a discrete-time logistic regression on an engineer-week panel with week and division fixed effects, predicting first use and retention (defined as use on 5 of 14 days from first use). The outcomes half combines a Bayesian structural time-series model with a synthetic control against a within-person fixed-effects Poisson regression, with a dose-response term for tool-use days per week. Telemetry and HR records supply the covariates, and a 609-response internal survey supplies qualitative themes and quotes.

## Notable Findings

**Adoption is a social process.** Having more than a quarter of skip-level peers already using the tool raises the odds of first use by **216%** — the largest single predictor in the model. Direct-manager use adds **82%**; reviewer peers above the same quarter threshold add **54%**. Prior IDE Copilot use at 60-plus pre-period days adds **83%**, and engineers already creating two or more pull requests per week add **34%**. Junior individual contributors are *less* likely to start (−13% to −14%); senior individual contributors at the IC5 band are more likely (+22%).

**Retention inverts one of those signals.** Prior IDE Copilot use, the second-strongest predictor of *starting*, is associated with **12% to 15% lower** retention. Familiarity with the completion-style tool brings engineers to the CLI agent and then does not keep them there.

**Merged-PR lift, and its shape.** The synthetic-control estimate is **+24.0%** merged pull requests per engineer per day (95% CI +14.5% to +33.7%), with no statistically significant decay: **+29.4%** in February against **+20.0%** across March and April. Dose-response is monotone and well separated — **+15.0%** at three tool-use days per week rising to **+50.1%** at five or more. The within-person comparison separates the two tools: Copilot CLI **+24.9%**, Claude Code **+11.4%**, a 2.2× difference at *p* < 0.0001.

**Nothing on security.** The paper reports no permission-model, approval-workflow, or incident observations, and acknowledges its own quality gap — *"the field still lacks agreed-upon measures."* For cost, it quotes Fortune's estimate that one Meta employee's token use over 30 days could have cost more than \$1.4 million.

## Strengths and Weaknesses

The design is the strongest yet published on this question: a within-person estimator, a synthetic control, a placebo test returning −1.1%, and a dose-response curve. The population is large and the instrumentation is real telemetry rather than self-report.

The result carries three cautions. Merged pull requests count throughput, not quality or complexity, so a result showing more merged PRs is consistent with smaller PRs and heavier review load, and the paper does not rule that out. Adoption is measured at one organization in one early window, and Microsoft is not a representative deployment environment. The tool comparison is not an experiment — engineers chose which agent to use, so the 2.2× gap is confounded by who chose what and for which work.

## Relations

- Supplies the adoption-side numbers for [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]. The strongest predictor of who tried Copilot CLI in a given week was whether the engineer's peers, especially the skip-level group, had already tried it. The study measured a sanctioned rollout and compares no governance program, so the reading that rollout outruns policy is the vault's.
- Complicates [[metr-rct-2025|the METR RCT]], whose randomized design found experienced open-source developers slower with AI assistance. The two are not in direct conflict, since the populations, tools, and outcome measures all differ, but neither result should be treated as settled.
- Reinforces [[shadow-automation|Shadow Automation]] by the vault's inference: a tool that spreads through peers can reach a codebase before a security team enumerates it.
- Refines [[collaboration-paradox|The Collaboration Paradox]] with a persistence result: the lift held for four months rather than reverting after novelty.
- Caps what [[securing-agentic-coding|Securing Agentic Coding]] can claim. Every control in that catalog governs what an agent may *do*, not whether the code is correct, and a sustained throughput lift makes review capacity the binding constraint that no control in the catalog addresses.
