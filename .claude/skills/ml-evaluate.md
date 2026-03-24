# ml-evaluate — Model Quality Report

Evaluate a trained model's quality with metrics, visualizations, and actionable recommendations.

## Trigger

User says `/ml-evaluate` or wants to assess a model's quality before shipping.

### Arguments

- `<weights_file>` — path to the model weights JSON
- `<test_data>` — path to held-out test data (or instructions to generate it)

## Instructions

### Step 1: Load and Predict

1. Load the model weights
2. Load or generate test data (must not overlap with training data)
3. Run predictions on all test examples
4. Collect (true_label, predicted_label, predicted_probability) triples

### Step 2: Compute Metrics

**Classification:**
- Accuracy
- Precision per class
- Recall per class
- F1 per class
- Confusion matrix
- ROC AUC (if binary)

**Regression / Scoring:**
- MAE, RMSE
- Correlation (Pearson + Spearman)
- Residual distribution

### Step 3: Feature Analysis

- **Feature importance**: absolute coefficient values (logistic) or feature importance (trees)
- **Top 5 most important features**: what drives the model
- **Dead features**: any with near-zero weight (candidates for removal)
- **Correlation check**: are any features redundant?

### Step 4: Error Analysis

- **Worst predictions**: the N examples where the model was most wrong
- **Pattern in errors**: do errors cluster on a specific feature value or category?
- **Confidence calibration**: when the model says 90% confident, is it right 90% of the time?

### Step 5: Baseline Comparison

Compare to:
- The existing hardcoded approach (if one exists)
- A random/majority-class baseline
- A simple heuristic (single best feature only)

The model must beat the hardcoded approach to be worth shipping.

### Step 6: Recommend

- **Ship it** — beats baseline by >5%, errors are acceptable
- **Iterate** — accuracy is close but error patterns suggest fixable feature gaps
- **Don't ship** — doesn't beat baseline, or errors are in high-stakes predictions
- **Simplify** — a single feature explains most of the variance, just use that as a threshold

### Output Format

```
## Model Evaluation: <model name>

### Metrics
- Accuracy: X% (baseline: Y%)
- Precision: X% | Recall: X% | F1: X%

### Top Features
1. feature_name (weight: X.XX) — what it means
2. ...

### Error Analysis
- N/M test examples wrong
- Pattern: <description of common errors>

### Recommendation: <Ship | Iterate | Don't ship | Simplify>
### Why: <1-2 sentences>
```

## Rules

- Always compare to baseline — absolute metrics are meaningless without context
- Always analyze errors — aggregate metrics hide important failure modes
- Be honest about overfitting risk with small test sets (<50 examples)
- Report confidence intervals when sample size is small
