---
type: gap
title: "PDP/PEP for Non-Tool-Mediated Agent Actions"
created: 2026-05-02
updated: 2026-05-03
tags:
  - gaps
  - reference-architecture
  - control-plane
  - egress-plane
  - mcp-security
  - deep-agents
  - cedar
  - coding-agents
status: open
question: "How does the wiki's PDP/PEP architecture (oversight-layer, Toolshed-style MCP proxy, ToolAnnotations) cover agents that bypass declared tools entirely — Claude-Code-style agents that write their own code and call internal APIs directly?"
why_it_matters: "The wiki's [[oversight-layer]] / [[mcp-security]] framing implicitly assumes tool-call mediation: every privileged action goes through a declared tool, the tool carries an annotation, and the proxy intercepts the call. When an agent skips the tool layer (writes Python that imports internal libraries, runs shell commands that hit local services, makes raw HTTP requests inside its sandbox), the PEP isn't on the path and the PDP has nothing to evaluate. This is a real coverage hole: the most capable production-grade agents in 2026 (Claude Code, Cursor in agent mode, AI-IDE assistants) operate this way by design, and the wiki's architecture as currently written is silent on them."
related:
  - "[[oversight-layer]]"
  - "[[mcp-security]]"
  - "[[toolshed]]"
  - "[[smokescreen]]"
  - "[[lethal-trifecta]]"
  - "[[lethal-bifecta]]"
  - "[[breaking-the-lethal-trifecta-talk]]"
  - "[[hooking-coding-agents-with-cedar-talk]]"
  - "[[cedar]]"
  - "[[sondera]]"
  - "[[agentic-ai-security-reference-architecture]]"
  - "[[agentic-ai-security-cmm-2026]]"
sources:
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_slides.pdf]]"
  - "[[.raw/talks/2026-03-04_Andrew-Bullen_Breaking-the-Lethal-Trifecta_transcript.md]]"
---

# PDP/PEP for Non-Tool-Mediated Agent Actions

## The gap

The wiki's [[oversight-layer|PDP+PEP]] architecture assumes a **tool-mediation chokepoint**: agents take privileged actions by calling declared tools, the tools are registered through a central proxy ([[toolshed|Toolshed]]-style), and the proxy is the policy-enforcement point. Annotations on tools provide the policy basis. This pattern works cleanly when:

- The agent framework only exposes declared tools.
- All MCP traffic flows through the central proxy.
- The agent cannot escape into general-purpose code execution.

It does **not** cover the production class of agents that has emerged in 2025–2026, exemplified by Claude Code, Cursor in agent mode, Antigravity, Kiro, Codex, Devin, and similar "deep agents" that:

- Write arbitrary Python / shell / TypeScript inside a sandbox.
- Import internal libraries and call internal services directly without a declared tool.
- Make raw HTTP requests to internal API endpoints.
- Read/write the filesystem and spawn subprocesses.

For deep agents, the tool-call PEP is bypassed by design — the *whole point* is that the agent decides what to call, not the developer.

## Origin

The gap was named explicitly by [[andrew-bullen|Andrew Bullen]] (Head of AI Security at Stripe) at [[unprompted-conference-march-2026|Unprompted, March 2026]]. Slide 12 ends with the question *"What about Claude Code style 'Deep' Agents?"* with no answer. Bullen's talk is otherwise the wiki's most concrete worked example of a tool-mediated PDP/PEP (Toolshed + `ToolAnnotations`), so when *he* admits the architecture doesn't cover this class, it's a gap worth elevating to the framework level rather than leaving as an aside on the talk page.

The transcript-only WIP fix, per Bullen:

> "Increasingly, agents don't need special tools — they'll just write their own code and hit random APIs on your existing services. So what do we do here? This is, like, work that is in progress right now, but the approach we're looking at is essentially proxying the connections coming out of agents, out of their sandboxes, and then using that as a choke point where you can similarly have annotations on the API endpoints that the agents are talking to."

So Stripe's intended answer has two layers:

1. **Move the egress PEP further down the stack** — proxy raw network egress out of the agent's sandbox, not at the tool-call boundary. [[smokescreen|Smokescreen]] is already this layer for Stripe.
2. **Annotate API endpoints, not just tools.** The same `ToolAnnotations` schema (or its equivalent) attaches to internal API endpoints; the egress proxy reads the annotations and enforces the policy.

This is **not shipped at Stripe** as of March 2026.

## Basis for framework-level classification

The wiki currently presents the [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] / [[guardian-agent|Guardian Agent]] / [[mcp-security|MCP Security]] story as if tool-mediation is the architectural answer. For deep agents this isn't an implementation detail to be filled in later — it's a structural mismatch between the assumed agent shape (calls declared tools) and the actual agent shape (writes arbitrary code in a sandbox). Specifically:

- **Control plane** of the [[agentic-ai-security-reference-architecture|RA]] is described as policy on tool calls. For deep agents, "tool calls" are not the right primitive.
- **CMM D3 (Control & Least-Agency)** evidence at L3 cites tool annotations + MCP gateway. For a deep-agent shop, those evidence artifacts are unavailable by construction; D3 needs an alternative evidence track.
- **CMM D5 (Egress & Network)** is the layer that *can* still apply — sandbox network egress is a real chokepoint regardless of whether the agent calls tools or writes code. This is why Stripe's WIP answer leans on D5 rather than D3.

