---
title: "Worklog Tuần 21"
date: 2026-11-22
weight: 21
chapter: false
pre: " <b> 1.21. </b> "
---

### Mục tiêu tuần 21:

* Tăng cường bảo mật: implement least-privilege IAM, encryption at rest/in transit.
* Thiết lập AWS Secrets Manager cho credential rotation.
* Cấu hình AWS KMS cho envelope encryption dữ liệu nhạy cảm.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Audit tất cả IAM role/policy với IAM Access Analyzer <br> - Loại bỏ permission không dùng, áp dụng condition key (aws:SourceArn, aws:SourceAccount) | 16/11/2026 | 16/11/2026 | <https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer.html> |
| 3 | - Tạo KMS Customer Managed Key cho: DynamoDB, S3, Firehose, CloudWatch Logs <br> - Bật automatic key rotation, định nghĩa key policy | 17/11/2026 | 17/11/2026 | <https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html> |
| 4 | - Di chuyển hardcoded secret sang AWS Secrets Manager <br> - Cấu hình auto-rotation cho DB credential, API key (30 ngày) | 18/11/2026 | 18/11/2026 | <https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html> |
| 5 | - Buộc TLS 1.2+ trên mọi endpoint: API Gateway, CloudFront, IoT Core <br> - Implement VPC Flow Logs đến CloudWatch cho network monitoring | 19/11/2026 | 19/11/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html> |
| 6 | - Chạy AWS Security Hub foundational security best practices check <br> - Khắc phục finding HIGH/CRITICAL <br> - Viết tài liệu security posture và bằng chứng compliance | 20/11/2026 | 20/11/2026 | <https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-standards.html> |

### Kết quả đạt được trong tuần 21:

* **PLANNED**: Giảm ~40% permission IAM policy nhờ Access Analyzer.
* **PLANNED**: Bật encryption at rest với CMK cho tất cả data store.
* **PLANNED**: Di chuyển 12 secret sang Secrets Manager với auto-rotation.
* **PLANNED**: Buộc TLS 1.2 tối thiểu trên mọi public endpoint.
* **PLANNED**: Cải thiện Security Hub score từ 65% lên 92%.
* **TODO**: Implement GuardDuty cho threat detection và Config rule cho drift detection.