---
title: "Draft 2 — bounded S3 uploads and immutable versions"
date: 2026-10-21
weight: 5
---

<!-- policy-assistant-managed:begin -->
## Primary application — documentation update, 6 November 2026

**The primary application is [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). October sections below describe another historical test environment.**

- Read the [current proposal]({{< relref "/2-Proposal/CurrentSolution" >}}) for the problem, scope, architecture inputs for the student's diagram, delivery plan, COST inputs and risks. The [primary release guide]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) retains features and evidence from successive releases.
- Implemented: public signup, private PDFs/text/OCR, cited answers, history, policy drafts/publication/effective dates, shop configuration, aggregate feedback and consented support. [Support collaboration]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}) adds claims, handovers and internal notes; it does not create a restricted staff role or access to customers' private PDFs.
- Latest recorded runtime snapshot: **322 Python passes, 11 live skips; 67 frontend passes; 20 HTTP checks passed**, three QA identities and the test ticket cleaned. Forty frontend files and six Lambda hashes matched. The 5 November UTC timestamp corresponds to 6 November in Vietnam; this is not a new run whenever the page is read. No model/PDF/OCR/email calls in that HTTP run. [Report](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/support-team-verification.md), [application CI](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42072920395).
- Shop content remains **DEMO**. Approved real policies and scoped browser/accuracy/load/restore/email checks remain pending. The USD 50 budget only alerts; the USD 6.63 estimate/USD 20 budget below belong to an older environment and do not price the current application.
- Actually published blogs, office/event evidence, a student-drawn diagram, Pricing Calculator and video remain personal deliverables. Draft articles are not published posts. Do not invent internship dates, daily activities, photographs or grades.

AI-assisted material requires student review, execution and explanation. DOM checks are not visual browser acceptance; artifacts/CI do not establish accuracy or SLA. Preserve previous failures, passes and images with their original environment.

### Historical material below
<!-- policy-assistant-managed:end -->

# Enforce the upload boundary before bytes become application data

**Unpublished technical draft, 2026-10-21. Based on actual source changes and local regressions; real S3 policy enforcement remains pending.**

The earlier upload session returned a presigned PUT. Submit checked size and MIME after the object had already reached S3. This left a cost/storage boundary open: an oversized object could be rejected by the application yet still occupy storage. Client-side size checks do not protect against a modified client.

The revised session uses `generate_presigned_post`, with server-selected bucket/key, exact `Content-Type: application/pdf`, success status 201, expiry 300 seconds and `content-length-range` from 5 bytes to 10 MiB. The frontend appends returned fields to FormData, appends the file last, and lets the browser generate the multipart boundary. It does not forward a Cognito Authorization header to S3. The signature/fields are temporary credentials and must not appear in report evidence.

Submit still validates the stored object. A signed MIME label is not proof that bytes are a PDF; the worker verifies the header/size. This is intentionally limited verification, not full parsing or malware detection. Submission freezes S3 version ID and an attempt ID. Reusing a valid upload capability can create a new current version, but cannot change the version already submitted. Download also signs that exact version, allowing the browser to compare original, worker and downloaded SHA-256.

Tests decode the generated POST policy to assert exact key/MIME and byte-range conditions. A frontend regression checks that all fields precede file and neither credentials nor manual multipart headers are forwarded. The local browser exercises FormData against the actual backend handlers and a simulated storage adapter, then compares downloaded bytes. Moto/local checks validate the request contract and application behavior; only a real S3 test proves AWS rejects oversized, expired or altered policies. Such a live test must use a bounded synthetic payload after account-owner approval.

One operational consequence is deployment coordination: backend protocol, S3 POST CORS and cached frontend must change together. Old cached PUT clients need refresh or coordinated rollback. Version retention also affects cost: current expiry creates delete markers, and older versions can remain after that. See [S3 POST policy conditions](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html) and the Workshop COST assumptions. Student review: explain where validation occurs, what overwriting means with versioning, and what `processed` does not guarantee.
