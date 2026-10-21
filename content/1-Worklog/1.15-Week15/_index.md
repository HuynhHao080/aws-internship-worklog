---
title: "Week 15 Worklog"
date: 2026-10-11
weight: 15
chapter: false
pre: " <b> 1.15. </b> "
---

### Week 15 Objectives:

* Implement VPC and networking foundation for IoT Platform.
* Create CloudFormation stacks for VPC, subnets, security groups, and VPC endpoints.
* Set up cross-account roles for CI/CD pipeline.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create VPC CloudFormation stack: VPC, 2 public subnets, 2 private subnets across 2 AZs <br> - Configure route tables, Internet Gateway, NAT Gateways | 05/10/2026 | 05/10/2026 | <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html> |
| Tue | - Add VPC Endpoints (Gateway) for S3 and DynamoDB <br> - Configure Security Groups for Lambda, IoT Core, and ALB | 06/10/2026 | 06/10/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html> |
| Wed | - Create IAM roles for Lambda execution, IoT Core rule actions, Kinesis access <br> - Apply least-privilege policies with condition keys | 07/10/2026 | 07/10/2026 | <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html> |
| Thu | - Set up CloudFormation nested stacks for modular infrastructure <br> - Add parameters for environment (dev/staging/prod) | 08/10/2026 | 08/10/2026 | <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-nested-stacks.html> |
| Fri | - Deploy infrastructure to dev account via CloudFormation <br> - Validate connectivity: Lambda → DynamoDB, Lambda → S3 via VPC endpoints <br> - Document network architecture diagram | 09/10/2026 | 09/10/2026 | |

### Week 15 Achievements:

* **PLANNED**: Deploy VPC with public/private subnets across 2 AZs.
* **PLANNED**: Configure VPC Gateway Endpoints for S3 and DynamoDB (no NAT traversal).
* **PLANNED**: Create least-privilege IAM roles for all service components.
* **PLANNED**: Implement nested CloudFormation stacks with environment parameters.
* **TODO**: Run network reachability tests and document any issues.