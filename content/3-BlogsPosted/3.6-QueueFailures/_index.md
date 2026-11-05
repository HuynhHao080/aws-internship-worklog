---
title: "Draft 3 — honest SQS failure handling"
date: 2026-10-21
weight: 6
---

<!-- policy-assistant-managed:begin -->
## Primary application — technical update, 4 November 2026

**This update describes the primary [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). The 21–23 October sections below are historical records of a different test environment.** Their results do not constitute acceptance of this release.

- Implemented: public signup, private PDFs, text extraction/OCR, cited answers, history, policy administration, consented support, shop configuration and answer feedback. Shop content remains **DEMO**; no real operator has approved business policies.
- The 4 November release report records **251 Python passes, 11 live skips; 54 frontend passes; 15 API/metadata/cleanup checks passed**. Frontend checks use synthetic DOM fixtures; quality API checks use synthetic data and two disposable identities without model calls. These are not new browser visual, AI accuracy or GitHub CI results.
- The [primary release guide and limits]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) provides a feature matrix, demo plan, cost inputs and sanitized evidence. On5November the owner authorized publication: [application CI passed](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42050386735), and [worklog GitHub Pages deployment passed](https://github.com/HuynhHao080/aws-internship-worklog/actions/runs/42049972955). The AWS frontend was republished;31files and6Lambda hashes match, with no backend artifact change needed. Local/unpublished statements below are snapshots preceding this publication.
- On 4 November the student reported not yet completing the queried published blogs, office/event evidence, self-drawn diagram and Pricing Calculator. These remain **PENDING**. This does not invalidate or invent the entire earlier daily record. Technical articles 3.4–3.6 remain drafts for personal review and publication.

New material is AI-assisted and requires the student's review, execution and explanation before submission. Preserve previous failures, passes, CI runs and images with their original dates/environments. The older USD 6.63 estimate and USD 20 budget do not cover the current AI/OCR deployment.

### Historical material below
<!-- policy-assistant-managed:end -->

# A successful Lambda invocation can contain a failed message

**Unpublished draft grounded in Policy Assistant worker changes and local tests, 2026-10-21. No claim of a live DLQ exercise.**

With SQS Standard, a worker must handle duplicates and transient failures. Catching an exception and returning normally can cause an unsuccessful message to be acknowledged. Throwing for an entire batch instead retries healthy records too. Policy Assistant now processes each record independently and returns failed message IDs in `batchItemFailures`; the SAM event mapping explicitly enables `ReportBatchItemFailures`.

Permanent invalid PDF is deliberately marked failed and acknowledged because retrying the same immutable bytes will not fix input. A transient storage/database exception instead returns a batch failure. At the third receive, metadata is best-effort marked PROCESSING_FAILED and the message still fails for DLQ redrive. If that failed state is seen again for the same attempt, the worker must not silently acknowledge it. Missing message IDs are treated as an invalid event rather than inventing an identifier.

Idempotency uses both document state and submission attempt. Duplicate processed records are harmless; stale messages from a prior attempt cannot update a later retry. Conditional terminal writes enforce this even when worker and owner retry race. S3 version pinning ensures retry checks the same bytes. Metadata update and queue send are separate operations; a scheduled recovery function queries due pending records and resends the same attempt. It reduces the crash gap but does not make those writes atomic.

Regression tests include a mixed batch containing one failing and one healthy record, returned failure identifiers, terminal failure behavior, duplicates/stale attempts and frozen versions. Moto verifies local state transitions; it does not demonstrate AWS visibility timeout, receive counts, event-source deletion or DLQ redrive. Current IaC uses BatchSize 1, visibility 180 seconds, worker timeout 30 seconds and maxReceiveCount 3. Mixed-batch support protects future batching changes without requiring one now.

Monitoring must match this protocol: Lambda can return a successful invocation with partial failures, so Errors alone is insufficient. The dashboard includes SQS age/backlog/inflight/DLQ alongside worker/recovery counters; safe logs identify the error class without raw SDK messages. Operators must repair the cause before a controlled retry/redrive, not blindly purge. See [AWS Lambda partial-batch handling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html). Student defense: distinguish permanent input failure, transient service failure, duplicate delivery and failed-state recording during a database outage.
