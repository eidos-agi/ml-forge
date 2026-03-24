---
id: "GUARD-004"
type: "guardrail"
title: "Minimize runtime deps — prefer stdlib + learned constants"
status: "active"
date: "2026-03-24"
---

## Rule
If a model can be reduced to a weight vector and a dot product, do that instead of shipping sklearn/numpy as a runtime dependency. Dev-only deps for training are fine. Runtime deps must be justified.

## Why
The projects using these patterns (apple-a-day, ship-forge, test-forge) have their own dep constraints. A 30MB sklearn dependency for what's really a weighted sum is the wrong trade.

## Violation Examples
- Requiring `import sklearn` at runtime when the model is logistic regression (just ship the coefficients)
- Adding numpy as a runtime dep for a single matrix multiply that could be a list comprehension
