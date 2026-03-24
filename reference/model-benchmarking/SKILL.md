---
name: model-benchmarking
description: Systematic model comparison framework for evaluating multiple ML algorithms on the same dataset. Benchmarks Linear Regression, Ridge, SVR, Neural Nets, and Gradient Boosting side-by-side with consistent preprocessing. Use when starting any new prediction problem to establish baselines and select the best approach.
---
# Model Comparison Frameworks
## When to Use This Skill
Use at the start of any new ML prediction problem. Before committing to one algorithm, benchmark several systematically. This prevents premature optimization and ensures you pick the right tool.

## Instructions
1. **Fixed Data Split** — Create ONE train/val/test split used by ALL models. Use stratification for classification. Set random_state for reproducibility.
2. **Preprocessing** — Apply identical preprocessing to all models (same scaler, same encoding, same feature set).
3. **Regression Benchmark Suite:**
   a. Linear Regression (baseline — if this wins, your features are strong)
   b. Ridge Regression (regularized — reduces overfitting)
   c. Lasso Regression (sparse — automatic feature selection)
   d. SVR with RBF kernel (nonlinear — grid search C, gamma)
   e. Gradient Boosting (ensemble — tune n_estimators, lr, depth)
   f. Random Forest (ensemble — tune n_estimators, max_depth)
   g. Neural Network (2-3 layers, ReLU, Adam)
4. **Classification Benchmark Suite:**
   a. Logistic Regression (baseline)
   b. Random Forest
   c. Gradient Boosting (XGBoost/LightGBM)
   d. SVM (RBF kernel)
   e. Neural Network with Dropout
   f. Neural Network with SMOTE + Dropout (if imbalanced)
5. **Metrics Table** — For each model, report: Train score, Val score, Test score, Training time. Use consistent metrics (R²/MAE for regression; Recall/F1/AUC for classification).
6. **Selection Criteria** — Best test metric wins, but also consider: training time, interpretability, deployment complexity, overfitting gap (train vs test).
7. **Document Results** — Save comparison table as CSV. Plot bar chart of test metrics across models.
