---
name: automated-eda
description: Automated Exploratory Data Analysis pipeline that produces a complete diagnostic report for any new dataset. Covers shape inspection, dtype analysis, missing values, distributions, correlations, class balance, and visual summaries using pandas, matplotlib, and seaborn. Use when encountering any new dataset before modeling.
---
# Automated EDA Pipeline
## When to Use This Skill
Use as the FIRST step when encountering any new dataset. Before any modeling, run this diagnostic to understand what you're working with. Prevents wasted time on bad data.

## Instructions
1. **Shape & Structure** — `df.shape`, `df.dtypes`, `df.head()`, `df.describe()` — report dimensions, column types, and basic stats
2. **Missing Values** — `df.isnull().sum()` — heatmap of missingness patterns with seaborn. Flag columns >20% missing.
3. **Distributions** — Histogram for every numeric column. Value counts for categoricals. Identify skewness (>1.0 = heavily skewed).
4. **Target Variable Analysis** — Distribution of target. If classification: class balance (ratio of positive/negative). If regression: check for log-normal distribution.
5. **Correlation Matrix** — `df.corr()` with seaborn heatmap. Flag pairs >0.8 (multicollinearity risk). Note features most/least correlated with target.
6. **Categorical Breakdowns** — For each categorical column: value counts, target rate per category (e.g., churn rate by geography).
7. **Outlier Detection** — Box plots for numeric features. IQR method: flag values > Q3+1.5*IQR or < Q1-1.5*IQR.
8. **Summary Report** — Generate markdown report with: row/column counts, missing data flags, distribution plots, correlation highlights, class balance, recommended preprocessing steps.
