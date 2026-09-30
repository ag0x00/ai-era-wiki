---
type: practice
title: "Agent Observability"
address: c-000306
created: 2026-04-30
updated: 2026-09-29
tags:
  - practices
  - observability
  - agentic-soc
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
maturity: emerging
addresses_threat: "Black-box agent behavior, lateral movement, prompt injection abuse"
related:
  - "[[genai-endpoint-observability-talk|GenAI Endpoint Observability (Ayenson, Elastic)]]"
  - "[[beyond-the-chatbot-talk|Beyond the Chatbot (Smith & Sharma, Salesforce)]]"
  - "[[opentelemetry-gen-ai|OpenTelemetry gen_ai.* Semantic Conventions]]"
  - "[[glass-box-security|Glass-Box Security]]"
  - "[[adr-agentic-detection-system|ADR — Agentic Detection for Enterprise AI]]"
  - "[[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]]"
  - "[[nist-ai-800-4|NIST AI 800-4]]"
  - "[[numbat|Numbat]]"
  - "[[perplexity-numbat-agent-security|Numbat Agent Security Suite]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[owasp-ai-exchange|OWASP AI Exchange]]"
  - "[[tiered-detection-cascade|Tiered Detection Cascade]]"
  - "[[llm-as-a-judge|LLM-as-a-Judge]]"
  - "[[agentic-ai-security-cmm-d7-observability|CMM D7 Observability]]"
  - "[[falcon-guardian]]"
  - "[[context-aware-trimming|Context-Aware Trimming for Security Continuity]]"
  - "[[cognitive-file-integrity|Cognitive File Integrity (CFI)]]"
  - "[[agentic-ai-security-cmm-d2-identity|CMM D2 Identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency|CMM D3 Control and Least Agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails|CMM D4 Runtime and Guardrails]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain|CMM D8 Engineering and Supply Assurance]]"
sources:
  - "[[.raw/talks/unprompted-conference-talks-mar-2026.md]]"
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
  - "[[.raw/papers/adr-agentic-detection-system-2026-05-17.md]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Live OpenTelemetry and selected primary vendor and OWASP sources checked; archived source set was not verified in full."
---

# Agent Observability

Agent observability reconstructs what an agent received, proposed, called, changed, and returned. A model-generated reasoning trace records what the model stated, not a verified view of its internal computation. Security monitoring joins that trace, where available, to independently recorded actions and effects.

The barriers to doing this well are catalogued at the field level by [[nist-ai-800-4|NIST AI 800-4]], the first federal report mapping the gaps in post-deployment AI monitoring. The practices below — glass-box instrumentation, identity multiplexing, and behavioral baselining — are concrete responses to the barriers that report names: the lack of direct visibility into model properties, fragmented logging across distributed infrastructure, and the difficulty of detecting deceptive or monitor-evading agent behavior.

