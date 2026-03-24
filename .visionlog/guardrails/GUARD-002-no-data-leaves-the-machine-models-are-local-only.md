---
id: "GUARD-002"
type: "guardrail"
title: "No data leaves the machine — models are local-only"
status: "active"
date: "2026-03-24"
---

## Rule
Every pattern ml-forge teaches must work entirely offline. No cloud APIs for training or inference. No telemetry. No data exfiltration. Models train on local data and ship as static artifacts.

## Why
Privacy and trust. Users' project data, app inventories, and code patterns are sensitive. The value proposition is ML intelligence without the cloud tax.

## Violation Examples
- A template that calls an external API for training data
- A skill that recommends sending features to a hosted model
- Any pattern that requires network access at inference time
