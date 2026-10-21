---
title: "Week 19 Worklog"
date: 2026-11-08
weight: 19
chapter: false
pre: " <b> 1.19. </b> "
---

### Week 19 Objectives:

* Build data lake pipeline: Kinesis Data Firehose → S3 (Parquet) → Athena.
* Implement historical data queries and aggregation views.
* Set up data partitioning strategy for cost-effective analytics.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create Kinesis Data Firehose delivery stream: Kinesis → S3 <br> - Configure Parquet conversion, GZIP compression, 60s/64MB buffering | 02/11/2026 | 02/11/2026 | <https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html> |
| Tue | - Design S3 partitioning: s3://weather-data/year=YYYY/month=MM/day=DD/hour=HH/ <br> - Enable S3 Object Lock for compliance (optional) | 03/11/2026 | 03/11/2026 | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html> |
| Wed | - Create AWS Glue Database and Crawler for Firehose data <br> - Define Athena tables with partition projection for auto-discovery | 04/11/2026 | 04/11/2026 | <https://docs.aws.amazon.com/athena/latest/ug/partition-projection.html> |
| Thu | - Build Athena views: daily_avg, hourly_max, device_health_summary <br> - Test queries: 30-day trends, anomaly detection (temp > 40°C) | 05/11/2026 | 05/11/2026 | <https://docs.aws.amazon.com/athena/latest/ug/views.html> |
| Fri | - Integrate Athena with QuickSight for BI dashboard <br> - Create SPICE dataset with 1-hour refresh schedule <br> - Document data lake architecture and query patterns | 06/11/2026 | 06/11/2026 | <https://docs.aws.amazon.com/quicksight/latest/user/working-with-data-sources.html> |

### Week 19 Achievements:

* **PLANNED**: Deploy Firehose delivery stream with Parquet output to S3.
* **PLANNED**: Implement Hive-style partitioning for time-series optimization.
* **PLANNED**: Create Glue Catalog tables with Athena partition projection.
* **PLANNED**: Build analytical views for daily/hourly aggregations.
* **PLANNED**: Connect QuickSight for business intelligence visualization.
* **TODO**: Implement data lifecycle policy (transition to IA after 30 days, Glacier after 365).