---
title: "InferGrid"
description: "Inference cost intelligence and autopilot for LLM serving."
summary: "Measure why your GPU bill is high, then tune batching, speculation, and quantization automatically."
categories: ["inference-engineering"]
tags: ["startup", "observability", "roofline", "gpu"]
date: 2026-05-18
draft: false
weight: 3
---

## Pitch

InferGrid is Datadog plus autopilot for LLM inference.

It tells teams whether they are memory-bound, compute-bound, cache-missing, over-prefilling, under-batching, or violating SLOs. Then it suggests or applies serving changes.

## Product Surface

- GPU roofline dashboard.
- Dollars per million tokens.
- TTFT and inter-token latency attribution.
- KV cache hit/miss analysis.
- Batch-size and speculation recommendations.
- Quantization and offload what-if planner.

## Control Loop

{{< mermaid >}}
flowchart LR
  Traces[Serving traces] --> Diagnose[Cost diagnosis]
  Counters[GPU counters] --> Diagnose
  Diagnose --> Plan[Optimization plan]
  Plan --> Apply[Apply knobs]
  Apply --> vLLM[vLLM / SGLang]
  vLLM --> Traces
{{< /mermaid >}}

## Customer

Teams self-hosting vLLM, SGLang, TensorRT-LLM, or custom inference stacks and spending enough on GPUs that a 10-20 percent savings is meaningful.

## Moat

The early product is observability. The long-term moat is the policy engine: a growing dataset of workload shapes and which interventions actually saved money.

## Risks

- Deep integration with inference engines is painful.
- Some companies already have internal dashboards.
- GPU counter collection must be low overhead.

