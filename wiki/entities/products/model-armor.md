---
type: entity
entity_type: product
title: "Model Armor"
created: 2026-09-30
updated: 2026-09-30
tags:
  - products
  - google-cloud
  - runtime-guardrails
status: developing
scope_axis:
  - sec-of-ai
origin: aggregated
parent_org: "[[google]]"
role: "Google Cloud prompt, response, document, and agent-interaction screening service"
homepage: "https://cloud.google.com/security/products/model-armor"
related:
  - "[[google]]"
  - "[[google-cloud-agentic-security-profile]]"
  - "[[gemini-enterprise-control-sheet]]"
  - "[[gemini-workspace-control-sheet]]"
sources:
  - "https://docs.cloud.google.com/model-armor/overview"
  - "https://docs.cloud.google.com/model-armor/integrations"
  - "https://docs.cloud.google.com/gemini/enterprise/docs/enable-model-armor"
  - "https://docs.cloud.google.com/model-armor/data-residency"
  - "https://docs.cloud.google.com/model-armor/model-armor-agent-gateway-integration"
verified: 2026-09-30
verified_against: []
verified_findings: 0
verified_note: "Whole-page source read against live Google documentation, including intermediate grounding-data integration."
---

# Model Armor

Google Cloud's [[google|Google]] Model Armor screens AI prompts, responses, and, on supported routes, documents and agent interactions. A policy can inspect and report or block content that triggers configured detectors. The [product description](https://cloud.google.com/security/products/model-armor) names [[prompt-injection|Prompt Injection]], malicious URLs, harmful content, and sensitive data among its detection classes. The deployed integration and template determine what it actually sees and enforces.

## Integration boundaries

The [integration matrix](https://docs.cloud.google.com/model-armor/integrations) distinguishes direct API calls from managed integrations. The REST API supports text, documents, and images, but returns a detection verdict that the calling application must enforce. Among managed integrations, Gemini Enterprise supports document screening. Google's [integration matrix](https://docs.cloud.google.com/model-armor/integrations) excludes images embedded in documents, while its newer [Gemini Enterprise setup guide](https://docs.cloud.google.com/gemini/enterprise/docs/enable-model-armor) says that images inside directly uploaded documents are screened. Test the exact upload and image path before claiming either result. Agent Gateway, Agent Platform, and the other listed managed integrations have their own text-only or route-specific coverage.

The [Gemini Enterprise app integration](https://docs.cloud.google.com/gemini/enterprise/docs/enable-model-armor) uses templates for user prompts and assistant responses. Its administrator can select inspect or block behavior and whether user interactions continue if Model Armor processing fails. Google states that this integration is available on all Gemini Enterprise editions without an additional charge, although it can add latency. It blocks a response that triggers configured Sensitive Data Protection detectors. It does not return a masked or de-identified response. The [integration matrix](https://docs.cloud.google.com/model-armor/integrations) also says this route screens intermediate grounding data and web-search responses. Test the deployed connector and agent path before crediting that coverage. Google's setup guide scopes the direct integration to the Enterprise assistant, employee-made Workflow Builder agents, and Google-made agents. Custom ADK, A2A, and Dialogflow agents need protection on their own route.

The [Agent Platform integrations](https://docs.cloud.google.com/model-armor/integrations) have different coverage and configuration choices. The [Agent Gateway integration](https://docs.cloud.google.com/model-armor/model-armor-agent-gateway-integration) screens ADK `streamQuery` ingress and only its listed egress payloads; other payloads may traverse the gateway without sanitization. The Gemini model integration supports floor settings or templates for supported non-streaming model calls. A direct model or tool route outside the configured point needs separate protection or an explicit exception.

## Control decision

For each deployment, record the integration, template or floor setting, detector thresholds, enforcement mode, processing-failure mode, region, and owner. Run a benign case and a controlled attack or sensitive-data canary through every claimed path; retain the result and logs. Check the [regional processing terms](https://docs.cloud.google.com/model-armor/data-residency) for the exact integration before using a regional endpoint as evidence of data residency. Model Armor can reduce exposure on a path it inspects, but it does not replace source permissions, action authorization, recipient restrictions, or incident reconstruction. [[gemini-enterprise-control-sheet|Gemini Enterprise Control Sheet]] and [[google-cloud-agentic-security-profile|Google Cloud Agentic Security Profile]] put those checks into distinct deployment routes.
