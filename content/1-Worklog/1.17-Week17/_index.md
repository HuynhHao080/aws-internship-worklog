---
title: "Week 17 Worklog"
date: 2026-10-25
weight: 17
chapter: false
pre: " <b> 1.17. </b> "
---

### Week 17 Objectives:

* Implement edge computing with CloudFront Functions and Lambda@Edge.
* Optimize data delivery for real-time dashboard with caching strategies.
* Add geographic routing for multi-region device deployments.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create CloudFront distribution for API Gateway custom domain <br> - Configure cache behaviors: cache GET /devices, no-cache for telemetry | 19/10/2026 | 19/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html> |
| Tue | - Implement Lambda@Edge Viewer Request: add security headers (CSP, HSTS) <br> - Implement Origin Request: normalize query parameters | 20/10/2026 | 20/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-at-edge.html> |
| Wed | - Implement Lambda@Edge Origin Response: compress responses, add cache-control <br> - Test edge function deployment and CloudWatch Logs in us-east-1 | 21/10/2026 | 21/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-testing-debugging.html> |
| Thu | - Configure CloudFront Functions for lightweight auth token validation <br> - Set up geographic restrictions and price class optimization | 22/10/2026 | 22/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html> |
| Fri | - Load test with 1000 req/s simulated dashboard traffic <br> - Measure latency improvement: origin vs edge-cached responses <br> - Document edge architecture and cache hit ratio targets | 23/10/2026 | 23/10/2026 | |

### Week 17 Achievements:

* **PLANNED**: Deploy CloudFront distribution with custom domain and ACM certificate.
* **PLANNED**: Implement Lambda@Edge for security headers, compression, cache control.
* **PLANNED**: Use CloudFront Functions for lightweight JWT validation at edge.
* **PLANNED**: Achieve >80% cache hit ratio for device registry API.
* **TODO**: Configure WAF rules for rate limiting and bot protection.