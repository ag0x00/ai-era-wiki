---
type: framework
title: "CLASP"
created: 2026-04-30
updated: 2026-05-29
tags:
  - frameworks
  - evaluation
  - agentic-ai
  - redteam-for-ai
status: developing
source_url: "https://arxiv.org/abs/2510.01654"
scope_axis:
  - sec-of-ai
adoption_signal: unknown
last_substantive_update: ""
published_by: "Airbnb (Mudita Khurana)"
current_version: ""
first_published: "2026-03"
scope: "Multi-dimensional capability evaluation for autonomous security agents"
audience: "Security agent builders, evaluators"
aliases:
  - "Capability-Centric Evaluation for Security Lifecycle"
coined_by:
  - "[[airbnb]]"
related:
  - "[[airbnb]]"
  - "[[unprompted-conference-march-2026]]"
  - "[[llm-as-a-judge]]"
  - "[[evidence-centered-benchmark-design]]"
sources:
  - "[[.raw/talks/unprompted-conference-talks-mar-2026.md]]"
---

# CLASP

**CLASP** (Capability-Centric Evaluation for Security Lifecycle) is a capability-centric rubric for assessing autonomous security agents beyond outcome-only benchmarks. It originates in [[airbnb|Airbnb]] staff security engineer Mudita Khurana's [[unprompted-conference-march-2026|Unprompted March 2026]] talk, *"Rethinking How We Evaluate Security Agents for Real-World Use."* The argument: a single benchmark success does not predict real-world performance, because it omits how the agent reached the result. Evaluators must ask **how an agent achieved success**, not only whether it did. Security work is a connected **find → confirm exploit → patch → validate** loop, and agents that score well on isolated tasks often fail across multi-stage transitions.

The rubric evaluates agents across **six capabilities** — Planning, Tool Use, Memory, Reasoning, Reflection, and Perception — grading each from **1 (Minimal) to 5 (Adaptive)**.

> [!gap] Acronym and leveling scale unverified against the source
> The talk abstract describes "a practical, capability-centric framework" with observability into how agents plan, reason, use tools, and carry context across the security lifecycle. The "CLASP" acronym and the detailed five-level scale below come from this wiki's legacy migrated page (created 2026-04-30, originally unsourced); neither is confirmed verbatim from Khurana's talk. Treat the leveling detail as illustrative until the full talk slides or transcript are obtained.

### **The CLASP Leveling Scale**

#### **Level 1: Minimal (Brittle/Static)**

At this level, agents operate using rigid, pre-defined logic with no ability to adapt to changes in environment or context.

- **Planning:** Uses static prompts with brittle, linear flows.
- **Tool Use:** Fires a single default tool regardless of the situation.
- **Memory/Reasoning:** Exhibits no persistence beyond immediate context and provides "one-pass" answers with no intermediate reasoning.
- **Reflection/Perception:** No self-checking of work; processes only a single data source.

#### **Level 2: Scripted (Pattern-Based)**

Agents at Level 2 utilize reusable templates and show early signs of awareness, though they remain limited to simple, non-branching flows.

- **Planning:** Employs pattern-based plans for common cases.
- **Tool Use:** Selects from a small, fixed set of tools based on general task types.
- **Memory/Reasoning:** Maintains ephemeral session notes but often loses specifics; reasoning is linear without branching.
- **Reflection/Perception:** Only checks its work after a clear, detectable failure occurs; processes structured single data sources.

#### **Level 3: Structured (Context-Aware)**

This level represents the "AI-augmented" stage where agents begin to change how work flows by reacting to the environment.

- **Planning:** Searches through options with memory and can re-plan locally if a failure occurs.
- **Tool Use:** Correctly matches specific tools to the current context and parses their output accurately.
- **Memory/Reasoning:** Key findings persist with enough detail to act upon later; reasoning is evidence-driven and multi-hypothesis.
- **Reflection/Perception:** Checks correctness within each current step; joins heterogeneous data feeds.

#### **Level 4: Autonomous (Strategic)**

Agents at Level 4 are "budget-aware" and can manage complex, multi-stage operations without human intervention.

- **Planning:** Strategic planning that is aware of resource constraints like time, steps, and API costs.
- **Tool Use:** Chains tools together, feeding the output of one directly into the input of the next.
- **Memory/Reasoning:** Full findings with provenance carry across all stages of a security lifecycle; the agent is uncertainty-aware.
- **Reflection/Perception:** Adjusts overall strategy based on internal signals and performance; maintains a consistent view of topology and asset state.

#### **Level 5: Adaptive (Self-Improving)**

This represents the highest level of "AI-native" capability where agents update their own internal logic based on outcomes.

- **Planning:** Automatically updates heuristics and policies based on outcomes from across different tasks.
- **Tool Use:** Discovers new tool combinations and recovers autonomously from unexpected tool failures.
- **Memory/Reasoning:** Accumulates and cross-references knowledge across multiple "episodes" or incidents; reasoning is causally aware and self-corrective.
- **Reflection/Perception:** Engages in long-term strategic reflection; updates its own internal world/state model in near real-time.

### **Context in Agent Evaluation**

The larger goal of this leveling is to identify **skill attribution**—understanding which capabilities are required for specific security tasks. For example, research found that for enumeration-heavy tasks like reconnaissance, breadth (Planning and Tool Use) matters more than reasoning depth. By using these rubrics (either via **[[llm-as-a-judge|LLM-as-a-judge]]** or **[[evidence-centered-benchmark-design|Evidence Centered Benchmark Design]]**), teams can move from "vibe-based" engineering to rigorous, statistical improvement of their agents. Practitioners are urged to only ship agents when they meet both outcome success and a **minimum capability threshold** for their assigned task.

<!-- sources:auto -->
## Sources

- [CLASP](https://arxiv.org/abs/2510.01654)
<!-- /sources -->
