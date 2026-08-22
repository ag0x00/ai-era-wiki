---
type: talk
title: "Guardrails Beyond Vibes: Shipping Security Agents"
created: 2026-05-03
updated: 2026-08-21
tags:
  - papers
  - talks
  - production-agents
  - threat-modeling-agent
  - security-routing-agent
  - llm-as-a-judge
  - evaluation-pipeline
  - multi-agent-architecture
  - human-in-the-loop
  - hallucination-mitigation
  - stripe
  - unprompted-2026
status: summarized
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
year: 2026
authors: ["Jeffrey Zhang", "Sid Shah"]
venue: "Unprompted Conference, San Francisco — Stage 1 Lecture 05, Tuesday March 3, 2026, 10:45"
no_public_url: "conference-only, slides + transcript via attendee Google Drive share"
archived_copy: ".raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_transcript.md"
key_claim: "Shipping security agents in production requires moving well beyond 'vibes' (manual inspection on individual runs) to a rigorous offline evaluation pipeline using golden-standard test cases + LLM-as-a-Judge semantic scoring. Architecture must be matched to task complexity: a modular multi-agent sequential pipeline for subjective open-ended threat modeling, but a single focused agent with a minimal toolset for the simpler triage task of security routing. Humans remain in the loop not as a fallback but as a structural co-author of the gold standard and a final reviewer; AlphaEvolve-style automated prompt evolution failed at Stripe's cost constraints because language is too open-ended for evolutionary search."
methodology: "Practitioner talk by a Stripe Security Engineer (Jeffrey Zhang) and Software Engineer (Siddh Shah). Describes two production agentic systems built by Stripe's security team: a threat modeling agent and a security request routing agent. Talk covers architecture design decisions, quality measurement via evaluation pipelines, phased user-facing rollout, and five concrete learnings. Day 1 of Unprompted (March 3, 2026); the companion Bullen talk on containment architecture was delivered the following day."
contradicts: []
supports:
  - "[[llm-as-a-judge]]"
  - "[[hitl]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[least-agency-principle]]"
related:
  - "[[jeffrey-zhang]]"
  - "[[sid-shah]]"
  - "[[stripe]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[andrew-bullen]]"
sources:
  - .raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_slides.pdf
  - .raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_transcript.md
aliases:
  - papers/guardrails-beyond-vibes-zhang-shah-talk
  - guardrails-beyond-vibes-zhang-shah-talk

---

# Guardrails Beyond Vibes: Shipping Security Agents in Production

