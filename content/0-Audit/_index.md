---
title: "Policy Assistant Evidence Reconciliation Audit"
date: 2026-10-16
weight: 0
chapter: false
pre: " <b> 0. </b> "
---

# Policy Assistant — Evidence Reconciliation Audit

This document reconciles the 25-week internship Worklog against the actual contents of the `policy-assistant` repository. It is an **audit-only** document: no files were modified, deployed, committed, or pushed during this review.

## Verified Repository Evidence

The following facts were confirmed by direct inspection of repository code, configuration, and test execution:

| Item | Verified Value | Source File |
|------|---------------|-------------|
| Project | Policy Assistant MVP (serverless policy retrieval and keyword search) | `README.md`, `template.yaml` |
| Region | `ap-southeast-1` | `samconfig.toml`, `template.yaml` |
| Stack name | `policy-assistant` | `samconfig.toml` |
| Lambda runtime | Python 3.12, x86_64 | `template.yaml` Globals |
| Lambda timeout | 10 seconds | `template.yaml` Globals |
| Lambda handler | `app.lambda_handler` in `policy_assistant/` | `template.yaml` |
| API Gateway type | HTTP API (v2), Stage: Prod | `template.yaml` |
| Endpoint 1 | `GET /policies/{policy_id}` | `template.yaml`, `policy_assistant/app.py` |
| Endpoint 2 | `GET /policies?keyword=xxx` | `template.yaml`, `policy_assistant/app.py` |
| DynamoDB table | `PolicyAssistantPolicies` | `template.yaml` |
| Partition key | `policy_id` (String, HASH) | `template.yaml` |
| Billing mode | PAY_PER_REQUEST | `template.yaml` |
| GSIs / LSIs | None | `template.yaml` comment |
| IAM permissions | `dynamodb:GetItem`, `dynamodb:Scan` on table ARN | `template.yaml` |
| CloudWatch Logs | Log group `/aws/lambda/PolicyAssistantFunction-...`, 30-day retention, JSON format | `template.yaml` |
| CORS origins | `http://localhost:3000`, `http://127.0.0.1:3000` | `template.yaml` |
| Sample data | 3 policies (POL-001, POL-002, POL-003) | `sample_data/policies.json` |
| Unit tests | 21 tests, all PASSED (mocked DynamoDB via moto) | `tests/unit/test_handler.py` |
| Integration tests | Exist but require deployed stack (skipped without `AWS_SAM_STACK_NAME`) | `tests/integration/test_api_gateway.py` |
| SAM CLI validation | Not available (`sam` command not found in environment) | Terminal check |
| Git repository | Not initialized (no `.git` directory) | Terminal check |

## What Is NOT in the Repository

The following were **searched for and not found** in any project source file (`policy_assistant/app.py`, `template.yaml`, `samconfig.toml`, test files, or configuration):

- Amazon Bedrock
- Amazon AgentCore
- Embeddings / vector store
- Knowledge retrieval / RAG
- Amazon Cognito
- AWS Secrets Manager
- AWS KMS
- Amazon S3
- Amazon Kinesis
- AWS IoT Core
- Amazon OpenSearch
- AWS X-Ray
- CloudWatch Alarms
- CI/CD pipeline (GitHub Actions, CodePipeline, CodeBuild)
- Frontend files (HTML/CSS/JS)
- `docs/` directory (referenced in README but does not exist)

## Worklog Evidence Reconciliation Table

