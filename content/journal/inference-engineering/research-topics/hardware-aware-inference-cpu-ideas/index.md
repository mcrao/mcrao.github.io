---
title: "Hardware-Aware AI CPU Ideas"
description: "Research ideas inspired by AI CPU, systolic array, memory hierarchy, and compiler backend discussions."
summary: "The software layer that becomes valuable if inference hardware shifts from GPU-first to AI CPU and custom accelerator designs."
categories: ["inference-engineering"]
tags: ["hardware", "systolic-arrays", "compiler", "hbm", "tokens-per-dollar"]
date: 2026-05-18
draft: false
weight: 10
---

## Core Thesis

The mentor hardware notes and Bjarke Roune document point toward a provocative idea:

> Future inference accelerators may look less like today's GPU-only stack and more like programmable AI CPUs with large SRAM, systolic arrays, DMA engines, and explicit memory hierarchy control.

For a student research group, the opportunity is not to fabricate a chip. It is to build the software, simulators, and empirical studies that make the tradeoffs visible.

## Strongest Research Directions

### 1. 1:2 vs 2:4 Sparsity Recipes

NVIDIA-style 2:4 sparsity is known. The mentor document highlights a possible 1:2 format with one bit indicating which value survives and seven bits for the value. The research question is whether Llama-class models can tolerate 1:2 sparsity with careful recipes.

### 2. Fused Lossy + Lossless Compression

Quantization is lossy. Huffman coding is lossless. The document's JPEG analogy is useful: combine them. A lookup table could map FP8 to lower-bit values while also assigning entropy codes.

### 3. Tiled Software Pipeline Library

Kernel authors still write too much hand-pipelined code. Triton helps, but the gap remains for declaring load/compute/store/network stages cleanly with fusion. This is a systems paper hiding in plain sight.

### 4. HBM Minimization Planner

Given a model, context length, traffic shape, and SLO, recommend quantization, KV cache policy, sharding, and offload strategy.

## Diagram

{{< mermaid >}}
flowchart TD
  Workload[Inference workload] --> Planner[Design-space planner]
  Planner --> Numerics[Precision / sparsity]
  Planner --> Memory[HBM / DRAM / SSD placement]
  Planner --> Kernel[Tiled kernel pipeline]
  Planner --> Network[MoE / sharding topology]
  Numerics --> Metric[Tokens per dollar]
  Memory --> Metric
  Kernel --> Metric
  Network --> Metric
{{< /mermaid >}}

## Novelty Opinion

This track is harder than KV cache papers because the evaluation story needs either a simulator, kernel prototype, or hardware counter study. But it is also where durable systems value may live.

## Tenure And Complexity

- **Small empirical sparsity study:** 1-2 months.
- **Compression study:** 1-3 months.
- **Tiled pipeline library:** 6-12 months.
- **Chip co-design search:** 12+ months unless heavily scoped.

