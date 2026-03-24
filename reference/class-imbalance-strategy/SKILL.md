---
name: class-imbalance-strategy
description: Handle class imbalance in ML classification problems using SMOTE, stratified splitting, recall optimization, and specialized evaluation metrics. Use for fraud detection, rare event prediction, medical diagnosis, anomaly detection, or any problem where the minority class matters most.
---
# Class Imbalance Strategy
## When to Use This Skill
Use whenever your positive class is <30% of the dataset. Most real-world problems are imbalanced: fraud (0.1%), churn (20%), rare diseases, anomalies, investment opportunities. Standard accuracy is misleading — a model predicting "no fraud" 99.9% of the time gets 99.9% accuracy but catches nothing.

## Instructions
1. **Diagnose** — `df['target'].value_counts(normalize=True)` — if minority <30%, you have an imbalance problem
2. **Stratified Splitting** — ALWAYS use `stratify=y` in train_test_split to preserve class ratios across splits
3. **SMOTE (Synthetic Minority Oversampling)**
   - Apply ONLY to training set (NEVER to validation or test)
   - `from imblearn.over_sampling import SMOTE`
   - `X_resampled, y_resampled = SMOTE(random_state=42).fit_resample(X_train, y_train)`
4. **Alternative Techniques** (when SMOTE isn't enough)
   - Class weights: `model.fit(class_weight={0: 1, 1: ratio})` — no synthetic data needed
   - Random undersampling: reduce majority class (loses data but simple)
   - ADASYN: adaptive synthetic sampling (focuses on harder examples)
   - Ensemble: BalancedRandomForest, EasyEnsemble
5. **Metric Selection** — NEVER use accuracy as primary metric for imbalanced data.
   - Recall (sensitivity): how many positives did we catch?
   - Precision: of those we predicted positive, how many were correct?
   - F1: harmonic mean of recall and precision
   - AUC-ROC: overall discrimination ability
   - Choose: if false negatives are costly → optimize RECALL. If false positives are costly → optimize PRECISION.
6. **Threshold Tuning** — Default threshold is 0.5. For imbalanced data, lower it (0.3-0.4) to catch more positives at the cost of more false positives. Plot precision-recall curve to find optimal threshold.
