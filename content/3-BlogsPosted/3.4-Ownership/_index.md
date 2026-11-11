---
title: "Draft 1 — document ownership beyond JWT"
date: 2026-10-21
weight: 4
---

<!-- policy-assistant-managed:begin -->
## Primary application — documentation update, 6 November 2026

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

# Authentication does not establish document ownership

**Technical draft based on the current Policy Assistant source and local tests. Not published; publication URL/date pending student action.** No claim is made about a historical internship day or live AWS result.

An HTTP API authorizer can reject invalid Cognito JWTs while an application still exposes another user's document by ID. A document ID is an identifier, not permission. Policy Assistant therefore has two different access models: the policy catalogue is shared among authenticated users, but document metadata and files belong to one owner.

The trusted identity comes from API Gateway authorizer claims `sub`. The API ignores a caller-supplied owner and chooses the storage key itself. `_owned_document` requires the stored owner to match. An unknown document and another user's document both return 404, avoiding disclosure of existence. Legacy records without an owner are denied rather than assigned automatically. The ByOwner library query and cursor validation also bind pagination to the current subject; protecting only the detail route would leave enumeration possible.

The frontend clears personal document state on logout/401. This matters when Alice signs out and Bob uses the same browser. UI hiding is still insufficient: the API must deny Bob when he manually supplies Alice's known ID. Signed storage URLs are short-lived bearer capabilities; possession permits use until expiry, so screenshots and logs must never retain them. S3 does not independently validate Cognito JWTs on these requests.

The meaningful evidence is the owner-negative tests in `tests/unit/test_documents.py` plus `tests/frontend/local.e2e.cjs`: Alice uploads/downloads, Bob sees his empty library and receives 404 for Alice's ID. These use real application handlers with moto/local fake identity. They do **not** validate Cognito signature handling or the deployed IAM/CORS configuration. After an authorized deployment the student must repeat all protected document endpoints using two actual Cognito accounts, missing/expired/invalid JWTs and tampered cursors.

The design avoids granting browsers general S3 credentials and distinguishes the IAM service role from end-user ownership. Remaining limits include shareable temporary capabilities and administrator onboarding. See [HTTP API JWT authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html) for the gateway contract and the Workshop for the complete acceptance matrix. Student review: explain where the trusted identity originates, why 404 is used, and why CORS alone provides no ownership protection.
