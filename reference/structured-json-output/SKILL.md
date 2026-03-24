---
name: structured-json-output
description: Engineer LLM prompts that produce parseable, validated JSON output for programmatic consumption. Covers prompt design for JSON compliance, error handling for malformed outputs, and schema validation. The bridge between conversational AI and backend automation. Use whenever LLM output needs to be consumed by code rather than humans.
---
# Structured JSON Output Engineering
## When to Use This Skill
Use whenever an LLM's output needs to be consumed by code, not humans. If the next step after the LLM is `json.loads()`, you need this skill. Critical for: API backends, data pipelines, automation workflows, agent tool calling.

## Instructions
1. **Prompt Design for JSON**
   - Explicitly state "Output ONLY valid JSON, no other text"
   - Provide exact schema with field names, types, and example values
   - Use few-shot examples showing the exact JSON format expected
   - Set temperature low (0.001-0.1) for consistency
2. **Schema Definition**
   ```python
   expected_schema = {
       "type": "object",
       "properties": {
           "category": {"type": "string", "enum": ["A", "B", "C"]},
           "tags": {"type": "array", "items": {"type": "string"}},
           "confidence": {"type": "number", "minimum": 0, "maximum": 1}
       },
       "required": ["category", "tags", "confidence"]
   }
   ```
3. **Parsing with Fallbacks**
   ```python
   import json, re
   def parse_llm_json(text):
       # Try direct parse
       try: return json.loads(text)
       except: pass
       # Try extracting JSON block from markdown
       match = re.search(r'```json?\s*(.*?)\s*```', text, re.DOTALL)
       if match:
           try: return json.loads(match.group(1))
           except: pass
       # Try extracting first { ... } block
       match = re.search(r'\{.*\}', text, re.DOTALL)
       if match:
           try: return json.loads(match.group(0))
           except: pass
       return None  # Log and handle failure
   ```
4. **Schema Validation** — Use jsonschema library to validate parsed output against expected schema. Reject and retry (up to 3 attempts) if invalid.
5. **Retry Strategy** — If JSON parsing fails: append "Your previous output was not valid JSON. Output ONLY the JSON object:" to prompt and retry.
6. **Batch Processing Pattern** — For multiple items: process sequentially, collect successes and failures separately, report success rate, review failures manually.
