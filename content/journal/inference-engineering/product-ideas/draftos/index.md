---
title: "DraftOS"
description: "A CPU-side speculative decoding co-processor."
summary: "Use idle CPU cores on GPU instances to draft tokens while the GPU verifies."
categories: ["inference-engineering"]
tags: ["startup", "speculative-decoding", "cpu", "gpu"]
date: 2026-05-18
draft: false
weight: 4
---

## Pitch

GPU instances ship with many CPU cores that often sit underused during LLM serving. DraftOS runs a small draft model on those CPU cores while the GPU verifies previous draft tokens.

## Why It Is Clever

Speculative decoding usually consumes extra GPU memory for the draft model. DraftOS tries to make the draft path "free" by using CPU resources the customer already rents.

## Pipeline

{{< mermaid >}}
sequenceDiagram
  participant CPU as CPU draft model
  participant GPU as GPU target model
  participant Out as Output stream
  CPU->>GPU: draft tokens for step t
  GPU->>GPU: verify tokens from step t-1
  GPU->>Out: accept/reject and emit
  CPU->>CPU: draft next tokens while GPU works
{{< /mermaid >}}

## MVP

- llama.cpp or GGML draft model.
- vLLM plugin or proxy.
- Adaptive draft length based on GPU verification time.
- Acceptance-rate dashboard.

## Risks

- CPU draft may not keep up under realistic traffic.
- Cross-device coordination overhead may eat gains.
- Best draft models may still need GPU acceleration.

