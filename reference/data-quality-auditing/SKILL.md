---
name: data-quality-auditing
description: Systematic data quality auditing framework for cleaning, validating, and documenting dataset issues before ML modeling. Covers type conversion, missing value strategies, deduplication, outlier handling, and data validation. Use before any modeling or analysis to ensure data integrity.
---
# Data Quality Auditing
## When to Use This Skill
Use before feeding any data into a model, dashboard, or report. Data quality is the most common source of model failure. This is your quality gate.

## Instructions
1. **Schema Validation** — Verify column names match expected schema. Check dtypes (numeric should be numeric, dates should parse). Flag unexpected columns.
2. **Completeness** — For each column: % null, % empty string, % zero. Columns >50% null: consider dropping. Columns 5-50%: impute or flag.
3. **Uniqueness** — Check for duplicates by primary key. Count exact duplicate rows. Identify near-duplicates (fuzzy matching on key fields).
4. **Type Coercion** — Convert strings to numeric where applicable. Convert True/False to 1/0. Parse dates to datetime. Encode categoricals (one-hot or label).
5. **Range Validation** — Min/max for each numeric column. Flag values outside expected business range (negative prices, future dates, ages >120).
6. **Outlier Assessment** — IQR method for each numeric feature. Don't auto-remove — document and decide per feature (some outliers are valid signal).
7. **Referential Integrity** — If joining tables: check foreign key completeness. How many records have no match? Are join keys consistent (case, spacing)?
8. **Output: Data Quality Report** — For each column: completeness %, type status, outlier count, unique count. Overall: row count, duplicate count, recommended actions list.