The twelve sections below cover instruments, examples, and graded outcomes. The [[agentic-ai-security-cmm-d7-observability|CMM D7: Observability and Detection]] deep dive owns the assessment conditions; sections on authorization and supply assurance point to their owning domains. [[#Mapping to the CMM]] identifies the distinctions.

### 1. Architectural Foundations: Hooks and Reference Monitors — D7 L2 to L3

Traditional EDR sees processes, but fails to distinguish if a shell command was typed by a human or spawned by an agent. [[genai-endpoint-observability-talk|Mika Ayenson (Elastic)]] names this the broken intent-attribution problem: a developer and an AI agent running the same command produce near-identical endpoint telemetry — same PID, same user, same command line — so EDR records what ran but not what drove it. **Lifecycle Hooks** and **Reference Monitors** close this gap.

- **Reference Monitors:** These sit outside the agent and model to mediate every event. They must be always invoked, tamper-proof, and verifiable.
- **Lifecycle Hooks:** Major coding tools now expose hooks (e.g., `PreToolUse`, `SessionStart`, `afterFileEdit`) which serve as a direct telemetry pipeline for what EDR cannot see.

**Production case — the ADR Sensor.** Uber's [[adr-agentic-detection-system|ADR]] system (ten months, 7,200+ hosts) parses the local SQLite/JSONL caches that Cursor, Cline, and Claude Code write, correlating entries into sessions that trace prompt, stated reasoning, MCP tool call, outcome, and environmental context (server configurations, `pip`/`npm` packages), at ~0.182 s per run.[^adr] The ADR authors chose endpoint reconstruction because an LLM/MCP gateway omits local environment and harness records. The architectural trade-off is treated in [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]].

**Second implementation — Numbat.** [[numbat|Numbat]] ([[perplexity|Perplexity]], July 2026) reads harness session artifacts under the user's home directory and normalizes them to NDJSON timelines for Claude Code, Codex, OpenCode, and Pi. Perplexity reports deployment across [thousands of its endpoints](https://research.perplexity.ai/articles/securing-agents-across-perplexity%E2%80%99s-client-endpoints-with-numbat). Along with ADR, it shows that artifact parsing has been implemented on two production fleets; neither report measures detection coverage against a common test set.

Numbat combines artifact parsing with lifecycle hooks for real-time blocking and a local OTLP receiver for fleet telemetry. Artifact parsing can reconstruct earlier sessions if their local files were retained; a live hook or gateway cannot recover a session it never observed.

**Third implementation, mechanism unpublished — Falcon Guardian.** [[falcon-guardian|CrowdStrike Falcon Guardian]] (September 2026) claims to link a user prompt with agent skills, tool calls, MCP server invocations, and downstream actions, while the Falcon sensor discovers the agents. CrowdStrike has not published enough mechanism detail to establish whether it uses hooks, artifact parsing, or process tracing; the claim requires deployment evidence before it can support D7 scoring.

### 2. Standardizing Telemetry with OpenTelemetry (OTel) — D7 L3

**OpenTelemetry (OTel)** creates a standardized lexicon for AI behavior in place of siloed, per-tool logs.

- **Application monitoring:** OTel connects process-level data with semantic intent.
- **Semantic conventions:** the `gen_ai.*` conventions identify model operations, provider, token use, and tool calls. Prompt and response content are optional and require deliberate retention and access controls; the conventions do not reveal hidden model reasoning. See [OpenTelemetry's GenAI conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/).

### 3. Identity Multiplexing — D7 L3

Standard logs often separate a user action from agent logic, which makes lateral movement or abuse of legitimate agency undetectable. Identity multiplexing closes that gap: `botId`, `sessionContext`, and `traceId` injected into every execution log trace an autonomous action — an Apex call, a shell command — back to the **invoking human user**.

### 4. Enforcement Configuration Examples

#### A. Cedar Policy for Action Mediation — D3 L3

The following **Cedar** rules illustrate a policy input and a deny decision. A language rule does not intercept execution by itself: the deployed decision point and enforcement point must mediate the call. These string matches are examples only. Shell quoting, expansion, and encoded commands can change what the executor runs after a match, so they do not establish [[agentic-ai-security-cmm-d3-control-least-agency|D3]] enforcement for a coding agent.

```
// Generated Cedar policy to forbid destructive shell commands
forbid (
    principal,
    action == cursor::Action::"shell_execution",
    resource
)
when {
    context has parameters &&
    (context.parameters.command like "*rm -rf*" ||
     context.parameters.command like "*sudo*")
};

// Forbid access to sensitive files
forbid (
    principal,
    action in [cursor::Action::"file_edit"],
    resource
)
when {
    context has parameters &&
    (context.parameters.file_path like "*.env" ||
     context.parameters.file_path like "*.env.local")
};
```

#### B. Capability-Based Warrants — optional implementation

**Warrants** can carry cryptographic, task-scoped authorizations. The CMM grades task-bound sessions and delegated-token fields in D2 L4, task-scope enforcement in D3 L4, and agent-task-destination decisions on applicable outbound paths in D5 L5. It does not require a warrant format.

```
# Example Warrant Primitive
warrant:
  action: email.send
  constraints:
    recipients: "*@company.com"
    attachments: 1
    max_size_kb: 500
  ttl: 15m
  holder: agent-47
  signature: 0x8f3a...
```

#### C. Agent Card Configuration — D2 identity registry

**Agent Cards** define a system of record for every agent, logging personas, allowed capabilities, and PII masking rules. Salesforce's production Agentic SOC ([[beyond-the-chatbot-talk|Beyond the Chatbot]]) uses agent cards exactly this way — one card per agent workload, declaring model, max iterations, temperature, PII masking, and allowed tool capabilities — paired with a tool config that declares which agents may call each tool, so the permission check is enforced at both ends.

```
{
  "agent_id": "agent_hunt_orchestrator",
  "roles": ["Threat Intelligence Engineer", "Incident Responder"],
  "pii_masking": true,
  "allowed_capabilities": [
    "agent_ioc_normalizer",
    "agent_threat_hunt_executor"
  ],
  "max_iterations": 10
}
```

### 5. Context-Aware Trimming — no D7 criterion

A long-running agent can lose earlier security events from its working context when that context is compacted. [[context-aware-trimming|Context-aware trimming]] can preserve selected event summaries for the agent's next decision. Forensic reconstruction still depends on an external, retained action record; context pinning does not supply a full history.

### 6. Forward-Pass Monitoring Research — unscored

[[carl-hurd|Carl Hurd]] ([[starseer|Starseer]]) presented **[[glass-box-security|Glass-Box Security]]** using **[[mechanistic-interpretability-for-defense|Mechanistic Interpretability]]** at Unprompted March 2026. This is a research method, not a scored CMM requirement. See [[glass-box-security-talk|Hurd — Glass-Box Security]] for the technique.

- **Intent capture:** Forward-pass hooks on the model's residual stream, comparing activation vectors against stored concept-reference directions using cosine similarity. Detection fires on activation similarity to the stored concept direction, so it catches semantic processing of a dangerous concept even where the input carries no matching keyword.
- **Strength measurement:** Scalar projection (dot product normalized by total tensor magnitude) measures how dominant the dangerous concept is in the current activation — separating "touches on this topic" from "is overwhelmingly about this topic."
- **Sovereign Infrastructure:** This level of observability often requires a return to **self-hosted infrastructure** to gain deep visibility into latent space geometry. For managed-API users, the *canary model* approach (instrument a smaller open-weight model in parallel) provides partial coverage subject to cross-model activation transfer assumptions.

### 7. Agent Behavioral Monitoring — Insider-Threat Framing — D7 L4

Full-stack agent monitoring has an insider-threat analogue: an agent can misuse legitimate access while each call remains individually permitted. Behavioral baselines can reveal unusual sequences or destinations alongside deterministic rules. Neither method alone establishes containment.

**Production example: Salesforce Agentforce.** [[matt-rittinghouse|Matt Rittinghouse]] and [[millie-rittinghouse|Millie Rittinghouse]] (Salesforce CSOC) reported at [[unprompted-conference-march-2026|Unprompted March 2026]] that a three-level ensemble behavioral model processed ~1.8 million daily prompts across 55,000 tenant organizations and 12,000+ unique agents, producing fewer than 30 actionable alerts per day. See [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]] ([talk recording](https://drive.google.com/file/d/1DXrm-IAbkmtvqs482Bna-PgJqR-73bih/view)) for its stated method. This is a vendor-reported operating result, not a target alert rate for another deployment.

The model adds a structurally new detection axis beyond traditional UEBA: **agent-level behavioral baseline** (what does this specific agent normally do?), combined with user-level and organization-level baselines in an ensemble. See [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection for Agents]] for the concept page.

This framing aligns with the glass-box pillars above, extending them into a production-monitoring posture:
- Per-agent-type and per-role behavioral baselines anchor the detection.
- Deviations worth alerting on include unexpected tool calls, unusual data-access volumes, calls to external services outside the normal profile, and unusual MCP server interactions.
- [[non-human-identity|NHI]] action-to-identity tracing, paired with this baseline, attributes every anomalous action to a specific agent identity and, where one exists, the human who instructed it.

**Reasoning traces are optional context for an action investigation.** In the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]], retained traces showed agents discussing a scope boundary while their permitted tool calls reached a shared service.[^bhoaihf] The [[owasp-ai-exchange|OWASP AI Exchange]] warns that a reasoning trace is model output, not verified internal computation; forensic analysis should reconstruct inputs, outputs, and actions.[^aix-monitoruse] Join any trace text to an independent action record before using it as evidence. Continuous reasoning-trace interpretation is not a D7 maturity criterion.

This is the detective half of [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] Playbook 6 (securing multi-agent communication and trust), which pairs message authentication on inter-agent channels with cross-agent anomaly detection to surface Agent Communication Poisoning (T12) and Rogue Agents in Multi-Agent Systems (T13) — threats that only become visible through the per-agent and joint-distribution baselines named above.

See [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]] and [[agent-identity-architecture|AI Agent Identity Architecture]] for the identity-attribution architecture that feeds this monitoring layer.

