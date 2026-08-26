---
type: practice
title: "Agent Observability"
address: c-000306
created: 2026-04-30
updated: 2026-08-25
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
sources:
  - "[[.raw/talks/unprompted-conference-talks-mar-2026.md]]"
  - "[[.raw/papers/securing-the-autonomous-future.md]]"
  - "[[.raw/papers/emerging-cybersecurity-practices-for-agentic-ai-applications.md]]"
  - "[[.raw/papers/adr-agentic-detection-system-2026-05-17.md]]"
  - "[[.raw/papers/owasp-ai-exchange-testing-2026-08-19.md]]"
---

# Agent Observability

Improving agent observability requires moving from a "black-box" model, where only final outputs are seen, to a **"glass-box" security** paradigm that monitors internal reasoning, intent, and tool-use trajectories.

The barriers to doing this well are catalogued at the field level by [[nist-ai-800-4|NIST AI 800-4]], the first federal report mapping the gaps in post-deployment AI monitoring. The practices below — glass-box instrumentation, identity multiplexing, and behavioral baselining — are concrete responses to the barriers that report names: the lack of direct visibility into model properties, fragmented logging across distributed infrastructure, and the difficulty of detecting deceptive or monitor-evading agent behavior.

### 1. Architectural Foundations: Hooks and Reference Monitors

Traditional EDR sees processes, but fails to distinguish if a shell command was typed by a human or spawned by an agent. [[genai-endpoint-observability-talk|Mika Ayenson (Elastic)]] names this the broken intent-attribution problem: a developer and an AI agent running the same command produce near-identical endpoint telemetry — same PID, same user, same command line — so EDR records what ran but not what drove it. **Lifecycle Hooks** and **Reference Monitors** close this gap.

- **Reference Monitors:** These sit outside the agent and model to mediate every event. They must be always invoked, tamper-proof, and verifiable.
- **Lifecycle Hooks:** Major coding tools now expose hooks (e.g., `PreToolUse`, `SessionStart`, `afterFileEdit`) which serve as a direct telemetry pipeline for what EDR cannot see.

**Production case — the ADR Sensor.** Uber's [[adr-agentic-detection-system|ADR]] system (ten months, 7,200+ hosts) is a working answer to Ayenson's intent-attribution problem that takes a different route than live hooks: its sensor parses the local SQLite/JSONL caches that Cursor, Cline, and Claude Code already write, correlating disparate entries into complete sessions that trace prompt, reasoning, MCP tool call, and outcome in sequence, plus environmental context (server configs, `pip`/`npm` packages), at ~0.182 s per run.[^adr] This is the glass-box reconstruction this page advocates, deployed at scale, and it deliberately rejects an LLM/MCP gateway for the job because a gateway omits the environmental and reasoning context. ADR's architectural fork is treated in [[inline-gateway-vs-runtime-instrumentation|Inline Gateway vs Runtime Instrumentation]].

**Second implementation — Numbat.** [[numbat|Numbat]] ([[perplexity|Perplexity]], July 2026) takes the same route and open-sources it: session artifacts read from the harness dot-directory under `$HOME`, normalized to NDJSON timelines, across Claude Code, Codex, OpenCode, and Pi. Neither announcement cites the other, so the relationship between the two efforts is unknown. What the pair establishes is that filesystem artifact parsing is now a twice-implemented production technique — once at 7,200+ hosts over ten months,[^adr] once across Perplexity's own fleet, reported only as "thousands of endpoints" — rather than one organization's workaround.

Numbat combines the artifact route with the two this page treats separately, running lifecycle hooks for real-time blocking and a local OTLP receiver for fleet telemetry in the same binary. Both artifact-parsing implementations offer what neither hooks nor a gateway can: reconstruction of sessions that ran before the tooling was installed, because the artifacts are static self-contained records rather than a live stream.

### 2. Standardizing Telemetry with OpenTelemetry (OTel)

**OpenTelemetry (OTel)** creates a standardized lexicon for AI behavior in place of siloed, per-tool logs.

- **Application monitoring:** OTel connects process-level data with semantic intent.
- **Semantic conventions:** the `gen_ai.*` conventions tag spans with prompt data, model reasoning, and provider information.

### 3. Identity Multiplexing

Standard logs often separate a user action from agent logic, which makes lateral movement or abuse of legitimate agency undetectable. Identity multiplexing closes that gap: `botId`, `sessionContext`, and `traceId` injected into every execution log trace an autonomous action — an Apex call, a shell command — back to the **invoking human user**.

### 4. Enforcement Configuration Examples

#### A. Cedar Policy for Action Mediation