**Source:** Unprompted Conference 2026, Stage 1 Lecture 05 (Jeffrey Zhang + Siddh Shah, Stripe). Transcript via attendee [Google Drive share](https://drive.google.com/file/d/1_3tB6w3FXYLarmHi4Cz9f0B-1iKgnfuW/view); slides PDF ingested 2026-05-03. Local copies: `.raw/talks/2026-03-03_Jeffrey-Zhang-and-Sid_Guardrails-beyond-Vibes_{transcript.md,slides.pdf}`.

A practitioner's account of putting two security agents into production at [[stripe|Stripe]], delivered by [[jeffrey-zhang|Jeffrey Zhang]] (Security Engineer) and [[sid-shah|Siddh Shah]] (Software Engineer). This is a **Day 1 talk** (Tuesday March 3, 2026) — the companion Stripe talk on containment architecture by [[andrew-bullen|Andrew Bullen]] ([[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta]]) was delivered the next morning. Together, the two talks cover the full shape of Stripe's internal AI security practice: Zhang + Shah focus on *how to build and evaluate* security agents; Bullen focuses on *how to contain* them once deployed.

This summary fuses the transcript and 14-slide deck. Slide-only contributions (architecture diagrams, the AlphaEvolve prompt-variation table, the phased-rollout flow, the garbage-input/output example, speaker LinkedIn handles) are noted where they add material the transcript does not.

## The two problems Stripe is solving

Slides and transcript are fully aligned on the framing. Two distinct problems drove two distinct agent designs:

| Agent | Problem | Constraint |
|---|---|---|
| **Threat Modeling Agent** | Too many security review requests; not enough security engineer time. "Rise of AI in the SDLC" is accelerating demand for security guidance. | Subjective output — no single correct threat model. Must match security-engineer-level threat coverage. False positives/hallucinations compound the backlog. |
| **Security Routing Agent** | Too many security teams at Stripe; developers need to reach the right one without friction. Hallucinations route developers to the wrong team, recreating the problem. | Open-ended question space. Keeping internal team info current without manual overhead. |

**Two agents, two design philosophies.** The Threat Modeling Agent is **async/batch** (a security review ticket comes in; the agent can take as long as it needs) and uses a **modular multi-agent sequential pipeline**. The Security Routing Agent is **conversational/real-time** (~30 s target) and uses a **single focused agent with a minimal toolset**. The same team shipped both, and learned that no architecture is universally correct — task complexity, latency requirements, and output determinism each select a different design.

## Architecture

### Threat Modeling Agent — modular sequential pipeline

The slide (Slide 5, "Agentic Design: Threat Models") shows a four-layer sequential pipeline:

```
Orchestrator Agent
    │  (security_review_category, ticket_identifier)
    ▼
Input Agents
    │  (additional_input_context — Google Docs, Slack threads, etc.)
    ▼
Security Child Agents  [run in parallel]
    │  (security_findings: [agent 1, agent 2, ...])
    ▼
Output Agents
    │  (threats, invariants → summarized / MITRE / conversational)
```

**Why sequential (not autonomous orchestrator)?** The team found that when the orchestrator was given too much agency, it did not reliably invoke the relevant specialized sub-agent. Sequential structure enforces predictable ordering — Input → Security (parallel) → Output — without sacrificing the parallelism within the security tier. They describe a future hybrid: a core baseline of required security agents determined by human input, plus orchestrator-driven expansion for vague review categories.

**Balancing determinism vs non-determinism:** Each security child agent has a "core baseline of required questions" — domain-specific invariants the threat model must address regardless of how the LLM reasons (e.g., data sensitivity, transport protocols, auth story for a third-party review). This is not a constraint on breadth, but a floor on required coverage.

**Internal guidance tools:** The team explicitly prioritized company-specific guidance tools over generic LLM knowledge. "LLMs are good at giving more generic security context, but what's really powerful is having company-specific guidance that aims to provide risks and mitigations that can be actionable." (Transcript.)

**Output multiplexing:** The canonical internal representation is semantics-first (risks and mitigations described in natural language). From this, Output Agents produce:
- **Summarized format** — for human reviewers
- **MITRE Framework format** — for the threat modeling tool used as the source of truth for metrics
- **Conversational Agent handoff** — for incomplete/vague tickets that need follow-up

### Security Routing Agent — single focused agent with minimal toolset

| Phase | Design | Runtime | Problem |
|---|---|---|---|
| **V1** | One-step LLM call, no tools, static context in prompt | Fast | Hallucinated on internal terms and Stripe-specific tooling; no RAG |
| **V2** | Agentic; many tools provided to let it self-research | Accurate | ~10 minutes; not conversational |
| **V3** | Iterative tool pruning — started wide, removed one at a time, re-scored | **~30 s** | Minimal toolset that preserved accuracy |

The slide headline (Slide 6, "Agentic Design: Security Routing") labels the extremes: "Fast & Focused Agent (Completed: 30 sec)" vs "Slow & Versatile Agent (Processing: 10%… Est. 10 min)." The target was ~30 s, conversational. The method to reach it: iterate down to the minimum toolset that preserved acceptable accuracy — "through a slow and iterative process of just taking a set of baseline questions we knew the answer to, testing the agent, plucking tools one by one."

Final toolset for the routing agent: **two tools** (specific internal tools, not publicly named).

## Quality and evaluation

### Threat modeling: why deterministic matching failed

Slide 7 ("Evaluating Agents: Threat Modeling") frames the problem directly:

| Approach | Method | Failure mode |
|---|---|---|
| **Deterministic matching** | Compare MITRE categories; pattern-match keywords | "Correct risk, wrong label = failure." The agent could identify the right risk but label it differently across two runs. |
| **LLM-as-a-Judge** | Semantic equivalence scoring | Captures whether the right risk was *conveyed*, regardless of label or phrasing. |

The chosen pipeline (slide diagram):

```
[Gold-Standard Test Cases] → [LLM-as-a-Judge Scorer] → [Iterate: Prompts → Models → Accuracy]
```

**The circularity problem and its resolution:** The team acknowledged the circular dependency: "We don't trust the LLM to give 100% correct threat models — so why trust it to evaluate them?" Their resolution: humans write the golden-standard test cases from past security reviews (human judgment on what constitutes a complete, correct threat model); the LLM is only tasked with semantic equivalence scoring between the expected output (gold standard) and actual output. "We wanted to take advantage of what humans are good at — creating golden standard test cases — and what LLMs are good at — semantic reasoning." Human judgment defines the content of ground truth; LLM judgment evaluates semantic match.

**What the eval pipeline unlocked (three uses):**

1. **Prompt engineering guidance** — identified low-scoring test cases, used them to guide prompt improvements rather than overfit (e.g., adding "always consider authorization and SSO as security domains" → +10% accuracy).
2. **Model selection** — created a mega-dataset (duplicated golden cases to reduce non-determinism variance) and used it to benchmark and swap in the best-performing base LLM → +10% accuracy.
3. **Regression detection** — the critical use. Adding JSON formatting instructions to the prompt looked fine on individual runs but showed -10% overall accuracy on the eval pipeline. Without the eval pipeline, that regression would have shipped. "This eval pipeline really gives us confidence in the changes we make to our prompt."

### Evaluation: threat modeling accuracy threshold decision

The team explicitly wrestled with what accuracy threshold to target before going user-visible:

> "If your agent is directly sending these threat models to engineering teams and making super noisy integrations, if you don't have very high accuracy, you're gonna get a lot of slack. Versus, if we add a human in the loop where the agent gets you 90% of the way there, but you still have that last step of a human confirming threats and mitigations are applicable, we can go for slightly less accuracy."

Decision: human-in-the-loop review as the model. Initial accuracy target: **~80%**, with continued iteration post-launch.

### Evaluation: security routing — user-feedback loop

Security routing used a different evaluation model because of its open-ended nature. No fixed gold standard exists for "which team should handle this question?" — instead, the team ran a **phased rollout with progressive user exposure** as the primary signal:

| Phase | Surface | Purpose |
|---|---|---|
| **Phase 1** | Internal webpage | Team-only; initial feedback |
| **Phase 2** | Slack | Context-dependence testing; broader internal |
| **Phase 3** | Internal Chat UI for agents at Stripe | All developers; production exposure |

Supplemented by demos and outreach. Slide 9 conclusion: "Iterative, open-ended cycle > Structured pipeline for dynamic security nature."

## Meeting users where they are

Three design choices made the agents adoptable:

**1. Phased rollout (threat modeling):** Began with a specific sub-category of security reviews where the domain was most constrained (similar risks, similar mitigations). Shadow mode first; only promoted to user-visible once eval pipeline showed acceptable accuracy. Starting narrow lets the team iterate without user impact.

**2. Multiple consumption methods (threat modeling):** The same semantics-first canonical risk representation is rendered differently for different audiences:
- Summarized format for security engineer review
- MITRE Framework format for the threat modeling tool (metrics source-of-truth)
- Conversational agent handoff for incomplete tickets

Slide 10 shows the JSON structure for the "operating with imperfect information" case (slide-only contribution):

```json
{
  "risk": "Unauthenticated inbound webhooks could allow spoofed transaction status updates.",
  "questions": [
    "Are inbound webhooks verified using cryptographic signature validation before processing?"
  ],
  "status": "Unknown"
}
```

The agent surfaces the risk it can identify, lists what information would resolve the unknown, and sets `status: "Unknown"` rather than hallucinating a resolution. "The threat model agent should call out where the missing information is, that the status is unknown, and set the security engineers up to bring it to the next step."

**3. Operating with imperfect information:** The agent is explicitly taught to behave like a security engineer who knows when to say "I don't know." Slide 13 ("Garbage in always means garbage out") shows a concrete example — a vague ticket ("Add webhook retries. Make delivery more reliable. Ship this week.") — and the contrast between initial (hallucinates "AES-256 encryption rotated every 24 hours") and taught behavior ("Not enough info to review security. The ticket does not describe the retry design or security controls. Needs more information.").

## Learnings (five)

### 1. Automated prompt evolution sounds cool — results say otherwise

The team tried **AlphaEvolve** (Google DeepMind's LLM-based algorithm/prompt evolution tool). Slide 12 shows the actual prompt variations produced:

| Column | Content |
|---|---|
| **Base Prompt** | "You are a support assistant for Stripe users. Answer the user's question directly. If you are unsure, say so clearly. Do not invent product features or policy details." |
| **Variation #1** | Added "but clearly" — no semantic change |
| **Variation #2** | Full paraphrase — no semantic change |

Finding: AlphaEvolve works well for mathematical/computational domains with constrained permutation spaces. For natural language, the open-ended search space makes it generate semantically equivalent variations at high compute cost. "We found that it didn't really work great, especially within the realms of the cost we were able to handle."

**AlphaEvolve failure mode for prompt engineering.** This is a rare public data point on the limits of automated prompt evolution tools. The failure mode is specific: evolutionary search produces paraphrases that are semantically equivalent but score identically, consuming many evaluation runs to learn nothing. The lesson generalizes: automated prompt optimization requires a search space that is either constrained (code, math) or has a strong fitness signal to distinguish nearby variants.

### 2. Humans in the loop aren't optional

"Eval pipelines validate — humans still discover." (Slide 13.) The HITL is not a fallback for when the agent fails — it is a structural component. The agent gets to ~80% on the eval pipeline; the security engineer reviews the AI-generated threat model, pushes forward applicable threats, and denies irrelevant ones. The eval pipeline defines the floor; human review is the ceiling.

### 3. Invest in your eval pipeline early

"Multiple test cases and a good scorer compound over time." (Slide 13.) If the team had waited to invest in the eval pipeline, edge-case-driven prompt engineering would have produced an overfit, inconsistently-accurate mess. The pipeline is the foundation — everything else (prompt improvements, model swaps, hallucination detection) builds on it.

### 4. Agent architecture depends on the task

"Specialized sub-agents help focused tasks, hurt open-ended ones." (Slide 13.) The multi-agent sequential pipeline was the right choice for threat modeling (well-defined sub-tasks, deterministic ordering required). It was the wrong choice for security routing (open-ended question space, latency requirements). No single architecture is universal.

### 5. Garbage in always means garbage out

"Your agent is only as good as the data and tools behind it." (Slide 14.) Vague, incomplete tickets produce hallucinated threat models unless the agent is explicitly trained to refuse to hallucinate and instead surface its unknowns. The fix is not architectural — it is behavioral: teach the agent that "I don't know" and "needs more information" are correct outputs.

## Q&A highlights (transcript only)

1. **Are you measuring accuracy only, or other metrics?** The answer reveals a planned iteration: currently offline accuracy (pre-release, against the golden set). The next step is **online evaluation** — when a new security review is threat-modeled by the agent, route the AI-generated findings into the existing threat modeling tool, let users click approve/deny on each threat, and harvest those interactions as online feedback signal. This converts the binary eval pipeline into a continuous learning loop.

2. **AlphaEvolve / Maestro:** Two audience members raised the **Maestro threat modeling framework** as a potential extension. Speakers acknowledged it could apply to AI-specific scenarios in future iterations; current work builds on existing MITRE-format processes.

## Slides-only vs transcript-only — what each input added

| Slides only | Transcript only |
|---|---|
| Architecture diagram: Orchestrator → Input → Security Child (parallel) → Output, with state labels (`ticket_identifier`, `additional_input_context`, `security_findings`, `threats/invariants`) | The "why sequential?" explanation — over-agentic orchestrators skipped specialized sub-agents |
| "Fast & Focused vs Slow & Versatile" labels with "Completed: 30 sec" / "Est. 10 min" visual | The V1 → V2 → V3 routing agent design history (pure prompt → fully agentic → minimal toolset) |
| Slide 7 comparison table (Deterministic Matching vs LLM-as-a-Judge) | The "circular dependency" framing and its resolution (humans write gold standard, LLM scores semantic match) |
| The JSON `status: "Unknown"` output structure for imperfect information handling | The 80% accuracy threshold decision rationale (HITL as the compensating mechanism) |
| AlphaEvolve prompt variation table (base / variation 1 / variation 2 actual text) | The circularity problem statement and resolution |
| Phased rollout flow diagram (Webpage → Slack → Internal Chat UI + demos/outreach feedback loops) | The online eval future direction (approve/deny feedback from the threat modeling tool) |
| Garbage-input/garbage-output slide (ticket text + initial vs taught response) | Maestro framework audience Q&A |
| LinkedIn handles: `siddhshah25` and `j778zhan` | |

## Placement in this wiki

| Wiki artifact | What this talk contributes |
|---|---|
| [[llm-as-a-judge\|LLM-as-a-Judge]] | First detailed production case study in the wiki. Resolves the circularity problem (human-curated gold standard + LLM semantic scoring). Three distinct uses: prompt engineering guidance, model selection, regression detection. The concept page was a stub; this talk fills it substantially. |
| [[hitl\|Human-in-the-Loop (HITL) for Agentic AI]] | Concrete accuracy-threshold reasoning: ~80% is acceptable with HITL; higher threshold required without it. Eval pipelines validate, humans discover. |
| [[stripe\|Stripe]] | Second Stripe talk now ingested. The "Guardrails beyond Vibes" catalog row in the Stripe entity page now has a full summary. Jeffrey Zhang and Siddh Shah added as named contributors. |
| [[breaking-the-lethal-trifecta-talk\|Breaking the Lethal Trifecta]] | Companion talk; cross-reference added. Bullen's talk = containment architecture; this talk = agent quality and evaluation. Both are needed for a complete picture of Stripe's AI security practice. |
| [[unprompted-conference-march-2026\|Unprompted Conference March 2026]] | Catalog row for this talk now has a full summary reference. |
| [[agentic-ai-security-cmm-2026\|Agentic AI Security CMM]] | CMM D7 (Observability & Detection) gains a production example: golden test cases, LLM-as-a-Judge, and an online feedback loop form a full offline-plus-online measurement program. CMM D3 (Control & Least-Agency) gains the "architecture must match task" evidence, which is the least-agency choice stated as a design rule. |

## Open questions this talk raises

> [!gap] Online eval loop not yet described publicly
> The planned online feedback loop (approve/deny on threat findings feeding back into scoring) would be a significant operational pattern — effectively a human-supervised RLHF loop for security agents. No implementation details are available as of March 2026.

> [!gap] AlphaEvolve benchmark: what "cost" means
> The talk says AlphaEvolve was tried but not viable "within the realms of the cost we were able to handle." This cost is not quantified (API calls? Latency? Engineering time?). Understanding what makes evolutionary prompt optimization viable vs non-viable requires knowing the cost/accuracy frontier more precisely.

> [!gap] Minimum-toolset selection method not fully generalizable
> The security routing agent's toolset was reduced iteratively (pluck one tool, re-score). This brute-force method assumes a small enough initial toolset to prune exhaustively. How does this scale to agents with dozens of tools?

## See also

- [[llm-as-a-judge|LLM-as-a-Judge]] · [[hitl|Human-in-the-Loop (HITL) for Agentic AI]]
- [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta (Without Ruining Your Agents)]] — companion Stripe talk (Day 2; containment architecture)
- [[stripe|Stripe]] · [[jeffrey-zhang|Jeffrey Zhang]] · [[sid-shah|Siddh Shah]] · [[andrew-bullen|Andrew Bullen]]
- [[unprompted-conference-march-2026|Unprompted Conference — AI Security Practitioner Conference (March 3–4, 2026)]]
