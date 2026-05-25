---
title: "Solve software problems with ML, not just LLMs"
type: "vision"
date: "2026-03-24"
---

ml-forge teaches agents how to build small, local, zero-cloud ML models that ship as static artifacts inside projects. When a problem is better solved by a trained model than a hardcoded checklist or an LLM call, ml-forge provides the patterns: feature extraction, training pipelines, model packaging, and integration.

The forge itself is knowledge — skills and templates. The actual models live in each project that uses them.

**Why this exists:** Across the Eidos forge ecosystem, we keep hitting the same wall — static checklists that miss context. ship-forge didn't run lint because nobody hardcoded it. test-forge tested a live site when changes were local. apple-a-day uses a hardcoded synonym list instead of learning similarity. These are all ML-shaped problems: given features about the current state, predict the right action. An LLM can do this but it's expensive, slow, and overkill. A 10KB logistic regression trained on project history is faster, cheaper, and ships as a constant.

**The pattern:** Extract local features → train offline → ship model as static artifact → infer at runtime with zero deps. No API calls, no data leaving the machine, no cloud. Just learned weights.
