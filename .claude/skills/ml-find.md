# ml-find — Search for Pre-trained Models Before Training

Before building a model from scratch, check if someone already solved it. HuggingFace has 500K+ models. CoreML has optimized on-device models. ONNX has cross-platform options. Don't reinvent the wheel.

## Trigger

User says `/ml-find` or asks "is there a model for this?" or when `/ml-pick` recommends L3+.

### Arguments

- `<task>` — what the model should do (e.g., "text similarity", "code classification", "NER")
- `<constraints>` — optional: size limits, platform, offline, language

## Instructions

### Step 1: Define the Search

Translate the task into model taxonomy terms:

| User says | Search for |
|-----------|-----------|
| "Are these texts similar?" | sentence-similarity, sentence-transformers |
| "Classify this text" | text-classification, zero-shot-classification |
| "Extract entities from text" | token-classification, NER |
| "Summarize this" | summarization |
| "Is this code buggy?" | text-classification (code domain) |
| "Match these images" | image-feature-extraction, CLIP |
| "Transcribe audio" | automatic-speech-recognition |
| "Detect objects" | object-detection |
| "Generate embeddings" | feature-extraction, sentence-transformers |

### Step 2: Search HuggingFace

Use web search to find models on HuggingFace matching the task:
- Search: `site:huggingface.co/models <task> <constraints>`
- Filter by: most downloaded, task type, model size, license

Key filters:
- **Size**: for local/edge deployment, prefer <1GB models
- **License**: Apache-2.0 or MIT for commercial use
- **Downloads**: more downloads = more battle-tested
- **Last updated**: prefer actively maintained models

### Step 3: Search Other Registries

- **Apple CoreML**: search Apple's ML models gallery for on-device options
- **ONNX Model Zoo**: cross-platform models at github.com/onnx/models
- **TensorFlow Hub**: TF-specific pre-trained models
- **PyTorch Hub**: torch.hub models

### Step 4: Evaluate Candidates

For each candidate model, assess:

| Criterion | What to Check |
|-----------|--------------|
| **Task fit** | Does it actually do what you need? Read the model card. |
| **Size** | Can it fit in memory? On disk? Download time? |
| **License** | Compatible with your project's license? |
| **Dependencies** | What does it need at runtime? (transformers, torch, etc.) |
| **Accuracy** | What benchmarks does it report? On what data? |
| **Platform** | Does it run on your target (macOS, Linux, CPU-only)? |
| **Quantization** | Is there a quantized version for faster/smaller inference? |
| **Active** | Last commit? Open issues? Maintained? |

### Step 5: Test Locally

Before committing:
1. Download the model
2. Run it on 5-10 real examples from your project
3. Measure: accuracy on your data, inference latency, memory usage
4. Compare to the cost of training your own (from /ml-train)

### Step 6: Recommend

```
## Model Search: <task>

### Found: <N> candidates
### Top pick: <model name> (<size>, <license>)
### Why: <1-2 sentences>
### Trade-off vs training your own: <comparison>
### Runtime deps: <what it needs>
### Next step: download and integrate, or train your own with /ml-train
```

## Size Reference

| Category | Size | Can run on |
|----------|------|-----------|
| Tiny | <100MB | Any CPU, edge devices |
| Small | 100MB-1GB | Laptop CPU |
| Medium | 1-4GB | Laptop with 8GB+ RAM, or GPU |
| Large | 4-16GB | Desktop GPU (8GB+ VRAM) |
| XL | >16GB | Cloud GPU or quantized |

For the forge ecosystem (GUARD-002: local only), prefer tiny-to-medium models.

## Common Picks (Known Good)

| Task | Model | Size | Notes |
|------|-------|------|-------|
| Text embeddings | all-MiniLM-L6-v2 | 80MB | Fast, good quality, sentence-transformers |
| Text similarity | all-mpnet-base-v2 | 420MB | Higher quality, slower |
| Code embeddings | microsoft/codebert-base | 500MB | Code-specific |
| Zero-shot classification | facebook/bart-large-mnli | 1.6GB | Classify without training data |
| NER | dslim/bert-base-NER | 430MB | Named entity extraction |
| Sentiment | nlptown/bert-base-multilingual-uncased-sentiment | 680MB | 1-5 star rating |

## Rules

- **Search before training.** If a pre-trained model gets 90% of the way there, use it.
- **Respect size constraints.** Don't suggest a 4GB model for a CLI tool.
- **Check the license.** Not all HuggingFace models are permissively licensed.
- **Test on your data.** Benchmark scores don't transfer. Always validate locally.
- **Consider the dep chain.** A model that needs `transformers + torch + tokenizers` is a heavy lift. Weigh that against training a simple model with zero deps.
