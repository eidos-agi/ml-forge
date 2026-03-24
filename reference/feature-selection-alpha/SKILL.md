---
name: feature-selection-alpha
description: Apply Lasso (L1) regularization and p-value statistical feature selection to identify which variables carry real predictive signal vs noise. Use when building financial models, trading signals, or any ML pipeline where feature selection determines model quality and interpretability.
---
# Feature Selection for Alpha Signals
## When to Use This Skill
Use when you have a wide feature set and need to separate signal from noise. Critical for financial modeling, trading signal engineering, and any domain where spurious correlations are costly.

## Instructions
1. **Baseline OLS Regression**
   - Fit OLS on all features using statsmodels
   - Extract p-values for each coefficient
   - Flag features with p < 0.05 as statistically significant
   - Note: low p-value alone doesn't guarantee economic significance
2. **Lasso (L1) Regularization**
   - Fit LassoCV with cross-validation to auto-select alpha
   - Extract coefficient vector — features with coef=0 are eliminated
   - Rank surviving features by absolute coefficient magnitude
   - These are your candidate signal features
3. **Cross-Validation Stability Check**
   - Run Lasso on 5-10 different train/test splits
   - Count how many times each feature survives (coefficient != 0)
   - Features surviving >80% of splits are robust signals
   - Features surviving <50% are unstable — treat with suspicion
4. **Correlation Audit**
   - Compute pairwise correlation matrix for selected features
   - If two features correlate >0.8, keep the one with lower p-value or stronger Lasso coefficient
   - Plot correlation heatmap with seaborn for visual inspection
5. **Domain Validation**
   - For each selected feature, ask: "Does this make economic/business sense?"
   - A feature can be statistically significant but economically meaningless
   - Document the rationale for each kept feature
6. **Output**
   - Final feature list with: name, Lasso coefficient, p-value, stability score, domain rationale
   - Reduced dataset ready for model training
