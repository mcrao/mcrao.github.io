---
title: "HaloscoreAI"
description: "Hallucination risk scoring from quantization divergence."
summary: "A low-latency uncertainty signal for regulated AI applications."
categories: ["inference-engineering"]
tags: ["startup", "hallucination", "compliance", "safety"]
date: 2026-05-18
draft: false
weight: 6
---

## Pitch

HaloscoreAI scores hallucination risk during inference by measuring how much quantized and higher-precision model paths disagree.

The product does not claim to prove truth. It identifies fragile tokens and responses that deserve citation checks, retrieval repair, or human review.

## Product Output

- Response-level risk score.
- Token-level heatmap.
- Drift report by model/version/precision.
- Compliance logs for regulated workflows.

## Architecture

{{< mermaid >}}
flowchart LR
  Response[Generated response] --> Quant[Production quantized logits]
  Response --> Shadow[Sampled FP16 shadow logits]
  Quant --> Divergence[Distribution divergence]
  Shadow --> Divergence
  Divergence --> Risk[Risk score]
  Risk --> Action[Accept / cite / escalate]
{{< /mermaid >}}

## Customer

Healthcare AI, legal AI, financial AI, and enterprise systems where unsupported claims create liability.

## Risks

- Divergence may not correlate strongly enough with factuality.
- Shadow precision checks add cost.
- Buyers may ask for formal guarantees the signal cannot provide.

