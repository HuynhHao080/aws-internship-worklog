---
title: "Worklog Tuần 19"
date: 2026-11-08
weight: 19
chapter: false
pre: " <b> 1.19. </b> "
---

### Mục tiêu tuần 19:

* Xây dựng pipeline data lake: Kinesis Data Firehose → S3 (Parquet) → Athena.
* Triển khai truy vấn dữ liệu lịch sử và view aggregation.
* Thiết lập chiến lược partitioning cho analytics tiết kiệm chi phí.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo Kinesis Data Firehose delivery stream: Kinesis → S3 <br> - Cấu hình chuyển đổi Parquet, nén GZIP, buffering 60s/64MB | 02/11/2026 | 02/11/2026 | <https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html> |
| 3 | - Thiết kế S3 partitioning: s3://weather-data/year=YYYY/month=MM/day=DD/hour=HH/ <br> - Bật S3 Object Lock cho compliance (tùy chọn) | 03/11/2026 | 03/11/2026 | <https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html> |
| 4 | - Tạo AWS Glue Database và Crawler cho dữ liệu Firehose <br> - Định nghĩa bảng Athena với partition projection để auto-discovery | 04/11/2026 | 04/11/2026 | <https://docs.aws.amazon.com/athena/latest/ug/partition-projection.html> |
| 5 | - Xây dựng Athena view: daily_avg, hourly_max, device_health_summary <br> - Test query: xu hướng 30 ngày, phát hiện bất thường (temp > 40°C) | 05/11/2026 | 05/11/2026 | <https://docs.aws.amazon.com/athena/latest/ug/views.html> |
| 6 | - Tích hợp Athena với QuickSight cho BI dashboard <br> - Tạo SPICE dataset với lịch refresh 1 giờ <br> - Viết tài liệu kiến trúc data lake và query pattern | 06/11/2026 | 06/11/2026 | <https://docs.aws.amazon.com/quicksight/latest/user/working-with-data-sources.html> |

### Kết quả đạt được trong tuần 19:

* **PLANNED**: Deploy Firehose delivery stream với output Parquet lên S3.
* **PLANNED**: Triển khai partitioning kiểu Hive cho tối ưu time-series.
* **PLANNED**: Tạo bảng Glue Catalog với Athena partition projection.
* **PLANNED**: Xây dựng view phân tích cho aggregation daily/hourly.
* **PLANNED**: Kết nối QuickSight cho trực quan hóa business intelligence.
* **TODO**: Implement data lifecycle policy (chuyển IA sau 30 ngày, Glacier sau 365).