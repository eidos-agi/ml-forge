---
name: price-prediction-pipelines
description: Build regression-based price prediction pipelines. Covers SVR with RBF kernel, Ridge/Lasso regression, Neural Networks, Gradient Boosting, and NLP sentiment feature engineering with TextBlob. Use when building any price prediction, valuation, or scoring model for real estate, assets, deals, or financial instruments.
---
# Price Prediction Pipelines
## When to Use This Skill
Use when building any regression-based prediction system: asset pricing, deal scoring, valuation models, or price forecasting. Especially relevant when combining structured features with NLP-derived sentiment scores.

## Instructions
1. **Data Preparation**
   - Load dataset with pandas, inspect shape and dtypes
   - Drop irrelevant columns (IDs, names, timestamps unless engineered)
   - Handle missing values (imputation or removal with justification)
   - Convert booleans to binary, ensure numeric types
2. **Feature Engineering**
   - One-hot encode categorical variables
   - Normalize continuous features with StandardScaler
   - If text data exists: compute TextBlob sentiment polarity (-1 to +1), average per entity, add as numeric feature
   - Log-transform skewed price targets if needed
3. **Feature Selection**
   - Run Lasso (L1) regression first — features with zero coefficients are dropped
   - Run OLS regression and keep features with p-value < 0.05
   - Compare selected feature sets, use intersection or union depending on signal strength
4. **Model Comparison Pipeline**
   - Always train these models in order as benchmarks:
     a. Linear Regression (baseline)
     b. Ridge Regression (L2 regularized)
     c. SVR with RBF kernel (tune C, gamma, epsilon via grid search)
     d. Gradient Boosting (tune n_estimators, learning_rate, max_depth)
     e. Neural Network (2-3 Dense layers, ReLU, Adam optimizer)
   - Train/Val/Test split: 90/5/5 or 80/10/10 with stratification if applicable
5. **Evaluation**
   - Report MAE, MSE, R² for all models on test set
   - Plot predicted vs actual scatter
   - Plot residual distribution
   - Select model with best MAE or R² depending on use case
6. **Production Considerations**
   - Serialize best model with joblib/pickle
   - Save scaler and encoder objects alongside model
   - Document feature list and preprocessing steps for reproducibility
## Libraries
```
scikit-learn (LinearRegression, Ridge, Lasso, SVR, GradientBoostingRegressor, StandardScaler, train_test_split, metrics)
keras (Sequential, Dense)
pandas, numpy, matplotlib, seaborn
textblob (TextBlob for sentiment)
```
