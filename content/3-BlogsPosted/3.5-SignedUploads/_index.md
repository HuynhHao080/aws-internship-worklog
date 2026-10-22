---
title: "Draft 2 — bounded S3 uploads and immutable versions"
date: 2026-10-21
weight: 5
---

# Enforce the upload boundary before bytes become application data

**Unpublished technical draft, 2026-10-21. Based on actual source changes and local regressions; real S3 policy enforcement remains pending.**

The earlier upload session returned a presigned PUT. Submit checked size and MIME after the object had already reached S3. This left a cost/storage boundary open: an oversized object could be rejected by the application yet still occupy storage. Client-side size checks do not protect against a modified client.

The revised session uses `generate_presigned_post`, with server-selected bucket/key, exact `Content-Type: application/pdf`, success status 201, expiry 300 seconds and `content-length-range` from 5 bytes to 10 MiB. The frontend appends returned fields to FormData, appends the file last, and lets the browser generate the multipart boundary. It does not forward a Cognito Authorization header to S3. The signature/fields are temporary credentials and must not appear in report evidence.

Submit still validates the stored object. A signed MIME label is not proof that bytes are a PDF; the worker verifies the header/size. This is intentionally limited verification, not full parsing or malware detection. Submission freezes S3 version ID and an attempt ID. Reusing a valid upload capability can create a new current version, but cannot change the version already submitted. Download also signs that exact version, allowing the browser to compare original, worker and downloaded SHA-256.

Tests decode the generated POST policy to assert exact key/MIME and byte-range conditions. A frontend regression checks that all fields precede file and neither credentials nor manual multipart headers are forwarded. The local browser exercises FormData against the actual backend handlers and a simulated storage adapter, then compares downloaded bytes. Moto/local checks validate the request contract and application behavior; only a real S3 test proves AWS rejects oversized, expired or altered policies. Such a live test must use a bounded synthetic payload after account-owner approval.

One operational consequence is deployment coordination: backend protocol, S3 POST CORS and cached frontend must change together. Old cached PUT clients need refresh or coordinated rollback. Version retention also affects cost: current expiry creates delete markers, and older versions can remain after that. See [S3 POST policy conditions](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html) and the Workshop COST assumptions. Student review: explain where validation occurs, what overwriting means with versioning, and what `processed` does not guarantee.
