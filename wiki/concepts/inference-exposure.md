---
type: concept
title: "Inference Exposure (and Retrieval Exposure)"
address: c-000307
created: 2026-05-01
updated: 2026-08-20
tags:
  - concepts
  - ai-data-security
  - access-control
  - knowledge-layer
status: developing
origin: aggregated
scope_axis:
  - sec-of-ai
complexity: intermediate
domain: ai-data-security
aliases:
  - "Inference Exposure"
  - "Retrieval Exposure"
  - "AI knowledge leak"
related:
  - "[[ai-data-security]]"
  - "[[oversharing-controls]]"
  - "[[ai-usage-control]]"
  - "[[three-retrieval-paths]]"
  - "[[indirect-prompt-injection]]"
  - "[[security-controls-for-ai-stacks]]"
  - "[[owasp-ai-exchange]]"
  - "[[model-layer-attacks]]"
  - "[[differential-privacy]]"
sources:
  - "[[.raw/articles/knostic-ai-data-security-2026-05-01.md]]"
  - "https://aclanthology.org/2024.emnlp-industry.94.pdf"
  - "https://arxiv.org/html/2408.02416v1"
  - "https://owaspai.org/go/disclosureinoutput/"
  - "https://owaspai.org/go/modelinversionandmembership/"
  - "https://owaspai.org/go/sensitiveoutputhandling/"
  - "https://owaspai.org/go/smallmodel/"
coined_by:
  - "[[knostic]]"
---

# Inference Exposure (and Retrieval Exposure)

Two paired AI-specific failure modes that bypass traditional file/network access controls. Together they describe a class of risk that does not exist in non-AI systems. The [[owasp-ai-exchange|OWASP AI Exchange]] supplies the named attack forms and the cross-taxonomy identifiers used below, and its control names appear in caps as the Exchange writes them.

## Inference Exposure

A user gains **unauthorized insights from AI outputs** without ever accessing the original documents.

The user cannot open the source files. Their RBAC role does not permit it. But the AI was trained on, retrieved, or summarized those files for some other purpose — and the user is able to ask questions whose answers depend on that material. The AI infers and exposes what the user could not access directly. The two supply routes behave differently under control. Retrieved and summarized content keeps an access model beside it and can be re-checked when the answer is composed. Content absorbed into the model's parameters during training or fine-tuning does not: the Exchange states that once training data is embedded in a model, the access-right variations that governed the original data cannot be controlled any more.[^aix-disclosureoutput]

This mode does not exist in non-AI systems: there is no file-server analog where reading an authorized file leaks knowledge of an unauthorized one.

## Retrieval Exposure

Content retrieved by the AI to craft an output is used **beyond what the user is authorized to see**.

A RAG pipeline pulls fragments from a corpus the user has *some* access to. The composition of those fragments — their joint meaning, the inferences enabled by their combination — exceeds the per-document access policy. The output is a derivative work that no individual permission grant authorized.

## Named attack forms on the trained-on path

The trained-on path has two named forms in the adversarial-ML literature, and the [[owasp-ai-exchange|OWASP AI Exchange]] catalogues both as runtime threats against training-data confidentiality.[^aix-inversion] Model inversion reconstructs part of the training set by optimizing input to maximize confidence indications in the output. Membership inference presents input identifying an entity and reads the output's confidence indications to infer whether that entity was in the training set. Both grow more feasible as a model overfits, and model capacity, model type, and regularization set how much it overfits.[^aix-inversion] The identifiers are [[mitre-atlas|MITRE ATLAS]] `AML.T0024.001` and `AML.T0024.000`, OWASP Top 10 for ML `ML03:2023` and `ML04:2023`, and NIST AI 100-2 §2.4.2.[^aix-inversion] The wiki's fuller treatment is [[model-layer-attacks|Model-Layer Attacks]], and the quantitative mitigation is [[differential-privacy|differential privacy]]; the Exchange's only threat-specific control is `SMALL MODEL`, a constraint on model capacity applied when the model is trained. The rest of its list for the pair is four general input controls — `MONITOR USE`, `RATE LIMIT`, `MODEL ACCESS CONTROL`, and `OBSCURE CONFIDENCE` — and, separately, sensitive data limitation ([`/go/datalimit/`](https://owaspai.org/go/datalimit/)) — a group of five data controls rather than an input control.[^aix-inversion][^aix-smallmodel][^aix-datalimit]

Disclosure of sensitive data in model output is the third form and needs no attack at all. The Exchange describes it as an unintentional fault of inclusion, reached through ordinary use as readily as through attacker provocation, and covering personal data and other sensitive material such as copyrighted text held in the training set or in augmentation data.[^aix-disclosureoutput] Its control is `SENSITIVE OUTPUT HANDLING`, which detects and blocks exposure-restricted data at output time and detects recitation of training content by matching output against an indexed training set.[^aix-soh]

## Failure of traditional controls

| Control | What it checks | Why it misses inference / retrieval exposure |
|---|---|---|
| File ACLs | Can the user open this specific file? | The user never opens the file. |
| Network firewall | Can this IP reach this service? | The retrieval is allowed; the exfiltration is via answer text. |
| DLP on file egress | Is sensitive content being uploaded/emailed? | The AI output is the user's own answer, not a document copy. |
| RBAC on documents | Does the role permit this document type? | Each retrieved fragment may be permitted; the combination is not. |
| Answer-time semantic enforcement | Does the meaning of this answer exceed the asker's need-to-know? | Reaches retrieved content, which keeps an access model beside it. Content absorbed into model parameters carries no access model to re-check. |

