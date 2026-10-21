---
title: "Worklog Tuần 14"
date: 2026-10-04
weight: 14
chapter: false
pre: " <b> 1.14. </b> "
---

### Mục tiêu tuần 14:

* Khởi động dự án IoT Weather Platform.
* Thiết kế kiến trúc serverless tổng thể cho giám sát thời tiết thời gian thực.
* Xác định phạm vi dự án, mốc thời gian và các deliverable.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Họp khởi động dự án với mentor <br> - Xem lại yêu cầu IoT Weather Platform: thiết bị edge, ingestion real-time, dashboard, alerting | 28/09/2026 | 28/09/2026 | Brief dự án nội bộ |
| 3 | - Thiết kế kiến trúc cấp cao: IoT Core → Kinesis → Lambda → DynamoDB/S3 → CloudFront <br> - Xác định dịch vụ AWS cho từng thành phần | 29/09/2026 | 29/09/2026 | <https://aws.amazon.com/iot-core/> |
| 4 | - Định nghĩa data model cho telemetry thời tiết (nhiệt độ, độ ẩm, áp suất, gió) <br> - Thiết kế schema DynamoDB với partition/sort key | 30/09/2026 | 30/09/2026 | <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html> |
| 5 | - Lập kế hoạch networking VPC: public/private subnet, VPC endpoint cho S3/DynamoDB <br> - Tạo khung CloudFormation template cho infrastructure | 01/10/2026 | 01/10/2026 | <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html> |
| 6 | - Viết Architecture Decision Records (ADRs) <br> - Thiết lập Git repository với branch strategy <br> - Đồng bộ hàng tuần với mentor | 02/10/2026 | 02/10/2026 | |

### Kết quả đạt được trong tuần 14:

* **PLANNED**: Xác định phạm vi dự án: IoT Weather Platform cho nghiên cứu phòng lab với dashboard real-time.
* **PLANNED**: Thiết kế kiến trúc serverless dùng IoT Core, Kinesis, Lambda, DynamoDB, S3, CloudFront.
* **PLANNED**: Tạo cấu trúc CloudFormation template ban đầu.
* **PLANNED**: Thiết lập Git workflow và chuẩn tài liệu.
* **TODO**: Xác thực kiến trúc với mentor và chốt lựa chọn công nghệ.