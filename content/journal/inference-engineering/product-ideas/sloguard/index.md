---
title: "SLOGuard"
description: "Priority-aware request scheduling for enterprise LLM serving."
summary: "Protect enterprise P99 latency without buying more GPUs."
categories: ["inference-engineering"]
tags: ["startup", "slo", "scheduler", "enterprise"]
date: 2026-05-18
draft: false
weight: 5
---

## Pitch

SLOGuard is a scheduler replacement for multi-tenant LLM serving.

It prevents a low-priority long generation from blocking a high-priority enterprise request.

## Features

- Priority preemption.
- KV migration to DRAM for paused requests.
- HBM reservation for premium tiers.
- SLA-aware admission control.
- Per-customer latency accounting.

## Diagram

{{< mermaid >}}
flowchart TD
  Requests[Incoming requests] --> Classifier[Tier classifier]
  Classifier --> Premium[Premium queue]
  Classifier --> Standard[Standard queue]
  Classifier --> Batch[Batch queue]
  Premium --> Scheduler[SLO scheduler]
  Standard --> Scheduler
  Batch --> Scheduler
  Scheduler --> HBM[HBM slots]
  Scheduler --> Spill[Spill / pause low priority]
{{< /mermaid >}}

## Customer

LLM API companies with enterprise SLAs, internal platforms serving multiple teams, and SaaS products with paid and free tiers.

## Risks

- Needs deep runtime integration.
- Customers must trust it not to starve lower-tier users.
- SLA contracts vary widely.

