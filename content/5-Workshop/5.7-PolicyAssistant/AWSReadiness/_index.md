---
title: "AWS readiness after B4"
date: 2026-10-21
weight: 2
---

## Current D1 status —2026-10-22

**B4 API/SDK19PASS; original B5 browserFAIL retained; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO bounded test demo, NO-GO production.** D1 is local review, no AWS/E2E or website publishing.

Approximately1diagnostic DLQ message/ALARM remains; alarm SNS topic has0subscriptions. Account20USD Budget is not a cap; email delivery unverified. No cleanup. **Owner has not set the environment end date.** Rotation/revocation proof for credential literals in old Git history is required before public push.

The fix uses virtual S3 addressing for regional POST/GET, without broadening IAM/CSP/CORS. Real Edge R7 used2PDFs/1,218bytes,3logins/3logouts,27requests; not a load benchmark. Deployed API has new ZIP; Worker/Recovery retain old ZIP. Shared-CodeUri source SAM is not a replay of the API-only candidate.

Evidence is in the local application repo: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md. Do not assume files are already pushed. Local/mocked screenshots do not replace AWS proof. Student must draw/review/explain their own architecture with official AWS icons; assistant reference diagrams do not establish student authorship.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements below are superseded by the D1 summary; original B5FAIL is not rewritten as PASS.



> **B5-R local remediation:** real local Edge confirms CSP connect-src blocks the global S3 hostname. One-line virtual addressing fix makes regional POST/GET and local hash pipeline pass. 129 backend/17 frontend/3 browser fixture/1 full-stack local plus SAM lint/build PASS;9 live integration skips=NOT_RUN. No CSP/CORS/IAM/template/frontend change, artifact upload or deployment; B5 AWS remains FAIL. Original AWS cause is LIKELY without exact violation capture; local mechanism CONFIRMED. Separate artifact/change-set/deploy/rerun approvals required; cleanup C separate.

> **2026-10-22 update: B5 browser FAIL/STOPPED**, 2PASS/1FAIL/7NOT_RUN. Real Edge TestA login/search passed; upload UI failed after session200, before S3 upload. Likely regional-only CSP/global S3 signing-host mismatch; exact violation not captured. One orphan uploading metadata record, no S3 object; main0/QueueAge OK, DLQ1/ALARM. No fix/deploy/rerun/cleanup; separate approval required. B4 API/SDK PASS below is earlier evidence, not browser acceptance. No successful browser screenshot exists.

B1/B2/B3 completed; real B4 API/SDK has 19 PASS and one browser NOT_RUN. The 45-Add candidate was executed once; test stack is CREATE_COMPLETE. Historical AVAILABLE/BLOCKED connection snapshots are not the current deployment status. See the adjacent AWS E2E chapter for evidence.

Four Retain resources: PoliciesTable, DocumentMetadata, DocumentsBucket, HTTPS bucket policy. DLQ/QueueAge ALARM follows the controlled poison; the message remains. No cleanup or changes to manufacture PASS. Old stack metadata unchanged, no full drift scan. Zero runtime IAM Allow Resource *; three test-only log grants. USD20 Budget is not a hard cap; email delivery unverified. Further changes after B4 need independent approval.
