---
title: "PrefillX"
description: "Speculative prefill and document KV cache as a service."
summary: "Cut TTFT for long-context document applications by precomputing and repairing reusable KV states."
categories: ["inference-engineering"]
tags: ["startup", "prefill", "rag", "kv-cache"]
date: 2026-05-18
draft: false
weight: 2
---

## Pitch

PrefillX is a prefill acceleration API for document-heavy AI applications.

Upload a long document once. PrefillX computes reusable state, validates quality, and makes future questions start almost instantly.

## Wedge

DocVault is the big network-effect company. PrefillX is the first sellable product:

- legal contract review,
- medical record summarization,
- codebase chat,
- financial report Q&A,
- internal policy assistants.

## Architecture

{{< mermaid >}}
flowchart TD
  Document --> Draft[Draft prefill]
  Document --> Calibrate[Full-model calibration points]
  Draft --> Approx[Approximate KV]
  Calibrate --> Validate[Validate / repair]
  Approx --> Validate
  Validate --> Store[Store accepted KV]
  Query --> Store
  Store --> Answer[Fast first token]
{{< /mermaid >}}

## Why It Is Fundable

The buyer already understands latency. A demo can show the same 50k-token document going from seconds of TTFT to near-instant interaction. That is more convincing than a dashboard.

## MVP

- FastAPI service around a single open model.
- Store chunk-level KV for one document collection.
- Expose `upload_document`, `precompute`, and `query_with_cache`.
- Show before/after TTFT and answer quality.

## Risks

- Approximate KV may not be stable enough.
- Exact reuse may require prompt structure constraints.
- Customers may prefer built-in provider prompt caching unless the savings are dramatic.

