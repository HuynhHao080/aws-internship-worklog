---
title: "AWS Policy Assistant — AI Pilot Acceptance"
date: 2026-10-25
weight: 26
chapter: false
pre: " <b> 1.26. </b> "
---

## Objective and architecture

Complete a bounded policy-answer pilot on AWS TEST. This is a dated project milestone, not a claim that internship week 26 or the entire internship is complete. Statements use retained evidence, not a fresh service-health check.

CloudFront serves a private S3 frontend. Cognito provides the session; API Gateway validates JWTs before Lambda. DynamoDB stores policies, owner-isolated document metadata and the AI quota/idempotency ledger. Presigned S3 uploads, SQS, a worker, DLQ and scheduled recovery support PDF byte/header/size/hash verification. This is not OCR, malware scanning or extracting uploaded policy text for AI.

AI follows Cognito → API Gateway → dedicated AI Lambda → Bedrock Nova Lite Converse → server validation → displayed answer/citation. The model is `apac.amazon.nova-lite-v1:0`, using the APAC profile from Singapore. Approved sample data may be processed in Singapore, Sydney, Tokyo, Seoul, Osaka or Mumbai. Credentials, user identifiers and private PDFs are not sent to the model.

Lookup/search, owned upload/status/download and the document library are implemented. Owner observations confirmed the refund-policy lookup, one refund-keyword match, three documents and preserved library statuses on refresh. Final AI acceptance did not repeat document uploads. The AI panel has one fixed question, no free-text input and no automatic inference on page load.

## Project timeline

| Date | Milestone | Evidence |
| --- | --- | --- |
| 24 October 2026 | TEST AI infrastructure deployed with feature OFF; application acceptance continued separately | Saved deployment/read-only evidence; successful deployment was not full acceptance |
| 25 October 2026 | Restricted AI-ON backend: Bedrock policy added and AI Lambda environment updated | Saved deployment/configuration checks; simulation alone was not inference proof |
| 25 October 2026 | One owner-operated TestA API acceptance | Response/citation PASS_BY_OWNER with bounded AWS runtime audit PASS |
| 25 October 2026 | Four-file frontend release, configuration last, integrity/cache/CSP checks | Saved release/static evidence |
| 25 October 2026 | Owner visibility and screenshot-derived live answer/source observations after one UI click | LIVE_UI_ACCEPTANCE_PASS_BY_OWNER, offline source/hash comparison and bounded AWS audit |
| Before activation acceptance | Synthetic CloudWatch alarm action → SNS → Gmail | END_TO_END_DELIVERY_PASS_BY_OWNER with successful AWS action history; not repeated |

TestA and TestB are role labels, not login identifiers. Scope remains **TestA-only, POL-001 and one fixed synthetic refund-period question**. TestB is not authorized for AI and did not see the panel in owner observations.

## Runtime evidence

The displayed answer correctly gave a 30-day period from purchase for defective, damaged or not-as-described products. The source was POL-001, `/policies/POL-001`, with the approved hash. The owner-transcribed answer matched the exact approved source excerpt/hash offline; this does not validate arbitrary refund advice.

The API/helper request and native UI click were distinct admissions. Each bounded window contained one application ModelAttempts and usage of **588 input /85 output tokens**. Actual Errors/Fallbacks/QuotaDenied datapoints were zero in those windows. The two-request total, 1176/170, is not an account-wide total. The first admission had one idempotency record and five counters at one; the UI admission added its own record/minute counters and increased shared day/month counters to two. No markers or ledger were reset.

## Limits, exceptions and lessons

- Atomic user/global limits are 2/minute and 5/day; the global limit is 100/month, using UTC buckets. Reservations remain after timeout. No retry, fallback or streaming.
- Body <=2048 bytes; system <=512; messages <=512; tool configuration <=768; output <=200 tokens. CountTokens has not been proven operational. Byte caps and post-response usage checks do not guarantee <=6000 actual input tokens before inference; the owner approved this pilot-specific bounded-input policy.
- Forced `submit_policy_answer` input is checked for schema, policy, exact quote, answer evidence and usage before the server creates a citation. No external tool or tool result is executed.
- UUID → Lambda remains **INFERRED**, provider request ID **UNKNOWN**. This is bounded audit, not full distributed tracing. Screenshot contents were owner-provided; the original image was not independently inspected in the final reconciliation.
- TestB batch-control remains **FAIL**; the original TestB procedure remains **INCONCLUSIVE**. The owner accepted denial security evidence with a documented exception, not retroactive authorization. No rerun erased the deviation.
- The native UI click was clarified later and preceded separate UI-click execution approval in the recorded workflow. Functional acceptance does not erase this procedural deviation.
- OFF recovery is prepared conditionally but **live rollback is NOT TESTED**. It does not cancel in-flight work; frontend hiding is not a backend kill switch.
- The monthly **USD 20 budget alert is not a hard spending cap**. Model and infrastructure costs are separate; billing and notifications may lag.

HTTP 200 is insufficient without output/evidence/citation validation. IAM simulation is not runtime authorization; missing metrics are not zero failures. Releases need pinned bytes/hashes and a verified rollback baseline. One-shot/session/batch controls also require provenance.

## Next steps

Complete handover and retain sanitized evidence before log expiry. Keep the narrow pilot. New requests, additional users/policies, free-text input, durable tracing and production rollout need separate review/approval. Do not publish credentials, private identifiers, raw logs or internal paths, or replay old helpers.

[Project repository](https://github.com/HuynhHao080/aws-policy-assistant). The deployed pilot includes local integration not yet published to main; documentation publication alone does not make deployment reproducible from that branch. This is not production certification.
