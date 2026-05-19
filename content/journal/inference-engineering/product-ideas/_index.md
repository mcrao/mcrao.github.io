---
title: "Product Ideas"
description: "Startup-shaped products derived from inference engineering research ideas."
summary: "Ten product directions built from KV cache reuse, roofline scheduling, speculative decoding, and inference observability."
categories: ["inference-engineering"]
tags: ["products", "startup", "inference", "yc"]
date: 2026-05-18
draft: false
weight: 20
---

These product ideas translate the research map into something a startup could plausibly build.

The filter I used here is practical: can a small technical team build a demo in months, can the pain be explained in one sentence, and does the idea compound into a moat if it works?

## Product Map

| Product | One-Line Pitch | Customer | Build Risk | YC Fit |
| --- | --- | --- | --- | --- |
| [DocVault](docvault/) | The CDN for LLM context: compute documents once, reuse forever. | RAG companies, legal AI, fintech AI | High research risk | Very high |
| [PrefillX](prefillx/) | Speculative prefill and document KV cache as an API. | Long-context document apps | High systems risk | High |
| [InferGrid](infergrid/) | Autopilot for LLM inference cost and GPU utilization. | Self-hosted LLM teams | Medium | High |
| [DraftOS](draftos/) | Use idle CPU cores as a speculative decoding co-processor. | vLLM / SGLang operators | Medium | Medium-high |
| [SLOGuard](sloguard/) | Priority-aware scheduler for enterprise LLM SLAs. | Multi-tenant LLM API companies | Medium | High |
| [HaloscoreAI](haloscoreai/) | Hallucination risk scoring from quantization divergence. | Regulated AI teams | Medium-high validation risk | Medium-high |
| [DistillAudit](distillaudit/) | Safety certification for distilled models. | Enterprise AI governance teams | Medium | Medium |
| [ConvoCache](convocache/) | Persistent attention-aware memory for AI assistants. | Assistant builders, CRM AI | Medium | Medium-high |
| [SpecDraft Cloud](specdraft-cloud/) | Managed speculative decoding that learns from production traffic. | API companies | Medium-high | Medium |
| [NeuralEdge](neuraledge/) | Thermal-aware inference runtime for robots and edge AI. | Robotics OEMs | High GTM complexity | Medium |

## The Strongest Bet

**DocVault plus PrefillX** is the most coherent company wedge.

Both depend on the same hard technical moat: position-aware, reusable document KV cache. DocVault is the network-effect version; PrefillX is the immediate enterprise wedge. One is the big story, the other is the first invoice.

{{< mermaid >}}
flowchart TD
  Docs[Documents and chunks] --> Hash[Normalize + hash content]
  Hash --> Library[Shared KV library]
  Library --> Hit{Cache hit?}
  Hit -->|Yes| Inject[Inject cached KV]
  Hit -->|No| Prefill[Compute prefill once]
  Prefill --> Library
  Inject --> Answer[Low TTFT answer]

  Library --> Network[Network effect: more customers, more cached docs]
  Network --> LowerCost[Lower cost and latency for everyone]
{{< /mermaid >}}

## Why This Category Matters

RAG apps often retrieve the same documents repeatedly. Existing prompt caching helps when the prompt prefix is identical and recent. The stronger product asks: can common documents become reusable infrastructure across sessions, tenants, and companies?

That is where the product gets interesting. The more document contexts are cached, the more valuable the cache becomes.

