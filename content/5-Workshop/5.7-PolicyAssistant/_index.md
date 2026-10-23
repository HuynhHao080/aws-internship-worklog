---
title: "5.7 Policy Assistant — complete workshop"
date: 2026-10-21
weight: 7
---

## D4 local handover review — 2026-10-22

B4: **19 PASS**; original B5: **FAIL**; B5-R7: **10 PASS**. D3-A: **BLOCKED_PENDING_OWNER_OPERATIONAL_CONFIRMATION**. The D3-A2 owner checklist is prepared, not answered; no disable/sign-out or credential reset has occurred. Public push/Pages and cleanup remain NO-GO pending separate gates/approval. D4 uses saved evidence, not fresh AWS health checks or new browser tests. Application-local files: `docs/d4-final-readiness-review.md`, `docs/d4-internship-project-summary.md`, `docs/d3a2-owner-operational-confirmation.md`; do not assume these files are published.


## Verified status — 23 October 2026

**B4 API/SDK: 19 PASS; original B5 browser: historical FAIL; B5-R7 browser: 10 PASS. GO for the accepted test-demo scope; NO-GO production.** B4 retained one browser NOT_RUN at that time; B5-R7 is separate browser evidence, not a rewrite of the historical result.

The legacy Cognito identity was **disabled and globally signed out (D3-A)**. Containment did not change its password or prove instant revocation of every offline-validated JWT or absence of unauthorized access. **D11 published sanitized application history; its first CI run FAILED**. **D15 published the test fix: [GitHub Actions 19/19 steps PASS](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39901988136), 0 FAIL/0 SKIPPED**. These are workflow steps, distinct from B4's 19 AWS API/SDK cases. The [D11 failed run](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39899173855) remains historical evidence. D17 changes documentation only, with no deployment or AWS E2E rerun.

