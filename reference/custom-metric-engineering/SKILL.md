---
name: custom-metric-engineering
description: Design domain-specific composite metrics from available data, calibrated against authoritative sources. Use when existing metrics don't capture what you need to measure and you need to design and validate a new one.
---
# Custom Metric Engineering
## When to Use This Skill
Use when standard metrics don't exist for what you're trying to measure. You need to invent a metric that is: meaningful, reproducible, calibrated against reality, and defensible.

## Instructions
1. **Define What You're Measuring** — State in one sentence what this metric captures. "AI Competition Intensity measures how directly AI threatens a specific occupation's core tasks."
2. **Identify Component Variables** — Break the concept into measurable components. Each should be independently observable.
   - Example: AI exposure score (how much of the job CAN be automated) × task automatable share (what PORTION is automatable)
3. **Design the Formula**
   - Multiplication (×): components amplify each other (both must be present)
   - Addition (+): components are independent contributions
   - Ratio (/): one component normalized by another
   - Keep it simple. If you can't explain the formula in 30 seconds, simplify.
4. **Set Thresholds** — Define what high/medium/low means. Use empirical distribution: top 25% = high, middle 50% = medium, bottom 25% = low. Or use domain knowledge to set meaningful cutpoints.
5. **Calibrate Against Ground Truth** — Find authoritative sources that measure related phenomena. Does your metric correlate with their findings? If your "AI risk" metric doesn't flag occupations that experts say are at risk, it's broken.
6. **Sensitivity Analysis** — Change each component by ±20%. Does the metric's ranking of entities change dramatically? If yes, it's fragile. If ranking is stable, metric is robust.
7. **Documentation** — For each metric: name, formula, component definitions, data sources, threshold definitions, calibration sources, limitations, update frequency.
