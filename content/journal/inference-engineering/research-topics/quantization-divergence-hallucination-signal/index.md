---
title: "Quantization Divergence As Hallucination Signal"
description: "Using precision-induced logit drift as an uncertainty signal during inference."
summary: "If FP8/INT4 and FP16 disagree sharply, the model may be in a fragile region."
categories: ["inference-engineering"]
tags: ["quantization", "hallucination", "uncertainty", "safety"]
date: 2026-05-18
draft: false
weight: 5
---

## Core Idea

Quantization is usually treated as error to minimize. This idea treats quantization error as a signal.

Run a production model in FP8 or INT4, then shadow a small sample of tokens in FP16. If the output distributions diverge sharply, the model may be relying on fragile activations or outlier pathways.

```text
risk(token) = KL(logits_fp16 || logits_quantized)
```

## Why It Might Work

Outlier-heavy computation is often where quantization hurts most. If those outliers appear when the model is uncertain, rare, or extrapolating, the divergence may correlate with hallucination risk.

This would not prove hallucination by itself. It would be a cheap warning light.

## System Sketch

{{< mermaid >}}
flowchart LR
  Request --> Fast[Production FP8/INT4 path]
  Request --> Sample[Sample 5 percent tokens]
  Sample --> Shadow[Shadow FP16 path]
  Fast --> KL[KL divergence]
  Shadow --> KL
  KL --> Score[Token risk heatmap]
  Score --> Policy[Escalate / cite / abstain]
{{< /mermaid >}}

## Experiment Plan

1. Select factual QA, summarization, and RAG hallucination benchmarks.
2. Run quantized and FP16 paths.
3. Measure token-level KL divergence.
4. Correlate divergence with:
   - factual errors,
   - unsupported claims,
   - low confidence outputs,
   - retrieval mismatch.
5. Calibrate thresholds per model and domain.

## Novelty Opinion

High. Most quantization work asks "how do we hide the error?" This asks "what does the error reveal?"

## Tenure And Complexity

- **Prototype:** 2-4 weeks.
- **Paper-grade:** 2-3 months.
- **Complexity:** Medium.
- **Main risk:** divergence may correlate with rare tokens rather than hallucination.

