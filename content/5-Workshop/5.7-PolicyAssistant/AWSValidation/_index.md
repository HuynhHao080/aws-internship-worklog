---
title: "AWS E2E — verified evidence"
date: 2026-10-21
weight: 5
---

## Current D1 status —2026-10-22

**B4 API/SDK19PASS; original B5 browserFAIL retained; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO bounded test demo, NO-GO production.** D1 is local review, no AWS/E2E or website publishing.

Approximately1diagnostic DLQ message/ALARM remains; alarm SNS topic has0subscriptions. Account20USD Budget is not a cap; email delivery unverified. No cleanup. **Owner has not set the environment end date.** Rotation/revocation proof for credential literals in old Git history is required before public push.

The fix uses virtual S3 addressing for regional POST/GET, without broadening IAM/CSP/CORS. Real Edge R7 used2PDFs/1,218bytes,3logins/3logouts,27requests; not a load benchmark. Deployed API has new ZIP; Worker/Recovery retain old ZIP. Shared-CodeUri source SAM is not a replay of the API-only candidate.

Evidence is in the local application repo: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md. Do not assume files are already pushed. Local/mocked screenshots do not replace AWS proof. Student must draw/review/explain their own architecture with official AWS icons; assistant reference diagrams do not establish student authorship.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements below are superseded by the D1 summary; original B5FAIL is not rewritten as PASS.



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
