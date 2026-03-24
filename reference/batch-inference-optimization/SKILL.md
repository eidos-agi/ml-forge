---
name: batch-inference-optimization
description: Optimize ML inference costs through batch processing, scheduling, and resource management. Target 50-75% cost reduction by running inference off-peak vs real-time. Use when running ML models at scale where latency is not critical.
---
# Batch Inference Cost Optimization
## When to Use This Skill
Use when running ML inference at scale and results don't need to be instant. Batch = cheaper. Apply to: daily portfolio scoring, nightly report generation, weekly model retraining, monthly satellite analysis.

## Instructions
1. **Identify Batch Candidates** — Any ML task where results can wait >1 hour: daily scoring, nightly ETL, weekly reports, monthly retraining
2. **Queue Architecture**
   - Input: S3 bucket or SQS queue with items to process
   - Compute: AWS Batch (auto-scales, pays only for usage) or Lambda (for <15 min tasks)
   - Output: results written to S3/database
   - Notification: SNS/email when complete
3. **Scheduling**
   - Run at off-peak hours (midnight-6am) for lowest spot instance prices
   - AWS Batch with spot instances: 60-90% cheaper than on-demand
   - Cron: `0 0 * * *` for daily midnight, `0 0 * * 0` for weekly Sunday midnight
4. **Batching Strategy**
   - Group items into batches of 100-1000 (reduces overhead per item)
   - Load model ONCE, process entire batch, then unload
   - Amortize model loading time across all items in batch
5. **Error Handling** — Failed items go to dead letter queue. Retry up to 3x. Log failures with input data for debugging. Never block entire batch for one failure.
6. **Cost Tracking** — Log compute time, cost per item, total batch cost. Compare vs real-time estimate. Target: >50% savings or don't bother batching.
7. **Monitoring** — Track batch completion time, failure rate, cost per batch. Alert if batch takes >2x expected time or failure rate >5%.