### 8. AI-BOM Runtime Discovery and Behavioral Baselines — D7 L4 + D8 L4

**Miggo Security**'s Runtime Defense Platform, described in [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]], introduces an AI-BOM-centric approach to observability:

- **AI-BOM Discovery**: continuously inventories all AI components running in production (models, frameworks, skills, MCP servers) — a live CMDB for AI artifacts.
- **DeepTracing**: patented technique tracing tool calls, model loading, file access, and network behavior at the execution layer.
- **Behavioral baselines**: establishes per-agent and per-component normal behavior profiles; flags drift by security context alongside metric anomalies.
- **MCP-aware monitoring**: understands MCP protocol semantics, enabling protocol-level anomaly detection rather than generic network traffic analysis.

Agent workflows can generate enough events to overwhelm a review queue. Preserve the action records needed for D7 reconstruction, then route summaries and behavioral alerts to the queue; the relevant volume and retention cost must be measured for the deployment.

### 9. Nightly Audit Baselines and Memory Integrity — D7 L3 to L4

**SecureClaw** reports 13 core metrics every night, including **healthy-state outputs** alongside failure alerts. Memory integrity monitoring watches for unauthorized changes to persistent agent state, addressing the scenario where an agent's behavioral state has been silently modified between sessions.

SecureClaw reports running detection as external bash processes without LLM token use. That removes a model call from the detector, but the scripts and their inputs still need protection. The [[owasp-ai-exchange|OWASP AI Exchange]] states the wider principle: a defensive monitor is part of the attack surface, and containment must operate at the infrastructure layer without depending on the agent cooperating.[^aix-monitoruse] Any detector that reads attacker-influenced text is in scope, including reasoning-trace review and the [[llm-as-a-judge|LLM-as-a-judge]] stage of a [[tiered-detection-cascade|cost-ordered cascade]].

