---
title: "Proposal"
date: 2026-10-21
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Verified status — 23 October 2026

**B4 API/SDK: 19 PASS; original B5 browser: historical FAIL; B5-R7 browser: 10 PASS. GO for the accepted test-demo scope; NO-GO production.** B4 retained one browser NOT_RUN at that time; B5-R7 is separate browser evidence, not a rewrite of the historical result.

The legacy Cognito identity was **disabled and globally signed out (D3-A)**. Containment did not change its password or prove instant revocation of every offline-validated JWT or absence of unauthorized access. **D11 published sanitized application history; its first CI run FAILED**. **D15 published the test fix: [GitHub Actions 19/19 steps PASS](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39901988136), 0 FAIL/0 SKIPPED**. These are workflow steps, distinct from B4's 19 AWS API/SDK cases. The [D11 failed run](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39899173855) remains historical evidence. D17 changes documentation only, with no deployment or AWS E2E rerun.

Test demo: [https://d3ataim7dfsl7t.cloudfront.net/](https://d3ataim7dfsl7t.cloudfront.net/). Current public source: [a044936](https://github.com/HuynhHao080/aws-policy-assistant/tree/a04493682a417761f534954a1f8d38cc75d8a87b). New source/CI does not imply a new AWS deployment: the API uses the B5-R6 regional virtual-hosted S3 fix while Worker/Recovery retain their prior artifacts. IAM/CSP/CORS were not broadened. Keep local/mocked screenshots labelled accordingly.

The configured **account-wide Budget is 20 USD/month** (Actual 80% = 16 USD, Actual 100% and Forecasted 100% = 20 USD); the **10 USD tag-filtered test Budget is PENDING**. Budgets are not hard spending caps and do not stop resources; email delivery remains unverified. The approximately 6.6306 USD/month test estimate is an assumed-load model, not an invoice or a current AWS forecast.

The AWS test environment remains maintained: **no cleanup and no owner-approved demo end/cleanup date**. The C0 snapshot recorded 45 resources/4 Retain, approximately one diagnostic DLQ message/ALARM and zero SNS subscriptions; those are not freshly queried AWS metrics today. Retained resources can continue accruing charges. This is not production-ready.

Public evidence: [B4](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-test-e2e-b4-result.md), [B5-R7](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-browser-b5r7-result.md), [C0](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/cleanup-c0-inventory-and-acceptance.md). Containment is supported by filtered D3-A evidence without publishing the username/password/tokens. Git rewriting cannot erase old clones/caches. Student architecture authorship and attendance evidence remain student-confirmation tasks, not invented replacements.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements and old security/publication gates below describe earlier plans/snapshots. Use the verified summary above for current reporting; keep the historical B5 FAIL. D17 does not publish the Worklog website.

# Policy Assistant — MVP completion proposal

## Problem and current scope

Provide policy lookup and owner-isolated PDF intake with traceable internship evidence. Current local source includes Cognito JWT, policy/document APIs, versioned S3, SQS worker, DynamoDB metadata, frontend and test CI. Changes are not pushed or deployed. See the **Policy Assistant** section for current evidence and limitations.

## Solution and priorities

Use SAM HTTP API, Python 3.12 Lambda, Cognito, two DynamoDB tables, private versioned S3, SQS/DLQ and CloudWatch. Bind sessions to authorizer sub, freeze submitted versions and verify PDF bytes/SHA-256. Processing does not mean OCR or policy extraction.

1. Resolve ownership, credential history and input validation.
2. Verify retry/idempotency, immutable versions and lossless pagination.
3. Finish polling, error handling and hash-checked browser download.
4. Pass offline tests/build/lint; separately obtain authorized live AWS evidence.
5. Reconcile documentation, internship activities and demo against actual evidence.

## Acceptance and future work

Record passing backend/frontend/browser checks and build/lint with environment details. With deployment authorization, verify live JWT, two-user isolation, CORS, worker/DLQ and SHA-256. Keep commit/date/environment traceability and explicit limits for Scan, nontransactional enqueue, pending recovery, S3 retention and local hosting.

Bedrock/RAG, semantic search and cited answers are future work requiring demand, access, an evaluation dataset and budget. A dated assumed-load cost model is now available in Workshop 5.7; replace its assumptions with measured request/storage/log usage after approved deployment. IoT worklog activities and the S3/VPC Endpoint workshop remain independent material; do not relabel them as Policy Assistant achievements.

## MVP v1 handover

Delivered local source includes direct login, personal library, hash-checked upload/worker/download, failure/retry states, scheduled recovery, a complete offline demo, verification/packaging tools and HTTPS hosting definitions. CloudFront/Cognito/IAM/DLQ still require authorized live AWS acceptance. See the project evidence section and application runbook for scope and the legacy two-index upgrade.


## Scope, timeline, cost and risks

Scope: shared policy lookup/search, native login and private PDF byte/hash library; no AI/OCR or public signup. Acceptance includes success, two-user denial, no false success on timeout/hash mismatch and explicit SQS failure handling.

This is a proposed 12-week organization plan, **not reconstructed historical achievement**: weeks 1–2 problem/account/budget; 3–4 catalogue/API/IaC; 5–6 identity/ownership/upload; 7–8 worker/version/recovery; 9–10 UI/testing/monitoring; 11 approved AWS demo; 12 student diagram, daily/blog/evidence reconciliation and presentation rehearsal. Student confirms actual dates.

Official rates checked 2026-10-21 produce a gross planning scenario of 6.6306 USD/month, 34.7081 USD at 10× use; see Workshop 5.7 COST for quantities and paid marginal rates before allowances. Not measured usage or a created Calculator estimate. Risks include old credentials requiring rotation, unverified AWS behavior, two-step legacy GSI upgrade, coordinated POST/cache rollout, Scan/retained-version cost, database/DLQ operator intervention and missing student evidence. No unnecessary VPC/NAT/AI services are added. Draw and explain the submitted architecture personally using the worksheet.
