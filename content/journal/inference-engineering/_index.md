---
title: "Inference Engineering"
description: "Research and product ideas around LLM inference systems, KV cache memory, serving economics, and startup opportunities."
summary: "A living notebook for inference engineering research topics and product ideas."
categories: ["inference-engineering"]
tags: ["inference", "llm", "systems", "journal"]
date: 2026-05-18
draft: false
weight: 20
---

Inference engineering sits in the part of AI where model quality, GPU memory, latency, hardware utilization, and product economics all collide.

This section organizes ideas from an inference engineering workshop, a follow-up reading of inference systems material, and a product brainstorm around what could become research papers, open-source projects, or startup wedges.

The recurring theme is simple: as LLMs move into production, the bottleneck is often not "can the model answer?" but "can we serve the answer fast, cheaply, reliably, and repeatedly?"

## Maps

- [Research Topics](research-topics/) - paper-shaped ideas, experiment plans, novelty notes, and implementation paths.
- [Product Ideas](product-ideas/) - startup-shaped ideas, customer wedges, moats, risks, and 6-12 month build plans.

## Mental Model

{{< mermaid >}}
flowchart LR
  Prompt[User prompt / RAG documents] --> Prefill[Prefill: compute KV cache]
  Prefill --> Cache[KV cache memory]
  Cache --> Decode[Decode: one token at a time]
  Decode --> Response[Response]

  Cache --> Cost[GPU memory cost]
  Cache --> Latency[TTFT and ITL]
  Decode --> Util[GPU utilization]
  Prefill --> Reuse[Prefix and document reuse]

  Reuse --> Products[Products: DocVault, PrefillX, ConvoCache]
  Util --> Products2[Products: InferGrid, DraftOS, SLOGuard]
  Cost --> Research[Research: quantization, tiering, scheduling]
{{< /mermaid >}}

## Best Current Bet

The strongest combined research-plus-startup direction is **position-invariant document KV caching**: compute a document's context once, store it in a reusable KV representation, and serve it across many RAG queries without paying full prefill every time.

This is not the same as ordinary prompt caching. Prompt caching usually rewards exact prefix reuse and short-lived locality. The harder version asks whether a document chunk can be cached independent of where it appears in a prompt. That requires dealing with RoPE position encoding, quality repair, cache invalidation, and cross-customer trust boundaries.

If it works, it becomes the "CDN for LLM context."

