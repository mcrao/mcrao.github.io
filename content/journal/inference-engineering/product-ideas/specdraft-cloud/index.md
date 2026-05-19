---
title: "SpecDraft Cloud"
description: "Managed speculative decoding that improves from production traffic."
summary: "A draft model service that learns from accepted and rejected tokens."
categories: ["inference-engineering"]
tags: ["startup", "speculative-decoding", "eagle", "inference"]
date: 2026-05-18
draft: false
weight: 9
---

## Pitch

SpecDraft Cloud wraps an LLM deployment with managed speculative decoding. Its draft heads improve over time using the customer's own accept/reject stream.

## Flywheel

More traffic produces more accept/reject labels. More labels improve the draft head. A better draft head increases acceptance rate. Higher acceptance rate reduces decode cost.

{{< mermaid >}}
flowchart LR
  Traffic --> Labels[Accept/reject labels]
  Labels --> Train[Draft-head tuning]
  Train --> Acceptance[Higher acceptance rate]
  Acceptance --> Savings[Lower latency and cost]
  Savings --> Traffic
{{< /mermaid >}}

## Customer

API companies and LLM SaaS products with domain-specific traffic patterns.

## Risks

- Needs enough traffic to personalize.
- Customers may not want a managed service in their inference path.
- Maintaining output equivalence is non-negotiable.

