---
title: "NeuralEdge"
description: "Thermal-aware inference runtime for robots and edge AI."
summary: "Schedule inference around thermal limits and split reflexes on-device from planning in the cloud."
categories: ["inference-engineering"]
tags: ["startup", "edge-ai", "robotics", "thermal"]
date: 2026-05-18
draft: false
weight: 10
---

## Pitch

NeuralEdge is an inference runtime for edge AI hardware that adapts to heat, power, and network uncertainty.

Robots and edge cameras do not run in clean data-center conditions. Their accelerators throttle, networks drop, and latency budgets are tied to physical motion.

## Architecture

{{< mermaid >}}
flowchart TD
  Sensors --> Edge[On-device reflex model]
  Edge --> Actuators
  Sensors --> Compress[Latent compression]
  Compress --> Cloud[Cloud world model planner]
  Cloud --> Plan[Semantic action plan]
  Plan --> Edge
  Thermal[Temperature / power monitor] --> Scheduler[Duty-cycle scheduler]
  Scheduler --> Edge
{{< /mermaid >}}

## Customer

Robotics OEMs, drone companies, warehouse automation teams, and industrial inspection vendors.

## Risks

- Robotics go-to-market is slow.
- Hardware heterogeneity is messy.
- Safety requirements are much higher than web AI.

