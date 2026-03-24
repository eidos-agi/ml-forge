---
name: sentiment-investment-signals
description: Build NLP sentiment pipelines that extract numeric sentiment scores from text data (earnings calls, SEC filings, news, reviews) and integrate them as features in ML models. Use for alternative data signal generation, earnings analysis, or any text-to-numeric-feature pipeline.
---
# Sentiment-Driven Investment Signals
## When to Use This Skill
Use when you need to convert unstructured text into numeric features for ML models: earnings call transcripts, SEC filings, news articles, social media, customer reviews. The pattern: text → sentiment score → feature → model.

## Instructions
1. **Text Collection** — Gather corpus (earnings transcripts, 10-K/10-Q sections, news articles)
2. **Preprocessing** — Clean HTML/formatting, sentence tokenize, remove boilerplate headers/footers
3. **Sentiment Scoring**
   - TextBlob: `TextBlob(text).sentiment.polarity` for quick baseline (-1 to +1)
   - For production: consider VADER (financial text tuned) or FinBERT
   - Score at sentence level, then aggregate (mean, median, std, min, max per document)
4. **Feature Engineering**
   - `sentiment_mean`: average sentiment across all sentences
   - `sentiment_std`: volatility of sentiment (mixed signals = uncertainty)
   - `sentiment_delta`: change from prior period (sentiment momentum)
   - `negative_ratio`: fraction of sentences with polarity < -0.1
   - `strong_positive_ratio`: fraction with polarity > 0.5
5. **Integration with Structured Data** — Join sentiment features to entity (ticker, property, deal) by ID and date. Ensure temporal alignment (no look-ahead bias).
6. **Validation** — Check correlation of sentiment features with target variable. If |r| < 0.02, the signal may be too weak. Test in-sample and out-of-sample.