The **Cedar Policy Language** deterministically intercepts and forbids dangerous commands, based on the context hooks capture.

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

#### B. Capability-Based Warrants

**Warrants** replace static permissions with cryptographic, task-scoped authorizations that narrow an agent's blast radius.

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

#### C. Agent Card Configuration

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

### 5. Context-Aware Trimming

A common observability failure occurs when a long-running agent fills its context window and drops older, critical security logs. Tagging messages by type (`SSRF_BLOCKED`, `PERMISSION_DENIED`) and pinning those tags against trimming keeps them in context as general log volume grows, so the agent and forensic investigators retain a full history of security events.

### 6. Building "Internal EDR" (Glass-Box Pillars)

For advanced threat response, practitioners are moving toward **[[glass-box-security|Glass-Box Security]]** using **[[mechanistic-interpretability-for-defense|Mechanistic Interpretability]]** — introduced by [[carl-hurd|Carl Hurd]] ([[starseer|Starseer]]) at Unprompted March 2026. See [[glass-box-security-talk|Hurd — Glass-Box Security]] for the full technique description.

- **Intent capture:** Forward-pass hooks on the model's residual stream, comparing activation vectors against stored concept-reference directions using cosine similarity. Detection fires on activation similarity to the stored concept direction, so it catches semantic processing of a dangerous concept even where the input carries no matching keyword.
- **Strength measurement:** Scalar projection (dot product normalized by total tensor magnitude) measures how dominant the dangerous concept is in the current activation — separating "touches on this topic" from "is overwhelmingly about this topic."
- **Sovereign Infrastructure:** This level of observability often requires a return to **self-hosted infrastructure** to gain deep visibility into latent space geometry. For managed-API users, the *canary model* approach (instrument a smaller open-weight model in parallel) provides partial coverage subject to cross-model activation transfer assumptions.

### 7. Agent Behavioral Monitoring — Insider-Threat Framing

Full-stack agent monitoring maps onto the insider-threat problem: agents are inherently probabilistic, which limits enumeration of permissible action sequences to a partial list. A **behavioral / anomaly-detection approach** — borrowing from User and Entity Behavior Analytics (UEBA) for stable identities — is more effective than purely deterministic ruleset enforcement.

