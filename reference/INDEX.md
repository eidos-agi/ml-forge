# ML-Forge Reference Library

Technique-specific skills on deck. Not production skills — a knowledge base that agents pull from when `/ml-pick` or `/ml-train` identifies a need for a specific technique.

## Core ML Techniques

| Skill | When to use |
|-------|------------|
| class-imbalance-strategy | Minority class <30% of dataset (fraud, anomaly, rare events) |
| ensemble-voting-systems | High-stakes classification where accuracy > speed (multi-model voting + HITL) |
| feature-selection-alpha | Wide feature set, need to separate signal from noise (L1/Lasso, p-values) |
| model-benchmarking | Comparing multiple algorithms on the same dataset systematically |
| statistical-significance | Validating that features/models/signals are real, not noise |
| transfer-learning-small-data | <100 labeled examples — adapt a pre-trained model instead of training from scratch |
| neural-architecture-search | Hyperparameter sweep and architecture comparison for neural nets |

## Production & Deployment

| Skill | When to use |
|-------|------------|
| model-compression-deployment | Shrinking models for edge/mobile (pruning, quantization, distillation) |
| batch-inference-optimization | Reducing inference costs through batching and scheduling |
| local-llm-deployment | Running LLMs locally with llama-cpp-python and GGUF models |

## Data & Features

| Skill | When to use |
|-------|------------|
| automated-eda | First look at a new dataset — distributions, nulls, correlations, outliers |
| data-quality-auditing | Cleaning and validating data before modeling |
| custom-metric-engineering | Building domain-specific composite metrics from available signals |

## Applied Patterns

| Skill | When to use |
|-------|------------|
| churn-attrition-prediction | User/customer churn prediction with neural nets + SMOTE |
| price-prediction-pipelines | Regression-based price/value prediction (SVR, Ridge, neural) |
| sentiment-investment-signals | NLP sentiment extraction from text for trading/analysis signals |

## LLM Integration

| Skill | When to use |
|-------|------------|
| llm-classification-systems | LLM-powered text classification (categorize, tag, prioritize) |
| structured-json-output | Getting parseable JSON from LLM prompts |
| multimodal-document-understanding | Processing docs with text + images (CLIP, BERT, ViT) |
