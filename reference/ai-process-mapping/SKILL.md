---
name: ai-process-mapping
description: Map business processes to identify highest-ROI AI insertion points. Covers software development, customer service, document processing, data analysis, and finance automation. Use when assessing a company or department for AI implementation opportunities.
---
# AI Business Process Mapping
## When to Use This Skill
Use when evaluating where AI can be inserted into a business for maximum ROI. Whether for your own operations, a client engagement, or a due diligence assessment.

## Instructions
1. **Process Inventory** — List all business processes by department. For each: what is it, who does it, how long does it take, how often, what data does it consume/produce?
2. **AI Opportunity Scoring** — For each process, rate (1-5):
   - Repetitiveness: is it the same pattern each time?
   - Data availability: is training data accessible?
   - Error cost: how expensive are mistakes?
   - Volume: how many times per day/week/month?
   - Current bottleneck: is this slowing down the business?
   - AI_ROI_Score = (Repetitiveness + Volume) × Data_Availability / Error_Cost
3. **AI Application Catalog by Function**
   - **Software/Engineering:** Code generation, completion, review, testing, debugging, CI/CD automation
   - **Customer Service:** Chatbots, personalized recommendations, IVR, ticket routing
   - **Document Processing:** Classification, extraction, summarization, OCR, knowledge mining
   - **Data & Finance:** Formula generation, pattern identification, anomaly detection, forecasting
   - **HR/Operations:** Resume screening, scheduling, onboarding automation
   - **Sales/Marketing:** Lead scoring, content generation, campaign optimization
4. **Prioritization Matrix** — Plot processes on 2x2: Effort (low/high) vs Impact (low/high). Start with low-effort/high-impact quadrant.
5. **Implementation Roadmap** — Phase 1 (0-30 days): Quick wins using existing LLM APIs. Phase 2 (30-90 days): Custom pipelines for medium-complexity tasks. Phase 3 (90-180 days): ML models for high-complexity, high-value tasks.
6. **ROI Measurement** — Before: hours/cost per task. After: hours/cost per task. Delta × volume = annual savings. Track and report monthly.