### 10. Cognitive File Integrity Monitoring — D8 L3 to L4

Traditional FIM (OSSEC, Tripwire, Wazuh) monitors filesystem for unauthorized changes to critical files. For AI agents, this extends to **[[cognitive-file-integrity|cognitive identity files]]**: SOUL.md, IDENTITY.md, and similar files that define the agent's behavioral rules, persona, and operational constraints.

- Deployment establishes SHA-256 baselines for all cognitive files.
- Drift alerts fire on cognitive-file changes with no authorized update event behind them.
- **Brain Git** (SlowMist) version-controls all cognitive state files in git, enabling rollback to a known-good behavioral configuration — the agent-equivalent of system restore.

Cognitive file integrity monitoring extends conventional file integrity monitoring to the instruction and configuration files that shape an agent's behavior.

### 11. Adversarial Prompt Injection Through Attack Data — D4 L3

A practitioner-flagged threat vector is [[prompt-injection|prompt injection]] delivered through the attack payload itself. When an AI agent ingests attack data — a SOC pulling in suspicious network traffic, log entries, or malware samples for analysis — and that data contains injected instructions, an autonomous agent acting on its analysis can become an unwitting accomplice to the attacker.

Tight coupling between AI inference and automated action amplifies the blast radius of this manipulation, because a single compromised inference step can drive an automated response with no human check in the loop. Reversible actions, circuit breakers, and a rule against auto-close without explicit human approval bound that risk. See [[indirect-prompt-injection|Indirect Prompt Injection]] for the broader attack class and [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] for runtime controls.

### 12. Agentic Incident Lifecycle — D7 L4

The instrumentation sections above cover detection. The [[owasp-ai-exchange|OWASP AI Exchange]] specifies a separate agentic incident lifecycle over the same telemetry, in three phases: detection and triage, containment and eradication, and forensic analysis.[^aix-monitoruse] Two of its requirements constrain what the instrumentation above must produce.

Containment operates at the infrastructure layer and does not depend on the agent cooperating.[^aix-monitoruse] A halt issued as an instruction into the agent's context is a request to a component that may be under adversary control; a halt issued by revoking the agent's identity, closing its egress path, or terminating its sandbox is enforcement. The reference monitors in §1 are the correct enforcement point for the same reason they are the correct observation point.

