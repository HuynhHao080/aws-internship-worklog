---
title: "Demo — 5 to 7 minutes"
date: 2026-10-21
weight: 3
---

<!-- policy-assistant-managed:begin -->
## Primary application — technical update, 4 November 2026

**This update describes the primary [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). The 21–23 October sections below are historical records of a different test environment.** Their results do not constitute acceptance of this release.

- Implemented: public signup, private PDFs, text extraction/OCR, cited answers, history, policy administration, consented support, shop configuration and answer feedback. Shop content remains **DEMO**; no real operator has approved business policies.
- The 4 November release report records **251 Python passes, 11 live skips; 54 frontend passes; 15 API/metadata/cleanup checks passed**. Frontend checks use synthetic DOM fixtures; quality API checks use synthetic data and two disposable identities without model calls. These are not new browser visual, AI accuracy or GitHub CI results.
- The [primary release guide and limits]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) provides a feature matrix, demo plan, cost inputs and sanitized evidence. This worklog update is prepared locally; GitHub Pages publication is not asserted.
- On 4 November the student reported not yet completing the queried published blogs, office/event evidence, self-drawn diagram and Pricing Calculator. These remain **PENDING**. This does not invalidate or invent the entire earlier daily record. Technical articles 3.4–3.6 remain drafts for personal review and publication.

New material is AI-assisted and requires the student's review, execution and explanation before submission. Preserve previous failures, passes, CI runs and images with their original dates/environments. The older USD 6.63 estimate and USD 20 budget do not cover the current AI/OCR deployment.

### Historical material below
<!-- policy-assistant-managed:end -->

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
