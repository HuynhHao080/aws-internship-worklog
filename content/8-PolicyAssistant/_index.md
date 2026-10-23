---
title: "Policy Assistant — Project Status"
date: 2026-10-21
weight: 8
chapter: false
pre: " <b> 8. </b> "
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
