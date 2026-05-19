---
title: "Attention Head Similarity Pruning"
description: "Input-adaptive pruning of redundant attention heads during inference."
summary: "Measure cross-head similarity on a prompt and skip heads that are redundant for that input."
categories: ["inference-engineering"]
tags: ["attention", "pruning", "heads", "inference"]
date: 2026-05-18
draft: false
weight: 8
---

## Core Idea

Multi-head attention gives the model many views of a token sequence. But for a specific prompt, some heads may behave redundantly.

The research idea is to compute pairwise similarity between head outputs or KV representations during prefill. If two heads are nearly identical, skip one during decode.

## Procedure

1. During prefill, compute head-level signatures.
2. Cluster or threshold heads by cosine similarity.
3. Keep a representative head per cluster.
4. Decode using only selected heads.
5. Restore full heads if quality or uncertainty triggers a fallback.

## Diagram

{{< mermaid >}}
flowchart LR
  Heads[Attention heads] --> Sim[Pairwise cosine similarity]
  Sim --> Cluster[Cluster redundant heads]
  Cluster --> Keep[Keep representatives]
  Cluster --> Skip[Skip redundant reads]
  Keep --> Decode[Decode]
  Skip --> Decode
{{< /mermaid >}}

## Novelty Opinion

Static head pruning is well-studied. The more interesting angle is per-input dynamic pruning at inference time. It may pair well with a "confidence rollback" policy: prune aggressively, but revert if logits drift too far.

## Experiment Plan

- Measure head similarity across tasks.
- Test fixed thresholds vs learned thresholds.
- Compare speed, memory reads, and quality.
- Evaluate whether similarity during prefill predicts redundancy during decode.

## Tenure And Complexity

- **Prototype:** 1-2 weeks.
- **Paper-grade:** 1-2 months.
- **Complexity:** Medium.
- **Main risk:** computing similarity may cost more than it saves unless amortized over long decode.

