---
type: maturity-model
title: "CMM D6: Data, Memory and RAG"
address: c-000139
created: 2026-05-24
updated: 2026-09-29
tags:
  - maturity-models
  - cmm
  - rag
  - data-security
  - recalibration
  - sec-of-ai
status: developing
origin: produced
scope_axis:
  - sec-of-ai
related:
  - "[[agentic-ai-security-cmm-2026]]"
  - "[[agentic-ai-security-cmm-recalibration-method-2026]]"
  - "[[inference-exposure]]"
  - "[[cognitive-file-integrity]]"
  - "[[agentic-cmm-regulated-fi-stress-test]]"
  - "[[differential-privacy]]"
  - "[[model-layer-attacks]]"
  - "[[nist-ai-600-1]]"
  - "[[microsoft-zt4ai]]"
  - "[[standards-review-microsoft-zt4ai-2026-Q2]]"
  - "[[threat-taxonomy-reconciliation]]"
  - "[[agentic-ai-threat-classes-2026]]"
  - "[[generative-coding-deployment-shape-2026]]"
  - "[[guardfall-shell-injection-audit]]"
  - "[[claude-code-github-action-credential-exposure]]"
  - "[[owasp-ai-exchange]]"
  - "[[memory-poisoning]]"
  - "[[agent-memory-isolation]]"
  - "[[agentic-ai-security-cmm-d7-observability]]"
  - "[[agentic-ai-security-cmm-dependency-rules]]"
  - "[[agentic-ai-security-cmm-d9-operations]]"
  - "[[agentic-ai-security-cmm-d8-supply-chain]]"
  - "[[agentic-ai-security-cmm-crosswalk]]"
  - "[[agentic-ai-security-cmm-crosswalk-us-fi]]"
  - "[[microsoft-sdl-evolving-security-practices]]"
  - "[[cyera-agent-guardian-release]]"
  - "[[cmm-stress-test-canadian-fi-google-2026-09]]"
  - "[[claude-cowork]]"
  - "[[agentic-ai-security-cmm-d2-identity]]"
  - "[[agentic-ai-security-cmm-d3-control-least-agency]]"
  - "[[agentic-ai-security-cmm-d4-runtime-guardrails]]"
  - "[[agentic-ai-security-cmm-measurement-protocol]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[dspm]]"
sources:
  - https://learn.microsoft.com/en-us/purview/ai-m365-copilot
  - https://learn.microsoft.com/en-us/purview/dlp-microsoft365-copilot-location-learn-about
  - https://learn.microsoft.com/en-us/sharepoint/restricted-content-discovery
  - https://owaspai.org/go/augmentationdataintegrity/
  - https://owaspai.org/go/augmentationdataleak/
  - https://owaspai.org/go/continuousvalidation/
  - https://owaspai.org/go/dataminimize/
  - https://owaspai.org/go/datapoison/
  - https://owaspai.org/go/dataqualitycontrol/
  - https://owaspai.org/go/devdataleak/
  - https://owaspai.org/go/obfuscatetrainingdata/
  - https://owaspai.org/go/ragtesting/
  - https://owaspai.org/go/shortretain/
  - https://owaspai.org/go/testingpromptinjection/
  - https://www.usenix.org/conference/usenixsecurity25/presentation/zou
verified: 2026-09-29
verified_against: []
verified_findings: 0
verified_note: "Targeted model-payload scope check against live OWASP data-minimization guidance; no archived source opened. Other source claims remain outside this pass."
---

# CMM D6: Data, Memory and RAG

## Domain decision and boundary

D6 grades a deployment's retrieval data, derived copies, fine-tuning data it supplies, and memory that agents write for later tasks. The assessor maps each corpus to its source, retrieval layer, authorization grain, copies, and operator. A corpus may be a document index, tenant mail and files, or a coding agent's repository working copy. A validation corpus is a held set of inputs and expected outputs used to check an agent or model change. The authorization grain follows the source's actual grants: document, repository or branch, tenant location, connector, or selected local folder. The inventory and entitlement test use the same grain.

