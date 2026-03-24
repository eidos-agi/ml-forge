---
name: model-compression-deployment
description: Compress ML models for production deployment using pruning, quantization, and knowledge distillation. Reduce inference cost, latency, and memory footprint while maintaining accuracy. Use when deploying models to production, edge devices, or cost-sensitive environments.
---
# Model Compression for Edge Deployment
## When to Use This Skill
Use when a model works in development but is too expensive, slow, or large for production. Reduce model size 2-10x while keeping 95%+ accuracy.

## Instructions
1. **Quantization** (easiest, biggest bang for buck)
   - Post-training quantization: convert FP32 weights to INT8 or INT4
   - For LLMs: GGUF format with quality levels (Q4_K_M → Q8_0)
   - For PyTorch: `torch.quantization.quantize_dynamic(model, {nn.Linear}, dtype=torch.qint8)`
   - Typical: 2-4x size reduction, <2% accuracy loss
2. **Pruning** (remove unnecessary weights)
   - Magnitude pruning: zero out weights below threshold
   - Structured pruning: remove entire neurons/filters
   - `torch.nn.utils.prune.l1_unstructured(layer, name='weight', amount=0.3)` — prune 30%
   - Retrain after pruning to recover accuracy
3. **Knowledge Distillation** (train small model to mimic large model)
   - Teacher: your large, accurate model
   - Student: smaller architecture (fewer layers/units)
   - Train student on teacher's soft outputs (logits), not just ground truth
   - Student learns the teacher's decision boundaries, not just labels
4. **ONNX Export** — Convert PyTorch/TF model to ONNX for cross-platform deployment. ONNX Runtime provides optimized inference.
5. **Benchmarking** — For each compression method, measure: model size (MB), inference time (ms), accuracy on test set, memory usage (MB). Accept if accuracy drop <2% for >2x speedup.
