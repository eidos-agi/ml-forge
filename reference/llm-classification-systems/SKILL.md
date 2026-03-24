---
name: llm-classification-systems
description: Build LLM-powered text classification systems that categorize, tag, prioritize, and auto-respond to unstructured text inputs using local or cloud LLMs. Use for document triage, email routing, deal flow classification, compliance flagging, or any text-to-structured-data pipeline.
---
# LLM-Powered Classification Systems
## When to Use This Skill
Use when you need to convert unstructured text into structured categories, tags, priorities, and automated responses. The pattern works for: support tickets, emails, deal flow, legal documents, compliance flags, customer feedback.

## Instructions
1. **Define Taxonomy** — List all categories (mutually exclusive). Define tag vocabulary (can be multi-label). Set priority levels (High/Medium/Low) with criteria.
2. **Design Prompt Template**
   ```
   You are a [domain] classifier. Given the following [input type], provide:
   1. Category: one of [list categories]
   2. Tags: relevant tags from [tag vocabulary]
   3. Priority: High/Medium/Low based on [criteria]
   4. Summary: 1-2 sentence summary
   5. Recommended Action: what should happen next

   Output as JSON with keys: category, tags, priority, summary, action

   Input: {text}
   ```
3. **LLM Configuration** — temperature=0.001 (deterministic for classification), max_tokens sufficient for JSON output, stop tokens to prevent runaway generation
4. **JSON Parsing with Error Handling**
   ```python
   import json
   try:
       result = json.loads(llm_output)
   except json.JSONDecodeError:
       # Attempt to extract JSON from mixed output
       # Fall back to regex extraction
       # Log failure for review
   ```
5. **Batch Processing** — Loop through inputs, collect results in DataFrame, save to CSV. Track processing time per item. Add progress logging.
6. **Validation** — Sample 10-20% of results for human review. Calculate agreement rate. Tune prompt if accuracy < 85%.
7. **Scaling** — For high volume: queue inputs, batch process, cache repeated patterns. Consider fine-tuning if same task runs >1000x/month.
