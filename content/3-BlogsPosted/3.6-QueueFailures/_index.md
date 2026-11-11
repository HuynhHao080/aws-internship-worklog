---
title: "Draft 3 — honest SQS failure handling"
date: 2026-10-21
weight: 6
---

<!-- policy-assistant-managed:begin -->
## Primary application — documentation update, 6 November 2026

**Primary application model:** Global Claude Haiku 5.5. The application ranks extracted PDF and policy text by keywords. Dated verification reports record their respective releases. [Model cutover evidence](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/haiku55-migration.md).

**[Submission pack]({{< relref "/5-Workshop/5.7-PolicyAssistant/Submission" >}})**

**The primary application is [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). October sections below describe another historical test environment.**

- Read the [current proposal]({{< relref "/2-Proposal/CurrentSolution" >}}) for the problem, scope, architecture inputs for the student's diagram, delivery plan, COST inputs and risks. The [primary release guide]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) retains features and evidence from successive releases.
- Implemented: public signup, private PDFs/text/OCR, cited answers, history, policy drafts/publication/effective dates, shop configuration, aggregate feedback and consented support. [Support collaboration]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}) adds claims, handovers and internal notes; it does not create a restricted staff role or access to customers' private PDFs.
- Latest recorded runtime snapshot: **322 Python passes, 11 live skips; 67 frontend passes; 20 HTTP checks passed**, three QA identities and the test ticket cleaned. Forty frontend files and six Lambda hashes matched. The 5 November UTC timestamp corresponds to 6 November in Vietnam; this is not a new run whenever the page is read. No model/PDF/OCR/email calls in that HTTP run. [Report](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/support-team-verification.md), [application CI](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42072920395).
- Shop content remains **DEMO**. Approved real policies and scoped browser/accuracy/load/restore/email checks remain pending. The USD 50 budget only alerts; the USD 6.63 estimate/USD 20 budget below belong to an older environment and do not price the current application.
- Actually published blogs, office/event evidence, a student-drawn diagram, Pricing Calculator and video remain personal deliverables. Draft articles are not published posts. Do not invent internship dates, daily activities, photographs or grades.

AI-assisted material requires student review, execution and explanation. DOM checks are not visual browser acceptance; artifacts/CI do not establish accuracy or SLA. Preserve previous failures, passes and images with their original environment.

### Historical material below
<!-- policy-assistant-managed:end -->

# A successful Lambda invocation can contain a failed message

**Unpublished draft grounded in Policy Assistant worker changes and local tests, 2026-10-21. No claim of a live DLQ exercise.**

With SQS Standard, a worker must handle duplicates and transient failures. Catching an exception and returning normally can cause an unsuccessful message to be acknowledged. Throwing for an entire batch instead retries healthy records too. Policy Assistant now processes each record independently and returns failed message IDs in `batchItemFailures`; the SAM event mapping explicitly enables `ReportBatchItemFailures`.

Permanent invalid PDF is deliberately marked failed and acknowledged because retrying the same immutable bytes will not fix input. A transient storage/database exception instead returns a batch failure. At the third receive, metadata is best-effort marked PROCESSING_FAILED and the message still fails for DLQ redrive. If that failed state is seen again for the same attempt, the worker must not silently acknowledge it. Missing message IDs are treated as an invalid event rather than inventing an identifier.

Idempotency uses both document state and submission attempt. Duplicate processed records are harmless; stale messages from a prior attempt cannot update a later retry. Conditional terminal writes enforce this even when worker and owner retry race. S3 version pinning ensures retry checks the same bytes. Metadata update and queue send are separate operations; a scheduled recovery function queries due pending records and resends the same attempt. It reduces the crash gap but does not make those writes atomic.

Regression tests include a mixed batch containing one failing and one healthy record, returned failure identifiers, terminal failure behavior, duplicates/stale attempts and frozen versions. Moto verifies local state transitions; it does not demonstrate AWS visibility timeout, receive counts, event-source deletion or DLQ redrive. Current IaC uses BatchSize 1, visibility 180 seconds, worker timeout 30 seconds and maxReceiveCount 3. Mixed-batch support protects future batching changes without requiring one now.

Monitoring must match this protocol: Lambda can return a successful invocation with partial failures, so Errors alone is insufficient. The dashboard includes SQS age/backlog/inflight/DLQ alongside worker/recovery counters; safe logs identify the error class without raw SDK messages. Operators must repair the cause before a controlled retry/redrive, not blindly purge. See [AWS Lambda partial-batch handling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html). Student defense: distinguish permanent input failure, transient service failure, duplicate delivery and failed-state recording during a database outage.
