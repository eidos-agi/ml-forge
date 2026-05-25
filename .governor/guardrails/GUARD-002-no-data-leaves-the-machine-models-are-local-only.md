---
id: "GUARD-002"
type: "guardrail"
title: "Project data stays local — web searches are fine"
status: "active"
date: "2026-03-24"
---

## Rule
Project data (code, features, training data, model inputs) must not be sent to external services. Web searches for models, documentation, and technique references are fine — that's searching, not leaking.

## Why
Privacy matters for user data. But searching HuggingFace for a pre-trained model or reading docs is normal development activity, not data exfiltration.

## What's OK
- Searching HuggingFace for pre-trained models (`/ml-find`)
- Web searches for ML techniques, documentation, benchmarks
- Downloading pre-trained model weights

## What's NOT OK
- Sending project source code to an external API for analysis
- Uploading training data to a cloud training service
- Using a cloud inference API where project data is the input

## Violation Examples
- A template that sends feature vectors to a hosted model
- A training pipeline that uploads labeled data to a cloud service
- Shipping a model that calls home with inference inputs
