---
title: "Week 21 Worklog"
date: 2026-11-22
weight: 21
chapter: false
pre: " <b> 1.21. </b> "
---

### Week 21 Objectives:

* Harden security: implement least-privilege IAM, encryption at rest/in transit.
* Set up AWS Secrets Manager for credential rotation.
* Configure AWS KMS for envelope encryption of sensitive data.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Audit all IAM roles/policies with IAM Access Analyzer <br> - Remove unused permissions, apply condition keys (aws:SourceArn, aws:SourceAccount) | 16/11/2026 | 16/11/2026 | <https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer.html> |
| Tue | - Create KMS Customer Managed Keys for: DynamoDB, S3, Firehose, CloudWatch Logs <br> - Enable automatic key rotation, define key policies | 17/11/2026 | 17/11/2026 | <https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html> |
| Wed | - Migrate hardcoded secrets to AWS Secrets Manager <br> - Configure automatic rotation for DB credentials, API keys (30-day) | 18/11/2026 | 18/11/2026 | <https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html> |
| Thu | - Enforce TLS 1.2+ on all endpoints: API Gateway, CloudFront, IoT Core <br> - Implement VPC Flow Logs to CloudWatch for network monitoring | 19/11/2026 | 19/11/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html> |
| Fri | - Run AWS Security Hub foundational security best practices check <br> - Remediate HIGH/CRITICAL findings <br> - Document security posture and compliance evidence | 20/11/2026 | 20/11/2026 | <https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-standards.html> |

### Week 21 Achievements:

* **PLANNED**: Reduce IAM policy permissions by ~40% using Access Analyzer.
* **PLANNED**: Enable encryption at rest with CMK for all data stores.
* **PLANNED**: Migrate 12 secrets to Secrets Manager with auto-rotation.
* **PLANNED**: Enforce TLS 1.2 minimum on all public endpoints.
* **PLANNED**: Improve Security Hub score from 65% to 92%.
* **TODO**: Implement GuardDuty for threat detection and Config rules for drift detection.