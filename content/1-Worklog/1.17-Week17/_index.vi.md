---
title: "Worklog Tuần 17"
date: 2026-10-25
weight: 17
chapter: false
pre: " <b> 1.17. </b> "
---

### Mục tiêu tuần 17:

* Triển khai edge computing với CloudFront Functions và Lambda@Edge.
* Tối ưu phân phối dữ liệu cho dashboard real-time với chiến lược caching.
* Thêm geographic routing cho triển khai thiết bị multi-region.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo CloudFront distribution cho custom domain API Gateway <br> - Cấu hình cache behavior: cache GET /devices, no-cache cho telemetry | 19/10/2026 | 19/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html> |
| 3 | - Implement Lambda@Edge Viewer Request: thêm security header (CSP, HSTS) <br> - Implement Origin Request: chuẩn hóa query parameter | 20/10/2026 | 20/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-at-edge.html> |
| 4 | - Implement Lambda@Edge Origin Response: nén response, thêm cache-control <br> - Test deploy edge function và CloudWatch Logs ở us-east-1 | 21/10/2026 | 21/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-testing-debugging.html> |
| 5 | - Cấu hình CloudFront Functions cho validation auth token nhẹ <br> - Thiết lập geographic restriction và price class optimization | 22/10/2026 | 22/10/2026 | <https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html> |
| 6 | - Load test với 1000 req/s traffic dashboard giả lập <br> - Đo cải thiện latency: origin vs edge-cached response <br> - Viết tài liệu kiến trúc edge và target cache hit ratio | 23/10/2026 | 23/10/2026 | |

### Kết quả đạt được trong tuần 17:

* **PLANNED**: Deploy CloudFront distribution với custom domain và chứng chỉ ACM.
* **PLANNED**: Triển khai Lambda@Edge cho security header, nén, cache control.
* **PLANNED**: Dùng CloudFront Functions cho validation JWT nhẹ tại edge.
* **PLANNED**: Đạt >80% cache hit ratio cho API device registry.
* **TODO**: Cấu hình WAF rule cho rate limiting và bot protection.