---
title: "Week 23 Worklog"
date: 2026-12-06
weight: 23
chapter: false
pre: " <b> 1.23. </b> "
---

### Week 23 Objectives:

* Conduct performance testing and optimization.
* Implement cost analysis and optimization strategies.
* Fine-tune DynamoDB, Lambda, and Kinesis for production scale.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Run load test: 500 devices × 1 msg/30s = 1000 msg/min sustained <br> - Measure end-to-end latency (p50, p95, p99), error rate, throughput | 30/11/2026 | 30/11/2026 | <https://docs.aws.amazon.com/whitepapers/latest/performance-efficiency-pillar/performance-efficiency-pillar.html> |
| Tue | - Optimize Lambda: right-size memory (128→512 MB), enable provisioned concurrency for API <br> - Tune Kinesis: increase shards, enable enhanced fan-out | 01/12/2026 | 01/12/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html> |
| Wed | - Optimize DynamoDB: enable on-demand → provisioned with autoscaling <br> - Add GSI for query patterns: device_id + time range, location + time | 02/12/2026 | 02/12/2026 | <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html> |
| Thu | - Run AWS Compute Optimizer and Cost Explorer analysis <br> - Identify idle resources, right-size recommendations, Savings Plans eligibility | 03/12/2026 | 03/12/2026 | <https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html> |
| Fri | - Implement cost allocation tags: Project=IoTWeather, Environment=prod <br> - Create Cost Anomaly Detection monitor <br> - Document optimization results: before/after cost, performance metrics | 04/12/2026 | 04/12/2026 | <https://docs.aws.amazon.com/cost-management/latest/userguide/cost-anomaly-detection.html> |

### Week 23 Achievements:

* **PLANNED**: Validate system handles 1000 msg/min with p99 latency < 2s.
* **PLANNED**: Reduce Lambda cold starts by 90% with provisioned concurrency.
* **PLANNED**: Optimize DynamoDB: 40% cost reduction with provisioned + autoscaling.
* **PLANNED**: Identify $180/month savings via Compute Optimizer recommendations.
* **PLANNED**: Implement cost allocation tags and anomaly detection.
* **TODO**: Run 24-hour soak test before production cutover.