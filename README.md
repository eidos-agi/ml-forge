# ml-forge

> Solve software problems with ML, not just LLMs.

ml-forge teaches AI agents how to build small, local ML models that ship as static artifacts inside projects. No cloud APIs, no data exfiltration, no heavy deps — just learned weights that make software smarter.

## Why

Every forge in the Eidos ecosystem hits the same wall: static checklists that miss context. A linter config exists but the preflight doesn't check it. A test suite exists but the runner doesn't know which tests are relevant. An app is redundant but nobody hardcoded the synonym.

These are all the same problem: **given features about the current state, predict the right action.** That's ML. And for most of these problems, a 10KB logistic regression trained on project history beats both a hardcoded list and an expensive LLM call.

## The Pattern

```
Extract local features → Train offline → Ship as static artifact → Infer at runtime
```

- **Features**: whatever signals are locally available (files, configs, system metadata, git history)
- **Training**: dev-time script, labeled from existing project data (synonym groups, CI configs, test results)
- **Artifact**: JSON weight vector or hardcoded constants — versioned in git
- **Inference**: stdlib Python, zero deps, zero network, fast and deterministic

## Skills

| Skill | Description |
|-------|-------------|
| `/ml-assess` | Is this problem ML-shaped? Evaluate features, labels, and value proposition |
| `/ml-extract` | Design feature extraction from local signals |
| `/ml-train` | Build offline training with proper evaluation |
| `/ml-ship` | Package model as minimal static artifact |
| `/ml-evaluate` | Precision, recall, feature importance, quality report |

## Guardrails

- No cloud APIs — everything runs locally
- Models ship as static artifacts — no training at runtime
- Minimize runtime deps — prefer `weights @ features` over `import sklearn`
- This repo is knowledge only — no installable software

## Use Cases

| Project | Problem | ML Solution |
|---------|---------|-------------|
| ship-forge | Static preflight misses project-specific checks | Train on CI configs → predict which checks matter |
| test-forge | Tests wrong target (live vs local) | Train on git/env signals → predict correct target |
| apple-a-day | Hardcoded app synonym list | Train on framework fingerprints → predict similarity |

## License

MIT — see [LICENSE](LICENSE)
