---
title: "Worklog Tuần 18"
date: 2026-11-01
weight: 18
chapter: false
pre: " <b> 1.18. </b> "
---

### Mục tiêu tuần 18:

* Thiết lập AWS IoT Core cho provisioning và quản lý thiết bị.
* Triển khai device certificate, thing group, fleet indexing.
* Tạo IoT rule để route telemetry đến Kinesis.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo IoT Thing Type cho trạm thời tiết với attribute <br> - Đăng ký 10 thiết bị test với chứng chỉ X.509 (JITR) | 26/10/2026 | 26/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/thing-types.html> |
| 3 | - Cấu hình Thing Group: lab-indoor, lab-outdoor, rooftop <br> - Bật Fleet Indexing cho tìm kiếm và dynamic group | 27/10/2026 | 27/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/thing-groups.html> |
| 4 | - Tạo IoT Rule: SELECT * FROM 'weather/+/telemetry' → Kinesis <br> - Thêm error action đến DLQ (SQS) cho rule evaluation thất bại | 28/10/2026 | 28/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html> |
| 5 | - Implement device shadow cho desired/reported state (phiên bản firmware, config) <br> - Test shadow delta update từ cloud xuống device | 29/10/2026 | 29/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html> |
| 6 | - Giả lập 10 thiết bị gửi telemetry mỗi 30s qua MQTT <br> - Xác thực luồng dữ liệu: IoT Core → Rule → Kinesis → Lambda → DynamoDB <br> - Theo dõi metric IoT Core: Connect, Publish, RuleMatch, RuleEval | 30/10/2026 | 30/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/monitoring-iot.html> |

### Kết quả đạt được trong tuần 18:

* **PLANNED**: Provision 10 thiết bị test với chứng chỉ JITR.
* **PLANNED**: Tổ chức thiết bị vào Thing Group logic.
* **PLANNED**: Tạo IoT Rule với query SQL route đến Kinesis + DLQ.
* **PLANNED**: Xác thực device shadow cho cập nhật cấu hình OTA.
* **PLANNED**: Xác nhận pipeline telemetry end-to-end ở quy mô.
* **TODO**: Implement Jobs cho cập nhật firmware toàn fleet.