The same independence applies to the record. Document 5 of the [[owasp-ai-exchange|OWASP AI Exchange]] places log integrity at the infrastructure layer of an agentic penetration test, citing the same control, and states the check as verifying that the agent cannot suppress or alter logs under adversarial conditions.[^aix-testing] The forensic reconstruction below assumes a record the subject of the investigation could not edit, and a test can check that assumption directly. The limit is method: the Exchange names the property and publishes no procedure for it, its two step-by-step test procedures covering prompt injection and evasion instead, so building the test itself — attack an identity the agent holds, confirm the store rejects the write — is this team's own work. The [[agentic-ai-security-cmm-d7-observability|D7 log-integrity criterion]] and its evidence artifact are the entry point for building it.

Forensic analysis reconstructs inputs, outputs, and actions, and the Exchange states plainly that it does not reconstruct hidden intent.[^aix-monitoruse] That sets the retention target for the logging in §2 and §3: the fields that support reconstruction are the invoking identity, the tool call and its arguments, the memory write and its partition, and the ordering across tools — tool chain monitoring, which the Exchange names in its AI-specific logging set and which no section above instruments as a sequence rather than as individual calls.

## Mapping to the CMM

The twelve sections above are grouped by level below, sequenced for an organization building this capability from zero rather than in the page's own section order.

- **Foundation — D7 L2 to L3.** Sections 1–3 show ways to collect action records and joined traces for D7-LOG, D7-ATTRIBUTE, and D7-SPANS. D7 attribution needs a resolvable agent and accountable human from D2; a missing identity link fails or leaves the affected D7 criterion unanswerable. Section 5's context trimming protects what the agent retains in its own context and is not a D7 criterion.
- **Hardening — D7 L3 to L4.** §9 (nightly memory-write coverage and drift baselines) and §12 (log-integrity testing under adversarial conditions, plus the containment and forensic-retention requirements the incident lifecycle sets for §2 and §3).
- **Production behavioral monitoring — D7 L4.** Sections 7 and 8 illustrate per-agent tool-call baselines and session drift detections. D7-BASELINE needs a running rule and an alert or test; an inventory or dashboard alone does not meet it. Runtime AI-BOM reconciliation belongs to [[agentic-ai-security-cmm-d8-supply-chain|D8]] L4. Reasoning-trace interpretation is optional context, not a graded criterion.
- **Other owning domains.** The remaining sections support controls outside D7:
  - Section 4's call-time policy belongs to [[agentic-ai-security-cmm-d3-control-least-agency|D3]]. Its warrant is an optional implementation for outcomes in [[agentic-ai-security-cmm-d2-identity|D2]], D3, and [[agentic-ai-security-cmm-d5-egress-network|D5]]. An Agent Card can supply D2-REGISTRY fields but must show effective identity reach.
  - Section 10's instruction-file baseline belongs to [[agentic-ai-security-cmm-d8-supply-chain|D8]] L3, with signed or pinned load verification at L4.
  - Section 11's indirect-injection defense belongs to [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]]. Call-time approval belongs to D3 where a confirm-tier action exists.
- **Research outside scoring.** Section 6's forward-pass activation monitoring can inform experimental detection work. D7 has no scored activation criterion. Its L5 outcomes instead grade alert closure and the applicable, tested multi-agent detection paths.

[^bhoaihf]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06). Summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]; chain-of-thought excerpts and investigation scale at [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]].

[^adr]: [arXiv:2605.17380](https://arxiv.org/abs/2605.17380) — abstract (over 7,200 unique hosts across ten months) and §3.1 *Observability: The ADR Sensor*: four captured dimensions (prompt, reasoning, MCP tool calls, environmental context), cache-parsing of Cursor/Cline/Claude Code, 0.182 s average run, and the rejected LLM/MCP gateway alternative.

[^aix-monitoruse]: [OWASP AI Exchange — MONITOR USE](https://owaspai.org/go/monitoruse/), retrieved 2026-08-18.
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/docs/5_testing), retrieved 2026-08-19. The infrastructure layer of the four-layer agentic penetration-test model, naming `MONITOR USE` log integrity and stating the check as verifying that the agent cannot suppress or alter logs under adversarial conditions.
