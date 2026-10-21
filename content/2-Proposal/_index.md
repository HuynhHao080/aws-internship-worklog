---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, you need to summarize the contents of the workshop that you **plan** to conduct.

# Policy Assistant
## Serverless Policy Retrieval and AI-Assisted Question Answering

### 1. Executive Summary / Tóm tắt điều hành

Policy Assistant is a serverless Minimum Viable Product (MVP) for retrieving and searching organizational policies. It is built with AWS SAM, Python 3.12, Amazon API Gateway HTTP API (v2), AWS Lambda, and Amazon DynamoDB, deployed in the `ap-southeast-1` region.

The current MVP exposes two endpoints:
- `GET /policies/{policy_id}` — retrieve a policy by exact ID.
- `GET /policies?keyword=xxx` — case-insensitive keyword search across title, category, content, and keywords.

The proposed evolution of this project is to add grounded AI-assisted question answering, allowing users to ask natural-language questions about policies and receive answers with source attribution. **Amazon Bedrock, RAG (Retrieval-Augmented Generation), embeddings, and vector search are NOT currently implemented.** They are evaluated as planned future enhancements only.

This proposal describes the current verified architecture, the planned target architecture, a 25-week development roadmap, budget assumptions, risk assessment, and expected outcomes. All claims about the current implementation are supported by repository code, configuration, or executed test results.

### 2. Problem Statement / Phát biểu vấn đề

#### Current retrieval workflow

Users interact with Policy Assistant through two API operations:

1. **Exact-ID lookup** — `GET /policies/{policy_id}` performs a DynamoDB `GetItem` using the partition key `policy_id`. This returns a single policy document if it exists, or a 404 response.

2. **Keyword search** — `GET /policies?keyword=xxx` performs a DynamoDB `Scan` with a `FilterExpression` across four lowercase fields (`title_lower`, `category_lower`, `content_lower`, `keywords_lower`). Pagination is handled via `LastEvaluatedKey`.

#### Verified limitations

The following limitations are supported by code inspection:

- **Scan-based search** — The keyword search reads the entire table (up to 1 MB per request). This is suitable for a small MVP dataset (currently 3 sample policies) but does not scale to large policy repositories. (`policy_assistant/app.py`, lines 128–164; `template.yaml` comment on line 38–39)
- **No semantic search** — Keyword matching uses `contains()` on lowercase fields. It cannot handle synonyms, related concepts, or natural-language questions. (`policy_assistant/app.py`, lines 132–135)
- **No authentication or authorization** — The API is publicly accessible. CORS is limited to `localhost:3000` and `127.0.0.1:3000`. No API keys, Cognito user pool, or Lambda authorizer is configured. (`template.yaml`, lines 72–82)
- **No source attribution for AI answers** — There is no AI component in the current implementation, so no citation or reference mechanism exists. (No Bedrock, RAG, or vector store code found in the repository)
- **No monitoring or alarms** — CloudWatch Logs is configured with 30-day retention, but no CloudWatch Alarms, dashboards, or X-Ray tracing are present. (`template.yaml`, lines 21–25)
- **No CI/CD pipeline** — Deployment is via `sam deploy --guided` (manual). No CodePipeline, CodeBuild, or GitHub Actions workflow exists. (`samconfig.toml`; no CI/CD files in repository)

#### Assumptions requiring evaluation

The following are assumptions, not verified facts:
- The volume of policies will grow beyond the MVP scale where Scan remains acceptable.
- Users will need natural-language question answering rather than exact-ID or keyword lookup.
- Source citations will be required for AI-generated policy answers.

These assumptions should be validated with stakeholders before committing to the RAG enhancement path.

### 3. Solution Architecture / Kiến trúc giải pháp

#### Current architecture (verified)

The current architecture consists of verified resources defined in `template.yaml` and implemented in `policy_assistant/app.py`:

```
[Client Applications]
    Web Frontend (localhost:3000)
    curl / API Client
         |
         | HTTPS GET
         v
[AWS Cloud (ap-southeast-1)]
    API Gateway HTTP API
    (PolicyAssistantApi, Stage: Prod)
         |
         | Invoke AWS_PROXY
         v
    Lambda Function
    (PolicyAssistantFunction, Python 3.12, 10 s timeout)
         |
         | GetItem / Scan
         v
    DynamoDB Table
    (PolicyAssistantPolicies, PK: policy_id, PAY_PER_REQUEST)
         |
         | Logs JSON
         v
    CloudWatch Logs
    (/aws/lambda/PolicyAssistantFunction-..., 30-day retention)
         ^
         | Assumes / Allows
    IAM Role
    (PolicyAssistantFunctionRole: dynamodb:GetItem, dynamodb:Scan)
```

