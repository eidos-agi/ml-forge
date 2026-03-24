# ml-assess — Is This Problem ML-Shaped?

Evaluate whether a software problem would benefit from a trained model vs a hardcoded rule or LLM call.

## Trigger

User says `/ml-assess` or asks "should we use ML for this?"

### Arguments

- `<problem>` — description of the problem to evaluate

## Instructions

### Step 1: Identify the Decision

What decision is being made at runtime? Frame it as: "Given X, predict Y."

Examples:
- "Given a project's files, predict which preflight checks to run"
- "Given two apps' metadata, predict whether they're substitutes"
- "Given a code change, predict which test suites are relevant"

If you can't frame it as a prediction, it's not ML-shaped. Stop here.

### Step 2: Identify Features

What local signals are available as inputs? List them with:
- **Source**: where the data comes from (filesystem, git, system APIs, config files)
- **Type**: categorical, numerical, boolean, set
- **Availability**: always available vs sometimes missing
- **Cost**: free (file read) vs moderate (subprocess) vs expensive (network)

Only features that are local, fast, and reliable should be considered.

### Step 3: Identify Labels

What training data exists? ML needs labeled examples — pairs of (features, correct answer).

Common label sources in the forge ecosystem:
- **Existing hardcoded lists** — synonym groups, check mappings, rule tables
- **Git history** — what checks caught real bugs, which tests actually failed
- **CI logs** — which steps pass/fail for which project configurations
- **User corrections** — "no, that was wrong" signals

If you can't identify at least 50 labeled examples, the model won't generalize. Consider whether the hardcoded approach is actually fine.

### Step 4: Compare Approaches

Score each approach 1-5:

| Criterion | Hardcoded | ML Model | LLM Call |
|-----------|-----------|----------|----------|
| Accuracy on known cases | | | |
| Generalization to new cases | | | |
| Runtime speed | | | |
| Runtime cost ($) | | | |
| Maintenance burden | | | |
| Explainability | | | |
| Dep footprint | | | |

### Step 5: Recommend

One of:
- **Use ML** — clear win on generalization, enough training data, reasonable feature extraction
- **Stay hardcoded** — not enough training data, or the hardcoded list covers 95%+ of cases
- **Use LLM** — problem requires natural language understanding that features can't capture
- **Hybrid** — ML for scoring + hardcoded fallback for known cases

### Output Format

```
## ML Assessment: <problem>

### Decision: Given <X>, predict <Y>
### Features: <N> signals identified, <M> are free/fast
### Labels: <source>, ~<N> examples
### Recommendation: <Use ML | Stay hardcoded | Use LLM | Hybrid>
### Why: <1-2 sentences>
### Next step: <what to do if ML is recommended>
```

## Rules

- Be honest about when ML is overkill — a 20-line if/else that works is better than a model
- Consider the maintenance cost: who retrains the model when the world changes?
- Always check if enough labeled data exists before recommending ML
- Never recommend cloud-based ML — local only (GUARD-002)
