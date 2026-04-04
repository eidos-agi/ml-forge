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

### Step 4: Try L0 First (MANDATORY)

Before comparing approaches, write the hardcoded solution. How many cases exist? Can you enumerate them? If the domain has <50 items, a lookup table almost certainly works.

**You must show the L0 sketch and identify where it fails before proceeding.** If you can't find concrete failures, the answer is: stay hardcoded.

### Step 5: Compare Approaches (only if L0 has proven failures)

Score each approach 1-5. **Criteria are weighted — simplicity wins ties.**

| Criterion | Weight | Hardcoded | ML Model | LLM Call |
|-----------|--------|-----------|----------|----------|
| Accuracy on known cases | 2x | | | |
| Runtime speed | 2x | | | |
| Runtime cost ($) | 2x | | | |
| Dep footprint | 2x | | | |
| Generalization to new cases | 1x | | | |
| Maintenance burden | 1x | | | |
| Explainability | 1x | | | |

Weighted total determines the winner. Simplicity criteria (speed, cost, deps) are double-weighted because complexity compounds over time. ML or LLM must win by a clear margin on the weighted total, not just edge out on generalization.

### Step 6: Recommend

One of:
- **Stay hardcoded** — the default. L0 covers the cases, or the domain is small enough that a lookup works. This is the answer until proven otherwise.
- **Use ML** — L0 provably fails on concrete examples, enough training data exists, and the weighted scorecard clearly favors ML
- **Use LLM** — problem requires natural language understanding that features can't capture, AND ensemble ML was considered and rejected
- **Hybrid** — L0 for known cases + ML for the uncertain middle. Each layer earns its place.

### Output Format

```
## ML Assessment: <problem>

### Decision: Given <X>, predict <Y>
### Domain size: <N items/cases — if <50, L0 is strongly favored>
### L0 sketch: <what the hardcoded solution looks like>
### L0 coverage: <N>/<total> cases covered
### L0 failures: <concrete examples where L0 gives wrong answer, or "none found">
### Features: <N> signals identified, <M> are free/fast
### Labels: <source>, ~<N> examples
### Recommendation: <Stay hardcoded | Use ML | Use LLM | Hybrid>
### Why: <1-2 sentences>
### Next step: <what to do — if "stay hardcoded", say so clearly>
```

## Rules

- **Hardcoded is the default.** ML must earn its place with concrete evidence.
- Be honest about when ML is overkill — a 20-line if/else that works is better than a model
- Consider the maintenance cost: who retrains the model when the world changes?
- Always check if enough labeled data exists before recommending ML
- Never recommend cloud-based ML — local only (GUARD-002)
- **Ensemble ML beats single LLM.** Before recommending L5+, ask: could 3 cheap classifiers voting do this?
- A domain with <50 items is almost always L0. Don't overthink it.
