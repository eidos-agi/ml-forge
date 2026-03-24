# CLAUDE.md — ml-forge

> Solve software problems with ML, not just LLMs.

## What This Is

ml-forge teaches agents how to build small, local, zero-cloud ML models that ship as static artifacts. When a problem is better solved by a trained model than a hardcoded checklist or an LLM call, ml-forge provides the patterns.

## Why It Exists

Across the Eidos forge ecosystem, static checklists keep missing context:
- **ship-forge** didn't run lint because nobody hardcoded it — a model trained on CI configs would know
- **test-forge** tested a live site when changes were local — a model could detect the right target from context
- **apple-a-day** uses hardcoded synonym groups instead of learning similarity from app features

These are ML-shaped problems: given features about the current state, predict the right action. An LLM can do this but it's expensive, slow, and overkill. A 10KB trained model is faster, cheaper, and ships as a constant.

## The Pattern

1. **Extract features** — locally, from whatever signals are available (files, configs, system state)
2. **Train offline** — dev-time script or CI job, using labeled data from the project itself
3. **Ship as static artifact** — JSON weight vectors, serialized coefficients, or hardcoded constants in source
4. **Infer at runtime** — fast, deterministic, zero deps, zero network

## Skills

| Skill | What It Does |
|-------|-------------|
| `/ml-assess` | Evaluate whether a problem is ML-shaped — features, labels, and value proposition |
| `/ml-extract` | Design a feature extraction pipeline from local signals |
| `/ml-train` | Build an offline training script with proper train/test split and evaluation |
| `/ml-ship` | Package a trained model as a minimal static artifact for the target project |
| `/ml-evaluate` | Evaluate model quality — precision, recall, confusion matrix, feature importance |

## Guardrails

1. **No software** — skills and templates only (forge standard)
2. **No data leaves the machine** — all training and inference is local
3. **Models ship as static artifacts** — no training at runtime
4. **Minimize runtime deps** — prefer stdlib + learned constants over sklearn at runtime

## Templates

- `train.py.tmpl` — offline training script scaffold
- `features.py.tmpl` — feature extraction module scaffold
- `model.py.tmpl` — runtime inference module (stdlib only)
- `evaluate.py.tmpl` — model evaluation and reporting

## Related Forges

- **ship-forge** — first customer: ML-powered preflight that knows what checks matter
- **test-forge** — second customer: context-aware target detection
- **forge-forge** — the meta-forge that created this one