## Effective approaches

The mitigation is **semantic boundary enforcement**: evaluate the *meaning* of retrieved or generated content at answer time, beyond the per-document permission already applied at retrieval time. This is the design space [[ai-usage-control|AI Usage Control]] occupies and that [[oversharing-controls|oversharing controls]] operationalize for AI-search products like Microsoft Copilot, Glean, Gemini.

Four concrete control patterns:

1. **Need-to-know enforcement at the knowledge layer.** Combine RBAC + ABAC + sensitivity labels + real-time output filters. The check happens at answer time, after retrieval but before delivery to the user.
2. **Continuous authorization within a session.** A one-time check at session start is insufficient — the conversation can drift across permission boundaries as it proceeds.
3. **Provenance and audit trail.** Capture prompt + retrieved context + applied policies + model output, every time, so post-incident investigation can reconstruct what the AI knew and what it disclosed.
4. **Pre-training data limitation, for the absorbed path.** The three patterns above act at answer time, which reaches retrieved content but not content already absorbed into model parameters. Sensitive data limitation acts earlier: the data is minimized, purpose-filtered, retention-bounded, or obfuscated before it reaches training ([`/go/dataminimize/`](https://owaspai.org/go/dataminimize/), [`/go/alloweddata/`](https://owaspai.org/go/alloweddata/), [`/go/shortretain/`](https://owaspai.org/go/shortretain/), [`/go/obfuscatetrainingdata/`](https://owaspai.org/go/obfuscatetrainingdata/)). The Exchange's own residual applies: obfuscation reduces the likelihood that training data can be reconstructed or linked back to individuals, and does not eliminate it.[^aix-obfuscate]

## Empirical evidence

The figures below measure system-prompt leakage, prompt extraction, and shadow-AI upload behaviour. They bound how readily an orchestration-layer artifact is recovered through conversation and how widely employees route corporate data through public tools. Recovery of training data or corpus content sits outside all three, and the inversion and membership-inference literature is its evidence base.

- Multi-turn prompt-leakage rates rose from 17.7% to 86.2% under specific attack patterns ([ACL 2024 systematic investigation](https://aclanthology.org/2024.emnlp-industry.94.pdf)).
- Defense techniques reduced prompt-extraction attacks 83.8% on Llama2-7B and 71.0% on GPT-3.5 ([arxiv 2408.02416](https://arxiv.org/html/2408.02416v1)) — non-trivial but not solved.
- 48% of employees admit uploading sensitive corporate data into public AI tools (cited in [[ai-data-security|AI Data Security (Knostic blog, 2026)]]) — primary contributor to retrieval exposure in shadow-AI deployments.

## See Also

- [[oversharing-controls|Oversharing Controls for AI Search]] — the practice that operationalizes inference- and retrieval-exposure mitigation
- [[ai-usage-control|AI Usage Control (AI-UC / UCON for AI)]] — UCON-based controls evaluated at answer time
- [[three-retrieval-paths|Three Retrieval Paths for Injection Payloads]] — vector/full-text/metadata; paths 2 and 3 are the practical inference-exposure risk vectors
- [[indirect-prompt-injection|Indirect Prompt Injection]] — adjacent failure mode (untrusted text steers the agent's retrieval/output)
- [[ai-data-security|AI Data Security (Knostic blog, 2026)]] — primary source
- [[security-controls-for-ai-stacks|Security Controls for AI Stacks]] — data layer (D6 Data, Memory & RAG)

## Notes

[^aix-disclosureoutput]: [OWASP AI Exchange — Disclosure of sensitive data in model output](https://owaspai.org/go/disclosureinoutput/), retrieved 2026-08-19. The disclosure mechanism, the augmentation-data path, and the statement that original access-right variations cannot be controlled once training data is embedded in the model.
[^aix-inversion]: [OWASP AI Exchange — Model inversion and membership inference](https://owaspai.org/go/modelinversionandmembership/), retrieved 2026-08-19. The two attack definitions, the overfitting relationship, the ATLAS / OWASP ML / NIST AI 100-2 identifiers, and the entry's control list, of which only `SMALL MODEL` is specific to these two threats.
[^aix-smallmodel]: [OWASP AI Exchange — SMALL MODEL](https://owaspai.org/go/smallmodel/), retrieved 2026-08-19. The capacity argument applied when the model is trained.
[^aix-soh]: [OWASP AI Exchange — SENSITIVE OUTPUT HANDLING](https://owaspai.org/go/sensitiveoutputhandling/), retrieved 2026-08-19. The exposure-restricted data classes, the output-time enforcement point, and recitation detection against an indexed training set.
[^aix-datalimit]: [OWASP AI Exchange — General controls for sensitive data limitation](https://owaspai.org/go/datalimit/), retrieved 2026-08-20. The group thesis and the five controls it holds.
[^aix-obfuscate]: [OWASP AI Exchange — OBFUSCATE TRAINING DATA](https://owaspai.org/go/obfuscatetrainingdata/), retrieved 2026-08-20. The risk-reduction statement that obfuscation reduces the likelihood that training data can be reconstructed or linked back to individuals, without eliminating it.

<!-- sources:auto -->
## Sources

- [knostic.ai](https://www.knostic.ai/blog/ai-data-security)
- [aclanthology.org](https://aclanthology.org/2024.emnlp-industry.94.pdf)
- [arxiv.org](https://arxiv.org/html/2408.02416v1)
<!-- /sources -->
