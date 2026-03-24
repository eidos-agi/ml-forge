---
name: local-llm-deployment
description: Deploy and run LLMs locally using llama-cpp-python with GGUF quantized models for private, air-gapped inference. Covers Mistral-7B deployment, quantization formats, CPU-only inference, and Hugging Face model management. Use when data privacy requires on-premise inference or when cloud API costs need to be eliminated.
---
# Local LLM Deployment
## When to Use This Skill
Use when you need private LLM inference without sending data to external APIs. Critical for: fund positions, client PII, legal documents, compliance-sensitive workflows, or simply reducing API costs to zero.

## Instructions
1. **Install Dependencies**
   ```bash
   pip install llama-cpp-python==0.1.85 huggingface_hub==0.20.3
   ```
2. **Download Model from Hugging Face**
   ```python
   from huggingface_hub import hf_hub_download
   model_path = hf_hub_download(
       repo_id="TheBloke/Mistral-7B-Instruct-v0.2-GGUF",
       filename="mistral-7b-instruct-v0.2.Q6_K.gguf"
   )
   ```
3. **Load Model**
   ```python
   from llama_cpp import Llama
   llm = Llama(model_path=model_path, n_ctx=1024, n_gpu_layers=0)
   ```
   - n_ctx=1024: context window (increase for longer docs, costs more RAM)
   - n_gpu_layers=0: CPU-only (set higher if GPU available)
4. **Inference**
   ```python
   output = llm(prompt, max_tokens=512, temperature=0.001, stop=["```"])
   result = output['choices'][0]['text']
   ```
   - temperature=0.001: near-deterministic (good for classification/extraction)
   - temperature=0.7: more creative (good for drafting)
5. **Quantization Options** (trade-off: size vs quality)
   - Q4_K_M: smallest, fastest, slight quality loss
   - Q5_K_M: balanced
   - Q6_K: highest quality quantized (recommended balance of size vs quality)
   - Q8_0: near-full-precision
6. **Model Options Beyond Mistral-7B**
   - Llama 2/3 (Meta) — strong general purpose
   - Phi-2/3 (Microsoft) — small but capable
   - Gemma (Google) — efficient small models
   - Qwen (Alibaba) — strong multilingual
7. **Production Considerations**
   - Batch requests to amortize model loading time
   - Cache model in memory between calls (don't reload per request)
   - For multiple users: wrap in FastAPI endpoint with queue
   - Monitor RAM usage: 7B Q6_K ≈ 6GB RAM