| Week | Existing Worklog Summary | Evidence Found in Repository | Status | Recommended Action |
|------|-------------------------|------------------------------|--------|-------------------|
| 1 | AWS account onboarding, MFA, Free Tier setup | No repository code; learning activity only | NEEDS EVIDENCE | Preserve as AWS learning activity; not Policy Assistant development |
| 2 | Cloud computing fundamentals, AWS global infrastructure | No repository code; learning activity only | NEEDS EVIDENCE | Preserve as AWS learning activity |
| 3 | IAM users, groups, roles, policies; least privilege | IAM role exists in `template.yaml` but only for Lambda execution; no evidence of IAM learning artifacts | PARTIALLY VERIFIED | Preserve as AWS learning activity; IAM role in template is for Lambda, not user management |
| 4 | EC2 fundamentals, AMIs, Security Groups, EBS | No EC2 resources in repository | NEEDS EVIDENCE | Preserve as AWS learning activity; not in Policy Assistant scope |
| 5 | S3 concepts, AWS CLI setup | No S3 resources in repository | NEEDS EVIDENCE | Preserve as AWS learning activity; not in Policy Assistant scope |
| 6 | VPC, subnets, routing, IGW, NAT, Security Groups vs NACLs | No VPC resources in repository | NEEDS EVIDENCE | Preserve as AWS learning activity; not in Policy Assistant scope |
| 7 | RDS and DynamoDB fundamentals | DynamoDB table exists in `template.yaml` but no evidence it was created during Week 7 | PARTIALLY VERIFIED | DynamoDB exists but cannot be linked to Week 7 from repository evidence alone |
| 8 | Lambda fundamentals, create and test a Lambda function | `policy_assistant/app.py` exists with Lambda handler; 21 unit tests pass | PARTIALLY VERIFIED | Lambda function exists but cannot be directly linked to Week 8 from repository evidence alone |
| 9 | CloudWatch, CloudTrail, Budgets | CloudWatch Logs log group exists in `template.yaml` (30-day retention, JSON); no alarms, dashboards, or CloudTrail | PARTIALLY VERIFIED | Logging exists; monitoring/alarms not implemented |
| 10 | SNS, SQS; CloudThinker Prove It Day 01 workshop | No SNS/SQS resources in repository | NEEDS EVIDENCE | Workshop participation is external; not Policy Assistant development |
| 11 | API Gateway fundamentals, API protection; CloudThinker Prove It Day 02 | API Gateway HTTP API exists in `template.yaml`; no API keys, throttling, or authorizers configured | PARTIALLY VERIFIED | API Gateway exists but security features not implemented |
| 12 | Containers, ECR, ECS, Fargate; CloudThinker Prove It Day 03 | No container resources in repository | NEEDS EVIDENCE | Workshop participation is external; not Policy Assistant development |
| 13 | Infrastructure as Code, CloudFormation fundamentals | `template.yaml` is a SAM/CloudFormation template; `samconfig.toml` exists | PARTIALLY VERIFIED | IaC exists but cannot be linked to Week 13 from repository evidence alone |
| 14 | **IoT Weather Platform kickoff**: IoT Core → Kinesis → Lambda → DynamoDB/S3 → CloudFront | No IoT Core, Kinesis, CloudFront, or S3 resources in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. Repository contains no evidence of this work. |
| 15 | **IoT Weather Platform**: VPC, CloudFormation nested stacks, cross-account roles | No VPC, nested stacks, or cross-account roles in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 16 | **IoT Weather Platform**: Lambda ingestion, API Gateway REST API, DynamoDB tables | Repository has 1 Lambda function and HTTP API (not REST API); no IoT ingestion pipeline | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 17 | **IoT Weather Platform**: CloudFront, Lambda@Edge, WAF | No CloudFront, Lambda@Edge, or WAF resources in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 18 | **IoT Weather Platform**: AWS IoT Core, thing groups, IoT rules, device shadow | No IoT Core resources in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 19 | **IoT Weather Platform**: Firehose, S3, Glue, Athena, QuickSight | No Firehose, S3, Glue, Athena, or QuickSight resources in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 20 | **IoT Weather Platform**: CloudWatch dashboard, X-Ray, runbooks | No X-Ray, dashboards, or runbooks in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 21 | **IoT Weather Platform**: IAM hardening, KMS, Secrets Manager, Security Hub | No KMS, Secrets Manager, or Security Hub in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 22 | **IoT Weather Platform**: CodePipeline, CodeBuild, CodeDeploy, CI/CD | No CI/CD pipeline files in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 23 | **IoT Weather Platform**: Load testing, cost optimization, Compute Optimizer | No load testing or cost optimization artifacts in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 24 | **IoT Weather Platform**: ADRs, OpenAPI spec, runbooks, knowledge transfer | No ADRs, OpenAPI spec, or runbooks in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |
| 25 | **IoT Weather Platform**: Final presentation, retrospective, internship closure | No presentation, retrospective, or closure artifacts in repository | POSSIBLE PROJECT MISMATCH | Worklog describes IoT Weather Platform, not Policy Assistant. |

## Key Findings

1. **Weeks 1–13** describe general AWS learning activities. Some services learned (DynamoDB, Lambda, API Gateway, CloudWatch Logs, IAM, CloudFormation) appear in the repository, but there is no dated commit or build evidence linking them to specific weeks.

2. **Weeks 14–25** describe the **IoT Weather Platform** project, not Policy Assistant. The `policy-assistant` repository contains no IoT Core, Kinesis, CloudFront, Lambda@Edge, S3, Glue, Athena, QuickSight, X-Ray, KMS, Secrets Manager, or CI/CD resources. These worklog entries cannot be verified against the Policy Assistant repository.

3. **The README references a `docs/` directory** (`docs/PROJECT_PROPOSAL.md`, `docs/ARCHITECTURE.md`, etc.) but this directory does not exist in the repository.

4. **The README references frontend files** (HTML/CSS/JS served locally) but no frontend files exist in the repository.

5. **The README claims 12 deployed API test cases passed** against `https://ctgt5w6emf.execute-api.ap-southeast-1.amazonaws.com/Prod/`. This is a documentation claim; no dated deployment record, test output, or CI evidence exists in the repository to verify it.

6. **The README claims 21 unit tests pass.** This was verified by direct test execution: all 21 tests pass.

7. **No git repository exists** in the project directory, so commit history cannot be inspected to verify when features were implemented.

## Recommendations

- Preserve Weeks 1–13 as AWS learning activities; do not rewrite them as Policy Assistant development.
- Preserve Weeks 14–25 as IoT Weather Platform activities; do not rename them as Policy Assistant work.
- Create a separate, clearly labeled **proposed 25-week Policy Assistant roadmap** in the proposal document (Section 5).
- Create the missing `docs/` directory and documentation files referenced by the README.
- Verify the deployed API test claims with actual deployment records if they exist outside this repository.
