---
title: "Roofline-Adaptive Inference Scheduler"
description: "A scheduler that changes batching and speculation based on live GPU utilization."
summary: "Move from static max_num_seqs to a feedback loop that chases the hardware ridge point."
categories: ["inference-engineering"]
tags: ["roofline", "scheduler", "vllm", "gpu", "research"]
date: 2026-05-18
draft: false
weight: 3
---

## Core Idea

Modern LLM serving stacks expose knobs like batch size, max concurrent sequences, chunked prefill size, speculation length, and quantization. These are often tuned manually.

The research idea is to close the loop:

> Measure the serving engine's actual arithmetic intensity and adjust scheduler knobs to keep the GPU near the roofline ridge point.

## Why It Matters

Prefill can be compute-bound. Decode is often memory-bound. Mixed workloads move back and forth. A static scheduler setting can be good for one traffic shape and bad for another.

[Orca](https://www.usenix.org/conference/osdi22/presentation/yu) introduced iteration-level scheduling, and [vLLM / PagedAttention](https://web.stanford.edu/class/cs240/readings/vllm.pdf) made continuous batching practical. The next step is hardware-counter-aware scheduling.

## Controller Sketch

{{< mermaid >}}
flowchart LR
  Counters[GPU counters: bytes, FLOPs, SM utilization] --> AI[Estimate arithmetic intensity]
  AI --> Controller[Scheduler controller]
  Controller --> Batch[Batch size]
  Controller --> K[Speculation window K]
  Controller --> Prefill[Chunked prefill policy]
  Controller --> Quant[Quantization tier]
  Batch --> Engine[vLLM / SGLang engine]
  K --> Engine
  Prefill --> Engine
  Quant --> Engine
  Engine --> Counters
{{< /mermaid >}}

## Experiment Plan

1. Instrument a vLLM deployment with NVIDIA counters through Nsight, DCGM, CUPTI, or lower-frequency telemetry.
2. Estimate the operating point: FLOPs per byte moved.
3. Build a simple controller:
   - if memory-bound, increase batch or speculation,
   - if compute-bound, reduce aggressive batching or defer long prefill,
   - if P99 latency is threatened, prioritize SLO requests.
4. Compare static vs adaptive settings under traffic replay.

## Metrics

- tokens per second,
- dollars per million tokens,
- P50/P99 TTFT,
- P50/P99 inter-token latency,
- GPU memory pressure,
- cache hit rate.

## Novelty Opinion

High as a systems paper if the controller is genuinely hardware-informed. Many serving systems schedule requests; fewer directly target the roofline operating point with live feedback.

## Tenure And Complexity

- **Prototype:** 3-6 weeks if using coarse telemetry.
- **Paper-grade:** 2-3 months with robust traffic traces.
- **Complexity:** Medium.
- **Main risk:** hardware counters may be too noisy or expensive to sample per iteration.