## Closure conditions

Three levels of resolution, in increasing order of ambition:

1. **Acknowledge it explicitly in the RA.** Update [[agentic-ai-security-reference-architecture|Agentic AI Security Reference Architecture]] §Control plane and §Egress plane with a "deep-agent variant" subsection that clarifies which controls apply to declared-tool agents vs deep agents. Honest framing: today the PEP shifts from tool-call boundary down to sandbox-egress boundary, and the PDP shifts from tool annotations to endpoint annotations.
2. **Ship a deep-agent control pattern.** Document a vendor-neutral sandbox-egress-proxy + endpoint-annotation pattern as a sibling practice to [[prompt-injection-containment|Prompt Injection Containment for Agentic Systems]] / [[mcp-security|MCP Security]]. Draw on Stripe's WIP, [[sondera|Sondera's Cedar hook harness]] ([[hooking-coding-agents-with-cedar-talk|Maisel, Unprompted March 2026]]; now ingested), Block goose's red-team findings, the Anthropic Claude Code hooks model, and the Cursor / Codex sandbox model. None of these alone is a vendor-neutral spec; the synthesis is the contribution.
3. **Add CMM evidence track for deep agents.** D3 L3+ should accept either tool-annotation evidence (declared-tool shops) OR sandbox-egress + endpoint-annotation evidence (deep-agent shops). Today D3 implicitly favors the former; Stripe-tier deep-agent programs will fail the rubric for the wrong reason.

Resolution 1 is achievable now. Resolution 2 needs at least one publicly-shipped example beyond Stripe's WIP — likely 2026 H2. Resolution 3 follows from 2.

## Related work

- **[[tenuo|Tenuo]] capability warrants ([[capability-based-authorization-talk|Niyikiza, Unprompted March 2026]])** — ingested 2026-05-03. **Partially closes this gap from the delegation-aware-capability angle.** Cryptographic warrants attenuate at every hop and are enforceable at four points (in-process / sidecar / gateway / MCP-proxy). Reports baseline 90%→0% multi-agent attack-success rate on Tenuo's own custom harness (no public benchmark exists yet). Important caveat: warrants are still tool-mediation-shaped — they bind named MCP tools or named API endpoints. For deep agents that call internal libraries or run shell commands inside a sandbox, capability enforcement still depends on the sandbox-egress proxy or the endpoint-annotation layer being on the path. So Tenuo addresses *delegation-aware* PDP/PEP; the *non-tool-mediated action* hole remains in the egress-proxy + endpoint-annotation track.
- **[[sondera|Sondera]] Cedar hook harness ([[hooking-coding-agents-with-cedar-talk|Maisel, Unprompted March 3, 2026]])** — ✅ ingested 2026-05-03. Reference monitor for Cursor / Claude Code / Gemini CLI using Cedar + Rust hooks. Introduces the **trajectory event model** (action/observation/control/state) as the correct structural primitive for coding-agent enforcement — superseding "tool call mediation" for deep agents. Open source. The harness intercepts shell commands, file writes, and code execution events *before* execution, feeds them through Cedar policies with YARA signatures and IFC taint tracking. This is the closest published answer to a vendor-neutral deep-agent PDP. Partially closes this gap from the **hook-based per-action enforcement** angle. The remaining open sub-question: how to extend this beyond instrumented agents (e.g. when the agent process itself is not under the hook wrapper).
- **Block goose** — open-source agent + Operation Pale Fire red-team. May contain the threat model that motivates the deep-agent control set.
- **Stripe (Bullen)** — the gap is named here; the WIP answer is here too.
- **Anthropic Claude Code hooks** — settings-level event handlers that fire on tool-call boundaries; would need extension to non-tool actions to fully cover the gap.
- **Cursor / Codex sandbox** — vendor-side sandbox is the *containment* layer but doesn't provide the *policy* layer this gap is about.

## Open questions tracked here

1. What's the right primitive for a deep-agent PDP — endpoint annotations, capability tokens (à la [[tenuo-warrant|Tenuo Warrants]]; see also [[capability-based-authorization|Capability-Based Authorization]]), or per-action policies expressed in a DSL like Cedar? Tenuo is now publicly shipped and answers the *delegation-aware* sub-question; the *non-tool-mediated action* sub-question is still open.
2. Can the same `ToolAnnotations` schema be re-used at the API-endpoint layer, or does deep-agent enforcement need a richer schema (e.g. data-class flowing through the call, not just per-endpoint sensitivity)?
3. Where does sandbox-escape sit in this picture? A deep-agent sandbox compromise reduces the egress proxy to "the only line of defense" — is that acceptable, or do we need defense-in-depth at the sandbox boundary too?
4. How does this interact with [[a2a-protocol|A2A]] when one agent's API call is the other agent's tool call?

## See also

- [[breaking-the-lethal-trifecta-talk|Breaking the Lethal Trifecta (Without Ruining Your Agents)]] — original disclosure of the gap.
- [[oversight-layer|Oversight Layer (PDP + PEP for Agentic AI)]] · [[mcp-security|MCP Security]] · [[toolshed|Toolshed (Stripe)]] · [[smokescreen|Smokescreen (Stripe)]]
- [[agentic-ra-open-design-questions|Agentic AI Security RA — Open Implementation Questions]] — sibling gap page; this one extends the "single-broker vs mesh" question downward to the sandbox-egress layer.
