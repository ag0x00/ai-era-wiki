---
type: concept
title: "Three Retrieval Paths for Injection Payloads"
created: 2026-04-30
updated: 2026-08-20
tags:
  - concepts
  - rag
  - prompt-injection
  - retrieval
  - threat-modeling
status: developing
aliases:
  - "Retrieval Path Taxonomy"
  - "RAG Injection Paths"
related:
  - "[[indirect-prompt-injection]]"
  - "[[rag-hardening]]"
  - "[[lethal-trifecta]]"
  - "[[mcp-security]]"
  - "[[securing-your-agents-talk]]"
  - "[[echoleak-copilot-zero-click]]"
  - "[[geminijack-gemini-enterprise-injection]]"
sources:
  - "[[.raw/talks/securing-your-agents-2026-04-30.md]]"
---

# Three Retrieval Paths for Injection Payloads

## Definition

A taxonomy from [[securing-your-agents-talk|Securing Your Agents]] (slide 28) that classifies how a malicious instruction reaches the LLM context window during agent operation. Three paths exist; they have very different attacker effort profiles; and **vector RAG attracts the most research attention while paths 2 and 3 carry most real-world risk**.

## The Three Paths

### Path 1 — Vector-Embedded RAG (HARDEST for attackers)

```
Doc → Chunk → Embed → Vector DB → top-k retrieval → LLM
```

The payload must:
1. Survive **chunking** (it might land at a chunk boundary)
2. Survive **embedding** (the semantic signal must persist through dimensionality reduction)
3. Be retrieved at top-k for a query the victim user is likely to ask

Effort: **HIGH**. But not impossible — research shows instructions retain semantic fidelity through embedding, and 5 carefully crafted documents in a corpus of millions can achieve ~90% retrieval-and-execution success against typical similarity thresholds.

### Path 2 — Full-Text / Direct Retrieval (BIGGEST PRACTICAL RISK)

```
Source → entire content into context → LLM
```

No chunking, no embedding. The full document hits the context window verbatim:
- Web pages fetched by a `browse(url)` tool
- Emails ingested by a mail-summary agent
- PDFs and Google Docs passed to a "review this document" flow
- MCP tool responses
- File contents read by coding agents
- Calendar invite bodies, meeting transcripts

Effort: **LOW**. The payload arrives intact with zero transformation. This is how [[echoleak-copilot-zero-click|EchoLeak]] and [[geminijack-gemini-enterprise-injection|GeminiJack]] operated, and it is the dominant pattern in [[johann-rehberger|Johann Rehberger's]] [[month-of-ai-bugs|Month of AI Bugs]] disclosures.

### Path 3 — Metadata and Hidden Fields (SNEAKIEST)

```
Hidden field → parsed by agent → LLM
```

The payload hides where humans don't look but agents parse:
- PDF metadata (Author, Title, Keywords, Subject)
- HTML comments (`<!-- … -->`)
- Zero-width Unicode characters in otherwise-normal text
- Image alt attributes
- Right-to-left override (RTL) and other invisible Unicode controls
- MCP tool description strings (the descriptions, not the responses)
- File system extended attributes (xattrs)
- Git commit message trailers

Effort: **LOW** — and the payload **survives human review** because the human never sees it. This is the path most likely to be missed by code review and red-team manual testing.

## Significance of the taxonomy

Each path requires a different defense:

| Path | Primary defense |
|---|---|
| 1 — Vector RAG | Pre-ingest content scanning; per-source canary tokens; trust attribution at chunk level |
| 2 — Full-text | Apply injection classifier to **every retrieved document** before assembling the prompt; trust-labeled boundary markers around retrieved content; never inline documents directly into the system prompt |
| 3 — Metadata | Strip hidden fields at ingest (HTML comment removal, Unicode normalization, PDF-metadata stripping); only display what humans can see; **render then re-extract** for documents being summarized |

A program that defends only path 1 (the academically interesting one) leaves the two paths attackers actually use wide open.

## Mapping to Real-World Attack Surfaces

| Surface | Dominant path |
|---|---|
| RAG over internal docs / knowledge base | Path 1 (also Path 2 if full-doc retrieval is used) |
| Mail-summary agent | Path 2 (email body) + Path 3 (HTML comments, hidden text) |
| Web-research agent | Path 2 (page body) + Path 3 (HTML comments, alt text) |
| Calendar / meeting agents | Path 2 (invite body) + Path 3 (metadata) |
| Coding agents | Path 2 (file content) + Path 3 (commit messages, README front matter) |
| MCP tool responses | Path 2 (response body) + Path 3 (tool description strings) |
| PDF document review | Path 2 (visible text) + Path 3 (PDF metadata, alt text, white-on-white text) |

## Defense Layering

The [[rag-hardening|RAG hardening]] practice operationalizes the multi-path defense:

1. **Ingest-time:** strip metadata, normalize Unicode, apply injection classifier per source, attach trust-source attribution.
2. **Assembly-time:** wrap each source with explicit trust-label boundary markers (see [[system-prompt-architecture|System Prompt Architecture (Boundary Markers + Trust Labels)]]); insert canary tokens between sources.
3. **Inference-time:** monitor for goal-hijack indicators in chain-of-thought (LlamaFirewall AlignmentCheck) and for canary-token appearances in output.
4. **Action-time:** if an action was triggered by a path-2 or path-3 retrieval (rather than direct user input), require human confirmation for any high-risk tool call. See [[least-agency-principle|Least Agency Principle]].

## See Also

- [[indirect-prompt-injection|Indirect Prompt Injection]] — the attack class this taxonomy decomposes
- [[rag-hardening|RAG Hardening]] — the practice page that uses this taxonomy
- [[mcp-security|MCP Security]] — path-2 and path-3 risks in the MCP protocol
- [[lethal-trifecta|Lethal Trifecta]] — the structural condition that makes any of these paths lethal

<!-- sources:auto -->
## Sources

- [billdx.github.io](https://billdx.github.io/Presentations/Securing%20Your%20Agents/securing-ai-agentic-apps.html)
<!-- /sources -->
