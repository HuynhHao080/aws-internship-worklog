---
title: "COST — official AWS rates"
weight: 1
date: 2026-10-21
---

<!-- policy-assistant-managed:begin -->
## Primary application — documentation update, 6 November 2026

**Primary application model:** Global Claude Haiku 5.5. The application ranks extracted PDF and policy text by keywords. Dated verification reports record their respective releases. [Model cutover evidence](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/haiku55-migration.md).

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

## Primary deployment estimate · 12 November 2026

**USD 7.10/month baseline and USD 11.97/month higher load.** This 30-day model uses assumed workloads and public rates before free allowances, credits and tax. Use these inputs for AWS Pricing Calculator and compare with CloudWatch after collecting real usage. Regional services use Singapore rates. CloudFront uses a Europe edge scenario for PriceClass_100. [AWS Price List API](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_pricing_GetProducts.html).

| Item | Unit | Baseline | Higher load | USD/unit | SKU or source |
|---|---|---:|---:|---:|---|
| API Gateway HTTP API | request | 20000 | 40000 | .00000125 | HTX6VZU3YCMMNRM5 |
| Lambda | request | 64700 | 87700 | .0000002 | 36D3E7396ZKUSWWW |
| Lambda x86 | GB-s | 5945 | 10720 | .0000166667 | 67ZV6RMYY72YSRDC |
| DynamoDB on-demand | RRU | 250000 | 750000 | .0000001425 | XF4WN9CQUFPSY9SQ |
| DynamoDB on-demand | WRU | 50000 | 150000 | .00000071 | PZTVP859DVDCGT8X |
| DynamoDB Standard | GB-month | .10 | .30 | .285 | YCMS8QPJ7DRVHAXD |
| DynamoDB PITR | GB-month | .10 | .30 | .228 | GSSDTS2TPQ4RHW4R |
| S3 Standard | GB-month | 2 | 6 | .025 | M6MTARCQFUQBQ2V3 |
| S3 PUT / COPY / POST / LIST | request | 2000 | 6000 | .000005 | VHRJWKRHEA6VHEFY |
| S3 GET and other | request | 5000 | 15000 | .0000004 | T469UNPYX78434EG |
| API / S3 → Internet | GB | 1 | 3 | .12 | SDHP4R7WGBVJPQPY |
| CloudFront HTTPS (Europe) | request | 10000 | 30000 | .0000012 | YA5QZD7XG45UGHCH |
| CloudFront egress (Europe) | GB | 1 | 3 | .085 | CloudFront |
| SQS Standard | request | 300000 | 600000 | .0000004 | PVF7MDBNRK7V9S5H |
| Cognito Lite | MAU | 50 | 150 | .0055 | CJUNATY8K3J8WJ5U |
| CloudWatch Logs ingest | GB | 1 | 3 | .70 | ZEKCE7VGMSM5XZPW |
| CloudWatch Logs retained | GB-month | .5 | 1.5 | .03 | RKCBFFXUWXG8KUQY |
| CloudWatch standard alarm | alarm-month | 9 | 9 | .10 | ERS2PGV5BX4XH4WQ |
| CloudWatch dashboard | dashboard-month | 1 | 1 | 3 | CloudWatch |
| SNS API | request | 100 | 300 | .0000005 | UNWF5P8PEVEH2AZ2 |
| SNS email | delivery | 100 | 300 | .00002 | SNS |
| Claude Haiku 5.5 Global input | MTok | 8 | 16 | .10 | 82DP2ZQEH38W5NDX |
| Claude Haiku 5.5 Global output | MTok | .9 | 1.8 | .50 | 58YZ69WTV49A9DVA |
| Textract AsyncTextPagesProcessed | page | 200 | 600 | .0015 | VDM7FRY6BHABK2PJ |

