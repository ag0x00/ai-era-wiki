---
type: paper
title: "AI Data Security"
created: 2026-05-01
updated: 2026-06-23
tags:
  - papers
  - ai-data-security
  - knowledge-layer
  - oversharing
  - vendor-content
status: summarized
year: 2026
authors:
  - "Knostic"
venue: "Knostic blog (knostic.ai)"
source_url: "https://www.knostic.ai/blog/ai-data-security"
archived_copy: ".raw/articles/knostic-ai-data-security-2026-05-01.md"
no_public_url: ""
key_claim: "AI data security is a distinct discipline from traditional IT security: prompts, retrieved context, and model outputs create inference-exposure and retrieval-exposure failure modes that bypass file/network access controls. The mitigation is a stack of zero-trust + dynamic policy + AI-specific observability + usage control + posture management — enforced at the knowledge layer, not the file layer."
methodology: "Vendor-content survey / synthesis. Cites NIST AI RMF, NIST SP 800-207 (Zero Trust), SP 800-53, SP 800-60, SP 800-92, ISO/IEC 27001, ISO/IEC 42001, GDPR, plus academic studies on prompt extraction (arxiv 2408.02416, ACL 2024) and PII masking (arxiv 2501.12465). Five platform-category positioning at the end is product marketing — read separately from the conceptual content."
contradicts: []
supports:
  - "[[security-controls-for-ai-stacks]]"
  - "[[rag-hardening]]"
  - "[[agent-observability]]"
  - "[[indirect-prompt-injection]]"
  - "[[three-retrieval-paths]]"
related:
  - "[[knostic]]"
  - "[[ai-coding-agent-governance]]"
  - "[[inference-exposure]]"
  - "[[ai-usage-control]]"
  - "[[ai-spm]]"
  - "[[dspm]]"
  - "[[oversharing-controls]]"
  - "[[shadow-ai]]"
  - "[[cyber-defense-matrix]]"
  - "[[ai-trism]]"
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
aliases:
  - papers/knostic-ai-data-security
  - knostic-ai-data-security

---

# AI Data Security (Knostic, 2026)

