---
name: llm-document-drafting
description: Use LLMs to draft, refine, and compare business documents (contracts, proposals, reports). Covers iterative refinement workflows that improve legal language precision, structure, and completeness. Use for contract drafting, proposal writing, report generation, or any document that benefits from AI refinement.
---
# LLM-Assisted Document Drafting & Comparison
## When to Use This Skill
Use when drafting or refining any business document: contracts, proposals, SOWs, reports, policies. The pattern: draft → AI refine → compare → iterate.

## Instructions
1. **Draft Phase** — Write initial document manually or from template. This captures your intent and domain knowledge.
2. **AI Refinement Prompt**
   ```
   Review and improve this [document type]. Focus on:
   - Legal/technical precision of language
   - Structural completeness (missing sections?)
   - Internal consistency (contradictions?)
   - Ambiguity reduction (vague terms?)
   - Industry standard compliance
   
   Preserve the original intent. Output the improved version.
   ```
3. **Comparison** — Place original and AI version side by side. Use diff tools or ask AI to produce a tracked-changes summary listing every modification and why.
4. **Iteration** — Review AI suggestions. Accept, reject, or modify each. Feed modified version back for another pass. Usually 2-3 rounds is optimal.
5. **Template Creation** — Once a document is refined, extract the structure as a reusable template with placeholders. Future documents start from the template, not scratch.
6. **Quality Checks**
   - Does the AI version still reflect YOUR business intent?
   - Are industry-specific terms used correctly?
   - Would a lawyer/expert approve the language?
   - Is the tone appropriate for the audience?
7. **Document Types This Works For** — Contracts/MSAs, SOWs, proposals, investor decks, technical documentation, policies, employee handbooks, compliance documents, client reports
