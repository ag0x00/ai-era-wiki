---
type: talk
title: "1.8M Prompts, 30 Alerts"
created: 2026-05-03
updated: 2026-08-21
tags:
  - papers
  - talks
  - agentic-ai
  - observability
  - behavioral-anomaly-detection
  - salesforce
  - agentforce
  - soc
  - unprompted-2026
status: summarized
scope_axis:
  - ai-in-sec-defense
  - sec-of-ai
year: 2026
authors: ["Matt Rittinghouse", "Millie Rittinghouse"]
venue: "Unprompted Conference, San Francisco — Stage 2 Lecture 08, Wednesday March 4, 2026, 13:30"
source_url: "https://drive.google.com/file/d/1DXrm-IAbkmtvqs482Bna-PgJqR-73bih/view"
archived_copy: ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
key_claim: "A three-level ensemble behavioral anomaly detection model applied to Salesforce Agentforce at scale — monitoring 12,000+ unique daily active agents across 55,000 tenant organizations — reduced approximately 1.8 million daily prompts to fewer than 30 actionable security alerts, with near-real-time scoring built on incremental historical profiling."
methodology: "Practitioner talk by two members of Salesforce's Cybersecurity Operations Center. Case study of production deployment of behavioral anomaly detection for the Agentforce platform. Architecture walk-through (three identity levels for anomaly context, ensemble model, data features), lessons learned from over-engineering failures, and roadmap toward inline auto-containment."
contradicts: []
supports:
  - "[[agent-observability]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[multi-agent-runtime-security]]"
related:
  - "[[unprompted-conference-march-2026]]"
  - "[[salesforce]]"
  - "[[matt-rittinghouse]]"
  - "[[millie-rittinghouse]]"
  - "[[agentforce]]"
  - "[[behavioral-anomaly-detection-for-agents]]"
  - "[[prompt-volume-to-alert-ratio]]"
  - "[[agent-observability]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[securing-workspace-genai-at-google-talk]]"
sources:
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_transcript.md"
  - ".raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_slides.pdf"
aliases:
  - papers/1-8m-prompts-30-alerts-rittinghouse-talk
  - 1-8m-prompts-30-alerts-rittinghouse-talk

---

# 1.8M Prompts, 30 Alerts: Hunting Abuse in a User-Defined Agent Ecosystem

