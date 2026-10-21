---
title: "Week 14 Worklog"
date: 2026-10-04
weight: 14
chapter: false
pre: " <b> 1.14. </b> "
---

### Week 14 Objectives:

* Kick off the IoT Weather Platform project.
* Design the overall serverless architecture for real-time weather monitoring.
* Define project scope, milestones, and deliverables.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Project kickoff meeting with mentor <br> - Review IoT Weather Platform requirements: edge devices, real-time ingestion, dashboard, alerting | 28/09/2026 | 28/09/2026 | Internal project brief |
| Tue | - Design high-level architecture: IoT Core → Kinesis → Lambda → DynamoDB/S3 → CloudFront <br> - Identify AWS services for each component | 29/09/2026 | 29/09/2026 | <https://aws.amazon.com/iot-core/> |
| Wed | - Define data model for weather telemetry (temperature, humidity, pressure, wind) <br> - Design DynamoDB table schema with partition/sort keys | 30/09/2026 | 30/09/2026 | <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html> |
| Thu | - Plan VPC networking: public/private subnets, VPC endpoints for S3/DynamoDB <br> - Create CloudFormation template skeleton for infrastructure | 01/10/2026 | 01/10/2026 | <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html> |
| Fri | - Document architecture decision records (ADRs) <br> - Set up Git repository with branch strategy <br> - Weekly sync with mentor | 02/10/2026 | 02/10/2026 | |

### Week 14 Achievements:

* **PLANNED**: Define project scope: IoT Weather Platform for lab research with real-time dashboard.
* **PLANNED**: Design serverless architecture using IoT Core, Kinesis, Lambda, DynamoDB, S3, CloudFront.
* **PLANNED**: Create initial CloudFormation template structure.
* **PLANNED**: Establish Git workflow and documentation standards.
* **TODO**: Validate architecture with mentor and finalize technology choices.