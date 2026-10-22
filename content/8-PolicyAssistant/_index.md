---
title: "Policy Assistant — Project Status"
date: 2026-10-21
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

## Current D1 status —2026-10-22

**B4 API/SDK19PASS; original B5 browserFAIL retained; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO bounded test demo, NO-GO production.** D1 is local review, no AWS/E2E or website publishing.

Approximately1diagnostic DLQ message/ALARM remains; alarm SNS topic has0subscriptions. Account20USD Budget is not a cap; email delivery unverified. No cleanup. **Owner has not set the environment end date.** Rotation/revocation proof for credential literals in old Git history is required before public push.

The fix uses virtual S3 addressing for regional POST/GET, without broadening IAM/CSP/CORS. Real Edge R7 used2PDFs/1,218bytes,3logins/3logouts,27requests; not a load benchmark. Deployed API has new ZIP; Worker/Recovery retain old ZIP. Shared-CodeUri source SAM is not a replay of the API-only candidate.

Evidence is in the local application repo: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md. Do not assume files are already pushed. Local/mocked screenshots do not replace AWS proof. Student must draw/review/explain their own architecture with official AWS icons; assistant reference diagrams do not establish student authorship.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements below are superseded by the D1 summary; original B5FAIL is not rewritten as PASS.



> Evidence update 2026-10-21: B1 stack/B2 frontend/B3 users completed; B4 API/SDK 19 PASS, real browser NOT_RUN. See AWS E2E chapter. Pending/not-deployed statements below are pre-B4 plans/history, not current status. Do not repeat poison or live demo writes without further approval.

# Policy Assistant — evidence-based progress

Updated 2026-10-21. Current changes exist in local working trees only; no push or AWS deployment was performed.

## Scope and architecture

Policy lookup, keyword search and owner-isolated PDF byte verification. Local frontend → Cognito / HTTP API → Python 3.12 Lambda → policy and document metadata DynamoDB tables. Private versioned S3 stores PDFs; submit freezes a version; SQS invokes the worker to check PDF bytes and SHA-256. Repeated failures can redrive to DLQ. CloudWatch contains logs and alarm definitions. There is no AI, RAG, OCR or policy text extraction.

## Handover anchors

Application: [aws-policy-assistant](https://github.com/HuynhHao080/aws-policy-assistant), `80c4eef`. Report: [aws-internship-worklog](https://github.com/HuynhHao080/aws-internship-worklog), `846d2ff`. These commits precede the current unpushed changes.

## Local improvements and evidence

Upload sessions bind to authorizer sub; submit/status/download enforce ownership and deny unowned legacy records. Submitted versions are frozen and used for verification/download. Worker exceptions propagate; duplicate success is idempotent. Search pagination no longer discards results behind the cursor. The browser polls, handles errors and compares original/worker/download hashes. Verification scripts prompt for passwords and do not print tokens or signed URLs.

Python 3.12.10: **120 unit tests passed** using moto. Frontend: **17 tests passed**; **2 real headless Edge scenarios passed** against mocked API/S3, checking upload/download, polling, 401/500, integrity and responsive layout. **9 live integration tests skipped** without explicit opt-in. Use the local application `docs/final-verification.md` for build/lint results and `docs/evidence/` for screenshots. These results do not prove current AWS deployment or live E2E behavior.

## Remaining acceptance work

Rotate passwords previously committed to history. Review local changes/change set; obtain deployment authorization before live validation of JWT, two-user ownership, S3 CORS, worker/DLQ and SHA-256. Capture sanitized evidence tied to commit/date/environment. Complete personal report sections using actual experience and confirm distinct publication links. Preserve IoT history and future plans; the S3/VPC Endpoint workshop is independent learning material. State Scan scaling, nontransactional enqueue, pending failure recovery, S3 retention and local hosting limitations. Bedrock/RAG remains optional future work.

## Complete MVP v1

Native Cognito login/challenges/refresh/logout, named owner-index library, terminal failure states, attempt-safe retries and bounded scheduled pending recovery are implemented locally. SAM defines private S3/CloudFront HTTPS/OAC/security headers, indexed queries and SNS alarms with an optional confirmed email recipient. Legacy table upgrades require the documented two-step index migration. No AWS deployment has occurred.

Run `python -m scripts.demo_server` for the complete offline app, `python -m scripts.verify` for offline acceptance, and `python -m scripts.build_release` for source/static ZIPs and SHA-256 manifest. The full-stack browser scenario uses actual Lambda handlers with simulated AWS/authentication, rather than stubbing API responses.

![Local full-stack application](/aws-internship-worklog/images/policy-assistant/full-stack-desktop.png)

*Local evidence, not an AWS deployment screenshot. No credentials or signed URLs are shown.*


## New P0 and FCAJ completion pass

Bounded signed POST key/MIME/5 bytes–10 MiB; JSON/base64/ID validation; redacted request/SDK logging; SQS partial failures; TLS-only buckets; queue SSE; explicit Cognito Lite; controlled HeadObject 403. Added selected Python lint/mypy/JS syntax and non-overwriting repeatable seed. Current local results: 120 backend, 17 frontend, two browser suites; SAM lint/build passed. Nine AWS integration skips are not passes.

Product Workshop 5.7 includes reproducible deployment/testing/operations/cleanup. Official AWS prices checked 2026-10-21 give a 6.6306 USD/month gross assumed-load scenario, not an invoice. Hugo builds 124 EN/123 VI outputs; 125 HTML pages and 11,638 internal references have no broken targets. Three theme deprecation warnings remain. Three distinct EN/VI project blog drafts (3.4–3.6) are unpublished. Daily Week 1–12 history is preserved pending student confirmation, real activity references, 10 office/3 events, self-evaluation/feedback and student-drawn architecture. No AWS deployment/push or invented evidence.