**Source:** Unprompted Conference 2026, Stage 2 Lecture 08 (Matt Rittinghouse + Millie Rittinghouse, Salesforce Cybersecurity Operations Center). Transcript via attendee [Google Drive share](https://drive.google.com/file/d/1DXrm-IAbkmtvqs482Bna-PgJqR-73bih/view). Local copies: `.raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_{transcript.md,slides.pdf}`.

> [!gap] Speaker name discrepancy
> The raw file, transcript title, and Google Drive share identify the speakers as "Millie and Matt Rittinghouse." However, the [[unprompted-conference-march-2026|Unprompted conference catalog page]] (built from the published agenda) attributes this talk to "Matt Rittinghouse + Millie Huang, Salesforce." Millie's last name may be Huang rather than Rittinghouse, or the published agenda may carry a typo. This page follows the transcript metadata (Rittinghouse for both); correct when confirmed.

Matt and Millie are both members of Salesforce's **Cybersecurity Operations Center**, working on the security of Agentforce — Salesforce's agentic platform deployed internally and across customer tenants.

**The headline number.** ~1.8 million daily prompts across the Agentforce platform, filtered through a behavioral anomaly detection model, yield **fewer than 30 actionable security alerts per day** — a signal-to-noise ratio of roughly 60,000:1. This is arguably the most concrete production-scale signal-to-noise figure published for agentic AI SOC operations as of early 2026.

## Scale and problem context

Salesforce's Agentforce platform operates at production scale:

- **55,000 tenant organizations** monitored daily
- **12,000+ unique daily active agents** (varying per-tenant custom logic)
- **~1.8 million daily prompts** flowing through the platform

The scale creates what the speakers call a **"black box defense challenge"**: no two agents behave identically (each is custom-built per tenant), so static signatures and rule-based detection are structurally inadequate. Defense cannot rely on knowing the "right" behavior in advance; it must learn what normal looks like and flag deviations.

A second constraint is **data privacy**: Salesforce cannot inspect prompt content because that is customer data. Defenses must operate on behavioral and execution metadata, not on the semantic content of user prompts.

The defense architecture therefore operates at the **execution layer** — sitting outside but coordinated with the reasoning layer — and focuses on *what the agent does* (tool calls, data access patterns, API invocations) rather than *what the agent says*.

## Threat taxonomy: two buckets

The talk organizes the threat landscape into two categories:

1. **Platform-target attacks** — Attackers find flaws in the platform's implementation. Examples: misconfigured permission sets, vulnerabilities in custom APEX skills exposed to the reasoning engine. These are exploits of the platform itself.

2. **Abuse of legitimate agency** — Threat actors use the agent's built-in, valid capabilities to perform actions that are individually authorized but malicious in context. This is the harder and more novel category.

The second category is the primary design target for the behavioral model. Traditional content moderation catches "bad words" but cannot distinguish a power user from a compromised agent operating within normal permissions.

## Insufficiency of content moderation alone

The speakers identify three structural gaps in content-moderation-only defenses:

1. **Reasoning vs. execution blindness.** Content moderation polices the reasoning layer (prompt/response dialogue), but cannot see the execution layer (actual system calls, API invocations, data queries). A prompt deemed safe can still trigger an agentic plan with unauthorized data access or privilege escalation.

2. **The blocking dilemma.** Content moderation often captures malicious intent but lacks the high-fidelity signal needed for automated inline blocking in an enterprise multi-tenant context. A behavioral model provides the statistical confidence necessary to move from "flag for review" to "automated response."

3. **Post-generation blindness.** Even a safe prompt can generate an agentic plan that involves unauthorized data access or privilege expansion. Traditional content filters cannot observe or act on the execution-layer consequences of an approved prompt.

## The detection architecture: three-level ensemble model

Matt Rittinghouse describes the core architecture as a **three-level ensemble model** combining anomaly signals at three identity axes:

| Level | Entity | What it measures |
|---|---|---|
| 1 | **User** | Per-user behavioral baseline (historical UEBA framing) |
| 2 | **Agent** | Per-agent behavioral baseline (new axis enabled by agentic systems) |
| 3 | **Organization** | Per-tenant behavioral baseline (Salesforce's multi-tenant context) |

The insight is that agentic systems create a *new* identity axis (the agent itself) that traditional UEBA lacked. Combining signals across all three levels allows noise from one axis to be suppressed by the others. This is the ensemble aspect: each individual signal may be noisy in isolation, but three correlated deviations are much harder to generate by chance.

**The new anomaly axis.** Prior to agentic AI, anomaly detection operated at the user level (and sometimes at the table/entity level). Agentforce introduces a second axis: **agent-level behavioral profiling**. Anomaly detection at the agent-in-organization context yields substantially lower false-positive rates than user-level detection alone because agent logic is more constrained and predictable than human behavior.

## Feature design: what worked

The model uses features derived from **data access patterns** along two dimensions:

**Breadth features (potential exfiltration detection):**
- Frequency of database calls per agentic session
- Proportion of total data accessed (is the agent pulling the entire dataset or a single record?)
- Ratio analysis to distinguish targeted access from bulk exfiltration

**Depth / sensitivity features:**
- Rarity of accessed data (how frequently do agents normally touch specific columns or fields?)
- PII / sensitive-data field flags (Salesforce exposes feature flags for PII-marked fields)
- Column-level access frequency profiles — agents that suddenly access fields they have never previously touched

**Temporal profiling:**
- Daily incremental historical profiling — every session builds the rolling baseline
- Standard-deviation scoring against that baseline — alerts are generated when deviations exceed statistical thresholds
- New-agent warm-up period: expect noise for approximately the first 14 days of a new agent's life

The scoring is deviation-based: every axis yields a confidence-interval-style measure of "how far outside normal is this session?" The overall alert priority is a function of combined deviations.

## Feature design: what did not work

Two notable failures from over-engineering:

**Query complexity calculator (abandoned).** The team built a custom query complexity scoring system, expecting that malicious actors would construct complex queries to exfiltrate data. The failure: agents themselves construct the queries, so complexity reflects agent design choices more than malicious intent. The lesson: *make sure your features are measuring what you think they are measuring*.

**Multi-table joins (refactored away).** The original model required expensive computational joins across multiple metadata tables to get contextual information about executed queries. After profiling, the team refactored to operate on a single table. Result: model retraining time reduced to one-third.

The meta-lesson: **simple features with strong predictive power beat complex features with low interpretability.** Invocation count and sensitive-asset frequency produced higher-fidelity signals than the custom query complexity calculator, at a fraction of the engineering cost.

## The alert and auto-containment pipeline

The current (pilot) detection pipeline runs in **batch mode** with 12–24 hour detection latency. The architecture is designed for a future **hot-path** upgrade:

1. **Batch training** (current): daily behavioral baselines built overnight; inference scores sessions against prior-day baselines.
2. **Hot-path target** (roadmap): push behavioral baselines into a high-speed cache; score sessions in-flight during execution.
3. **Auto-containment trigger** (roadmap): if a session crosses a "statistically impossible" threshold, skip SOC triage — kill the session, revoke the token, or trigger a bot-level lockdown immediately.

The current model yields **fewer than 30 active alerts per day** requiring investigation, from a starting population of ~1.8M daily prompts.

### Agentic triage: LLM-as-alert-explainer

Alerts are structured as JSON payloads, then fed to a **secondary agent** that synthesizes a plain-English explanation:

- The LLM consumes the feature vector and explains *why* this session is anomalous
- The explanation is customer-facing — investigators do not need a security or data-science background to understand the alert
- This is the model's interpretability design choice made explicit: the primary detection algorithm is intentionally simple (distance-based) so that the LLM explainer can faithfully render the rationale

**LLM-as-alert-explainer as a design choice.** The team made an explicit decision to sacrifice potential accuracy gains from a more complex algorithm to maintain interpretability. The simple distance-based algorithm can be explained by an LLM accurately; a deep neural network cannot. This is a concrete production example of the accuracy-vs-interpretability trade-off resolved in favor of interpretability for enterprise SOC use.

## Lessons learned

The speakers close with four operational lessons:

### 1. Observability gap (the "identity multitasking" lesson)

Early on, Salesforce had engineering logs that recorded *what* happened but not *who or which agent* caused it. Security telemetry lacked the structured events linking `invoking_user_id` to `agent_id`. Without this join, distinguishing a power user from a compromised agent is impossible.

**The fix:** Partner with engineering teams early to ensure agentic logs are structured events — not debug strings — with explicit user-to-agent-to-action linkage. The speakers call this **"identity multitasking"** — logs must carry context at all three identity levels simultaneously.

This lesson directly maps to the "structured identity telemetry" requirement in [[agent-observability|Agent Observability]] (§3 — Identity Multiplexing): inject `botId`, `sessionContext`, and `traceId` into every execution log.

### 2. Profile normal before an attack occurs

Do not wait for a known attack to build the behavioral baseline. With 12,000+ unique daily active agents, a signature-based approach is structurally insufficient. Profiling must begin before attack patterns are known, so the statistical mean is already established when deviation occurs.

This is the core argument for the **preventive / posture** approach to agent observability rather than reactive signature matching.

### 3. Avoid over-engineering features (the "perfect feature" trap)

Practitioners frequently invest significant time in complex features (e.g., the query complexity calculator) that introduce latency and complexity without proportionate signal. The guidance: use PCA-style feature selection — measure predictive contribution, then cull to the minimal set with adequate predictive power. Operationalize this as hyperparameter tuning: simulate outcomes across feature sets, tune detection logic to desired alert-volume output.

### 4. Build a warm-up period into the SOC playbook

Expect elevated noise for the **first 14 days** of a new agent's life while the baseline is being established. Build this warm-up period into the SOC playbook explicitly — do not surface these alerts to analysts without the warm-up caveat, or analysts will be burned out on day one and lose trust in the model.

## Q&A highlights

**On curse of dimensionality and feature culling:**
The speaker drew on PCA principles — enumerate candidate features, measure each feature's contribution to correctly predicting abuse, then cull to the minimum set. This is structurally identical to hyperparameter tuning in traditional ML: define the outcome, run simulations, tune the feature set to achieve target detection volume.

**On confidence scores and feedback loops:**
Deviation-based scoring already gives a per-axis confidence-interval analog. Formal alert-scoring and annotation (was this alert correct?) is in progress. The team is working with internal groups to score historical alerts and build a feedback loop for continuous calibration.

**On auto-containment readiness:**
Auto-containment is approaching via a **purple-team exercise**: a red team generates attacks in a controlled environment; the model is tuned by catching those attacks in the wild. Only after passing the purple-team gate will auto-containment be rolled out to customers.

## Cross-references in this wiki

| Wiki artifact | Relevance |
|---|---|
| [[agent-observability\|Agent Observability]] | This talk is the primary production case study for §7 (Agent Behavioral Monitoring — Insider-Threat Framing) and §3 (Identity Multiplexing). Adds the first concrete production-scale signal-to-noise ratio. |
| [[multi-agent-runtime-security\|Multi-Agent Runtime Security]] | The three-level ensemble model (user / agent / org) is a production instantiation of the "aggregate behavioral baselines" pattern described there. |
| [[agentic-ai-security-reference-architecture\|Agentic AI Security Reference Architecture]] | Evidence for the Observability plane (behavioral baselines, identity telemetry) and Runtime plane (execution-layer defense) at production scale. |
| [[agentic-ai-security-cmm-2026\|Agentic AI Security CMM 2026]] | D7 (Observability & Detection) L3/L4 evidence: per-agent behavioral baselines, incremental historical profiling, 12–24 hr detection latency (current), roadmap toward inline scoring. |
| [[breaking-the-lethal-trifecta-talk\|Breaking the Lethal Trifecta (Bullen)]] | Complementary defense surface: Bullen covers the egress / tool-policy plane; this talk covers the behavioral-anomaly / observability plane. Both are Salesforce-adjacent (Bullen: Stripe; Rittinghouse: Salesforce). |
| [[securing-workspace-genai-at-google-talk\|Securing Workspace GenAI (Lidzborski)]] | Lidzborski's "execution layer" framing and post-generation blindness observation are independently corroborated here. Both talks argue that content moderation at the reasoning layer is insufficient without execution-layer visibility. |
| [[behavioral-anomaly-detection-for-agents\|Behavioral Anomaly Detection for Agents]] | This talk is the founding production evidence for that concept page. |
| [[prompt-volume-to-alert-ratio\|Prompt-Volume-to-Alert Ratio]] | This talk introduces the metric into the wiki with the first quantified production example: ~1.8M prompts → <30 alerts (ratio ~60,000:1). |

## Sources

See frontmatter `sources:`. Transcript was the primary ingest source. Slide deck (`.raw/talks/2026-03-04_Millie-and-Matt-Rittinghouse_1.8M-Prompts-30-Alerts_slides.pdf`) was also archived but not separately extracted at ingest time — content from the transcript is sufficient for a complete summary.
