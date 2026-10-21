---
title: "Worklog Tuần 16"
date: 2026-10-18
weight: 16
chapter: false
pre: " <b> 1.16. </b> "
---

### Mục tiêu tuần 16:

* Xây dựng backend serverless: Lambda cho ingestion, xử lý dữ liệu, và API.
* Triển khai API Gateway REST API với Lambda proxy integration.
* Thiết lập bảng DynamoDB cho device registry và dữ liệu thời tiết time-series.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo bảng DynamoDB: DeviceRegistry (PK: device_id), WeatherData (PK: device_id, SK: timestamp) <br> - Bật TTL trên WeatherData để giữ 90 ngày | 12/10/2026 | 12/10/2026 | <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html> |
| 3 | - Phát triển Lambda: IoT Core rule action → Kinesis Data Streams <br> - Implement logic validation và transform dữ liệu | 13/10/2026 | 13/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html> |
| 4 | - Phát triển Lambda: Kinesis consumer → DynamoDB batch write <br> - Xử lý partial failure và retry với exponential backoff | 14/10/2026 | 14/10/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html> |
| 5 | - Tạo API Gateway REST API: GET /devices, GET /devices/{id}/telemetry <br> - Cấu hình Lambda proxy integration, request validation, CORS | 15/10/2026 | 15/10/2026 | <https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy-for-lambda.html> |
| 6 | - Unit test Lambda với mock event <br> - Deploy lên dev qua SAM/CloudFormation <br> - Test end-to-end: simulated device → IoT Core → Kinesis → Lambda → DynamoDB → API | 16/10/2026 | 16/10/2026 | <https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html> |

### Kết quả đạt được trong tuần 16:

* **PLANNED**: Tạo bảng DynamoDB với key schema phù hợp và TTL.
* **PLANNED**: Xây dựng pipeline Lambda: IoT Core → Kinesis → DynamoDB.
* **PLANNED**: Public REST API qua API Gateway với Lambda proxy.
* **PLANNED**: Xác thực luồng dữ liệu end-to-end với payload thiết bị giả lập.
* **TODO**: Thêm integration test và implement dead-letter queue cho record Kinesis thất bại.