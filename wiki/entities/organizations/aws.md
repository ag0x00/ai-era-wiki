---
type: entity
entity_type: organization
org_type: vendor
title: "AWS (Amazon Web Services)"
created: 2026-05-04
updated: 2026-06-21
tags:
  - entities
  - organizations
  - hyperscaler
  - cloud
  - ai-platform
  - identity
  - glasswing
status: seed
scope_axis:
  - sec-of-ai
  - ai-in-sec-defense
homepage: "https://aws.amazon.com"
related:
  - "[[cedar]]"
  - "[[firecracker]]"
  - "[[miggo-security]]"
  - "[[cosai-org]]"
  - "[[aws-agentic-ai-security-scoping-matrix]]"
  - "[[aaron-brown]]"
  - "[[madhur-prashant]]"
  - "[[matt-saner]]"
  - "[[trajectory-aware-post-training-talk]]"
  - "[[glasswing]]"
  - "[[anthropic-glasswing-announcement]]"
  - "[[mythos]]"
sources:
  - "https://aws.amazon.com/security"
  - "https://aws.amazon.com/bedrock"
  - "[[.raw/articles/aws-agentic-ai-security-scoping-matrix-2026-05-07.md]]"
  - "https://www.anthropic.com/glasswing"
---

# AWS (Amazon Web Services)

**Sources:** [AWS (homepage)](https://aws.amazon.com) · [AWS Security](https://aws.amazon.com/security) · [Amazon Bedrock](https://aws.amazon.com/bedrock)

> [!gap] Stub
> Hyperscaler whose primitives anchor several wiki pages: [[cedar|Cedar]] (AWS-led OSS policy language, primary RA Control Plane PDP reference), [[firecracker|Firecracker]] (KVM-backed MicroVM, RA Runtime Plane sandbox reference), AWS Nitro Enclaves (TEE foundation referenced in [[miggo-security|Miggo]] guardrail-attestation pilots and L5+ Leading Edge tier), Amazon Bedrock (managed inference platform), AWS Secrets Manager + IAM (NHI baseline), Amazon Q (agent platform). March 2026 AI governance release for Cedar.
>
> Pending content: company overview, AI security product portfolio, AWS Marketplace AI security listings (Agent Guard, etc.), key personnel.

## Project Glasswing partnership (May 2026)

AWS is a named launch partner in [[glasswing|Project Glasswing]] (Anthropic's coalition initiative applying Mythos Preview to defensive vulnerability discovery on critical software). **Amy Herzog** (VP and CISO) is the quoted executive. AWS makes Mythos Preview available to Glasswing participants via **Amazon Bedrock**, has been testing Mythos in its own security operations applying it to critical AWS codebases, and is "bringing deep security expertise to our partnership with Anthropic and helping to harden Claude Mythos Preview." AWS's published "400 trillion network flows per day" telemetry framing is the scale context for its participation. See [[anthropic-glasswing-announcement|the Glasswing announcement paper page]].

## Frameworks authored

- [[aws-agentic-ai-security-scoping-matrix|AWS Agentic AI Security Scoping Matrix]] (2025-11-21) — four-scope agency/autonomy ladder + six security dimensions, authored by [[aaron-brown|Aaron Brown]] and [[matt-saner|Matt Saner]]. Extends AWS's earlier Generative AI Security Scoping Matrix.
- [[trajectory-aware-post-training-talk|Trajectory-Aware Post-Training]] (Unprompted March 2026) — [[aaron-brown|Aaron Brown]] and [[madhur-prashant|Madhur Prashant]] on trajectory-aware post-training of open-weight models for security agents, releasing the Open Trajectory Gym.
