---
name: ensemble-voting-systems
description: Build multi-model ensemble voting systems with confidence thresholds and human-in-the-loop routing for high-stakes classification. Use for fraud detection, compliance, medical, investment signals, or any domain where accuracy is critical and errors are costly.
---
# Ensemble Voting for High-Stakes Classification
## When to Use This Skill
Use when accuracy matters more than speed or cost. Fraud detection, compliance flagging, medical diagnosis, investment signal confirmation, wildfire damage assessment. The pattern: multiple models vote, disagreement triggers human review.

## Instructions
1. **Train Diverse Models** — Diversity is more important than individual accuracy. Use models with different architectures:
   - Model A: Random Forest
   - Model B: Gradient Boosting (XGBoost)
   - Model C: Neural Network
   - Model D: SVM (RBF kernel)
   - Model E: Logistic Regression (or different NN architecture)
2. **Voting Strategies**
   - Hard voting: majority wins (3 out of 5 agree = prediction)
   - Soft voting: average predicted probabilities, pick highest
   - Weighted voting: better models get more weight (calibrate on validation set)
3. **Confidence Scoring**
   - For soft voting: confidence = max average probability
   - For hard voting: confidence = fraction of models agreeing (5/5 = 1.0, 3/5 = 0.6)
4. **Human-in-the-Loop Routing**
   - confidence >= 0.8 → auto-approve (high agreement)
   - 0.6 <= confidence < 0.8 → flag for quick review
   - confidence < 0.6 → route to human expert (models disagree)
   - Track: what % of cases hit each tier? Adjust thresholds accordingly.
5. **Monthly Retraining** — Retrain all models on accumulated data including human-corrected cases. Track model drift over time.
6. **Cost Optimization** — Batch processing overnight for 75% cost reduction vs real-time. Queue non-urgent classifications for batch inference.
7. **Monitoring** — Track per-model accuracy, ensemble accuracy, confidence distribution, human review rate. Alert if any model's solo accuracy drops >5%.
