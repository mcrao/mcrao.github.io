---
title: "SLO-Aware KV Cache Tiering"
description: "Placing KV cache blocks across HBM, CPU DRAM, and SSD based on request priority."
summary: "Premium users get hot KV blocks; batch users spill to cheaper memory tiers."
categories: ["inference-engineering"]
tags: ["kv-cache", "slo", "scheduler", "pagedattention", "serving"]
date: 2026-05-18
draft: false
weight: 7
---

## Core Idea

Paged KV cache systems already think in blocks. The next question is where each block should live.

Instead of treating all requests equally:

- premium interactive requests keep KV in GPU HBM,
- standard requests can spill to CPU DRAM,
- batch jobs can spill further to SSD,
- the scheduler prefetches blocks before each decode step.

## Architecture

{{< mermaid >}}
flowchart TD
  Scheduler[SLO-aware scheduler] --> Classify[Classify request tier]
  Classify --> HBM[HBM: premium hot blocks]
  Classify --> DRAM[CPU DRAM: warm standard blocks]
  Classify --> SSD[SSD: cold batch blocks]
  Scheduler --> Prefetch[Predict next sequences]
  Prefetch --> HBM
  HBM --> Decode[Decode step]
  DRAM --> Prefetch
  SSD --> Prefetch
{{< /mermaid >}}

## Background

[FlexGen](https://huggingface.co/papers/2303.06865) and [DeepSpeed ZeRO-Inference](https://www.deepspeed.ai/2022/09/09/zero-inference.html) show the value of offloading. [TensorRT-LLM KV cache reuse](https://developer.nvidia.com/blog/introducing-new-kv-cache-reuse-optimizations-in-nvidia-tensorrt-llm/) includes priority-based eviction and KV cache events. The research contribution here is connecting memory placement to explicit SLO contracts.

## Research Questions

- Which requests deserve HBM under contention?
- Can prefetch hide DRAM/SSD latency?
- Is proactive preemption better than waiting for OOM?
- How does the policy affect P99 TTFT for premium users vs throughput for batch users?

## Novelty Opinion

Medium-high. Pieces exist, but an SLO-native policy with cache placement, prefetching, and admission control would be valuable.

## Tenure And Complexity

- **Prototype:** 4-8 weeks in a simulator.
- **vLLM-grade implementation:** 3-5 months.
- **Complexity:** Medium-high.
- **Main risk:** migration overhead can erase scheduling gains.

