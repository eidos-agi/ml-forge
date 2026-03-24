---
id: "GUARD-003"
type: "guardrail"
title: "Models ship as static artifacts — no training at runtime"
status: "active"
date: "2026-03-24"
---

## Rule
Training happens offline (dev time, CI, or a one-time script). The shipped artifact is a static file — pickled weights, JSON coefficients, a hardcoded weight vector. Runtime inference must be fast and deterministic.

## Why
Runtime training is fragile, slow, and non-reproducible. A static artifact is auditable, versioned in git, and runs the same every time.

## Violation Examples
- A pattern that trains on first run
- A model that retrains every time new data appears (without explicit user action)
- Shipping sklearn as a runtime dep when a weight vector would suffice
