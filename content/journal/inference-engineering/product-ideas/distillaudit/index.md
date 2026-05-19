---
title: "DistillAudit"
description: "Safety certification for distilled AI models."
summary: "Detect hidden preference transfer from teacher models to students."
categories: ["inference-engineering"]
tags: ["startup", "distillation", "audit", "governance"]
date: 2026-05-18
draft: false
weight: 7
---

## Pitch

DistillAudit checks whether a distilled model inherited unwanted preferences, biases, or behavioral quirks from its teacher.

The product is a test suite, report, and ongoing monitoring service for companies releasing smaller models.

## Workflow

1. Connect teacher and student models.
2. Run structured probes across sensitive behavior categories.
3. Compare response distributions.
4. Flag unexpected transfer not present in the explicit training data.
5. Produce a model governance report.

## Why Now

Distillation is becoming routine because inference cost matters. Governance teams need a way to say not only "the student is accurate" but also "the student did not inherit hidden behavior we cannot explain."

## Risks

- Compliance sales cycles are slow.
- Probe coverage is always incomplete.
- The product needs credibility, benchmarks, and probably academic validation.

