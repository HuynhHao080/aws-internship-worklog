---
title: "Draft 2 — bounded S3 uploads and immutable versions"
date: 2026-10-21
weight: 5
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

# Enforce the upload boundary before bytes become application data

**Unpublished technical draft, 2026-10-21. Based on actual source changes and local regressions; real S3 policy enforcement remains pending.**

The earlier upload session returned a presigned PUT. Submit checked size and MIME after the object had already reached S3. This left a cost/storage boundary open: an oversized object could be rejected by the application yet still occupy storage. Client-side size checks do not protect against a modified client.

The revised session uses `generate_presigned_post`, with server-selected bucket/key, exact `Content-Type: application/pdf`, success status 201, expiry 300 seconds and `content-length-range` from 5 bytes to 10 MiB. The frontend appends returned fields to FormData, appends the file last, and lets the browser generate the multipart boundary. It does not forward a Cognito Authorization header to S3. The signature/fields are temporary credentials and must not appear in report evidence.

Submit still validates the stored object. A signed MIME label is not proof that bytes are a PDF; the worker verifies the header/size. This is intentionally limited verification, not full parsing or malware detection. Submission freezes S3 version ID and an attempt ID. Reusing a valid upload capability can create a new current version, but cannot change the version already submitted. Download also signs that exact version, allowing the browser to compare original, worker and downloaded SHA-256.

Tests decode the generated POST policy to assert exact key/MIME and byte-range conditions. A frontend regression checks that all fields precede file and neither credentials nor manual multipart headers are forwarded. The local browser exercises FormData against the actual backend handlers and a simulated storage adapter, then compares downloaded bytes. Moto/local checks validate the request contract and application behavior; only a real S3 test proves AWS rejects oversized, expired or altered policies. Such a live test must use a bounded synthetic payload after account-owner approval.

One operational consequence is deployment coordination: backend protocol, S3 POST CORS and cached frontend must change together. Old cached PUT clients need refresh or coordinated rollback. Version retention also affects cost: current expiry creates delete markers, and older versions can remain after that. See [S3 POST policy conditions](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html) and the Workshop COST assumptions. Student review: explain where validation occurs, what overwriting means with versioning, and what `processed` does not guarantee.
