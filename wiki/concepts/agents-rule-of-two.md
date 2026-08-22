---
type: concept
title: "Agents Rule of Two"
address: c-000244
created: 2026-07-30
updated: 2026-08-16
tags:
  - concepts
  - prompt-injection
  - least-agency
  - meta
  - agentic-coding
status: developing
scope_axis:
  - sec-of-ai
source_url: "https://ai.meta.com/blog/practical-ai-agent-security/"
aliases:
  - "Rule of Two"
related:
  - "[[agentic-ai-security-cmm-d3-control-least-agency|D3 Control & Least-Agency]]"
  - "[[lethal-trifecta|Lethal Trifecta]]"
  - "[[lethal-bifecta|Lethal Bifecta]]"
  - "[[least-agency-principle|Least-Agency Principle]]"
  - "[[hitl|Human-in-the-Loop]]"
  - "[[claude-code-github-action-credential-exposure|Claude Code GitHub Action Credential Exposure]]"
  - "[[securing-agentic-coding|Securing Agentic Coding]]"
  - "[[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]]"
  - "[[meta|Meta]]"
  - "[[prompt-injection-containment|Prompt Injection Containment]]"
  - "[[accidental-meltdown|Accidental Meltdown]]"
  - "[[openai-hugging-face-agent-incident|OpenAI–Hugging Face Agent Incident]]"
  - "[[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]"
  - "[[evaluation-containment-failure|Evaluation Containment Failure]]"
sources:
  - https://ai.meta.com/blog/practical-ai-agent-security/
  - https://www.microsoft.com/en-us/security/blog/2026/06/05/securing-ci-cd-in-agentic-world-claude-code-github-action-case/
---

# Agents Rule of Two

The **Agents Rule of Two** states that within a single session an agent should hold no more than two of three properties: **[A]** processing untrustworthy input, **[B]** access to sensitive systems or private data, **[C]** the ability to change state or communicate externally. An agent that needs all three should not run autonomously — it runs under supervision, with human approval on the actions that complete the triple.

[[meta|Meta]] published the rule on 2025-10-31 in [*Agents Rule of Two: A Practical Approach to AI Agent Security*](https://ai.meta.com/blog/practical-ai-agent-security/), crediting Chromium's security policy for the structure and [[lethal-trifecta|Simon Willison's lethal trifecta]] for the property set.

## Relationship to the Lethal Trifecta

The two framings name the same condition and differ in what they hand the engineer. The trifecta is diagnostic: it explains why a class of attacks works. The Rule of Two is a design constraint: it tells a system designer which property to remove, and it supplies a defined fallback — supervision — for the case where none can be removed.

That fallback is where the rule earns its keep and also where it is weakest. "Operate under supervision" is a real answer only when the supervising human can see the action that matters. In the deployment shapes where all three properties are unavoidable, the human is usually the one who is absent.

## Origin in coding agents

A coding agent in continuous integration holds all three properties by construction. It reads issue bodies and pull-request descriptions written by anyone (**A**), it holds repository credentials and a model API key (**B**), and it pushes commits and calls out to the network (**C**). The [[claude-code-github-action-credential-exposure|Microsoft Defender research on the Claude Code GitHub Action]] (2026-06-05) leads its recommendations with the Rule of Two for exactly this reason, and it puts that formulation ahead of the trifecta as its primary framing.

The interactive shape is different. A developer at a terminal supplies the supervision the rule calls for, which is why the same harness produces a different risk profile depending only on where it runs. See [[generative-coding-deployment-shape-2026|Generative Coding Deployment Shapes]] for the per-shape breakdown.

## Applying It

Removing a property is the whole technique, and each removal has a standard implementation:

| Property to remove | Implementation | Cost |
| --- | --- | --- |
| **A** — untrusted input | Restrict the agent to repository content from trusted branches; exclude fork pull requests, issue bodies, and external comments | The agent stops doing triage work, which is often the point of deploying it |
| **B** — sensitive access | Credentials held by a proxy outside the agent boundary; scoped, short-lived, one key per workflow | Requires the [[credential-proxy-pattern\|credential proxy]] infrastructure to exist |
| **C** — state change and egress | Default-deny egress allowlist; agent proposes a diff rather than pushing it | Loses the unattended-completion property that motivated the deployment |

The rule does not resolve the tension it exposes. Every removal costs the capability that made the deployment attractive, which is why organizations reach for supervision as the default answer and why the quality of that supervision — not its presence — is the thing an assessor should test.

**A removed property is verified where the agent's dependencies end, not where its own network policy ends.** OpenAI's evaluation configuration read as a compliant split: agents held **B** (evaluation data and model-internal state) and, on the operator's assessment, not **C**, because a per-workload sandbox disabled the internet. Whether **A** was present is itself contested — see [[lethal-trifecta|the trifecta page]] on optimization pressure as a substitute for attacker-supplied content — but the split failed on **C** regardless. Two things were wrong with that reading. The sandbox addressed only the external-communication half of **C** and left state change untouched, and even the half it addressed was assessed at the wrong boundary. It had been assessed from the network policy on the agent's own pod, while the single permitted dependency, an internal JFrog Artifactory package manager and caching proxy, held broad internet access of its own; an SSRF against it produced indirect egress on 2026-05-26 with the pod policy still correctly enforced, and the fleet-shared repository behind it had already become a write channel between isolated runs (Dalton and Wallace, *The 'Breaking' News: The OpenAI–Hugging Face Incident*, Black Hat USA 2026, summarized at [[openai-hugging-face-incident-blackhat-2026|OpenAI–Hugging Face Incident Reconstruction]]). The assessment rule that follows: enumerate every destination the agent may still reach, and for each one record its own egress reach and whether other workloads can write to it. **C** is removed only when no destination on that list can reach further than the agent may.

## Limits

The rule has four limits. It is per-session and says nothing about a property acquired across sessions through persistent memory or a written configuration file, which is the [[memory-poisoning|memory-poisoning]] path. It treats the three properties as binary when each is a gradient — a narrowly scoped token still satisfies **B**. It offers no test for whether supervision is real, so a workflow that satisfies the rule on paper by routing every action through an approval prompt no one reads has satisfied nothing. The fourth limit sits upstream of the other three: the rule counts properties but does not say how one is measured, and the [[openai-hugging-face-agent-incident|OpenAI–Hugging Face incident]] shows an organization scoring **C** as absent from a network policy that governed only the agent's own pod.

A fifth limit is about scope rather than measurement: the rule's three properties are drawn from an injection threat model, and [[accidental-meltdown|accidental meltdown]] satisfies none of the preconditions the rule assumes. The rule still holds against it, and for a reason unrelated to its design — capping what an agent can reach bounds the damage whether or not an adversary is present. The four [[evaluation-containment-failure|evaluation containment failures]] disclosed in 2026 are the worked cases.

The rule is a narrowing of [[least-agency-principle|least agency]] rather than a replacement for it. Least agency says strip every capability you can, which requires a capability inventory to act on; the Rule of Two gives an architect a bounded test that can be applied without one. [[agentic-ai-security-cmm-d3-control-least-agency|D3]] treats it as the design-constraint restatement of the [[lethal-trifecta|lethal-trifecta]] test.
