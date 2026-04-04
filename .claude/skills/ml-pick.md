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

## Core Principle: Gravity Points Down

**Every level above L0 requires a compelling reason.** The burden of proof is on escalation, never on simplicity. You must demonstrate — with a concrete failing example — that the current level cannot solve the problem before moving up. "It might generalize better" is not a compelling reason. "Here are 3 real inputs where L0 gives the wrong answer and L1 fixes them" is.

**Ensemble ML (L1-L2) is often better than LLM calls (L5-L6).** A bag of small, fast, local models frequently outperforms a single expensive LLM call — cheaper, faster, more reliable, fully private. Before reaching for an LLM, ask: could 3 cheap classifiers voting beat one expensive generation?

## Instructions

### Step 1: Frame the Problem

What are you predicting / deciding / classifying / generating? Write it as a one-liner.

Examples:
- "Which preflight checks to run for this project" → prediction
- "Whether two apps are functional substitutes" → classification
- "What's wrong with this code" → reasoning + generation
- "Summarize this PR" → generation

### Step 2: Try L0 First (MANDATORY)

Before considering any ML, write the L0 solution. Literally sketch it:
- What would the if/else or lookup table look like?
- How many cases does it cover?
- What inputs would it get wrong?

**If you cannot name 3+ real inputs where L0 gives the wrong answer, stop here. Use L0.**

A 20-line lookup table that covers 15 known cases is not a problem that needs embeddings. The number of cases matters: <50 items is almost always L0 territory.

### Step 3: Check Constraints

| Constraint | Impact |
|-----------|--------|
| Must work offline? | Eliminates L5, L6 |
| Zero runtime deps? | Eliminates L2, L3 (unless model ships as constants) |
| No data can leave machine? | Eliminates L5, L6 |
| Must be deterministic? | Favors L0-L2 |
| Latency budget <100ms? | Eliminates L3-L6 |
| No GPU available? | Eliminates L4 (or limits to CPU-friendly SLMs) |

### Step 4: Check Data Availability

| Available data | Best level |
|---------------|-----------|
| Exhaustive list of all cases | L0 (just hardcode it) |
| 50-500 labeled examples with clear features | L1 |
| 500+ labeled examples, complex features | L2 |
| No labeled data, but need semantic understanding | L3 |
| No labeled data, need reasoning | L4 or L5 |
| Need to generate text/code | L4, L5, or L6 |

### Step 5: Check if a Pre-trained Model Exists

Before training anything, search:
- **HuggingFace** (`/ml-find`) — thousands of pre-trained models for common tasks
- **Apple's CoreML models** — on-device models optimized for macOS/iOS
- **ONNX Model Zoo** — cross-platform pre-trained models

If a pre-trained model does what you need at L3, don't train from scratch at L1-L2. But also ask: do you actually need L3, or did you skip Step 2?

### Step 6: Justify the Escalation (MANDATORY for L2+)

You may NOT recommend L2 or above without filling in ALL of these:

1. **L0 fails because:** <concrete example where the lookup/rule gives the wrong answer>
2. **L(N-1) fails because:** <concrete example where the level below your pick gives the wrong answer>
3. **The cost is justified because:** <what you gain vs what you pay in deps/latency/complexity>
4. **Ensemble alternative considered?** Could multiple L1-L2 models voting outperform a single L3+?

If you can't fill these in with real examples (not hypotheticals), drop back down.

### Step 7: Recommend

Output:

```
## Intelligence Pick: <problem>

### L0 attempt: <what the hardcoded solution looks like>
### L0 covers: <N>/<total> known cases
### L0 fails on: <specific examples, or "nothing — use L0">
### Recommended: L<N> — <name>
### Escalation evidence: <the concrete failing examples that justify this level>
### Why not higher: <what you'd gain isn't worth the cost>
### Ensemble considered: <yes/no — if yes, what combination>
### Hybrid option: <if applicable>
### Constraints satisfied: <list>
### Next step: <which ml-forge skill to run>
```

## Decision Shortcuts

These common patterns have known-good starting points. **But you still must try L0 first — these shortcuts tell you where you'll likely land, not where you start.**

| Pattern | Likely level | Why | But try L0 if... |
|---------|-------------|-----|-------------------|
| "Which of these N things to pick" + labeled history | L1 | It's a classifier with features | N < 50 and categories are distinct |
| "Are these two things similar" + structured metadata | L1 | Pairwise feature Jaccard + learned weights | Metadata fields are few and well-defined |
| "Are these two things similar" + only text descriptions | L3 | Need embeddings for semantic similarity | Descriptions contain obvious keywords |
| "What's wrong with this" | L5 | Needs reasoning | Failure modes are known and enumerable |
| "Summarize / explain / generate" | L5-L6 | Language generation | — (L0 can't generate) |
| "Classify this text into categories" + many examples | L2-L3 | Fine-tuned BERT or classical ML | Categories map cleanly to keywords |
| "Classify this text into categories" + few examples | L5 | Few-shot prompting | Categories are obvious from structure |
| "Detect anomalies in time series" | L2 | Isolation forest, no LLM needed | Thresholds are known |
| "Route this to the right handler" | L0-L1 | Keyword matching or feature-based routing | Handlers are few and well-described |

## Rules

- **L0 is the default.** Everything else is escalation that must be earned.
- **Escalation requires concrete failing examples.** "I think we need an LLM" is not evidence. "Here are 3 real inputs where L0 gives the wrong answer" is evidence.
- **Prefer ensembles of cheap models over single expensive ones.** Three L1 classifiers voting often beats one L3 embedding model — and costs nothing at runtime.
- **Respect project constraints.** If the project has zero-dep or offline requirements, those gate the decision.
- **Hybrid is usually the answer.** L0 for the known cases + L1 for the fuzzy middle + L5 fallback for the truly weird. Each layer earns its place.
