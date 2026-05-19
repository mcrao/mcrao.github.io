---
title: "Speculative Prefill"
description: "Using a draft model to approximate KV cache before the full model computes every token."
summary: "Speculative decoding is common; this asks whether speculation can reduce long-prompt prefill latency."
categories: ["inference-engineering"]
tags: ["prefill", "speculative-decoding", "kv-cache", "research"]
date: 2026-05-18
draft: false
weight: 4
---

## Core Idea

Speculative decoding speeds up output generation by letting a small model draft tokens that a larger model verifies. The workshop extension is:

> Can a smaller model draft the KV cache for a long prompt so the full model only recomputes layers or spans where the approximation is bad?

This targets time-to-first-token for document-heavy apps.

## Proposed Flow

{{< mermaid >}}
flowchart TD
  Prompt[Long prompt or document] --> Draft[Small draft model prefill]
  Prompt --> Checkpoints[Full model checkpoint positions]
  Draft --> ApproxKV[Approximate KV]
  Checkpoints --> Compare[Compare hidden states / attention]
  ApproxKV --> Compare
  Compare --> Accept{Similarity high?}
  Accept -->|Yes| Use[Accept draft KV]
  Accept -->|No| Recompute[Full recompute for span/layer]
  Use --> Decode[Decode with target model]
  Recompute --> Decode
{{< /mermaid >}}

## Background

[EAGLE](https://huggingface.co/papers/2401.15077) is a major speculative decoding family, but it focuses on decode. Prefill remains expensive for long inputs because every prompt token must be processed before the first generated token.

## Research Questions

- Can hidden-state similarity predict whether approximate KV is safe?
- Is approximation more stable in lower layers than upper layers?
- Does the draft model need the same tokenizer and architecture family?
- Can partial recomputation repair cross-chunk interactions?

## Experiment Plan

1. Use a small/large pair from the same family.
2. Run full prefill and draft prefill on the same prompt.
3. Compare hidden states, attention maps, logits, and downstream answers.
4. Accept draft KV by layer/span thresholds.
5. Measure TTFT savings vs quality loss.

## Novelty Opinion

High. The failure mode is also interesting: if approximate KV is too brittle, the paper can still map where draft prefill breaks.

## Tenure And Complexity

- **Prototype:** 4-8 weeks.
- **Paper-grade:** 3-5 months.
- **Complexity:** High.
- **Main risk:** approximate KV errors compound during decode.