Test demo: [https://d3ataim7dfsl7t.cloudfront.net/](https://d3ataim7dfsl7t.cloudfront.net/). Current public source: [a044936](https://github.com/HuynhHao080/aws-policy-assistant/tree/a04493682a417761f534954a1f8d38cc75d8a87b). New source/CI does not imply a new AWS deployment: the API uses the B5-R6 regional virtual-hosted S3 fix while Worker/Recovery retain their prior artifacts. IAM/CSP/CORS were not broadened. Keep local/mocked screenshots labelled accordingly.

The configured **account-wide Budget is 20 USD/month** (Actual 80% = 16 USD, Actual 100% and Forecasted 100% = 20 USD); the **10 USD tag-filtered test Budget is PENDING**. Budgets are not hard spending caps and do not stop resources; email delivery remains unverified. The approximately 6.6306 USD/month test estimate is an assumed-load model, not an invoice or a current AWS forecast.

The AWS test environment remains maintained: **no cleanup and no owner-approved demo end/cleanup date**. The C0 snapshot recorded 45 resources/4 Retain, approximately one diagnostic DLQ message/ALARM and zero SNS subscriptions; those are not freshly queried AWS metrics today. Retained resources can continue accruing charges. This is not production-ready.

Public evidence: [B4](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-test-e2e-b4-result.md), [B5-R7](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-browser-b5r7-result.md), [C0](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/cleanup-c0-inventory-and-acceptance.md). Containment is supported by filtered D3-A evidence without publishing the username/password/tokens. Git rewriting cannot erase old clones/caches. Student architecture authorship and attendance evidence remain student-confirmation tasks, not invented replacements.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements and old security/publication gates below describe earlier plans/snapshots. Use the verified summary above for current reporting; keep the historical B5 FAIL. D17 does not publish the Worklog website.

> Evidence update 2026-10-21: B1 stack/B2 frontend/B3 users completed; B4 API/SDK 19 PASS, real browser NOT_RUN. See AWS E2E chapter. Pending/not-deployed statements below are pre-B4 plans/history, not current status. Do not repeat poison or live demo writes without further approval.

# Authenticated policy retrieval and private PDF verification

Reviewed against local source/IaC on 2026-10-21. Application baseline `80c4eef`, Worklog `846d2ff`, plus uncommitted changes. These baselines alone do not contain the complete current implementation. This workshop is for `HuynhHao080/aws-policy-assistant`. The older 5.1–5.6 S3/VPC Endpoint lab is independent and must not be deployed as an application dependency. AWS live acceptance remains pending.

## 1. Outcome and prerequisites

The product provides native Cognito login, shared policy ID/keyword lookup, an owner-isolated document library and PDF upload → asynchronous byte verification → hash-checked download. There is no AI, OCR, RAG, full PDF parser or malware scanner. `processed` means the submitted version passed size/header verification and has a SHA-256.

Use Git, Python 3.12, Node.js 22+, npm, AWS CLI v2, SAM CLI and Hugo 0.167.0. Browser tests use installed Edge on Windows or `npx playwright install --with-deps chromium` on Linux. AWS later requires a reviewed sandbox account, short-lived CLI credentials and owner-approved deployment permissions. Rotate previously committed demo passwords first; deleting a string does not revoke credentials. Do not show secrets in screenshots.

From the **application repository root** on Windows, inspect an existing environment before recreating it:

```powershell
uv venv --python 3.12 .venv
uv pip install --python .venv/Scripts/python.exe -r policy_assistant/requirements.txt -r tests/requirements.txt
npm ci
.venv/Scripts/python.exe -m scripts.verify
.venv/Scripts/python.exe -m scripts.demo_server
```

Alternatively use `pwsh -File scripts/start-demo.ps1`. Linux: `python3.12 -m venv .venv`, `.venv/bin/python -m pip install -r policy_assistant/requirements.txt -r tests/requirements.txt`, then use `.venv/bin/python` for commands above. Local browser tests use a temporary server; the manual demo normally prints `http://127.0.0.1:8080`. Keep the **OFFLINE** banner visible. Alice/Bob are fake identities without passwords; AWS state is simulated and lost on shutdown. This URL is not a deployed demo link.

## 2. Actual architecture and data flow

| Component | Actual role | Reason / tradeoff |
|---|---|---|
| CloudFront, frontend S3, OAC | HTTPS static UI with private origin | Managed hosting; coordinated config/cache rollout |
| Cognito Lite and HTTP API | Login/challenges/refresh; gateway JWT issuer/audience validation | Managed identity; administrator onboarding, MFA defaults OFF |
| API Lambda Python 3.12 | Validation, ownership, search and document API | No idle VM; separate SDK role |
| DynamoDB PoliciesTable | Shared authenticated policy catalogue | On-demand GetItem/bounded Scan; not large-scale full-text search |
| DynamoDB DocumentMetadata | Owner/state/version/attempt, ByOwner and ByStatus indexes | Conditional writes and bounded queries; eventual index consistency |
| Private versioned document S3 | Exact submitted PDF version, signed POST/GET | Direct transfer; short-lived bearer capability |
| SQS Standard, worker, DLQ | Hash/header/size verification; retries and failed-work buffer | At-least-once delivery; conditional attempt prevents stale writes |
| EventBridge rule, recovery Lambda | Re-enqueue due pending attempts every 2 minutes | Repairs metadata-to-queue crash gap; not a cross-service transaction |
| CloudWatch, SNS | 30-day JSON logs, dashboard, five alarms, optional confirmed email | Small operational baseline; no delivery proof yet |

Browser loads CloudFront UI and signs into Cognito. HTTP API validates JWT; Lambda receives trusted `sub` from authorizer claims, never owner identity from request body. `/health` is public liveness only. Policy catalogue is shared; documents are private. API chooses key, creates owned session and signs POST fields constrained to that key, PDF MIME and **5 bytes–10 MiB**. Browser appends every field, then file last, without Cognito Authorization or a manually set multipart header.

Submit checks S3 metadata, freezes version/attempt, writes pending then sends SQS. Scheduled recovery queries due pending metadata if a crash leaves a send gap. Worker reads only the frozen version and conditionally updates the current attempt. Duplicate success/stale messages have no harmful state change. Invalid PDF is permanently failed/acknowledged. Transient per-record errors return `batchItemFailures`; the event mapping enables `ReportBatchItemFailures`. Worker timeout is 30 s, visibility 180 s, max receives 3, DLQ retention 14 days. Browser polls, requests signed version-specific GET and compares original/worker/download SHA-256 before success.

**Student diagram pending.** Download [official AWS icons](https://aws.amazon.com/architecture/icons/), draw the diagram yourself and keep editable source/revision history. Application `docs/architecture-self-draw.md` lists resource IDs, every arrow, trust boundary, decisions and defense checklist. Draw internet/browser, global edge, regional ap-southeast-1 managed services, API authentication, owner authorization, SDK roles, S3 bearer capabilities, retries/DLQ/recovery and monitoring. No VPC/NAT/AI service exists in this template. Existing assistant Mermaid/PNG is reference material, not student-authored submission evidence. Insert your own diagram, authorship and date here when completed.

Well-Architected review: Security—owner checks, scoped IAM, private encrypted TLS-only buckets, constrained upload; Reliability—immutable versions, conditional attempts, partial batches, DLQ/recovery; Operations—verifier/runbook/logs/alarms; Cost—on-demand resources, dated load model and explicit retained versions. Performance—bounded Scan remains small-dataset only. Sustainability—avoid unused infrastructure and retained artifacts. Live fault recovery, restore tests and measured latency remain unverified. This is not AWS certification.

## 3. Prepare a deployment review locally

Preserve current user changes. Inspect `git status --short`, `git diff -- template.yaml`, `docs/deployment-review.md`, `docs/operations.md` and the COST chapter. From app root:

```powershell
$env:PATH = "$PWD/.venv/Scripts;" + $env:PATH
sam validate --lint
sam build
.venv/Scripts/python.exe -m scripts.seed_policies
.venv/Scripts/python.exe -m scripts.estimate_cost
```

Seed defaults to preview and makes no AWS call. The verifier removes live-test opt-in variables and records `docs/evidence/offline-acceptance.json`. A skip is not a pass. No AWS change set was created by local checks.

## 4. Deploy only after account-owner authorization

The following are **future operator steps**, not actions already performed. Privately verify CLI profile/account with `aws sts get-caller-identity` and region `ap-southeast-1`. Review cost, IAM, replacements/deletions, retention and rollback first.

```powershell
sam deploy --guided --stack-name policy-assistant --region ap-southeast-1 --no-execute-changeset
```

Even this command uploads artifacts/creates a change set and needs AWS-write authorization. Inspect the real change set, then execute that reviewed change set via console only after approval. A fresh table uses `EnableOwnerIndex=true`. A legacy metadata table without GSIs needs first false to create ByStatus, wait ACTIVE, then true in a second reviewed update for ByOwner. **Never switch false on an existing ByOwner index.** Listing is unavailable during the intermediate stage. Do not recreate data tables/buckets.

Read stack outputs PolicyAssistantApiUrl, CognitoUserPoolId, CognitoClientId, FrontendUrl, FrontendBucketName and FrontendDistributionId. Then:

```powershell
.venv/Scripts/python.exe -m scripts.export_frontend_config --stack policy-assistant --region ap-southeast-1
.venv/Scripts/python.exe -m scripts.seed_policies --stack policy-assistant --region ap-southeast-1 --apply
```

Config export reads AWS and writes only public API URL/region/client ID. Seed writes only missing synthetic policy IDs to the stack's PoliciesTableName; existing values are preserved. The owner must authorize the write and verify target account. In Cognito console provision two test users administratively, coordinate passwords privately, test NEW_PASSWORD_REQUIRED and refresh/logout. Client has no secret and supports USER_PASSWORD_AUTH/REFRESH_TOKEN_AUTH. Do not put passwords in CLI arguments or recordings.

Publish exactly `index.html`, `app.js`, `styles.css`, `config.json` from frontend/ to the output bucket. Example operator command: `aws s3 cp frontend/app.js s3://<FrontendBucketName>/app.js`, repeated for the four files. Replace placeholders with reviewed real outputs; do not execute angle brackets. Never sync repository root. Invalidate named assets using `aws cloudfront create-invalidation --distribution-id <FrontendDistributionId> --paths /index.html /app.js /styles.css /config.json`. Publishing also requires authorization. Open FrontendUrl and run live acceptance.

API CORS includes localhost:3000, 127.0.0.1:3000, LocalFrontendOrigin (default localhost:8080) and generated HTTPS frontend. Document S3 CORS allows LocalFrontendOrigin/generated HTTPS frontend and POST/GET. Configure any different origin deliberately; CORS is not authorization. Roll out backend, S3 POST CORS and frontend together; cached old PUT clients need reload. Preserve prior artifact/config for rollback.

## 5. Testing, demo and evidence

| Action | Expected outcome | Local evidence | AWS status |
|---|---|---|---|
| Alice login; lookup POL-001 and keyword | Correct catalogue result | Browser real local handlers | Pending Cognito/API proof |
| Signed POST PDF, submit, poll, download | processed and equal original/worker/download hash | Policy/unit + browser downloaded bytes | Pending real S3/SQS proof |
| Bob lists/reads/submits Alice's ID | Own library excludes document; private routes 404 | Ownership unit/browser regression | Pending real two-user proof |
| Bad JSON/ID/MIME/size/key | 400/404/409, no unauthorized mutation | Parameterized unit checks | Pending real storage boundary |
| Mixed batch with transient failure | Failed ID only; other record succeeds | Worker/mapping regression | Pending retry/redrive |
| Invalid bytes, duplicate or stale attempt | Failed input or harmless no-op | State/version unit checks | Pending controlled live test |
| HTTP error, polling timeout, bad hash | No saved file or false success | Frontend tests | Relevant local result only |

`scripts.verify` runs fatal-category Python lint, mypy on app.py, JS syntax, unit/frontend tests, two browser suites and SAM lint/build; it does not claim whole-repo strict typing. AWS integration tests require explicit opt-in and are not counted as live success. For live proof record actual revision/diff, date/region, expected/actual results, hash and CloudWatch time window. Verify missing/invalid/expired JWT, CORS, tampered/expired signed POST, overwrite after submit, duplicate/retry, recovery and controlled DLQ. Fault injection needs its own approved safe plan. Never capture Authorization, signed URL/fields, password or browser Network request bodies.

5–7 minute demo: problem/scope → your diagram → login/search/upload/hash/library → second-user rejection → queue/log/metric evidence → COST/limits. Use app `docs/demo-script.md`. A local offline product demo is useful evidence but does not satisfy proof of an AWS deployment.

## 6. Monitoring and troubleshooting

Dashboard separates API counters, worker/recovery counters, queue/DLQ visible/inflight/oldest age and average Lambda duration. Five alarms: API Errors, API maximum Duration, recovery Errors, queue age and DLQ visible messages. Optional SNS email requires supplied AlertEmail and recipient confirmation; no email has been sent by this work.

Partial record errors may leave Lambda Errors zero; inspect safe `Worker record failed` logs plus SQS age/backlog/DLQ. Caught API 500 responses may also not increment Lambda Errors, so inspect HTTP results/safe logs. This is a baseline, not a production SLO.

| Symptom | Check and action |
|---|---|
| 401 | Check issuer/audience/client/region/token expiry; login/refresh, never disable authorization |
| Document 404 | Check owning user/ID; never modify owner_sub to bypass protection |
| Upload 403/CORS | Exact origin, fields/key/MIME/expiry; new session, file last, no JWT/manual multipart header |
| Submit 409/503 | Check uploaded object/state/versioning/queue; retry same pending ID; invalid PDF needs new upload |
| Empty recent library | Check GSI ACTIVE/eventual consistency and two-phase upgrade; do not recreate table |
| Long pending | Check queue age/worker/recovery logs and DLQ; diagnose dependency, no blind purge |
| Hash mismatch | Stop success/save, verify exact version and bytes; never disable comparison |
| Old upload code | Check deployed four files and CloudFront cache, coordinate rollout/invalidation |

DLQ action: inspect sanitized failure/attempt, repair dependency, choose authorized owner retry or controlled redrive. Older attempts cannot overwrite newer state. A database outage can prevent failed-state recording; UI alone is insufficient.

## 7. COST, budgets and cleanup

Read child **COST** page for dated official rates, per-service quantities, gross monthly table, sensitivity and Calculator inputs. No Calculator estimate/share URL exists yet. Set budget only after approval; no notifications are sent automatically by this local work.

Current retention: Lambda logs 30 days, DLQ 14 days, S3 current expiry 365 days and noncurrent expiry another 365 after becoming noncurrent. Versioned bytes may remain roughly 730 days; do not claim all data is deleted at 365 days. Review retained S3/DynamoDB, versions/delete markers/backups/logs/CloudFront/SNS after teardown. `sam delete` alone may leave retained/nonempty resources. Obtain permission to export/delete data; do not use blind recursive bucket deletion. Record retained inventory and verify Billing after approved cleanup. `docs/deployment-review.md` contains rollback/retention gates.

## 8. Build the report website

In the Worklog repository: `hugo --destination <temporary-output-directory>` with Hugo 0.167.0, then run application `python -m scripts.check_worklog_links --site-root <temporary-output-directory>`. This checks generated local links, not publication or all external sites. Student must add real attendance/events, confirmed dates/daily evidence, publication links and their own architecture before claiming submission readiness.


## Actual AWS preparation, 2026-10-21

Read-only account/region/existing-stack inspection and an isolated unexecuted test change set now exist. Read child **AWS test deployment — pending approval** and **Demo — 5 to 7 minutes**. New-release AWS E2E/endpoint is still pending; no premature PASS.


Current hardening revision: 128 backend tests, 17 frontend tests and two local browser suites passed. The 45-resource candidate is NOT EXECUTED; fresh AWS/artifact checks are blocked by network. See [AWS readiness](AWSReadiness/).