**Legend:** All components above are **Current** (implemented and verified in the repository).

#### Proposed target architecture (planned enhancements)

The target architecture adds planned components for AI-assisted question answering. Planned components are **not** implemented and are marked accordingly:

```
[Client Applications]
    Web Frontend (planned)
    curl / API Client
         |
         | HTTPS GET
         v
[AWS Cloud (ap-southeast-1)]
    API Gateway HTTP API
    (PolicyAssistantApi, Stage: Prod)
         |
         | Invoke AWS_PROXY
         v
    Lambda Function
    (PolicyAssistantFunction, Python 3.12)
         |
         | GetItem / Scan (current)
         | Invoke Bedrock (planned)
         v
    DynamoDB Table
    (PolicyAssistantPolicies, PK: policy_id)
         |
         | Vector search / retrieval (planned)
         v
    [Planned: Amazon Bedrock Knowledge Base]
    [Planned: Embeddings model]
    [Planned: Vector index]
         |
         | Logs JSON
         v
    CloudWatch Logs
         ^
         | Assumes / Allows
    IAM Role
    (Least-privilege; planned: add Bedrock, Cognito permissions)
```

**Legend:**
- **Current** — implemented and verified in the repository.
- **Planned** — proposed future enhancement, not implemented.

#### AWS services used (current vs planned)

| Service | Current | Planned | Evidence |
|---------|---------|---------|----------|
| AWS Lambda | ✅ Implemented | — | `template.yaml`, `policy_assistant/app.py` |
| Amazon API Gateway HTTP API | ✅ Implemented | — | `template.yaml` |
| Amazon DynamoDB | ✅ Implemented | — | `template.yaml`, `sample_data/policies.json` |
| Amazon CloudWatch Logs | ✅ Implemented | — | `template.yaml` |
| AWS IAM | ✅ Implemented (Lambda role only) | Planned (Cognito, least-privilege expansion) | `template.yaml` |
| Amazon Bedrock | — | Planned (RAG, question answering) | Not found in repository |
| Amazon Cognito | — | Planned (authentication/authorization) | Not found in repository |
| AWS Secrets Manager | — | Planned (credential management) | Not found in repository |
| AWS KMS | — | Planned (encryption at rest) | Not found in repository |
| Amazon OpenSearch | — | Planned (alternative to Scan for search) | Not found in repository |

### 4. Technical Implementation / Triển khai kỹ thuật

#### Current implementation (verified)

**Endpoints:**

| Endpoint | Method | Description | Status |
|----------|--------|-------------|--------|
| `/policies/{policy_id}` | GET | Retrieve a policy by exact ID using DynamoDB `GetItem` | IMPLEMENTED — VERIFIED |
| `/policies` | GET | Case-insensitive keyword search using DynamoDB `Scan` | IMPLEMENTED — VERIFIED |

**Data model (`sample_data/policies.json`):**

Each policy item contains:
- `policy_id` (String, partition key)
- `title`, `category`, `content` (String)
- `keywords` (List of String)
- `title_lower`, `category_lower`, `content_lower`, `keywords_lower` (lowercase copies for case-insensitive search)

Three sample policies are included: POL-001 (Customer Refund Policy), POL-002 (Privacy Policy), POL-003 (Terms of Service).

**Lambda behavior (`policy_assistant/app.py`):**

- Reads `POLICIES_TABLE_NAME` from environment variables (defaults to `PolicyAssistantPolicies`).
- Strips API Gateway stage prefix from the request path for routing.
- Returns `405 Method Not Allowed` for non-GET methods.
- Returns `400 Bad Request` for missing or empty `policy_id` or `keyword`.
- Returns `404 Not Found` for missing policies or invalid endpoints.
- Returns `500 Internal Server Error` for DynamoDB `ClientError` or unexpected exceptions.
- All responses include CORS headers (`Access-Control-Allow-Origin: http://localhost:3000`).
- Logging is JSON-formatted via CloudWatch Logs.

**Testing assets:**

- `tests/unit/test_handler.py` — 21 unit tests using `moto` to mock DynamoDB. **All 21 tests pass** (verified by execution).
- `tests/integration/test_api_gateway.py` — Integration tests requiring a deployed stack (skipped without `AWS_SAM_STACK_NAME`).
- `test_event.json` — Sample API Gateway HTTP API v2 event for local invocation.

**Infrastructure configuration:**

- `template.yaml` — AWS SAM template defining Lambda, API Gateway HTTP API, DynamoDB table, CloudWatch Logs group, and IAM role.
- `samconfig.toml` — SAM CLI configuration (stack name: `policy-assistant`, region: `ap-southeast-1`, capabilities: `CAPABILITY_IAM`).

