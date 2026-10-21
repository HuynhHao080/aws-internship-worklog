---
title: "Worklog Tuần 20"
date: 2026-11-15
weight: 20
chapter: false
pre: " <b> 1.20. </b> "
---

### Mục tiêu tuần 20:

* Triển khai monitoring toàn diện với CloudWatch dashboard và alarm.
* Thiết lập distributed tracing với AWS X-Ray cho Lambda function.
* Tạo runbook vận hành cho các sự cố phổ biến.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo CloudWatch Dashboard: metric IoT Core, Kinesis IncomingRecords, Lambda Duration/Errors/Throttles, DynamoDB ConsumedCapacity | 09/11/2026 | 09/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html> |
| 3 | - Cấu hình CloudWatch Alarm: Lambda Errors > 5/phút, Kinesis IteratorAge > 300s, DynamoDB ThrottledRequests > 0 <br> - Thiết lập SNS topic cho thông báo alarm | 10/11/2026 | 10/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html> |
| 4 | - Bật X-Ray tracing trên tất cả Lambda function <br> - Cấu hình sampling rule và annotate trace với device_id <br> - Phân tích service map để xác định bottleneck | 11/11/2026 | 11/11/2026 | <https://docs.aws.amazon.com/xray/latest/devguide/xray-getting-started.html> |
| 5 | - Tạo CloudWatch Logs Insights query cho: error pattern, cold start duration, data latency <br> - Export log ra S3 để lưu trữ lâu dài | 12/11/2026 | 12/11/2026 | <https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html> |
| 6 | - Viết runbook: "High Lambda Error Rate", "Kinesis Processing Lag", "DynamoDB Throttling" <br> - Thực hiện tabletop exercise incident response với mentor <br> - Viết tài liệu escalation path và on-call contact | 13/11/2026 | 13/11/2026 | |

### Kết quả đạt được trong tuần 20:

* **PLANNED**: Xây dựng dashboard vận hành với 15+ metric quan trọng.
* **PLANNED**: Cấu hình 8 alarm critical với thông báo SNS.
* **PLANNED**: Bật X-Ray tracing cho tất cả Lambda function.
* **PLANNED**: Tạo Logs Insights query cho debug nhanh.
* **PLANNED**: Viết tài liệu 3 runbook sự cố với bước khắc phục.
* **TODO**: Implement Synthetic Canaries cho health check API.