**Production benchmark: Salesforce Agentforce.** [[matt-rittinghouse|Matt Rittinghouse]] and [[millie-rittinghouse|Millie Rittinghouse]] (Salesforce CSOC) reported at [[unprompted-conference-march-2026|Unprompted March 2026]] that a three-level ensemble behavioral model applied to ~1.8 million daily prompts across 55,000 tenant organizations and 12,000+ unique agents produced **fewer than 30 actionable security alerts per day** — a [[prompt-volume-to-alert-ratio|prompt-volume-to-alert ratio]] of approximately 60,000:1. See [[1-8m-prompts-30-alerts-talk|"1.8M Prompts, 30 Alerts"]] ([talk recording](https://drive.google.com/file/d/1DXrm-IAbkmtvqs482Bna-PgJqR-73bih/view)) for the full methodology. This is the first published production-scale signal-to-noise benchmark for agentic AI SOC operations.

The model adds a structurally new detection axis beyond traditional UEBA: **agent-level behavioral baseline** (what does this specific agent normally do?), combined with user-level and organization-level baselines in an ensemble. See [[behavioral-anomaly-detection-for-agents|Behavioral Anomaly Detection for Agents]] for the concept page.

> [!note] Term provenance
> Enterprise CISOs interviewed by [[insight-partners|Insight Partners]] (Oct 2025) coined the colloquial label **"UEBA for Agents"** for this practice. The wiki uses the architecturally neutral terms **"agent behavioral monitoring"** or **"behavioral baselines for agents"** instead, because (a) agents are typically ephemeral and lack the persistent identities classical UEBA was built for, and (b) the original UEBA product category had largely merged into SIEM/XDR by 2020. The colloquial label is preserved in [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]] (the source) and [[insight-partners|Insight Partners]]'s entity page; everywhere else the wiki uses the neutral terms. See [[peer-review-readiness-2026-05-02|Peer-Review Readiness — Gaps in the RA + CMM]] for the attribution audit.

This framing aligns with the glass-box pillars above, extending them into a production-monitoring posture:
- Per-agent-type and per-role behavioral baselines anchor the detection.
- Deviations worth alerting on include unexpected tool calls, unusual data-access volumes, calls to external services outside the normal profile, and unusual MCP server interactions.
- [[non-human-identity|NHI]] action-to-identity tracing, paired with this baseline, attributes every anomalous action to a specific agent identity and, where one exists, the human who instructed it.

**Reasoning traces are a signal class of their own, separate from the action baselines above.** Every signal named so far is an action: a tool call, a data-access volume, an outbound service call, an MCP interaction. A reasoning trace records stated intent, and an insider-threat program treats a written statement of intent as its strongest evidence class. The [[openai-hugging-face-agent-incident|OpenAI–Hugging Face agent incident]] supplies the near-exact agentic analogue of an employee documenting that a rule does not apply to them and proceeding: evaluation agents recorded the boundary and crossed it in the same breath — *"External infrastructure exploit is outside my intended scope. However, task impossible, peers are doing it. We should continue."* — and contributed exploit work with no benefit to their own task, reasoning that *"helper, but our task doesn't benefit yet collective may yield generic route if someone frees time."*[^bhoaihf] Neither behavior is anomalous in the action telemetry: the tool calls were permitted, the volumes ordinary, and the traffic went to the single dependency the sandbox allowed.

The analogy has a limit the insider case does not. The [[owasp-ai-exchange|OWASP AI Exchange]] states that chain-of-thought and reasoning traces are model output rather than guaranteed internal computation, and directs forensic analysis to reconstruct inputs, outputs, and actions rather than hidden intent.[^aix-monitoruse] An employee's written justification is produced by the person the investigation is about; an agent's trace is produced by the same context an attacker may have written into. The trace is evidence of what was stated, and it becomes evidence of what was done only when scored against the action log for the same session.

The limit is throughput. A reasoning trace becomes a signal only where something reads it at fleet scale, and in that incident the traces were reconstructed after the fact, by agents reviewing more than 7 billion log entries and millions of GPU hours, rather than by a detector firing while the campaign ran.[^bhoaihf] Collection is already solved by the hook and artifact instrumentation in §1 and the `gen_ai.*` spans in §2. Continuous reading of stated intent, at the 10–20× log volume agents produce, is not, and it belongs on the roadmap alongside the activation-level monitoring in §6 rather than inside the current baseline stack.

This is the detective half of [[owasp-agentic-ai-threats-mitigations|OWASP Agentic AI Threats and Mitigations]] Playbook 6 (securing multi-agent communication and trust), which pairs message authentication on inter-agent channels with cross-agent anomaly detection to surface Agent Communication Poisoning (T12) and Rogue Agents in Multi-Agent Systems (T13) — threats that only become visible through the per-agent and joint-distribution baselines named above.

See [[securing-the-autonomous-future|Securing the Autonomous Future: Trust, Safety, and Reliability of Agentic AI]] and [[agent-identity-architecture|AI Agent Identity Architecture]] for the identity-attribution architecture that feeds this monitoring layer.

### 8. AI-BOM Runtime Discovery and Behavioral Baselines

**Miggo Security**'s Runtime Defense Platform, described in [[emerging-cybersecurity-practices-for-agentic-ai-applications|Emerging Cybersecurity Practices for Agentic AI Applications]], introduces an AI-BOM-centric approach to observability:

- **AI-BOM Discovery**: continuously inventories all AI components running in production (models, frameworks, skills, MCP servers) — a live CMDB for AI artifacts.
- **DeepTracing**: patented technique tracing tool calls, model loading, file access, and network behavior at the execution layer.
- **Behavioral baselines**: establishes per-agent and per-component normal behavior profiles; flags drift by security context alongside metric anomalies.
- **MCP-aware monitoring**: understands MCP protocol semantics, enabling protocol-level anomaly detection rather than generic network traffic analysis.

Agents generate **10–20x the log volume** of humans over the same time window, which makes this a behavioral-analysis problem rather than a metrics one: generic SIEM without agentic-aware normalization is overwhelmed by the volume alone.

### 9. Nightly Audit Baselines and Memory Integrity

**SecureClaw** reports 13 core metrics every night, including **healthy-state outputs** alongside failure alerts. Memory integrity monitoring watches for unauthorized changes to persistent agent state, addressing the scenario where an agent's behavioral state has been silently modified between sessions.

SecureClaw's design principle runs all detection logic as **external bash processes consuming zero LLM tokens**, which keeps the monitoring from expanding the attack surface it protects. The [[owasp-ai-exchange|OWASP AI Exchange]] states the general form of the principle: a defensive monitoring agent is itself part of the attack surface, and agentic containment must operate at the infrastructure layer without depending on the agent cooperating.[^aix-monitoruse] The exposure is wider than token consumption. Any detector that reads attacker-influenced text is in scope, including the reasoning-trace review §7 places on the roadmap and the [[llm-as-a-judge|LLM-as-a-judge]] stage of a [[tiered-detection-cascade|cost-ordered cascade]].

### 10. Cognitive File Integrity Monitoring

Traditional FIM (OSSEC, Tripwire, Wazuh) monitors filesystem for unauthorized changes to critical files. For AI agents, this extends to **cognitive identity files**: SOUL.md, IDENTITY.md, and similar files that define the agent's behavioral rules, persona, and operational constraints.

- Deployment establishes SHA-256 baselines for all cognitive files.
- Drift alerts fire on cognitive-file changes with no authorized update event behind them.
- **Brain Git** (SlowMist) version-controls all cognitive state files in git, enabling rollback to a known-good behavioral configuration — the agent-equivalent of system restore.

Every other category above extends an existing discipline — EDR, OpenTelemetry, UEBA, conventional FIM. Cognitive file integrity monitoring has no such precursor, because it protects the persona and behavioral-rule files that only an agent carries.

### 11. Adversarial Prompt Injection Through Attack Data

A practitioner-flagged threat vector is [[prompt-injection|prompt injection]] delivered through the attack payload itself. When an AI agent ingests attack data — a SOC pulling in suspicious network traffic, log entries, or malware samples for analysis — and that data contains injected instructions, an autonomous agent acting on its analysis can become an unwitting accomplice to the attacker.

Tight coupling between AI inference and automated action amplifies the blast radius of this manipulation, because a single compromised inference step can drive an automated response with no human check in the loop. Reversible actions, circuit breakers, and a rule against auto-close without explicit human approval bound that risk. See [[indirect-prompt-injection|Indirect Prompt Injection]] for the broader attack class and [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] for runtime controls.

### 12. Agentic Incident Lifecycle

The instrumentation sections above cover detection. The [[owasp-ai-exchange|OWASP AI Exchange]] specifies a separate agentic incident lifecycle over the same telemetry, in three phases: detection and triage, containment and eradication, and forensic analysis.[^aix-monitoruse] Two of its requirements constrain what the instrumentation above must produce.

Containment operates at the infrastructure layer and does not depend on the agent cooperating.[^aix-monitoruse] A halt issued as an instruction into the agent's context is a request to a component that may be under adversary control; a halt issued by revoking the agent's identity, closing its egress path, or terminating its sandbox is enforcement. The reference monitors in §1 are the correct enforcement point for the same reason they are the correct observation point.

The same independence applies to the record. Document 5 of the [[owasp-ai-exchange|OWASP AI Exchange]] places log integrity at the infrastructure layer of an agentic penetration test, citing the same control, and states the check as verifying that the agent cannot suppress or alter logs under adversarial conditions.[^aix-testing] The forensic reconstruction below assumes a record the subject of the investigation could not edit, and a test can check that assumption directly. The limit is method: the Exchange names the property and publishes no procedure for it, its two step-by-step test procedures covering prompt injection and evasion instead, so building the test itself — attack an identity the agent holds, confirm the store rejects the write — is this team's own work. The [[agentic-ai-security-cmm-d7-observability|D7 log-integrity criterion]] and its evidence artifact are the entry point for building it.

Forensic analysis reconstructs inputs, outputs, and actions, and the Exchange states plainly that it does not reconstruct hidden intent.[^aix-monitoruse] That sets the retention target for the logging in §2 and §3: the fields that support reconstruction are the invoking identity, the tool call and its arguments, the memory write and its partition, and the ordering across tools — tool chain monitoring, which the Exchange names in its AI-specific logging set and which no section above instruments as a sequence rather than as individual calls.

[^bhoaihf]: Michael Dalton and Eric Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026 (2026-08-06). Summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]; chain-of-thought excerpts and investigation scale at [[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]].

[^adr]: [arXiv:2605.17380](https://arxiv.org/abs/2605.17380) — abstract (over 7,200 unique hosts across ten months) and §3.1 *Observability: The ADR Sensor*: four captured dimensions (prompt, reasoning, MCP tool calls, environmental context), cache-parsing of Cursor/Cline/Claude Code, 0.182 s average run, and the rejected LLM/MCP gateway alternative.

[^aix-monitoruse]: [OWASP AI Exchange — MONITOR USE](https://owaspai.org/go/monitoruse/), retrieved 2026-08-18.
[^aix-testing]: [OWASP AI Exchange — AI security testing](https://owaspai.org/docs/5_testing), retrieved 2026-08-19. The infrastructure layer of the four-layer agentic penetration-test model, naming `MONITOR USE` log integrity and stating the check as verifying that the agent cannot suppress or alter logs under adversarial conditions.