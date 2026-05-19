---
title: "Temporal TurboQuant KV Tiering"
description: "Combining recency-aware KV cache precision with TurboQuant-style rotation."
summary: "Recent tokens stay high precision, older tokens degrade to INT4 or INT2, and TurboQuant makes the low-bit tiers less painful."
categories: ["inference-engineering"]
tags: ["turboquant", "kv-cache", "quantization", "long-context"]
date: 2026-05-18
draft: false
weight: 2
---

## Core Idea

Not every token in the KV cache deserves the same number of bits.

Recent tokens often matter more for local coherence. Very old tokens may still matter, but many of them contribute weakly or episodically. The workshop idea was to make KV precision a function of token age:

- last `N` tokens: FP16 or FP8,
- middle tokens: INT4,
- old tokens: INT2,
- optional sink tokens: protected at higher precision.

TurboQuant adds a second ingredient: rotate the vectors before quantization so low-bit representations preserve inner products better.

## Architecture

{{< mermaid >}}
flowchart TD
  KV[KV cache blocks] --> Age[Age / recency classifier]
  Age --> Recent[Recent tier: FP16 or FP8]
  Age --> Warm[Warm tier: TurboQuant INT4]
  Age --> Cold[Cold tier: TurboQuant INT2]
  Recent --> Attention[Attention kernel]
  Warm --> Dequant[Mixed precision dequant]
  Cold --> Dequant
  Dequant --> Attention
{{< /mermaid >}}

## Background

[TurboQuant](https://arxiv.org/abs/2504.19874) reports strong KV cache quantization quality using rotation and quantized Johnson-Lindenstrauss correction. [StreamingLLM](https://arxiv.org/abs/2309.17453) shows that attention sinks and recent-window retention are powerful for streaming settings. The gap is the combination: tiered temporal precision plus rotation-aware low-bit storage.

## Research Questions

- Are fixed recency boundaries enough, or should boundaries be content-aware?
- Does TurboQuant preserve attention score quality better for old tokens than uniform INT4?
- Should keys and values have different tier policies?
- Can the kernel avoid losing all gains to mixed-precision dequant overhead?

## Experiment Plan

Start with a toy PyTorch attention implementation before touching vLLM:

1. Implement full-precision baseline.
2. Add temporal tiers without TurboQuant.
3. Add TurboQuant per tier.
4. Add attention-sink protection.
5. Evaluate on LongBench, RULER, needle-in-a-haystack, and summarization.

The clean ablation table:

| Method | Memory | TTFT | ITL | Quality |
| --- | --- | --- | --- | --- |
| FP16 KV | baseline | baseline | baseline | baseline |
| Uniform INT4 | lower | faster | faster | possible loss |
| Temporal tiers | lower | faster | faster | unknown |
| TurboQuant only | lower | faster | faster | expected strong |
| Temporal TurboQuant | lowest | target | target | main claim |

## Novelty Opinion

Medium-high to high. Temporal retention exists. KV quantization exists. The combined system, especially with a practical mixed-tier attention kernel, is a real systems contribution.

## Tenure And Complexity

- **Prototype:** 2-4 weeks.
- **Kernel-level version:** 2-4 months.
- **Complexity:** Medium-high.
- **Main risk:** the attention kernel becomes slower because it must handle too many precision formats.

