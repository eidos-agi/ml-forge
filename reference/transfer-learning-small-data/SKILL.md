---
name: transfer-learning-small-data
description: Apply cross-domain transfer learning to adapt pre-trained models to new domains with minimal data. Covers fine-tuning, few-shot, and zero-shot techniques with foundation models. Use when you have a small dataset in a niche vertical (specialty finance, agribusiness, healthcare) and need to leverage models trained on larger domains.
---
# Cross-Domain Transfer Learning
## When to Use This Skill
Use when you have <1000 labeled examples in a niche domain but need production-quality models. Transfer learning lets you leverage models pre-trained on millions of examples and fine-tune them on your small dataset.

## Instructions
1. **Assess Data Availability**
   - >10K labeled examples → train from scratch or fine-tune
   - 100-10K examples → fine-tune pre-trained model
   - 10-100 examples → few-shot learning (in-context examples in prompt)
   - 0 examples → zero-shot (rely on model's pre-training)
2. **Text Tasks: Fine-tune BERT/RoBERTa**
   ```python
   from transformers import AutoModelForSequenceClassification, Trainer
   model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=N)
   # Freeze early layers, train last 2-3 layers + classifier head
   for param in model.bert.embeddings.parameters(): param.requires_grad = False
   for param in model.bert.encoder.layer[:10].parameters(): param.requires_grad = False
   ```
3. **Image Tasks: Fine-tune ViT/ResNet**
   - Load pre-trained model, replace final classification layer
   - Freeze backbone, train only new head for 5-10 epochs
   - Unfreeze backbone, train end-to-end at low LR for 5-10 more epochs
4. **Zero-Shot with LLMs** — Describe the task and categories in a prompt. Provide no examples. Works surprisingly well for classification, extraction, summarization.
5. **Few-Shot with LLMs** — Include 3-5 labeled examples in the prompt before the actual input. Choose diverse, representative examples.
6. **Validation** — Always hold out 20% for testing. Even with small data, you need to measure real performance. Use stratified k-fold if data is very small.
7. **Domain Adaptation Tips**
   - Use domain-specific pre-trained models when available (FinBERT for finance, BioBERT for medical)
   - Augment small dataset with paraphrasing, synonym replacement, back-translation
   - Synthetic data from LLMs: generate labeled examples for underrepresented classes
