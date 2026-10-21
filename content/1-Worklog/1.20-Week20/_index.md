---
title: "Week 20 Worklog"
date: 2026-11-15
weight: 20
chapter: false
pre: " <b> 1.20. </b> "
---

### Week 20 Objectives:

* Implement comprehensive monitoring with CloudWatch dashboards and alarms.
* Set up distributed tracing with AWS X-Ray for Lambda functions.
* Create operational runbooks for common incidents.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create CloudWatch Dashboard: IoT Core metrics, Kinesis IncomingRecords, Lambda Duration/Errors/Throttles, DynamoDB ConsumedCapacity | 09/11/2026 | 09/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html> |
| Tue | - Configure CloudWatch Alarms: Lambda Errors > 5/min, Kinesis IteratorAge > 300s, DynamoDB ThrottledRequests > 0 <br> - Set up SNS topics for alarm notifications | 10/11/2026 | 10/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html> |
| Wed | - Enable X-Ray tracing on all Lambda functions <br> - Configure sampling rules and annotate traces with device_id <br> - Analyze service map for bottleneck identification | 11/11/2026 | 11/11/2026 | <https://docs.aws.amazon.com/xray/latest/devguide/xray-getting-started.html> |
| Thu | - Create CloudWatch Logs Insights queries for: error patterns, cold start duration, data latency <br> - Export logs to S3 for long-term retention | 12/11/2026 | 12/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html> |
| Fri | - Write runbooks: "High Lambda Error Rate", "Kinesis Processing Lag", "DynamoDB Throttling" <br> - Conduct incident response tabletop exercise with mentor <br> - Document escalation paths and on-call contacts | 13/11/2026 | 13/11/2026 | |

### Week 20 Achievements:

* **PLANNED**: Build operational dashboard with 15+ key metrics.
* **PLANNED**: Configure 8 critical alarms with SNS notification.
* **PLANNED**: Enable X-Ray tracing across all Lambda functions.
* **PLANNED**: Create Logs Insights queries for rapid debugging.
* **PLANNED**: Document 3 incident runbooks with mitigation steps.
* **TODO**: Implement Synthetic Canaries for API health checks.