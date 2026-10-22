---
title: "Draft 3 — honest SQS failure handling"
date: 2026-10-21
weight: 6
---

# A successful Lambda invocation can contain a failed message

**Unpublished draft grounded in Policy Assistant worker changes and local tests, 2026-10-21. No claim of a live DLQ exercise.**

With SQS Standard, a worker must handle duplicates and transient failures. Catching an exception and returning normally can cause an unsuccessful message to be acknowledged. Throwing for an entire batch instead retries healthy records too. Policy Assistant now processes each record independently and returns failed message IDs in `batchItemFailures`; the SAM event mapping explicitly enables `ReportBatchItemFailures`.

Permanent invalid PDF is deliberately marked failed and acknowledged because retrying the same immutable bytes will not fix input. A transient storage/database exception instead returns a batch failure. At the third receive, metadata is best-effort marked PROCESSING_FAILED and the message still fails for DLQ redrive. If that failed state is seen again for the same attempt, the worker must not silently acknowledge it. Missing message IDs are treated as an invalid event rather than inventing an identifier.

Idempotency uses both document state and submission attempt. Duplicate processed records are harmless; stale messages from a prior attempt cannot update a later retry. Conditional terminal writes enforce this even when worker and owner retry race. S3 version pinning ensures retry checks the same bytes. Metadata update and queue send are separate operations; a scheduled recovery function queries due pending records and resends the same attempt. It reduces the crash gap but does not make those writes atomic.

Regression tests include a mixed batch containing one failing and one healthy record, returned failure identifiers, terminal failure behavior, duplicates/stale attempts and frozen versions. Moto verifies local state transitions; it does not demonstrate AWS visibility timeout, receive counts, event-source deletion or DLQ redrive. Current IaC uses BatchSize 1, visibility 180 seconds, worker timeout 30 seconds and maxReceiveCount 3. Mixed-batch support protects future batching changes without requiring one now.

Monitoring must match this protocol: Lambda can return a successful invocation with partial failures, so Errors alone is insufficient. The dashboard includes SQS age/backlog/inflight/DLQ alongside worker/recovery counters; safe logs identify the error class without raw SDK messages. Operators must repair the cause before a controlled retry/redrive, not blindly purge. See [AWS Lambda partial-batch handling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html). Student defense: distinguish permanent input failure, transient service failure, duplicate delivery and failed-state recording during a database outage.