**Source:** [Knostic — AI Data Security](https://www.knostic.ai/blog/ai-data-security) (2026). Local copy: `.raw/articles/knostic-ai-data-security-2026-05-01.md`.

## Key Claim

AI data security is **structurally different** from traditional IT security. Traditional security prevents unauthorized access to files, networks, and databases. AI systems add two new failure modes that bypass those controls entirely:

- **Inference exposure** — a user gains unauthorized insight from AI outputs without ever accessing the source documents.
- **Retrieval exposure** — content retrieved by the AI to craft outputs is used beyond what the user is authorized to see.

The mitigation is a layered stack: **zero-trust** at the knowledge layer, **dynamic policy enforcement**, **AI-specific observability** that traces RAG pipelines end-to-end, and **usage control (UCON)** that continues past "can the user open this file?" into "how may they use or disclose what the AI tells them?"

## Methodology

Vendor-content synthesis with academic anchoring:
- Prompt-extraction defense study ([arxiv 2408.02416](https://arxiv.org/html/2408.02416v1)) — 83.8% reduction for Llama2-7B, 71.0% for GPT-3.5
- Prompt-leakage study (ACL 2024) — leakage rates rising from 17.7% to 86.2% under multi-turn attacks
- PII-masking framework ([arxiv 2501.12465](https://arxiv.org/abs/2501.12465)) — F1 0.95 for masking sensitive identifiers
- NIST and ISO standards anchoring

Read the conceptual sections (CIA-A for AI, inference vs retrieval exposure, top risks, AI-UC, AI-SPM, DSPM) seriously. Read the "5 best AI Data Security Platforms" section as vendor marketing — useful for category-naming, less for objective comparison.

## Notable Findings

### CIA + Auditability adapted for AI

The classic CIA triad gets re-interpreted at the knowledge layer:

- **Confidentiality** — extends to inference: an authorized user may still derive *unauthorized* knowledge from model outputs. Inference-exposure controls are required *in addition to* access controls.
- **Integrity** — covers AI outputs and models, including resistance to prompt injection and data poisoning.
- **Availability** — extends beyond DoS to **degraded model performance** under adversarial inputs or data drift.
- **Auditability** — must include prompt capture, retrieved-context snapshots, applied policies, and model output versions to form a **complete decision provenance chain**.

### Eight Top Enterprise AI Risks

| # | Risk | Distinguishing AI characteristic |
|---|---|---|
| 1 | Oversharing in LLM search | RBAC-permitted but contextually inappropriate combination of retrieved fragments |
| 2 | Prompt injection / jailbreaks | Hidden instructions in inputs / multi-step chains |
| 3 | Hallucinations with confidence | GPT-4 ~3% in RAG; legal LLMs 17–34% — Deloitte: 38% of executives admit acting on hallucinated outputs |
| 4 | Data leakage | 48% of employees admit uploading sensitive data to public AI tools |
| 5 | Vector / index poisoning | Single manipulated document distorts retrieval over time |
| 6 | Adversarial AI attacks | Evasion, poisoning, inference attacks at different layers |
| 7 | Missing AI explainability | Vector embeddings opaque; multi-stage RAG pipelines unauditable by default |
| 8 | Shadow AI | 75% of knowledge workers use GenAI; 78% bring their own tools (Samsung incident as canonical example) |

### The strategy stack

Six discrete layers articulated in the article, each with its own enforcement point:

1. **Access controls** — Zero Trust + ABAC + RBAC at the knowledge layer (not the file layer); see [[oversharing-controls|Oversharing Controls for AI Search]]
2. **Data classification + AI policy enforcement** — ISO 27001 ISMS baseline; NIST SP 800-60 impact-level mapping
3. **AI monitoring** — SIEM-integrated detections for oversharing patterns, retrieval anomalies, prompt injection
4. **AI observability** — RAG-pipeline tracing: prompt → retrieval → grounding scores → output, with full lineage
5. **AI Usage Control (AI-UC)** — UCON applied at answer time: Authorizations + Obligations + Conditions, evaluated continuously; see [[ai-usage-control|AI Usage Control (AI-UC / UCON for AI)]]
6. **AI Security Posture Management (AI-SPM)** — inventory of models, prompts, tools, connectors, datasets, indexes, caches; misconfiguration detection; see [[ai-spm|AI Security Posture Management (AI-SPM)]]

Underpinned by **Data Security Posture Management (DSPM)** that maps where sensitive data lives before AI touches it; see [[dspm|Data Security Posture Management (DSPM) for AI]].

### Standards anchoring

The article maps every recommendation to a published standard, which is unusual for vendor content:

- Zero Trust → [NIST SP 800-207](https://nvlpubs.nist.gov/nistpubs/specialpublications/NIST.SP.800-207.pdf)
- Access controls → NIST SP 800-53 AC + AU families
- Risk management → NIST AI RMF
- Log management → NIST SP 800-92
- Data classification → NIST SP 800-60
- ISMS structure → ISO/IEC 27001
- AI management → ISO/IEC 42001
- Usage control concepts → [Sandhu UCON ABC paper](https://profsandhu.com/journals/tissec/ucon-abc.pdf)

## Strengths and Weaknesses

**Strengths:**
- Clear articulation of inference exposure vs retrieval exposure as AI-specific failure modes that traditional access controls do not address.
- AI-UC as a distinct layer beyond access control — UCON Authorizations + Obligations + Conditions evaluated continuously is a sound conceptual frame.
- Strong standards anchoring throughout. Most vendor content treats standards as background; this treats them as the spine.
- The CIA + Auditability re-interpretation is concise and usable in enterprise risk conversations.

**Weaknesses:**
- The "5 platforms" section is product positioning. The DSPM, AI-SPM, Zero-Trust UCON, and LLM Observability platform categories are real and valuable; the implication that Knostic is the only category that exists at the *knowledge layer* is editorial.
- No coverage of agentic-specific risks — [[tool-abuse-chains|tool-abuse chains]], [[lethal-trifecta|lethal trifecta]], autonomous-action containment. Article is scoped to AI search / RAG, not agents broadly.
- No incident response specificity. What does "detect" actually mean in the AI context — IoCs? Behavioral baselines? Not articulated.
- Hallucination-rate and adoption statistics cited without primary source references in several places.

## Relations

- Supports: [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] (data layer + observability layer), [[rag-hardening|RAG Hardening]], [[agent-observability|Agent Observability]], [[indirect-prompt-injection|Indirect Prompt Injection]], [[three-retrieval-paths|Three Retrieval Paths for Injection Payloads]]
- Concepts introduced (or solidified): [[inference-exposure|Inference Exposure (and Retrieval Exposure)]], [[ai-usage-control|AI Usage Control (AI-UC / UCON for AI)]], [[shadow-ai|Shadow AI]], [[oversharing-controls|Oversharing Controls for AI Search]]
- Practices introduced: [[ai-spm|AI Security Posture Management (AI-SPM)]], [[dspm|Data Security Posture Management (DSPM) for AI]]
- Frameworks referenced: [[cyber-defense-matrix|Cyber Defense Matrix]] (Sounil Yu), [[ai-trism|Gartner AI TRiSM]] (Gartner), [[nist-ai-rmf|NIST AI Risk Management Framework (AI RMF)]], [[iso-iec-42001|ISO/IEC 42001 — AI Management Systems]]
- Companion to [[ai-coding-agent-governance|AI Coding Agent Governance (Knostic, 2025–2026)]] from the same vendor — that piece focuses on coding-agent governance; this piece focuses on AI search / RAG / knowledge layer