#### Planned enhancements (not implemented)

The following enhancements are **planned** and are not part of the current implementation:

1. **API validation and OpenAPI documentation** — Add request validation at API Gateway level; publish OpenAPI 3.0 specification.
2. **Search optimization and retrieval evaluation** — Replace or supplement DynamoDB Scan with GSI or OpenSearch; evaluate retrieval relevance and recall.
3. **Security and least-privilege IAM** — Refine IAM policies; add resource-based policies where applicable.
4. **Authentication/authorization** — Add Amazon Cognito or API Gateway API keys / Lambda authorizer.
5. **Monitoring and alarms** — Add CloudWatch Alarms, dashboards, and optional X-Ray tracing.
6. **CI/CD and deployment verification** — Add GitHub Actions or AWS CodePipeline/CodeBuild for automated testing and deployment.
7. **Optional frontend implementation** — Add a web UI for policy search (currently referenced in README but no files exist).
8. **Bedrock/RAG feasibility and implementation** — Evaluate Amazon Bedrock for grounded question answering; implement source attribution, insufficient-evidence handling, and prompt-injection safeguards.
9. **AI evaluation** — Establish metrics for answer accuracy, citation quality, and refusal rate for unanswerable questions.

**Never mix planned enhancements with the current feature list.** The current implementation contains only the two GET endpoints, DynamoDB storage, CloudWatch Logs, and IAM execution role.

### 5. Timeline & Milestones / Tiến độ và các mốc

The following is a **proposed 25-week roadmap** for Policy Assistant development. It is **not** derived from historical Worklog entries. Weeks 1–13 of the existing Worklog describe general AWS learning; Weeks 14–25 describe IoT Weather Platform activities. Neither is Policy Assistant development history.

| Phase | Week Range | Planned Activities | Expected Deliverables | Verifiable Acceptance Criteria |
|-------|-----------|-------------------|----------------------|-------------------------------|
| 1. Requirements & Baseline Assessment | 1–2 | Review current repository; document verified features; identify gaps; stakeholder interviews | Requirements document; gap analysis | All current features listed with evidence references; gap analysis approved |
| 2. AWS Serverless Foundations | 3–5 | Review SAM template, IAM role, DynamoDB design; validate template syntax | Validated `template.yaml`; IAM least-privilege review | `sam validate` passes (when SAM CLI available); IAM permissions scoped to table ARN |
| 3. API & Data Implementation | 6–8 | Implement and unit-test both endpoints; add pagination; validate error responses | Updated `policy_assistant/app.py`; unit tests | 21+ unit tests pass; all error cases return correct status codes |
| 4. Testing & Test Evidence | 9–10 | Expand unit test coverage; create integration test fixtures; document test execution | Test reports; coverage metrics | Unit tests pass in CI-like environment; integration tests documented |
| 5. Infrastructure Documentation | 11–12 | Create `docs/` directory with ARCHITECTURE.md, TEST_REPORTING.md, CLOUDWATCH_OPERATIONS.md | Documentation files | README links resolve; diagrams match `template.yaml` resources |
| 6. Search Evaluation | 13–14 | Evaluate GSI vs OpenSearch vs Bedrock for search; document tradeoffs | Search evaluation report | Recommendation documented with cost and performance rationale |
| 7. Security & Observability | 15–16 | Add CloudWatch Alarms; evaluate Cognito/API keys; review IAM permissions | Alarms configured; security review | Alarms defined for error rate and latency; IAM permissions reviewed |
| 8. Deployment Automation | 17–18 | Add CI/CD pipeline (GitHub Actions or CodePipeline); document deployment | Pipeline configuration; deployment runbook | Pipeline runs unit tests on pull request; deployment documented |
| 9. RAG Feasibility | 19–20 | Evaluate Amazon Bedrock for grounded QA; prototype retrieval and answer generation | Feasibility report; optional prototype | Prototype answers questions with source citations; refusal behavior for unanswerable questions |
| 10. Optional RAG Prototype | 21–22 | Implement Bedrock Knowledge Base or equivalent; add prompt-injection safeguards | RAG prototype (if approved) | Prototype demonstrates grounded answers with citations |
| 11. Final Handover | 23–25 | Final documentation; knowledge transfer; release preparation | Release package; handover notes | All documentation complete; code reviewed; release tagged |

### 6. Budget Estimation / Ước tính ngân sách

#### Cost model approach

This section provides a **transparent cost model** rather than invented totals. Actual monthly costs depend on usage volumes that are not yet known.

#### Cost categories

