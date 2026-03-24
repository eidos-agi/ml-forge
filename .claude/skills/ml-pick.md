# ml-pick — Choose the Right Intelligence Level

Not every problem needs a trained model. Not every problem needs an LLM. This skill helps agents pick the right tool from the full intelligence spectrum — from hardcoded rules to SOTA cloud models.

## Trigger

User says `/ml-pick` or asks "how should we solve this?" for a problem that might involve ML, AI, or learned intelligence.

## The Spectrum

There are 7 levels of intelligence you can throw at a software problem. Each has different costs, capabilities, and constraints. **Always start from the bottom and escalate only when the lower level provably fails.**

| Level | What | Runtime Cost | Latency | Privacy | When |
|-------|------|-------------|---------|---------|------|
| **L0: Hardcoded rules** | if/else, lookup tables, regex | Zero | Instant | Full | Problem is known, finite, and stable |
| **L1: Learned weights** | Logistic regression, weight vectors, decision trees shipped as constants | Zero | Instant | Full | Pattern exists in local data, enough labels, simple features |
| **L2: Classical ML** | sklearn models (random forest, SVM, gradient boosting) | sklearn dep | Fast | Full | More complex patterns, nonlinear interactions, >500 examples |
| **L3: Pre-trained embeddings** | BERT, sentence-transformers, CLIP — downloaded once, run locally | ~500MB model | Moderate | Full | Semantic similarity, text/image classification, no training data |
| **L4: Small local models (SLMs)** | Phi-3, Qwen-2, Gemma-2, Mistral-7B — run on device | 2-8GB VRAM | Seconds | Full | Reasoning over text needed, privacy required, offline |
| **L5: LLM calls** | claude -p, GPT-4o — cloud API | $/token | Seconds | Partial | Complex reasoning, few-shot, natural language understanding |
| **L6: SOTA / frontier** | Latest research models, fine-tuned specialists | $$$  | Variable | Depends | When accuracy is everything, cost is acceptable, one-off |

## Instructions

### Step 1: Frame the Problem

What are you predicting / deciding / classifying / generating? Write it as a one-liner.

Examples:
- "Which preflight checks to run for this project" → prediction
- "Whether two apps are functional substitutes" → classification
- "What's wrong with this code" → reasoning + generation
- "Summarize this PR" → generation

### Step 2: Check Constraints

| Constraint | Impact |
|-----------|--------|
| Must work offline? | Eliminates L5, L6 |
| Zero runtime deps? | Eliminates L2, L3 (unless model ships as constants) |
| No data can leave machine? | Eliminates L5, L6 |
| Must be deterministic? | Favors L0-L2 |
| Latency budget <100ms? | Eliminates L3-L6 |
| No GPU available? | Eliminates L4 (or limits to CPU-friendly SLMs) |

### Step 3: Check Data Availability

| Available data | Best level |
|---------------|-----------|
| Exhaustive list of all cases | L0 (just hardcode it) |
| 50-500 labeled examples with clear features | L1 |
| 500+ labeled examples, complex features | L2 |
| No labeled data, but need semantic understanding | L3 |
| No labeled data, need reasoning | L4 or L5 |
| Need to generate text/code | L4, L5, or L6 |

### Step 4: Check if a Pre-trained Model Exists

Before training anything, search:
- **HuggingFace** (`/ml-find`) — thousands of pre-trained models for common tasks
- **Apple's CoreML models** — on-device models optimized for macOS/iOS
- **ONNX Model Zoo** — cross-platform pre-trained models

If a pre-trained model does what you need at L3, don't train from scratch at L1-L2.

### Step 5: Evaluate the Escalation

If you're tempted to go above L2, ask:
1. **Did you try L0-L1 first?** If not, try them. They often work.
2. **What specifically fails at the lower level?** Be concrete.
3. **Is the accuracy gap worth the cost?** L1 at 85% accuracy with zero deps vs L5 at 92% accuracy with API costs and latency.
4. **Can you hybrid?** L1 for the common case + L5 fallback for the uncertain case.

### Step 6: Recommend

Output:

```
## Intelligence Pick: <problem>

### Recommended: L<N> — <name>
### Why not lower: <what fails at L(N-1)>
### Why not higher: <what you'd gain isn't worth the cost>
### Hybrid option: <if applicable>
### Constraints satisfied: <list>
### Next step: <which ml-forge skill to run>
```

## Decision Shortcuts

These common patterns have known-good answers:

| Pattern | Level | Why |
|---------|-------|-----|
| "Which of these N things to pick" + labeled history | L1 | It's a classifier with features |
| "Are these two things similar" + structured metadata | L1 | Pairwise feature Jaccard + learned weights |
| "Are these two things similar" + only text descriptions | L3 | Need embeddings for semantic similarity |
| "What's wrong with this" | L5 | Needs reasoning |
| "Summarize / explain / generate" | L5-L6 | Language generation |
| "Classify this text into categories" + many examples | L2-L3 | Fine-tuned BERT or classical ML |
| "Classify this text into categories" + few examples | L5 | Few-shot prompting |
| "Detect anomalies in time series" | L2 | Isolation forest, no LLM needed |
| "Route this to the right handler" | L1 | Feature-based routing with learned weights |

## Rules

- **Always try L0 first.** If a 20-line if/else works, stop there.
- **Escalation requires evidence.** "I think we need an LLM" is not evidence. "L1 gets 62% accuracy because the features can't capture semantic meaning" is evidence.
- **Respect project constraints.** If the project has zero-dep or offline requirements, those gate the decision.
- **Hybrid is usually the answer.** L1 for 90% of cases + L5 fallback for the weird 10% beats pure L5 on cost and latency.
