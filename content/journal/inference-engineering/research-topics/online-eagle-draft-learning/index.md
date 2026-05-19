---
title: "Online EAGLE Draft Learning"
description: "Training a speculative decoding draft head from accept/reject feedback."
summary: "Speculative decoding throws away a useful supervision signal: which draft tokens were accepted."
categories: ["inference-engineering"]
tags: ["speculative-decoding", "eagle", "online-learning", "research"]
date: 2026-05-18
draft: false
weight: 6
---

## Core Idea

Speculative decoding produces a clean feedback signal:

- accepted draft tokens were good,
- rejected draft tokens were bad.

Instead of discarding that signal, use it to continuously improve a draft head for the actual production query distribution.

## Background

[EAGLE](https://huggingface.co/papers/2401.15077) accelerates autoregressive decoding by predicting future features or tokens and letting the target model verify them. The typical training loop is offline. The production accept/reject stream is a natural online dataset.

## Training Loop

{{< mermaid >}}
flowchart TD
  Traffic[Production traffic] --> Draft[Draft head proposes tokens]
  Draft --> Verify[Target model verifies]
  Verify --> Accepted[Accepted tokens]
  Verify --> Rejected[Rejected tokens]
  Accepted --> Buffer[Online training buffer]
  Rejected --> Buffer
  Buffer --> Update[Small draft-head update]
  Update --> Draft
{{< /mermaid >}}

## Research Questions

- How much online updating is safe before distribution drift hurts?
- Can updates be batched nightly instead of truly online?
- Does domain-specific traffic improve acceptance rate enough to matter?
- Can the system prevent catastrophic degradation with rollback tests?

## Metrics

- acceptance rate,
- tokens per target forward pass,
- latency improvement,
- quality equivalence to vanilla decoding,
- stability over time.

## Novelty Opinion

High and very clean. The signal is already present, the model being updated is small, and the outcome is easy to measure.

## Tenure And Complexity

- **Prototype:** 4-6 weeks.
- **Paper-grade:** 2-4 months.
- **Complexity:** Medium.
- **Main risk:** production traffic may be too sparse or non-stationary for reliable online updates.