| Category | Current MVP | Planned (Optional) |
|----------|-------------|-------------------|
| Amazon API Gateway HTTP API | Pay per request | — |
| AWS Lambda | Pay per request + GB-seconds | — |
| Amazon DynamoDB | On-demand (PAY_PER_REQUEST) | — |
| Amazon CloudWatch Logs | Pay per GB ingested/stored | — |
| Amazon Bedrock | — | Per model invocation / per token |
| Amazon Cognito | — | Monthly active users (MAU) |
| AWS KMS | — | Per key + per request |
| AWS Secrets Manager | — | Per secret + per API call |
| Amazon OpenSearch | — | Per instance-hour + storage |

#### Assumptions required for a monthly estimate

The following inputs are **unavailable** and must be provided before a numeric estimate can be calculated:

- Monthly request volume (API Gateway + Lambda invocations)
- Average Lambda execution duration and memory allocation
- DynamoDB table size and read/write capacity usage
- CloudWatch Logs ingestion volume and retention period
- Bedrock model invocations and token counts (if RAG is implemented)
- Cognito monthly active users (if authentication is added)
- Data transfer out volume
- AWS Support plan (if applicable)

#### Existing MVP baseline vs optional AI/RAG costs

- **Existing MVP baseline:** Lambda, API Gateway HTTP API, DynamoDB on-demand, CloudWatch Logs. These services have free tiers that may cover low-volume development usage. Actual cost depends on request volume.
- **Optional AI/RAG costs:** Amazon Bedrock model invocations, vector storage, and embedding generation. These are usage-dependent and cannot be estimated without token volume and model selection.

**No monthly cost figure is stated in this proposal** because the required usage inputs are not available. The estimate is pending collection of the assumptions listed above.

### 7. Risk Assessment / Đánh giá rủi ro

| Risk | Impact | Mitigation |
|------|--------|------------|
| Search performance and scalability | DynamoDB Scan reads entire table; 1 MB limit per request; pagination required | Evaluate GSI or OpenSearch for larger datasets; document Scan limitations |
| Incorrect or outdated policy content | DynamoDB is schemaless; no content validation or versioning | Add schema validation in Lambda; implement content review workflow |
| Unsupported AI answers | No AI component currently; future RAG may hallucinate | Implement source attribution; refusal behavior for unanswerable questions; human review |
| Sensitive data exposure and authorization gaps | API is public; no authentication configured | Add Cognito or API keys; least-privilege IAM; encryption at rest evaluation |
| Prompt injection | Not applicable to current MVP; relevant if RAG is added | Input sanitization; system prompt guardrails; output validation |
| Uncontrolled AI costs | Not applicable to current MVP; relevant if Bedrock is added | Budget alerts; usage monitoring; model selection based on cost/performance |
| Deployment failures and configuration drift | Manual `sam deploy`; no CI/CD | Add CI/CD pipeline; infrastructure-as-code review; change sets |
| Insufficient test evidence | 21 unit tests pass; integration tests require deployed stack | Expand test coverage; document test execution; CI integration |
| Inconsistency between Worklog and actual project | Weeks 14–25 describe IoT Weather Platform, not Policy Assistant | Maintain separate audit document; clearly label proposed roadmap |
| Scope creep | Proposed enhancements may exceed internship capacity | Phase-gated roadmap; acceptance criteria per phase; stakeholder sign-off |

### 8. Expected Outcomes / Kết quả kỳ vọng

#### Current capabilities supported by evidence

- `GET /policies/{policy_id}` returns a policy by exact ID (21 unit tests pass).
- `GET /policies?keyword=xxx` performs case-insensitive keyword search (21 unit tests pass).
- Error responses return 400, 404, 405, or 500 with JSON bodies (21 unit tests pass).
- CORS headers are present on responses (21 unit tests pass).
- Infrastructure is defined in AWS SAM (`template.yaml`, `samconfig.toml`).
- DynamoDB table `PolicyAssistantPolicies` with partition key `policy_id` is defined.
- CloudWatch Logs group with 30-day retention and JSON format is defined.
- IAM role grants `dynamodb:GetItem` and `dynamodb:Scan` on the table ARN only.

#### Expected outcomes after planned work

- Documented, validated infrastructure matching `template.yaml`.
- Expanded test coverage with CI integration.
- Search optimization recommendation (GSI, OpenSearch, or Bedrock).
- Security review with least-privilege IAM and optional authentication.
- Monitoring with CloudWatch Alarms.
- Deployment automation pipeline.
- RAG feasibility report; optional prototype with source citations.

#### Acceptance criteria for declaring the project successful

- All unit tests pass in a clean environment.
- Infrastructure deploys via `sam deploy` without manual intervention.
- Current API endpoints return correct status codes for all documented cases.
- Planned enhancements are explicitly marked as planned or implemented with evidence.
- No fabricated metrics, costs, or deployment claims appear in documentation.
