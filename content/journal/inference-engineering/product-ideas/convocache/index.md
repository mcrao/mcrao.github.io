---
title: "ConvoCache"
description: "Attention-aware persistent memory for AI assistants."
summary: "Store and rehydrate the conversation state that actually mattered."
categories: ["inference-engineering"]
tags: ["startup", "memory", "assistant", "kv-cache"]
date: 2026-05-18
draft: false
weight: 8
---

## Pitch

ConvoCache gives AI assistants a persistent memory layer based on what the model actually attended to.

Instead of summarizing everything or relying only on vector retrieval, it tracks which past turns influenced later responses and retains those turns' KV state longer.

## Memory Policy

{{< mermaid >}}
flowchart TD
  Turns[Conversation turns] --> Attention[Observed attention weights]
  Attention --> Score[Retention score]
  Score --> Hot[Hot memory]
  Score --> Warm[Warm memory]
  Score --> Evict[Evict or summarize]
  Hot --> Rehydrate[Future session rehydration]
  Warm --> Rehydrate
{{< /mermaid >}}

## Customer

AI assistant builders, CRM copilots, sales assistants, and customer-support systems.

## Differentiation

Most memory systems store what was mentioned. ConvoCache stores what the model used.

## Risks

- KV persistence across model versions is hard.
- Storage costs can grow quickly.
- Attention is not always a faithful explanation of importance.

