---
title: "Draft 1 — document ownership beyond JWT"
date: 2026-10-21
weight: 4
---

# Authentication does not establish document ownership

**Technical draft based on the current Policy Assistant source and local tests. Not published; publication URL/date pending student action.** No claim is made about a historical internship day or live AWS result.

An HTTP API authorizer can reject invalid Cognito JWTs while an application still exposes another user's document by ID. A document ID is an identifier, not permission. Policy Assistant therefore has two different access models: the policy catalogue is shared among authenticated users, but document metadata and files belong to one owner.

The trusted identity comes from API Gateway authorizer claims `sub`. The API ignores a caller-supplied owner and chooses the storage key itself. `_owned_document` requires the stored owner to match. An unknown document and another user's document both return 404, avoiding disclosure of existence. Legacy records without an owner are denied rather than assigned automatically. The ByOwner library query and cursor validation also bind pagination to the current subject; protecting only the detail route would leave enumeration possible.

The frontend clears personal document state on logout/401. This matters when Alice signs out and Bob uses the same browser. UI hiding is still insufficient: the API must deny Bob when he manually supplies Alice's known ID. Signed storage URLs are short-lived bearer capabilities; possession permits use until expiry, so screenshots and logs must never retain them. S3 does not independently validate Cognito JWTs on these requests.

The meaningful evidence is the owner-negative tests in `tests/unit/test_documents.py` plus `tests/frontend/local.e2e.cjs`: Alice uploads/downloads, Bob sees his empty library and receives 404 for Alice's ID. These use real application handlers with moto/local fake identity. They do **not** validate Cognito signature handling or the deployed IAM/CORS configuration. After an authorized deployment the student must repeat all protected document endpoints using two actual Cognito accounts, missing/expired/invalid JWTs and tampered cursors.

The design avoids granting browsers general S3 credentials and distinguishes the IAM service role from end-user ownership. Remaining limits include shareable temporary capabilities and administrator onboarding. See [HTTP API JWT authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html) for the gateway contract and the Workshop for the complete acceptance matrix. Student review: explain where the trusted identity originates, why 404 is used, and why CORS alone provides no ownership protection.
