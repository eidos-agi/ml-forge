---
name: churn-attrition-prediction
description: Build neural network churn and attrition prediction models using TensorFlow/Keras with SMOTE for class imbalance, recall optimization, and systematic architecture comparison. Use for customer churn, investor attrition, employee retention, or any binary classification with imbalanced classes.
---
# Churn & Attrition Prediction
## When to Use This Skill
Use for any binary classification where one class is rare and costly to miss: customer churn, fraud, investor departure, employee attrition, loan default, medical diagnosis.

## Instructions
1. **Data Preparation**
   - Load data, check class distribution (positive/negative ratio)
   - Drop irrelevant IDs and names
   - Stratified train/val/test split: 60/20/20 (preserves class ratio in each split)
   - One-hot encode categoricals (Geography, Gender, etc.)
   - StandardScaler on continuous features (CreditScore, Age, Balance, etc.)
2. **Handle Class Imbalance**
   - Apply SMOTE (imbalanced-learn) to training set ONLY (never to val/test)
   - `SMOTE(random_state=42).fit_resample(X_train, y_train)`
   - Verify balanced class distribution after SMOTE
3. **Build Model Variants** (all Sequential API):
   - Model 0: Dense(64,relu)→Dense(32,relu)→Dense(1,sigmoid), SGD(lr=0.001), 100 epochs — BASELINE
   - Model 1: Dense(64,relu)→Dense(64,relu)→Dense(1,sigmoid), Adam, 20 epochs — better optimizer
   - Model 2: Add Dropout(0.3) after each Dense — regularization
   - Model 3: SMOTE + SGD — test imbalance fix alone
   - Model 4: SMOTE + Adam — combine both improvements
   - Model 5: SMOTE + Adam + Dropout — FULL TREATMENT (usually wins)
4. **Loss & Metrics** — binary_crossentropy loss. Primary metric: RECALL (catching positives). Secondary: F1, Precision, AUC.
5. **Evaluation** — confusion_matrix, classification_report, roc_curve for each model. Compare validation recall across all 6 variants. Best recall on val set = best model.
6. **Key Insights to Extract**
   - Churn rate by category (geography, product count, activity status)
   - Which features correlate most with churn
   - 100% churn in extreme categories (e.g., 4-product customers) — small N warning
## Libraries
```python
tensorflow==2.15.0, scikit-learn==1.2.2, imbalanced-learn==0.10.1
pandas==1.5.3, numpy==1.25.2, matplotlib==3.7.1, seaborn==0.13.1
```
