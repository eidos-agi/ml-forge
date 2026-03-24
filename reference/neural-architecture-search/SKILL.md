---
name: neural-architecture-search
description: Systematic hyperparameter sweep and neural network architecture comparison for finding optimal model configurations. Covers optimizer selection, regularization, data augmentation, and ablation-based variant comparison. Use when tuning neural networks for any classification or regression task.
---
# Neural Architecture Search (Practical)
## When to Use This Skill
Use after selecting neural networks as your model family. This skill systematically finds the best architecture, optimizer, and regularization combination for your specific dataset.

## Instructions
1. **Baseline Model** — Start simple: 2 Dense layers, SGD optimizer, no regularization, no data augmentation. This is your floor.
2. **Ablation Strategy** — Change ONE variable per experiment:
   - Experiment 1: Swap SGD → Adam (test optimizer effect)
   - Experiment 2: Add Dropout (test regularization effect)
   - Experiment 3: Add SMOTE (test data augmentation effect)
   - Experiment 4: Adam + Dropout (combine best optimizer + regularization)
   - Experiment 5: SMOTE + Adam + Dropout (full treatment)
3. **Architecture Variations**
   - Width: try 32, 64, 128 units per layer
   - Depth: try 2, 3, 4 layers
   - Activation: ReLU (default), LeakyReLU, GELU
   - Dropout rate: 0.2, 0.3, 0.5
4. **Training Parameters**
   - Batch size: 16, 32, 64
   - Epochs: use early stopping with patience=5-10
   - Learning rate: try 0.001 (default), 0.0001, 0.01
5. **Tracking** — Log every experiment: architecture, optimizer, LR, batch size, epochs run, train/val metrics per epoch. Use a DataFrame or MLflow.
6. **Selection** — Best model = highest target metric on VALIDATION set (not test). Only evaluate test ONCE with final model.
7. **Results Table** — Model name | Architecture | Optimizer | SMOTE | Dropout | Val Recall | Val F1 | Train Time
