---
name: statistical-significance
description: Test and validate statistical significance of features, models, and signals. Covers p-value analysis, correlation interpretation, hypothesis testing, and avoiding spurious correlations. Critical for financial modeling where false positives are costly. Use when validating whether a signal, feature, or model result is real vs noise.
---
# Statistical Significance Testing
## When to Use This Skill
Use when you need to determine if a result is real or noise. Critical for: financial signal validation, A/B testing, feature selection, model comparison, and any decision where "is this actually significant?" matters.

## Instructions
1. **Hypothesis Formulation** — State null (H0: no effect) and alternative (H1: effect exists). Be specific about what "effect" means in your domain.
2. **Correlation Analysis** — Pearson r for linear relationships. Spearman rho for monotonic. Interpret: |r|<0.1 negligible, 0.1-0.3 weak, 0.3-0.7 moderate, >0.7 strong. ALWAYS check scatter plot — correlation doesn't capture nonlinear relationships.
3. **p-value Testing** — For features: OLS regression p-values. For group comparisons: t-test (2 groups) or ANOVA (3+ groups). For proportions: chi-squared test. Threshold: p<0.05 standard, p<0.01 for high-stakes decisions.
4. **Multiple Comparison Correction** — If testing many features/signals simultaneously: apply Bonferroni correction (divide alpha by number of tests) or use FDR (Benjamini-Hochberg). Without correction, 5% of tests will appear significant by chance.
5. **Effect Size** — A tiny p-value with tiny effect size is statistically significant but practically useless. Report Cohen's d for group comparisons, R² for regressions.
6. **Out-of-Sample Validation** — Statistical significance in-sample means nothing if it doesn't hold out-of-sample. Always validate on held-out data. For time series: walk-forward validation (no future leakage).
7. **Domain Sanity Check** — Can you explain WHY this relationship exists? If not, it may be spurious. Calibrate against known relationships from authoritative sources.