The *asking principal* is the human who starts a run. Retrieval must resolve that person's current source rights, including items returned from an index or cache. Event and scheduled runs instead use a task-bound scope. [[agentic-ai-security-cmm-d2-identity|D2]] grades whether the agent's tool access stays within the human's rights; D6 grades content selected by a retrieval layer, even if that layer exposes a tool interface. A corpus uniformly readable by every asking principal has no per-person grant difference to trim, but still needs applicable provenance, integrity, and handling controls.

*Agent memory* is content an agent writes for a later task, conversation, or agent to read as context. A checkpoint used only to resume the same task is session state. A business record written through a tool is an action. A store serving both retrieval and memory takes both sets of criteria. [[agentic-ai-security-cmm-d4-runtime-guardrails|D4]] grades screening when retrieved content enters context; [[agentic-ai-security-cmm-d8-supply-chain|D8]] grades admission of standing instruction files. Supplier-operated retrieval and memory remain in D6 scope. The assessor tests customer-visible paths, inspects scoped supplier evidence, and records an applicable but unobservable control as unanswerable under the [[agentic-ai-security-cmm-measurement-protocol|Assessor's Handbook]].

## Failure paths

- A service identity retrieves a document the asker cannot open. Test two principals with different source grants and inspect the returned items before answer generation. [OWASP's RAG testing guidance](https://owaspai.org/go/ragtesting/) places this test at retrieval.
- A poisoned item enters an index, or a memory entry steers a later task. Put test content through the real ingest or write route; inspect the admission decision, later read, and removal path. Targeted poison may resemble normal data and pass an anomaly detector.[^aix-dataqualitycontrol]
- A source deletion leaves a retrievable copy or embedding. Trace one source item through every derived store and test removal within the stated period.

## L1–L5 progression

| Level | Observable D6 outcome |
|---|---|
| L1 | Data reach and memory state have no consistent assessment. |
| L2 | Data classes, retrieval origins, extensions, and corpus reach are recorded. |
| L3 | Retrieval follows source entitlements; corpus admission, copies, scope, and validation data have tested controls. |
| L4 | Memory writes and reads are confined and verifiable; labels, poisoning detection, and recovery controls operate. |
| L5 | The deployment tests named cumulative disclosure risks, measures poisoning limits by threat class, detects drift and source conflicts, rehearses recovery, and verifies item provenance before context entry. |

## Criterion catalogue

Each bold ID has one canonical determination. A criterion applies when its named object exists in the deployment; a test sample must cover every distinct authorization layer and storage location. A failed facet remains a finding even if another facet passes. A test record identifies source and retrieval revisions, grants, configured decision point, expected result, and actual result. The [OWASP AI Exchange augmentation-data integrity control](https://owaspai.org/go/augmentationdataintegrity/) informs the memory criteria. The levels and pass conditions are this CMM's synthesis, not an OWASP grading scheme.

### L2 detail

- **D6-CLASSIFY.** The organization records a class for each corpus at its authorization grain, each system an agent's tools read, and each memory store at the highest class it holds. A corpus-wide default counts only if it explicitly covers every unit at that grain. An unclassified unit fails. Not applicable if agents read nothing beyond prompts and write no memory. *Evidence:* classification rules and inventory reconciled to corpus units, tool-read systems, and memory stores.
- **D6-EXTEND.** A named reviewer approves each new source or extension before it first serves retrieval. Extensions include a grounding source, connector, index-feeding plugin, MCP index server, or editor indexer. Blanket permission for makers to add unreviewed sources fails. Not applicable without retrieval. *Evidence:* source and extension inventory, first-use dates, and individual approval records.
- **D6-ORIGIN.** The retrieval layer attaches each returned item's source identity before model context: document identity, repository/ref/path, or tenant site, drive, or mailbox as the source permits. A source name generated only by the model fails. Not applicable without retrieval. *Evidence:* traces or returned-item records showing origins.
- **D6-REACH.** A dated assessment covers every corpus location and compares principals who can reach it through the agent with its intended readers. It records oversharing from either broad source grants or retrieval under a broader service identity. A sampled subset of tenant sites or repositories cannot establish complete coverage. Not applicable without retrieval or where every corpus is uniformly public. *Evidence:* location inventory, access and owner records, assessment scope, and findings.

### L3 detail

- **D6-ENTITLE.** Each retrieval for an asking principal returns only items that person can open under the source's current grants, including items served from indexes, embeddings, and caches.[^aix-augleak] A service identity may fetch broadly only if the retrieval layer trims every result to the asker before context entry. Not applicable without human-initiated retrieval or where all corpora are uniformly public. *Evidence:* the authorization grain for each corpus and a two-principal retrieval test per grain; for supplier-held layers, scoped supplier enforcement evidence and a customer-side comparison where possible.
- **D6-ENTITLE-TASK.** Retrieval in an event or scheduled run uses a scope bound by its starter to the task's needed repositories, sites, or mailboxes. A whole-estate scope or one the agent can widen fails. Not applicable when every retrieval run has an asking principal. *Evidence:* startup binding, effective identity grants, and the task's required corpus set.
- **D6-HOLDOUT.** The organization stores each validation corpus apart from the training data, model artifacts, code, and instructions it holds. An identity that can write those assets cannot write the validation corpus.[^aix-continuousvalidation] A split with the same write grants fails. Not applicable without an organization-held validation corpus. *Evidence:* store locations and write policies compared across these assets.
- **D6-HOLDOUT-TRANSFER.** Every external copy of an organization-held validation corpus has a transfer record naming the recipient and an access policy at least as restrictive as the source corpus.[^aix-devdataleak] An unrecorded extract fails. Not applicable without an organization-held validation corpus. *Evidence:* external-copy register and the policy for each recipient; a no-copy record closes the population when no transfer occurred.
- **D6-REACH-REMEDIATE.** Each D6-REACH finding closes within a stated period through corrected grants, removed content, or an owner-approved exception naming accepted readers. Hiding a site from search while its broad grants remain is interim containment, as Microsoft's Restricted Content Discovery illustrates.[^rcd] Not applicable when D6-REACH is not applicable. *Evidence:* each finding linked to its owner decision, effective change, and closure date.
- **D6-SCAN.** The deployment applies a configured, threat-model-appropriate poisoning check to new corpus items before retrieval and to organization-supplied fine-tuning items before training. It rechecks held items on a stated cadence, including after a material source change. The method may use integrity or provenance checks, targeted content tests, or anomaly detection; two numeric thresholds are optional, and an anomaly method that cannot distinguish relevant poison needs a different control.[^aix-dataqualitycontrol] The configured hold, reject, or alert decision operates on the real ingest path. Investigable alerts reach named triage, and an alert-only rule records exposure before review. Not applicable only if the deployment has neither retrieval nor organization-supplied fine-tuning data. *Evidence:* threat model, check configuration, treatment rule, new-item and rescan records, and exercised hold or alert.
- **D6-SCAN-INJECT.** Before an item becomes retrievable, the ingest path checks it for model-directed instructions and holds or rejects a detected item. A user-channel prompt test alone cannot establish this control.[^aix-testing] Not applicable without retrieval. *Evidence:* ingest-path rule and a crafted item tested through that route, or a recorded ingest detection.
- **D6-SCAN-TEST.** A targeted test challenges each D6-SCAN method with planted poison matching the named threat cases and benign items from the same source class. Record, by treatment decision, the poison detected and the benign items wrongly held, rejected, or alerted; investigate misses and false holds before retaining the method.[^aix-dataqualitycontrol] A configuration without a route-specific test fails. Not applicable when D6-SCAN is not applicable. *Evidence:* planted and benign sets, source route, test revision, decisions, and resulting rates.
- **D6-SCOPE.** A recorded decision names the fields and records each corpus and supplied fine-tuning dataset needs. Its derived copies follow that scope. A source-control deployment also identifies excluded paths, and a tenant deployment identifies excluded locations.[^aix-dataminimize] The decision identifies which source fields, text segments, and tool results each model route needs for the task. Request construction keeps excluded material out of model payloads. An excluded item in a copy or outbound model request fails. Not applicable only if the deployment has no retrieval, supplied fine-tuning data, or model request carrying deployment data. *Evidence:* scope decision and sampled source, index, embedding, dataset content, and outbound model payloads for each applicable route.
- **D6-SCOPE-IDENTIFIERS.** The organization lists identifiers retained in supplied fine-tuning data solely for removal requests or lifecycle work and excludes them from the training run.[^aix-dataminimize] Not applicable without supplied fine-tuning data. *Evidence:* exception list and training-field selection.
- **D6-STORE-ACCESS.** A store holding corpus copies or embeddings permits direct reads only by named identities with documented operational roles and scoped grants.[^aix-augleak] Broad account-level access fails. Not applicable where retrieval keeps no copy. *Evidence:* store policy, effective reader list, and each identity's role and scope.
- **D6-STORE-ENCRYPT.** Every store encrypts copied corpus content and embeddings at rest.[^aix-augleak] Not applicable where retrieval keeps no copy. *Evidence:* encryption configuration for each store and volume.
- **D6-STORE-RETAIN.** Each store deletes an item's copies and embeddings after source deletion or scope exit within its stated period.[^aix-shortretain] Not applicable where retrieval keeps no copy. *Evidence:* retention rule, deletion or rebuild job, and a source item traced to absence in each store.
- **D6-TRUST.** A component outside the model attaches a recorded source trust level to each retrieved item and carries it into context. The model's own trust claim does not count. Not applicable without retrieval or where a corpus has only one source trust class. *Evidence:* trust scale, attachment rule, and returned items with source and level.

### L4 detail

- **D6-DETECT.** Production detection reads agent writes to memory or a corpus and raises an investigable alert for a suspected poisoned entry. A passive report without triage fails.[^aix-augintegrity] Not applicable if agents write neither memory nor corpus content. *Evidence:* coverage rule, alert route, and a live or tested alert.
- **D6-DETECT-CLASSES.** Planted tests measure each applicable D6-SCAN and D6-DETECT method separately for sabotage on ordinary queries and targeted poison triggered by chosen inputs.[^aix-datapoison][^poison] A single pooled rate hides the targeted case and fails. Not applicable if neither detector applies. *Evidence:* planted sets, decisions, and detection share by method and threat class.
- **D6-DETECT-PROTECT.** Identities able to write ingest data, corpora, or memory cannot change the associated detector logic, configuration, or baselines. An integrity record captures authorized detector changes.[^aix-dataqualitycontrol] Not applicable if neither D6-SCAN nor D6-DETECT applies. *Evidence:* effective write grants compared with data writers and detector change records.
- **D6-LABEL-CARRY.** Logic outside the model shows an answer's highest source class and applies that class to content the agent creates from the answer.[^labels] An unlabeled derived document fails. Not applicable without corpus retrieval. *Evidence:* source classes compared with answers and resulting files or messages.
- **D6-LABEL-GATE.** External policy excludes a named class from retrieval or blocks an answer drawing on it for disallowed principals or channels.[^dlp] A restricted answer delivered through a named channel fails. Not applicable without corpus retrieval. *Evidence:* policy and a blocked answer or equivalent test.
- **D6-MEMORY-LOG.** Each memory store sends state changes to an append-only log that the agent's identity cannot alter or delete; the log supports replay.[^aix-augintegrity] Not applicable without agent memory. *Evidence:* store and log configuration, effective write grants, and sample changes.
- **D6-MEMORY-PARTITION.** Every memory entry belongs to an agent, session, principal, or other policy-defined partition. An external read check denies partitions outside the trusted identity's scope; a model-supplied filter alone fails.[^aix-augintegrity] Not applicable without agent memory. *Evidence:* key, policy, and cross-partition refusal.
- **D6-MEMORY-PROVENANCE.** The memory writer or store records each entry's source, distinct agent or session writer, time, and partition outside model control.[^aix-augintegrity] A shared service account alone does not identify the writer. Not applicable without agent memory. *Evidence:* entries and writer-binding record.
- **D6-MEMORY-RESET.** Between tasks, a rule outside the model reviews carried context and resets content the next task's policy does not authorize.[^aix-augintegrity] Unreviewed carryover fails. Not applicable when every task starts fresh and agents write no memory. *Evidence:* carryover rule and a test showing disallowed content reset while permitted memory remains.
- **D6-MEMORY-VERIFY.** Before a memory entry enters context, a verifier compares it with a protected integrity record set at write and rejects or quarantines a mismatch.[^aix-augintegrity] Not applicable without agent memory. *Evidence:* verifier configuration and failed-entry test.
- **D6-MEMORY-WRITE.** Policy outside the model refuses writes to memory partitions outside the agent's or session's grant.[^aix-augintegrity] Not applicable without agent memory. *Evidence:* effective write policy and a denied cross-partition write.
- **D6-OBFUSCATE.** Exposure-restricted fields retained in supplied fine-tuning data are masked, tokenized, generalized, or otherwise obfuscated before training.[^aix-obfuscate] Not applicable without supplied fine-tuning data or restricted fields. *Evidence:* field inventory and transformed training input.
- **D6-OBFUSCATE-RESIDUAL.** The data owner records residual quasi-identifiers after obfuscation, with each one's removal, generalization, or named risk acceptance.[^aix-obfuscate] Not applicable when D6-OBFUSCATE does not apply. *Evidence:* residual field review compared with the training data.
- **D6-OBFUSCATE-TABLES.** Token reversal tables have access protection at least as strict as the data they can reconstruct.[^aix-obfuscate] Not applicable without such tables. *Evidence:* table and source-data access policies.
- **D6-REACH-CADENCE.** D6-REACH repeats over its full location frame on a stated schedule and after a new corpus or extension; each new finding follows the D6-REACH-REMEDIATE period. A missed review fails. Not applicable when D6-REACH does not apply. *Evidence:* schedule, complete run history, and finding dispositions.
- **D6-ROLLBACK.** Every applicable memory, index, and vector store can restore a recorded earlier state, and a test shows the state actually restored.[^aix-dataqualitycontrol] Rebuilding only from current sources does not prove rollback. Not applicable without any such store. *Evidence:* per-store snapshot and restore result.
- **D6-SCOPE-MEASURE.** Decisions to keep or remove each supplied fine-tuning field include a recorded measure of its effect on correctness, robustness, or fairness.[^aix-dataminimize] Not applicable without supplied fine-tuning data. *Evidence:* field decisions, experiments, and measured outcomes.
- **D6-SCOPE-PROPAGATE.** A source correction or deletion reaches derived training datasets, corpus items, copies, and embeddings within a stated period. A source-to-derived record makes the route traceable.[^aix-dataminimize] Not applicable where no such derivation exists. *Evidence:* lineage record and one propagated correction or deletion through each relevant store.
- **D6-TRUST-WEIGHT.** Retrieval uses D6-TRUST levels to rank or withhold equally relevant items; a lower-trust item cannot outrank its higher-trust peer solely because of content it controls. Not applicable when D6-TRUST does not apply. *Evidence:* ranking rule and a paired-item test.

### L5 detail

- **D6-ATTEST.** Before context entry, the retrieval layer verifies authenticated provenance and integrity for each returned item and refuses a failed item. An ingest signature, protected digest manifest, signed source version, or hash chain can supply the proof if the verifier checks the item actually returned. An unchecked source signature fails. Not applicable without retrieval. *Evidence:* item and verifier records, decision point, and a refused tampered or unauthenticated item.
- **D6-CONTRADICT.** Before release, a detection checks answers using more than one source for conflicting claims. A flagged answer is withheld, qualified, or routed to review under a recorded rule, with the source items retained. A retrospective sample alone fails. Not applicable without multi-source answers. *Evidence:* rule and a flagged production answer or route-matched test.
- **D6-ENTITLE-INFER.** The deployment identifies material combinations of individually readable items that would disclose a restricted inference to a named role. Retrieval policy records the prohibited inference and uses earlier disclosures in the active session to withhold a later item or hold the combination pending review. Test both a prohibited combination and a permitted one; item-by-item grants alone fail. The criterion covers the specified, tested combination risks, not every inference a model might make. Not applicable without human-initiated retrieval, with uniformly public corpora, or when a scoped threat assessment substantiates that no material combination risk applies. *Evidence:* threat and policy record, session decision state, and paired tests.
- **D6-ROLLBACK-DRILL.** Each quarter, the operator restores an applicable memory or index store to an earlier state using production configuration and measures time against a stated recovery target. A missed quarter fails. Not applicable when D6-ROLLBACK does not apply. *Evidence:* quarterly drill reports, restored state, time, and target.
- **D6-SCAN-BOUND.** For each applicable corpus, memory store, or supplied fine-tuning dataset, the owner records poisoning risk by threat class: tested misses, benign items wrongly held, rejected, or alerted, test coverage, and the consequence of each failure. The owner sets limits for the tested scenarios on miss and wrongful-treatment rates and a required response, then justifies configured treatment and residual acceptance against the results. A pooled tolerated share of poisoned items cannot bound a targeted or backdoor case.[^aix-datapoison][^aix-dataqualitycontrol] Not applicable if both D6-SCAN and D6-DETECT do not apply. *Evidence:* class-specific tests, treatment settings, and owner-approved residual decision.
- **D6-SCAN-DRIFT.** Production detection compares incoming corpus content with a recorded baseline and alerts when its distribution departs beyond a stated rule. A chart without an alert route fails. Not applicable without retrieval. *Evidence:* baseline, detection rule, and production or tested alert.

## Prerequisites and blockers

D6-ENTITLE requires current source grants and a trustworthy principal binding at every retrieval layer, including copies. D6-TRUST-WEIGHT requires source trust labels. D6-MEMORY-VERIFY requires a protected write-time record. D6-SCAN-TEST requires the actual ingest route and representative poison and benign samples. D6-SCAN-BOUND uses those results with class-specific consequences to support a residual decision. Record a missing dependency against the affected criterion and target. The [[threat-taxonomy-reconciliation|threat taxonomy reconciliation]] locates corpus, augmentation-store, memory, and validation-data attacks among several owners. D6 grades the data controls on those paths. [[agentic-ai-security-cmm-d7-observability|D7]] receives investigable screening findings and memory-write events. D6 grades the data decision that produced them.

## Deployment-shape differences

| Shape | Applicable decision |
|---|---|
| Public knowledge assistant | Public content still needs applicable origin, poisoning treatment, and integrity evidence. Per-asker entitlement is not applicable when all items are uniformly public. |
| Internal RAG assistant | Compare two users with different source grants at retrieval; include indexes, embeddings, and caches. |
| Coding agent | Repository or branch read grants set entitlement grain; excluded paths and editable instruction files add scope and memory questions. |
| Vendor-held RAG or tenant assistant | Customer-owned source grants and location reach remain assessable. Test what the customer can observe; require scoped supplier evidence for hidden ingest, index, and memory controls. An applicable opaque control is unanswerable, not exempt. |
| Agent without a corpus | Corpus and retrieval criteria are not applicable. Classification of tool-read systems and any supplied fine-tuning or validation data can still apply; D2 grades the identity and delegated-access bound on tool calls. |
| Memory-bearing agent | Partition, write, integrity, provenance, reset, log, and rollback criteria apply to content used across tasks. |

## Implementation and effort drivers

The L2 location frame must cover the complete corpus, not only frequently used sites. L3 effort includes source-grant cleanup, a two-principal retrieval test, and a poisoning treatment that works on the deployment's ingest route. Scan operation needs benign-case sampling and triage capacity: an overactive hold rule can remove useful material, while blended poison can evade anomaly checks.[^aix-dataqualitycontrol] L4 adds source-to-copy deletion lineage, memory isolation, protected detector configuration, and recovery tests. Supplied fine-tuning data brings separate minimization, obfuscation, and validation assets.

L5 item verification can use authenticated source or ingest records, a protected digest manifest, and a retrieval-time verifier. The outcome is a refused changed or unauthenticated item, regardless of construction. The investment case should price version maintenance, verifier latency, false holds, owner review of residual poisoning risk, and recovery drills. A supplier-operated path may make some evidence expensive or unavailable; the report keeps that uncertainty visible.

## Sources and material limits

The [OWASP AI Exchange augmentation-data confidentiality control](https://owaspai.org/go/augmentationdataleak/) informs entitlement and copy protection; its [integrity control](https://owaspai.org/go/augmentationdataintegrity/) informs memory provenance and partitioning. Its [data-quality control](https://owaspai.org/go/dataqualitycontrol/) offers several detection methods and a two-threshold example, while warning that anomaly thresholds can fail and rare valid items can be held. The [data-poisoning threat entry](https://owaspai.org/go/datapoison/) separates sabotage from targeted backdoors, which normal-case tests may miss. D6's acceptance criteria and levels are this CMM's synthesis. They do not establish a universal upper bound on poisoning or on inferences from permitted data. Data already embedded in model weights lies outside these retrieval and memory criteria.

[^aix-augintegrity]: [OWASP AI Exchange — AUGMENTATION DATA INTEGRITY](https://owaspai.org/go/augmentationdataintegrity/), retrieved 2026-08-18. Memory partition, provenance, integrity, and replay controls.
[^aix-augleak]: [OWASP AI Exchange — Direct augmentation data leak](https://owaspai.org/go/augmentationdataleak/), retrieved 2026-08-18. Rights and storage protection for retrieved content and vectors.
[^aix-continuousvalidation]: [OWASP AI Exchange — CONTINUOUS VALIDATION](https://owaspai.org/go/continuousvalidation/), retrieved 2026-08-19. Separate validation-data access and backdoor test limits.
[^aix-dataminimize]: [OWASP AI Exchange — DATA MINIMIZE](https://owaspai.org/go/dataminimize/), retrieved 2026-08-20. Data scope, training exclusions, and source corrections.
[^aix-datapoison]: [OWASP AI Exchange — Data poisoning](https://owaspai.org/go/datapoison/), retrieved 2026-08-20. Sabotage and targeted poisoning classes.
[^aix-dataqualitycontrol]: [OWASP AI Exchange — DATA QUALITY CONTROL](https://owaspai.org/go/dataqualitycontrol/), retrieved 2026-08-20. Detection methods, treatment examples, tests, and anomaly limits.
[^aix-devdataleak]: [OWASP AI Exchange — Development-time data leak](https://owaspai.org/go/devdataleak/), retrieved 2026-08-25. Exposure of training and validation data.
[^aix-obfuscate]: [OWASP AI Exchange — OBFUSCATE TRAINING DATA](https://owaspai.org/go/obfuscatetrainingdata/), retrieved 2026-08-20. Restricted fields, quasi-identifiers, and reversal tables.
[^aix-shortretain]: [OWASP AI Exchange — SHORT RETAIN](https://owaspai.org/go/shortretain/), retrieved 2026-08-20. Retention as data minimization.
[^aix-testing]: [OWASP AI Exchange — Testing against prompt injection](https://owaspai.org/go/testingpromptinjection/), retrieved 2026-08-19. Tests follow the untrusted insertion route.
[^dlp]: [Microsoft Learn — Protect Copilot interactions with Purview DLP](https://learn.microsoft.com/en-us/purview/dlp-microsoft365-copilot-location-learn-about), read 2026-09-24. Label-aware exclusions from responses.
[^labels]: [Microsoft Learn — Manage data security for Microsoft 365 Copilot](https://learn.microsoft.com/en-us/purview/ai-m365-copilot), read 2026-09-24. Source-label display and inheritance.
[^poison]: [USENIX Security — PoisonedRAG](https://www.usenix.org/conference/usenixsecurity25/presentation/zou), 2025. Targeted RAG knowledge corruption.
[^rcd]: [Microsoft Learn — Restrict discovery of SharePoint sites and content](https://learn.microsoft.com/en-us/sharepoint/restricted-content-discovery), read 2026-09-24. Temporary search restriction that leaves grants intact.
