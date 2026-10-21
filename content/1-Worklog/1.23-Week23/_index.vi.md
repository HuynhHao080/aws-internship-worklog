---
title: "Worklog Tuần 23"
date: 2026-12-06
weight: 23
chapter: false
pre: " <b> 1.23. </b> "
---

### Mục tiêu tuần 23:

* Thực hiện performance testing và tối ưu hóa.
* Implement phân tích chi phí và chiến lược tối ưu.
* Fine-tune DynamoDB, Lambda, Kinesis cho quy mô production.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Chạy load test: 500 thiết bị × 1 msg/30s = 1000 msg/phút sustained <br> - Đo end-to-end latency (p50, p95, p99), error rate, throughput | 30/11/2026 | 30/11/2026 | <https://docs.aws.amazon.com/whitepapers/latest/performance-efficiency-pillar/performance-efficiency-pillar.html> |
| 3 | - Tối ưu Lambda: right-size memory (128→512 MB), bật provisioned concurrency cho API <br> - Tune Kinesis: tăng shard, bật enhanced fan-out | 01/12/2026 | 01/12/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html> |
| 4 | - Tối ưu DynamoDB: on-demand → provisioned với autoscaling <br> - Thêm GSI cho query pattern: device_id + time range, location + time | 02/12/2026 | 02/12/2026 | <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html> |
| 5 | - Chạy AWS Compute Optimizer và Cost Explorer analysis <br> - Xác định resource idle, khuyến nghị right-size, Savings Plans eligibility | 03/12/2026 | 03/12/2026 | <https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html> |
| 6 | - Implement cost allocation tag: Project=IoTWeather, Environment=prod <br> - Tạo Cost Anomaly Detection monitor <br> - Viết tài liệu kết quả tối ưu: before/after cost, performance metric | 04/12/2026 | 04/12/2026 | <https://docs.aws.amazon.com/cost-management/latest/userguide/cost-anomaly-detection.html> |

### Kết quả đạt được trong tuần 23:

* **PLANNED**: Xác thực hệ thống xử lý 1000 msg/phút với p99 latency < 2s.
* **PLANNED**: Giảm 90% Lambda cold start nhờ provisioned concurrency.
* **PLANNED**: Tối ưu DynamoDB: giảm 40% chi phí với provisioned + autoscaling.
* **PLANNED**: Xác định tiết kiệm $180/tháng qua Compute Optimizer.
* **PLANNED**: Triển khai cost allocation tag và anomaly detection.
* **TODO**: Chạy soak test 24 giờ trước production cutover.