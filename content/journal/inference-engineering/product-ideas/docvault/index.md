---
title: "DocVault"
description: "A shared KV cache network for RAG documents."
summary: "Compute any document's context once, serve it to every user forever."
categories: ["inference-engineering"]
tags: ["startup", "rag", "kv-cache", "docvault"]
date: 2026-05-18
draft: false
weight: 1
---

## Pitch

DocVault is the CDN for LLM context.

Every RAG application repeatedly computes the same document context. DocVault precomputes and stores reusable document KV representations, then serves them to future queries with much lower prefill latency.

## Customer Pain

A legal AI product may answer thousands of questions over the same SEC filing, contract library, or compliance manual. Without reusable document state, each session pays the prefill cost again.

The pain is easy to explain:

> You are paying the GPU to read the same document over and over.

## Product Flow

{{< mermaid >}}
flowchart LR
  Upload[Upload document] --> Normalize[Normalize and chunk]
  Normalize --> Compute[Compute reusable KV]
  Compute --> Vault[DocVault library]
  Query[Future RAG query] --> Retrieve[Retrieve chunks]
  Retrieve --> Vault
  Vault --> Inject[Inject cached KV]
  Inject --> LLM[LLM response]
{{< /mermaid >}}

## Technical Moat

The moat is position-invariant KV reuse. Prompt caching and prefix caching already exist, and systems like [Cache-Craft](https://arxiv.org/abs/2502.15734), [RAGCache](https://arxiv.org/abs/2404.12457), and [TurboRAG](https://arxiv.org/abs/2410.07590) are important prior art. DocVault's differentiated version would need:

- cross-customer document cache sharing,
- RoPE-aware position re-injection,
- quality repair when chunks move,
- tenant-aware access control,
- per-model KV libraries.

## Business Model

- Pay once to precompute a private document.
- Pay per cache hit or per saved input token.
- Public document library becomes cheaper as more customers use it.

## MVP

1. Pick one model, one RAG stack, one document domain.
2. Build a cache server around chunk hashes.
3. Start with position-aligned reuse as the fallback.
4. Add RoPE-aware re-anchoring as the research moat.
5. Demo TTFT reduction on repeated documents.

## Risks

- RoPE re-injection may not preserve quality.
- Existing RAG KV systems may close the gap quickly.
- Cross-customer sharing requires strong privacy and licensing boundaries.

## YC Sentence

We compute document context once and serve it forever, the same way a CDN caches web pages.

