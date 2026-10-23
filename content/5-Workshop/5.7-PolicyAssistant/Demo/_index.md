---
title: "Demo — 5 to 7 minutes"
date: 2026-10-21
weight: 3
---

> Status 23 October 2026: B4 API/SDK 19 PASS; historical B5 FAIL; B5-R7 browser 10 PASS; D3-A contained; D11 sanitized publication/CI FAIL; D15 CI 19/19 steps PASS. Test demo: [https://d3ataim7dfsl7t.cloudfront.net/](https://d3ataim7dfsl7t.cloudfront.net/). Not production-ready; new login/upload/poison activity requires suitable separate approval.

# FCAJ demo script — 5 to 7 minutes

**The test environment was deployed and accepted within the B5-R7 scope.** D17 updates documentation only, without a new availability check or AWS test data. Keep the OFFLINE label for local demonstrations and do not present the legacy stack as this release. The environment remains maintained with no approved cleanup date.

| Time | Action | Visible proof |
|---|---|---|
| 0:00–0:40 | Problem: shared policy search and private document library. Scope is PDF byte/hash verification, no AI/OCR/RAG. | UI, exact environment and revision |
| 0:40–1:40 | Explain your own diagram: Cognito/API/Lambda, two tables, signed S3 POST/version, SQS/worker/DLQ/recovery, CloudWatch. | Student editable diagram/date with official icons |
| 1:40–3:10 | Alice native login, POL-001/refund lookup, small synthetic PDF upload, processing and verified download/library. | Real AWS final state; original/worker/download SHA-256; password field cleared |
| 3:10–4:10 | Logout, Bob login, own library and Alice-ID 404. Explain JWT versus ownership. | Actual two-user UI/HTTP evidence |
| 4:10–5:10 | Show dated oversized/invalid-format rejection and saved retry/DLQ evidence. Explain why the retry wait is prerecorded for a short talk. | Actual mapping/log/metric window; distinguish direct mixed-batch Invoke from SQS redrive |
| 5:10–6:10 | Dashboard, literal alarm states and costs. About USD 6.63/month under explicit load, before credits/tax and excluding the existing environment. | Dated service table, resource inventory and retained-version/cleanup notes |
| 6:10–6:40 | Measured outcome and limits: bounded Scan, eventual indexes, enqueue/recovery gap, no full PDF safety or production certification. | Actual acceptance matrix, unresolved items explicit |

Never display Network details, Authorization, passwords, complete auth replies, signed URLs/POST fields or private documents. Use synthetic data and sanitized evidence.

## Defense questions — answer in your own words

| Question | Answer checkpoints |
|---|---|
| Why HTTP API/Lambda/DynamoDB? | Managed small workload without idle VM; GetItem versus dataset-dependent bounded Scan, no invented full-text engine. |
| Where is JWT verified? | Gateway validates issuer/audience; Lambda trusts authorizer sub. A unit/direct Invoke event does not prove signature verification. |
| Why Bob gets 404? | Backend owner check denies unknown/unowned IDs; list/cursor is owner-bound. CORS and hidden buttons do not authorize data. |
| Does S3 validate Cognito? | No: temporary signed capability plus IAM/policy. Possession grants access until expiry; fields must remain private. |
| Why POST/versioning? | Storage-side key/MIME/size bound; frozen version survives overwrite. Header/MIME check is not parsing or malware scanning. |
| How are retries safe? | At-least-once queue, conditional state/attempt, harmless duplicates/stale messages, partial responses, receive limit and DLQ. Distinguish permanent invalid input from transient failure. |
| Is metadata-to-SQS atomic? | No; sparse due-time index and scheduled recovery repair a crash gap with safe duplicates. Database outage still needs an operator. |
| What proves partial failure? | Name the experiment: real direct mixed Invoke versus actual BatchSize 1 retry/redrive. Do not claim natural mixed SQS delivery without evidence. |
| Why Errors can be zero? | Partial response/caught HTTP 500 can be successful invocations; inspect HTTP status, safe logs, queue age/backlog/DLQ and actual alarms. |
| How is test isolated? | Separate stack/prefixed tables/private buckets/new users/scoped data ARNs. Existing resources remain unchanged. |
| What does USD 6.63 cover? | Assumed-load intended-system month including logs/alarms/dashboard, before allowances/credits/tax; existing stack, optional extras and actual use are separate. |
| Rollback and cleanup? | Stop failed rollout, restore compatible reviewed test artifacts/mapping as needed, retain document/table data. Deleting retained data needs owner-approved inventory. |

## Student architecture exercise

Use `docs/architecture-self-draw.md` (the worksheet; application repository) and [official AWS icons](https://aws.amazon.com/architecture/icons/). Draw it yourself, map every box to a real logical resource, label protocol/identity/data on every arrow and show global/regional/auth/owner/capability boundaries plus retry/DLQ/recovery. Save editable source/date. Practice explaining without reading generated answers. Assistant Mermaid/PNG is reference only and must not be submitted as student-authored design. The actual test endpoint is linked above; the student must still supply their own editable architecture diagram.

## Budget and operation boundaries

The account Budget is 20 USD/month, not a hard spending cap; the tag-filtered 10 USD test Budget is PENDING. Use saved retry/DLQ evidence. Do not create messages or delete data to improve alarm appearance; cleanup/Retain operations need separate approval.
