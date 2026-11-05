---
title: "Draft 1 — document ownership beyond JWT"
date: 2026-10-21
weight: 4
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

# Authentication does not establish document ownership

**Technical draft based on the current Policy Assistant source and local tests. Not published; publication URL/date pending student action.** No claim is made about a historical internship day or live AWS result.

An HTTP API authorizer can reject invalid Cognito JWTs while an application still exposes another user's document by ID. A document ID is an identifier, not permission. Policy Assistant therefore has two different access models: the policy catalogue is shared among authenticated users, but document metadata and files belong to one owner.

The trusted identity comes from API Gateway authorizer claims `sub`. The API ignores a caller-supplied owner and chooses the storage key itself. `_owned_document` requires the stored owner to match. An unknown document and another user's document both return 404, avoiding disclosure of existence. Legacy records without an owner are denied rather than assigned automatically. The ByOwner library query and cursor validation also bind pagination to the current subject; protecting only the detail route would leave enumeration possible.

The frontend clears personal document state on logout/401. This matters when Alice signs out and Bob uses the same browser. UI hiding is still insufficient: the API must deny Bob when he manually supplies Alice's known ID. Signed storage URLs are short-lived bearer capabilities; possession permits use until expiry, so screenshots and logs must never retain them. S3 does not independently validate Cognito JWTs on these requests.

The meaningful evidence is the owner-negative tests in `tests/unit/test_documents.py` plus `tests/frontend/local.e2e.cjs`: Alice uploads/downloads, Bob sees his empty library and receives 404 for Alice's ID. These use real application handlers with moto/local fake identity. They do **not** validate Cognito signature handling or the deployed IAM/CORS configuration. After an authorized deployment the student must repeat all protected document endpoints using two actual Cognito accounts, missing/expired/invalid JWTs and tampered cursors.

The design avoids granting browsers general S3 credentials and distinguishes the IAM service role from end-user ownership. Remaining limits include shareable temporary capabilities and administrator onboarding. See [HTTP API JWT authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html) for the gateway contract and the Workshop for the complete acceptance matrix. Student review: explain where the trusted identity originates, why 404 is used, and why CORS alone provides no ownership protection.
