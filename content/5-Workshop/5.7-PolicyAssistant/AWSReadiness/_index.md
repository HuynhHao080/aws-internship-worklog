---
title: "AWS readiness after B4"
date: 2026-10-21
weight: 2
---

## Verified status — 23 October 2026

**B4 API/SDK: 19 PASS; original B5 browser: historical FAIL; B5-R7 browser: 10 PASS. GO for the accepted test-demo scope; NO-GO production.** B4 retained one browser NOT_RUN at that time; B5-R7 is separate browser evidence, not a rewrite of the historical result.

The legacy Cognito identity was **disabled and globally signed out (D3-A)**. Containment did not change its password or prove instant revocation of every offline-validated JWT or absence of unauthorized access. **D11 published sanitized application history; its first CI run FAILED**. **D15 published the test fix: [GitHub Actions 19/19 steps PASS](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39901988136), 0 FAIL/0 SKIPPED**. These are workflow steps, distinct from B4's 19 AWS API/SDK cases. The [D11 failed run](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39899173855) remains historical evidence. D17 changes documentation only, with no deployment or AWS E2E rerun.

Test demo: [https://d3ataim7dfsl7t.cloudfront.net/](https://d3ataim7dfsl7t.cloudfront.net/). Current public source: [a044936](https://github.com/HuynhHao080/aws-policy-assistant/tree/a04493682a417761f534954a1f8d38cc75d8a87b). New source/CI does not imply a new AWS deployment: the API uses the B5-R6 regional virtual-hosted S3 fix while Worker/Recovery retain their prior artifacts. IAM/CSP/CORS were not broadened. Keep local/mocked screenshots labelled accordingly.

The configured **account-wide Budget is 20 USD/month** (Actual 80% = 16 USD, Actual 100% and Forecasted 100% = 20 USD); the **10 USD tag-filtered test Budget is PENDING**. Budgets are not hard spending caps and do not stop resources; email delivery remains unverified. The approximately 6.6306 USD/month test estimate is an assumed-load model, not an invoice or a current AWS forecast.

The AWS test environment remains maintained: **no cleanup and no owner-approved demo end/cleanup date**. The C0 snapshot recorded 45 resources/4 Retain, approximately one diagnostic DLQ message/ALARM and zero SNS subscriptions; those are not freshly queried AWS metrics today. Retained resources can continue accruing charges. This is not production-ready.

Public evidence: [B4](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-test-e2e-b4-result.md), [B5-R7](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-browser-b5r7-result.md), [C0](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/cleanup-c0-inventory-and-acceptance.md). Containment is supported by filtered D3-A evidence without publishing the username/password/tokens. Git rewriting cannot erase old clones/caches. Student architecture authorship and attendance evidence remain student-confirmation tasks, not invented replacements.

### Earlier snapshots — historical, not current status

Pending/not-deployed/browser-incomplete statements and old security/publication gates below describe earlier plans/snapshots. Use the verified summary above for current reporting; keep the historical B5 FAIL. D17 does not publish the Worklog website.

> **B5-R local remediation:** real local Edge confirms CSP connect-src blocks the global S3 hostname. One-line virtual addressing fix makes regional POST/GET and local hash pipeline pass. 129 backend/17 frontend/3 browser fixture/1 full-stack local plus SAM lint/build PASS;9 live integration skips=NOT_RUN. No CSP/CORS/IAM/template/frontend change, artifact upload or deployment; B5 AWS remains FAIL. Original AWS cause is LIKELY without exact violation capture; local mechanism CONFIRMED. Separate artifact/change-set/deploy/rerun approvals required; cleanup C separate.

> **2026-10-22 update: B5 browser FAIL/STOPPED**, 2PASS/1FAIL/7NOT_RUN. Real Edge TestA login/search passed; upload UI failed after session200, before S3 upload. Likely regional-only CSP/global S3 signing-host mismatch; exact violation not captured. One orphan uploading metadata record, no S3 object; main0/QueueAge OK, DLQ1/ALARM. No fix/deploy/rerun/cleanup; separate approval required. B4 API/SDK PASS below is earlier evidence, not browser acceptance. No successful browser screenshot exists.

B1/B2/B3 completed; real B4 API/SDK has 19 PASS and one browser NOT_RUN. The 45-Add candidate was executed once; test stack is CREATE_COMPLETE. Historical AVAILABLE/BLOCKED connection snapshots are not the current deployment status. See the adjacent AWS E2E chapter for evidence.

Four Retain resources: PoliciesTable, DocumentMetadata, DocumentsBucket, HTTPS bucket policy. DLQ/QueueAge ALARM follows the controlled poison; the message remains. No cleanup or changes to manufacture PASS. Old stack metadata unchanged, no full drift scan. Zero runtime IAM Allow Resource *; three test-only log grants. USD20 Budget is not a hard cap; email delivery unverified. Further changes after B4 need independent approval.
