---
title: "Position-Invariant Document KV Cache"
description: "A research plan for reusable document KV caches in RAG systems."
summary: "Can document KV states be cached independent of prompt position and reused across RAG queries?"
categories: ["inference-engineering"]
tags: ["kv-cache", "rag", "rope", "prefix-caching", "research"]
date: 2026-05-18
draft: false
weight: 1
---

## Core Idea

RAG systems repeatedly retrieve the same document chunks. Today, many deployments still pay prefill cost every time those chunks appear in a prompt. Prefix caching helps when text appears as the same prefix, but RAG chunks can appear at different positions and alongside different neighboring chunks.

The research question:

> Can we store a document chunk's reusable KV representation once, then re-inject the correct positional information at query time?

The hard part is RoPE. A key vector is not just "what the token means"; it also carries position. The same chunk at position 0 and position 4000 does not produce identical K vectors after rotary embedding.

## Proposed Mechanism

Store the pre-RoPE semantic key:

```text
K_semantic = X W_K
K_positioned = RoPE(K_semantic, position)
```

Cache `K_semantic` by document hash. At serving time, retrieve the semantic K, apply RoPE for the actual prompt position, and compose it with the live query context.

{{< mermaid >}}
flowchart LR
  Chunk[Document chunk] --> Tokenize[Tokenize]
  Tokenize --> Linear[Compute pre-RoPE K and V]
  Linear --> Store[Store semantic KV by chunk hash]
  Store --> Query[Future RAG query]
  Query --> ReRoPE[Apply position for this prompt]
  ReRoPE --> Attend[Attention with live query]
{{< /mermaid >}}

## What Already Exists

- [OpenAI prompt caching](https://platform.openai.com/docs/guides/prompt-caching) and [Anthropic prompt caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) reduce cost and latency for repeated prefixes.
- [vLLM automatic prefix caching](https://docs.vllm.ai/en/v0.8.3/design/automatic_prefix_caching.html) and [SGLang RadixAttention](https://sgl-project-sglang-93.mintlify.app/concepts/radix-attention) manage shared prefix state in self-hosted systems.
- [RAGCache](https://arxiv.org/abs/2404.12457), [TurboRAG](https://arxiv.org/abs/2410.07590), and [Cache-Craft](https://arxiv.org/abs/2502.15734) are directly relevant. Cache-Craft is especially important because it targets reusable RAG chunk KV and reports large reductions in redundant computation.

So the novelty should not be framed as "RAG KV caching does not exist." It should be framed as:

- position-invariant semantic K storage,
- RoPE re-anchoring as the central mechanism,
- cross-tenant reusable document library as a system design,
- quality repair when chunks move or interact with new neighbors.

## Experiment Plan

1. Use Llama 3.1 8B or another open-weight RoPE model.
2. Select a RAG-style dataset with repeated chunks.
3. Compare four modes:
   - full recomputation,
   - exact prefix caching,
   - chunk KV reuse with no position correction,
   - semantic K storage with RoPE re-injection.
4. Measure:
   - TTFT,
   - throughput,
   - perplexity or answer F1,
   - attention similarity to full recomputation,
   - cache hit rate under shuffled chunk ordering.

## Novelty Opinion

This is the highest-upside research direction because it has both a paper story and a product story. The risk is also real: context interactions may mean cached chunk KV is not enough without partial recomputation or quality repair.

The first publishable result may be a negative-but-useful map:

> When is document KV reusable, and when does position/context drift break it?

## Tenure And Complexity

- **Prototype:** 4-6 weeks with hooks into Hugging Face attention modules.
- **Paper-grade system:** 3-6 months if integrated with vLLM or SGLang.
- **Complexity:** High.
- **Best venue shape:** MLSys, SOSP workshop, NeurIPS systems workshop, or arXiv-first systems paper.

## Inspiration

This is Cloudflare's CDN logic applied to inference context. Web CDNs cache bytes; this caches the model's internal representation of documents.

