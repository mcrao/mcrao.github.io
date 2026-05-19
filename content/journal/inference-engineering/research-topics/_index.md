---
title: "Research Topics"
description: "Research ideas from the inference engineering workshop and follow-up reading."
summary: "Novel and practical research directions around KV cache compression, scheduling, speculation, quantization, and hardware-aware serving."
categories: ["inference-engineering"]
tags: ["research", "kv-cache", "inference", "llm-systems"]
date: 2026-05-18
draft: false
weight: 10
---

The workshop discussion kept circling one bottleneck: inference is becoming a memory, scheduling, and reuse problem as much as a modeling problem.

Below is the organized research map. The individual pages go deeper on the highest-leverage ideas. The broader list is kept here so the whole brainstorm remains searchable.

## Priority Research Tracks

| Track | Core Question | Novelty | Complexity | Time Horizon |
| --- | --- | --- | --- | --- |
| [Position-Invariant Document KV Cache](position-invariant-document-kv-cache/) | Can we cache document KV independent of prompt position? | High | High | 3-6 months for paper prototype |
| [Temporal TurboQuant KV Tiering](temporal-turboquant-kv-tiering/) | Can old tokens be stored at lower precision than recent tokens? | High | Medium-high | 2-4 months |
| [Roofline-Adaptive Inference Scheduler](roofline-adaptive-inference-scheduler/) | Can the scheduler chase the GPU ridge point in real time? | High | Medium | 2-3 months |
| [Speculative Prefill](speculative-prefill/) | Can a draft model precompute approximate KV for long prompts? | High | High | 3-5 months |
| [Quantization Divergence Hallucination Signal](quantization-divergence-hallucination-signal/) | Can FP8/INT4 vs FP16 logit drift signal uncertainty? | High | Medium | 2-3 months |
| [Online EAGLE Draft Learning](online-eagle-draft-learning/) | Can accepted/rejected draft tokens train the draft head online? | High | Medium | 2-4 months |
| [SLO-Aware KV Cache Tiering](slo-aware-kv-cache-tiering/) | Can premium users get HBM while batch jobs spill to DRAM/SSD? | Medium-high | Medium-high | 3-5 months |
| [Attention Head Similarity Pruning](attention-head-similarity-pruning/) | Can redundant heads be pruned per input during inference? | Medium | Medium | 1-2 months |
| [Unlearning Layer in Attention](unlearning-layer-in-attention/) | Can an attention mask adapter weaken specific associations? | Medium-high | Medium-high | 3-6 months |
| [Hardware-Aware Inference CPU Ideas](hardware-aware-inference-cpu-ideas/) | What software layer is needed if AI CPUs become real? | Medium-high | High | 6-12 months |

## Full Idea Inventory

### Workshop-Derived Ideas

1. **Temporal / distance-aware dynamic quantization of KV cache** - keep recent tokens in high precision, compress old tokens more aggressively.
2. **TurboQuant for pre-softmax attention scores** - test whether rotation-based quantization reduces score-matrix outliers.
3. **TurboQuant with LoRA fine-tuning** - study whether task adapters can compensate for inference-time KV quantization error.
4. **TurboQuant plus temporal compression** - combine recency tiers with rotation-aware quantization.
5. **Input-adaptive attention head pruning** - prune heads that become redundant on a specific prompt.
6. **Unlearning layer inside MHA** - use an inference-time or lightly trained mask to weaken token associations.
7. **KV sharing via prefix hashing** - already partly deployed as prefix caching, but position-awareness still limits reuse.
8. **KV compression for video models** - natural fit because frame time maps to token distance.
9. **Parallel transformer blocks vs sequential depth** - useful but crowded architecture search territory.

### Book-Derived Ideas

1. Roofline-aware adaptive batching.
2. Cross-layer KV cache aliasing by cosine similarity.
3. Speculative prefill with a draft model.
4. SLO-differentiated KV cache tiering.
5. Attention-sink sliding windows with RoPE re-anchoring.
6. Different quantization for prefill and decode GPU pools.
7. CUDA graph capture for padding-free packed variable-length batches.
8. Subliminal preference transfer auditing for distilled models.
9. CPU draft plus GPU verify speculative decoding.
10. Position-invariant prefix caching via RoPE-agnostic keys.
11. Continuous batching with SLO priority preemption.
12. FlashAttention-style tiling for Mamba / SSM selective scan.
13. Ridge-chasing speculative decoding window size.
14. Multi-turn KV persistence with forgetting curves.
15. Quantization error as an uncertainty / hallucination signal.
16. MoE-style attention head routing.
17. Thermal-budget-aware edge inference.
18. Shared document KV cache for RAG.
19. Online learning for EAGLE draft heads.
20. Disaggregated world models for embodied AI.

### Mentor / Hardware Ideas

The mentor material and Bjarke Roune document push the same theme down to silicon:

- AI CPUs with systolic arrays and large SRAM.
- Compiler backend studios for new accelerators.
- HBM minimization planners.
- SSD-backed long memory.
- DMA compression engines that combine lossy quantization with lossless entropy coding.
- 1:2 and 2:4 sparsity toolkits.
- Memory hierarchy explorers.
- MoE token-router and network-topology co-design.
- Tiled software pipeline libraries.
- Tokens-per-dollar observability.
- AI chip co-design search.

## Background Reading

- [TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate](https://arxiv.org/abs/2504.19874)
- [Efficient Streaming Language Models with Attention Sinks](https://arxiv.org/abs/2309.17453)
- [FlashAttention](https://arxiv.org/abs/2205.14135)
- [vLLM automatic prefix caching](https://docs.vllm.ai/en/v0.8.3/design/automatic_prefix_caching.html)
- [PagedAttention / vLLM paper](https://web.stanford.edu/class/cs240/readings/vllm.pdf)
- [Cache-Craft: Managing Chunk-Caches for Efficient RAG](https://arxiv.org/abs/2502.15734)
- [RAGCache](https://arxiv.org/abs/2404.12457)
- [TurboRAG](https://arxiv.org/abs/2410.07590)
- [Orca: iteration-level scheduling](https://www.usenix.org/conference/osdi22/presentation/yu)
- [EAGLE speculative decoding](https://huggingface.co/papers/2401.15077)
- [LoRA](https://arxiv.org/abs/2106.09685)
- [FlexGen](https://huggingface.co/papers/2303.06865)
- [Mamba](https://github.com/state-spaces/mamba)

