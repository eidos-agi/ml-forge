# ml-train — Build Offline Training Pipeline

Build a training script that learns from labeled data and produces a shippable model artifact.

## Trigger

User says `/ml-train` or is ready to train a model (features and labels identified).

### Arguments

- `<project>` — the target project
- `<features_module>` — path to the feature extractor from /ml-extract

## Instructions

### Step 1: Prepare Training Data

1. Load labeled examples (from /ml-assess's label source)
2. Extract features for each example using the feature extractor
3. Split: 80% train, 20% test (stratified if classification)
4. Report: N examples, class balance, feature matrix shape

If fewer than 50 examples: warn that the model may not generalize. Consider augmentation or a simpler model.

### Step 2: Choose Model Complexity

Match model to data size:

| Training examples | Recommended model | Runtime dep |
|-------------------|-------------------|-------------|
| 50-200 | Logistic regression | None (ship weights as constants) |
| 200-1000 | Small random forest (max_depth=5, n_estimators=20) | None (export as nested if/else or JSON rules) |
| 1000+ | Gradient boosted trees | Optional (sklearn or lightgbm, dev-only) |

**Default: logistic regression.** It's interpretable, ships as a weight vector, and runtime inference is a dot product. Only increase complexity if logistic regression's accuracy is insufficient.

### Step 3: Train

Generate training script from `train.py.tmpl`:

```python
# Pattern: train offline, export weights
from sklearn.linear_model import LogisticRegression
import json

model = LogisticRegression()
model.fit(X_train, y_train)

# Export as shippable artifact — no sklearn needed at runtime
weights = {
    "coefficients": model.coef_.tolist(),
    "intercept": model.intercept_.tolist(),
    "feature_names": feature_names,
    "threshold": 0.5,
    "trained_on": len(X_train),
    "accuracy": float(accuracy_score(y_test, model.predict(X_test)))
}
with open("model_weights.json", "w") as f:
    json.dump(weights, f, indent=2)
```

### Step 4: Evaluate

Report:
- **Accuracy** on held-out test set
- **Precision/Recall** per class (for classification)
- **Confusion matrix** (what does it get wrong?)
- **Feature importance** — which features matter most? (coefficients for logistic, importance for trees)
- **Baseline comparison** — how does it compare to the existing hardcoded approach?

### Step 5: Iterate or Ship

- If accuracy > baseline + 5%: proceed to /ml-ship
- If accuracy ≈ baseline: the model isn't worth the complexity. Stay hardcoded.
- If accuracy < baseline: features are wrong. Go back to /ml-extract.

## Output

- Training script (dev-only, not shipped with the project)
- Model weights file (JSON, shipped with the project)
- Evaluation report with comparison to baseline

## Rules

- Training deps (sklearn, pandas) are dev-only — never runtime
- Always compare to the existing approach. "92% accuracy" means nothing without "vs 78% from the hardcoded list"
- Always report what the model gets wrong — false positives and false negatives
- Reproducibility: set random seeds, log the training data hash
- Never train on the test set (sounds obvious, but automated pipelines mess this up)