- Lambda includes 20,000/40,000 API calls at 512 MiB. Assume 3 seconds for 1,000/2,000 AI calls and 0.25 seconds for the others. Add 43,200 recovery calls at 256 MiB × 0.15 seconds and 1,500/4,500 background calls at 512 MiB × 0.6 seconds. The recovery schedule stays constant. Durations are assumptions.
- DynamoDB request units account for scans, transactions, indexes and retries. S3 storage includes PDFs, versions, quarantine, extracted text and web assets. Direct API/PDF egress is separate from CloudFront traffic. SQS includes polling and retries.
- The deployed stack has nine alarms. The estimate includes one dashboard, log volume and SNS email deliveries. Recovery rule target invocations are included under Lambda. The USD 50 budget is an alert threshold. Taxes, paid support and workloads outside these scenarios are excluded.
- Enter the units above into [AWS Pricing Calculator](https://calculator.aws/). For unavailable model or OCR dimensions, add the two token rows and OCR row separately and check for double counting. The Calculator share link or export is pending.

Public sources for rows without a SKU are [CloudFront](https://aws.amazon.com/cloudfront/pricing/pay-as-you-go/), [CloudWatch](https://aws.amazon.com/cloudwatch/pricing/), [SNS](https://aws.amazon.com/sns/faqs/) and [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/pricing/).

# Monthly cost planning — verified AWS rates, 2026-10-21

Region: **Asia Pacific (Singapore), ap-southeast-1**. Currency: USD, 30-day month. This is a planning model, not measured consumption or an AWS invoice. Run `python -m scripts.estimate_cost` to reproduce the itemized table in `cost/monthly-table.md` (cost/monthly-table.md; application repository). The complete per-service Calculator input quantities, SKU, rate dimensions, publication dates, source URLs and catalogue SHA-256 are in `cost/aws-rates-2026-10-21.json` (cost/aws-rates-2026-10-21.json; application repository). Result: `cost/estimate-result.json` (cost/estimate-result.json; application repository).

## Load and Calculator inputs

100 monthly active users; 100,000 HTTP API calls (20,000 bounded keyword scans, 77,000 exact policy lookups, 3,000 document/library/status calls). 500 PDF uploads of 2 MiB; 1,000 downloads of 2 MiB. Direct regional internet egress allowance in this model is 2.1 GB including API responses. Upload ingress is free. CloudFront serves the static UI: 20,000 HTTPS requests, 5 GB outbound, modeled at Europe edge prices because the template uses PriceClass_100. Actual edge usage and response sizes change the result. S3-to-CloudFront origin transfer is not counted again as direct internet egress.

Lambda x86: API 100,000 × 0.2 s × 0.125 GB; worker 500 × 1 s × 0.25 GB; scheduled recovery 21,600 × 0.1 s × 0.125 GB. Total 122,100 invocations and 2,895 GB-seconds. Durations are assumptions; replace with CloudWatch measurements after an approved deployment. The legacy EventBridge scheduled rule runs every 2 minutes, not EventBridge Scheduler; no custom events or Scheduler workload is purchased. Its target Lambda execution is included above. See [EventBridge pricing](https://aws.amazon.com/eventbridge/pricing/) and [scheduled rule documentation](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule-schedule.html).

DynamoDB on-demand: 1,050,000 read request units and 10,000 write request units including both GSI writes; 0.1 GB base + index storage; PITR on 0.05 GB base data. This is a conservative dataset model, not a benchmark. A 4 KiB eventually consistent item read costs 0.5 RRU; Scan reads data before filtering. Scan pages, growing policy data and library polling can increase consumption substantially. Measure consumed capacity; if search grows, reconsider indexing rather than adding an expensive search service to this demo.

S3: 25 GB document versions plus 0.05 GB frontend = 25.05 GB Standard stock, 1,000 POST/PUT/LIST, 5,000 GET/HEAD including worker reads, validation, origin and downloads. Stock includes retained older versions, not just new monthly uploads. Standard SQS: 300,000 requests, including Lambda idle polling and retry reserve; message payload is below 64 KiB. SNS gross reserve: 100 publishes and 100 emails, only if a student confirms an alert subscription. CloudWatch: 1 GB logs ingested, 1 GB retained, 5 standard alarms, 1 dashboard. Cognito **Lite**, 100 MAU; no SMS, advanced security or machine-to-machine token service is assumed.

AI/Bedrock/OpenSearch/OCR cost: **0, not implemented**. VPC/NAT Gateway/EC2/RDS: **0, not provisioned by this template**. The document worker validates bytes and hashes; the product must not be presented as an AI policy reasoning service.

## Gross, allowances and limitations

The itemized total applies paid marginal list rates to all modeled usage **before** account-specific allowances, Free Tier, promotional credits and taxes. DynamoDB storage/SNS catalogues have zero-priced initial ranges; their paid marginal rates are deliberately applied to all planning units to expose cost without those allowances. Cognito, CloudFront, Lambda, SQS and CloudWatch may also have applicable allowances. Eligibility, shared account consumption and account plan must be checked in Billing; this document does not assume a zero bill. Support plans, domain registration, optional SMS, dashboard API retrieval outside the small console demo, VAT and exchange-rate charges are excluded. AWS Pricing Calculator **has not been created**; no estimate/share URL exists. Enter these quantities at [AWS Pricing Calculator](https://calculator.aws/), save the actual estimate, and attach its URL/export only after it exists. Calculator may automatically apply allowances; preserve both gross and net views.

For sensitivity use the generated 10× scenario, with fixed schedule and alarm/dashboard/SNS baseline. That scenario scales data stock, MAU, calls, downloads and logs together; it is a budget stress model, not a traffic forecast. Free allowances and price-tier transitions are not applied. Largest avoidable drivers are retained S3 versions, scan volume, repeated polling/retries, overly verbose logs, internet downloads and the fixed dashboard/alarm baseline.

## Budget, retention and cleanup

The configured `policy-assistant-account-monthly` Budget is **20 USD/month, account-wide**, without region/service/tag filters, using UnblendedCost and excluding Credit/Refund. Alerts are Actual 80% (16 USD), Actual 100% (20 USD) and Forecasted 100% (20 USD). No Budget Actions; the existing 5 USD Budget is unchanged.

The **10 USD/month tag-filtered test Budget remains PENDING**, because cost-allocation tag eligibility has not been verified. The 20 USD Budget is not a hard spending cap, does not stop services and is not extra AWS credits. Billing/forecast can lag; email delivery is unverified. Do not publish recipient addresses. D17 documents saved evidence without changing Budgets or reading fresh Billing data.

The test environment remains maintained: **no cleanup and no owner-approved end date**. C0/cost snapshots are not a current invoice. See [Budgets pricing](https://aws.amazon.com/aws-cost-management/aws-budgets/pricing/) when reviewing future changes; no paid actions/reports are enabled by this documentation.

Current IaC: 30-day Lambda logs; SQS DLQ 14 days; current object expiry 365 days and noncurrent expiry 365 days. In a versioned bucket, current expiry creates a delete marker, so a version can remain roughly another year after becoming noncurrent. **This is not a guarantee that all bytes disappear after 365 days.** Do not shorten existing data retention without owner approval. Frontend static bucket and retained DynamoDB/S3 resources require explicit post-stack cleanup review.

Before teardown export required policy/document/report evidence with owner approval. Inspect CloudFormation retention policies, S3 versions/delete markers, DynamoDB backups/PITR, log groups, CloudFront and SNS subscriptions. `sam delete` alone cannot be assumed to remove nonempty or retained buckets/tables. Do not run recursive bucket deletion as a casual workshop step. Use the approval/rollback checklist in `deployment-review.md` (deployment-review.md; application repository), record retained resource inventory and inspect Billing after deletion.

## Official price sources

Rates were downloaded from the public [AWS Price List bulk API](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/price-changes.html), with full URLs and effective/publication dates recorded per row in the snapshot. Public catalogue queries require no AWS credentials. Human-readable supporting pages: [Lambda](https://aws.amazon.com/lambda/pricing/), [API Gateway](https://aws.amazon.com/api-gateway/pricing/), [DynamoDB](https://aws.amazon.com/dynamodb/pricing/on-demand/), [S3](https://aws.amazon.com/s3/pricing/), [SQS](https://aws.amazon.com/sqs/pricing/), [SNS](https://aws.amazon.com/sns/pricing/), [CloudWatch](https://aws.amazon.com/cloudwatch/pricing/), [Cognito](https://aws.amazon.com/cognito/pricing/), [CloudFront](https://aws.amazon.com/cloudfront/pricing/). Refresh dated prices before a later deployment; do not label this as a future quote.


## Itemized model

| Component | Monthly quantity | USD/unit | USD/month |
|---|---:|---:|---:|
| Lambda requests | 122100 Request | 0.0000002000 | 0.024420 |
| Lambda x86 GB-seconds | 2895 Lambda-GB-Second | 0.0000166667 | 0.048250 |
| HTTP API requests | 100000 Requests | 0.0000012500 | 0.125000 |
| DynamoDB read request units | 1050000 ReadRequestUnits | 0.0000001425 | 0.149625 |
| DynamoDB write request units | 10000 WriteRequestUnits | 0.0000007100 | 0.007100 |
| DynamoDB storage | 0.1 GB-Mo | 0.2850000000 | 0.028500 |
| DynamoDB PITR | 0.05 GB-Mo | 0.2280000000 | 0.011400 |
| S3 Standard storage including versions | 25.05 GB-Mo | 0.0250000000 | 0.626250 |
| S3 POST PUT LIST requests | 1000 Requests | 0.0000050000 | 0.005000 |
| S3 GET HEAD requests | 5000 Requests | 0.0000004000 | 0.002000 |
| SQS Standard requests including idle polls | 300000 Requests | 0.0000004000 | 0.120000 |
| SNS publish | 100 Requests | 0.0000005000 | 0.000050 |
| SNS email notifications | 100 Notifications | 0.0000200000 | 0.002000 |
| CloudWatch log ingestion | 1 GB | 0.7000000000 | 0.700000 |
| CloudWatch log storage | 1 GB-Mo | 0.0300000000 | 0.030000 |
| CloudWatch standard alarms | 5 Alarms | 0.1000000000 | 0.500000 |
| CloudWatch dashboard | 1 Dashboards | 3.0000000000 | 3.000000 |
| Cognito Lite monthly active users | 100 CognitoUserPoolsMAU | 0.0055000000 | 0.550000 |
| CloudFront HTTPS Europe | 20000 Requests | 0.0000012000 | 0.024000 |
| CloudFront egress Europe GB | 5 GB | 0.0850000000 | 0.425000 |
| Singapore direct internet egress GB | 2.1 GB | 0.1200000000 | 0.252000 |
| **Total before allowances/credits/tax** | | | **6.6306** |
