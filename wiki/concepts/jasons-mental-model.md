---
type: concept
title: "Jason's Mental Model: Security Architecture Breakdown"
created: 2026-04-30
updated: 2026-07-30
tags:
  - concepts
  - security-architecture
status: developing
no_public_url: "Internal analytical construct (named contributor's mental model); no external source"
complexity: intermediate
domain: enterprise-security
aliases:
  - "Security Architecture Functional Breakdown"
related:
  - "[[security-controls-for-ai-stacks]]"
  - "[[agentic-ai-security-reference-architecture]]"
sources: []
---

# Jason's Mental Model

A four-layer functional breakdown of an enterprise Security Architecture organization: Governance / Conceptual / Domain / (with the implied fourth being delivery). The model frames Security Architecture as "the connective tissue between business strategy and engineering execution".

## Original Content

In our view, Security Architecture is more than drawing diagrams; it is the connective tissue between business strategy and engineering execution. It ensures that every dollar spent on modernization actually reduces the bank's residual risk profile.

Here is our proposed functional breakdown for your Security Architecture organization.

1. Architectural Board & Governance (The Steering Layer)

This function acts as the "Supreme Court" of security design. It ensures alignment with enterprise standards, regulatory requirements (like DORA or Basel III), and the bank’s risk appetite.

| Objective   | To provide oversight, resolve technical debt disputes, and ensure cross-domain consistency.            |
| ----------- | ------------------------------------------------------------------------------------------------------ |
| Key Outputs | • Security Reference Architecture (SRA): The master blueprint of "how we do security" across the bank.<br>• Architecture Decision Records (ADRs): Formal documentation of why specific technology paths were chosen (or rejected).<br>• Exception Register: Tracking and time-boxing deviations from the standard to prevent permanent technical debt.<br>• Security Standards Library: High-level mandates (e.g., "All internal traffic must be encrypted via TLS 1.3"). |

  

  

2. Conceptual Architecture (The Strategy Layer)

This function sits between the CISO and the Business Units. It focuses on the "Why" and the "When," translating business goals (e.g., "Launch a new digital-only mortgage product") into security requirements.

| Objective   | To align security investments with the business roadmap and define the evolution of the security stack.                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Key Outputs | • Security Capability Roadmap: A 3-year vision of how security tools will evolve (e.g., the transition from VPN to Zero Trust).<br>• Threat Models (Conceptual): High-level analysis of new business ventures to identify systemic risks before a single line of code is written.<br>• Security Value Streams: Mapping security activities to business outcomes to prove ROI to the board.<br>• Merger & Acquisition (M&A) Security Playbooks: Standardized frameworks for assessing and integrating the security posture of third-party acquisitions. |

  

  

3. Domain/Physical Architecture (The Specialist Layer)

These are your Subject Matter Experts (SMEs). They are embedded within specific technology domains (Cloud, Identity, Data, Network) to ensure the high-level strategy is physically possible and correctly implemented.

A. Cloud & Infrastructure Architecture

 * Focus: AWS/Azure/GCP landing zones, container security, and serverless.

 * Outputs: Infrastructure-as-Code (IaC) security templates, Golden Image specifications, and Cloud Guardrails.

B. Identity & Access Management (IAM) Architecture

 * Focus: The "new perimeter"—Customer IAM (CIAM) and Workforce IAM.

 * Outputs: Authentication patterns (OIDC/SAML), Privileged Access Management (PAM) workflows, and Directory schemas.

C. Data & Application Architecture

 * Focus: Securing the CI/CD pipeline and protecting the bank's "crown jewel" data.

 * Outputs: Data Encryption standards (at rest/in transit), API Security specifications, and Secure Coding patterns.

D. Cyber Defense & Operations Architecture

 * Focus: Ensuring the SOC has the telemetry it needs.

 * Outputs: Logging and Monitoring standards, SIEM/SOAR integration blueprints, and Incident Response automation workflows.

Summary of the "Architectural Flow"

In a modernized bank, the flow should be seamless:

 * Governance sets the policy.

 * Conceptual translates that policy into a multi-year roadmap.

 * Domain Architects build the specific blueprints to make that roadmap a reality.

This structure moves your 700-person team from a reactive stance (fixing things that are broken) to a proactive stance (building things that are inherently secure).