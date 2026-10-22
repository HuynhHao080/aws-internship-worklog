---
title: "Proposal"
date: 2026-10-21
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Current D1 status —2026-10-22

**B4 API/SDK19PASS; original B5 browserFAIL retained; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO bounded test demo, NO-GO production.** D1 is local review, no AWS/E2E or website publishing.

Approximately1diagnostic DLQ message/ALARM remains; alarm SNS topic has0subscriptions. Account20USD Budget is not a cap; email delivery unverified. No cleanup. **Owner has not set the environment end date.** Rotation/revocation proof for credential literals in old Git history is required before public push.

The fix uses virtual S3 addressing for regional POST/GET, without broadening IAM/CSP/CORS. Real Edge R7 used2PDFs/1,218bytes,3logins/3logouts,27requests; not a load benchmark. Deployed API has new ZIP; Worker/Recovery retain old ZIP. Shared-CodeUri source SAM is not a replay of the API-only candidate.

Evidence is in the local application repo: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md. Do not assume files are already pushed. Local/mocked screenshots do not replace AWS proof. Student must draw/review/explain their own architecture with official AWS icons; assistant reference diagrams do not establish student authorship.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements below are superseded by the D1 summary; original B5FAIL is not rewritten as PASS.



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
