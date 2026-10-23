---
title: "AWS E2E — verified evidence"
date: 2026-10-21
weight: 5
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

> **B5-R local remediation:** real local Edge confirms CSP connect-src blocks the global S3 hostname. One-line virtual addressing fix makes regional POST/GET and local hash pipeline pass. 129 backend/17 frontend/3 browser fixture/1 full-stack local plus SAM lint/build PASS;9 live integration skips=NOT_RUN. No CSP/CORS/IAM/template/frontend change, artifact upload or deployment; B5 AWS remains FAIL. Original AWS cause is LIKELY without exact violation capture; local mechanism CONFIRMED. Separate artifact/change-set/deploy/rerun approvals required; cleanup C separate.

> **2026-10-22 update: B5 browser FAIL/STOPPED**, 2PASS/1FAIL/7NOT_RUN. Real Edge TestA login/search passed; upload UI failed after session200, before S3 upload. Likely regional-only CSP/global S3 signing-host mismatch; exact violation not captured. One orphan uploading metadata record, no S3 object; main0/QueueAge OK, DLQ1/ALARM. No fix/deploy/rerun/cleanup; separate approval required. B4 API/SDK PASS below is earlier evidence, not browser acceptance. No successful browser screenshot exists.

# AWS API/SDK E2E — 2026-10-21

**19 PASS / 0 FAIL / 0 BLOCKED / 1 NOT_RUN (real browser)**. Test window 16:47:11–16:57:34 UTC; alarm follow-up 16:59:27 UTC. Account …0098, ap-southeast-1, policy-assistant-fcaj-test. B1 created 45 resources; B2 published four static assets; B3 created exactly two synthetic users. B4 ran once; an interrupted chat resumed observation of the same process, not another poison send.

| Scope | Evidence |
|---|---|
| Cognito | Both users authenticated; wrong password rejected; missing/invalid JWT HTTP401; email_verified=false unchanged |
| API | Health, lookup, search, pagination, validation and CORS PASS; three synthetic policy fixtures seeded |
| Documents | Two PDFs: constrained presigned POST → submit → SQS worker → status → download; original/worker/download SHA-256 match |
| Ownership | Six cross-user status/download/submit HTTP404 responses; foreign documents absent from own list |
| Validation | 10 MiB accepted, +1 byte rejected; MIME/key tampering rejected; non-PDF INVALID_PDF/download409 |
| Version/idempotency | New current version does not change frozen download; resubmit keeps processed |
| Partial failure | Direct real Lambda event returns only failed record; healthy record already processed, proving idempotent acknowledgement |
| Retry/DLQ | One poison, three main receives plus one diagnostic DLQ read; marker/message ID match; no delete/purge/config change |
| Logs | 158 events, no matched secrets in bounded exact-value/pattern scan; raw logs not exported |
| CloudFront browser flow | **NOT_RUN**; local browser suites are not AWS browser acceptance |

Mapping remains BatchSize=1; direct mixed event is not natural multi-record SQS delivery. Actual upload is **POST**, not PUT. The product checks size/header/hash, not full PDF parsing, malware, OCR or AI/RAG.

DLQ arrival was observed at 16:57:30 UTC. Initially its alarm remained OK; follow-up showed **DLQ ALARM and QueueAge ALARM**, three other alarms OK. This is controlled fault injection; no alarm reset or message deletion to manufacture OK. Metrics publish late; zero/empty series do not override queue attributes. Lambda Errors=0 does not disprove retries when partial failures are returned.

Budget remains COST/MONTHLY/USD20, whole account, UnblendedCost excluding Credit/Refund, three alerts, no Actions. Post-test Actual5.479/Forecast8.539 USD is a delayed Billing snapshot, not B4-only measured cost. Prior owner Console reports Active(legacy), verification0/1; email delivery unverified. Budget is not a hard cap.

Local evidence: application repo `docs/aws-test-e2e-b4-result.md`, `docs/evidence/approval-b4-e2e.json`, `approval-b4-alarm-followup.json`. No commit/push/website publication or fabricated screenshots. DPAPI credentials remain private.

**GO for backend/API-SDK acceptance review; browser acceptance remains incomplete. STOP after B4.** Retain synthetic data, four document sessions, three policies and one DLQ message pending separately approved cleanup. PoliciesTable, DocumentMetadata, DocumentsBucket and HTTPS policy retain; keep HTTPS protection with the bucket. Further fixes/deployment/cleanup require independent approval. Attendance/events and student-drawn architecture still require authentic evidence.
