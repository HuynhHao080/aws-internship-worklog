---
title: "Worklog Tuần 10"
date: 2026-09-06
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

> **Đối soát minh chứng 21/10/2026:** Các dòng ngày/kết quả bên dưới là nội dung có sẵn, chưa được sinh viên xác nhận trong phiên rà soát này. Link tài liệu tham khảo không chứng minh đã thực hiện hoặc có mặt. Không tự chuyển PLANNED/TODO thành hoàn thành. Cần bổ sung artifact theo ngày, kết quả thật và xác nhận mốc 12 tuần.


### Mục tiêu tuần 10:

* Hiểu giao tiếp đồng bộ và bất đồng bộ.
* Tìm hiểu vai trò của Amazon SNS và Amazon SQS.
* Hiểu cách queue và topic giúp giảm sự phụ thuộc giữa các thành phần ứng dụng.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học kiến thức tích hợp ứng dụng: giao tiếp đồng bộ, bất đồng bộ, tightly coupled và loosely coupled | 31/08/2026 | 31/08/2026 | <https://aws.amazon.com/application-integration/> |
| 3 | - Tìm hiểu Amazon SQS: queue, message, producer, consumer, visibility timeout, retention period và dead-letter queue | 01/09/2026 | 01/09/2026 | <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html> |
| 4 | - So sánh SQS Standard với FIFO <br> - Tìm hiểu thứ tự message, deduplication và các trường hợp sử dụng phổ biến | 02/09/2026 | 02/09/2026 | <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/standard-queues.html> |
| 5 | - Tìm hiểu Amazon SNS: topic, publisher, subscriber, message filtering và kiến trúc fan-out | 03/09/2026 | 03/09/2026 | <https://docs.aws.amazon.com/sns/latest/dg/welcome.html> |
| 6 | - So sánh SNS với SQS <br> - Tìm hiểu cách kết hợp SNS và SQS để xây dựng ứng dụng loosely coupled | 04/09/2026 | 04/09/2026 | <https://docs.aws.amazon.com/sns/latest/dg/sns-common-scenarios.html> |
| 7 | - Tham gia **Day 01: GOVERN · Observe Before You Act** thuộc chuỗi CloudThinker × FCAJ Prove It cùng Team **HCM15 (Tyrex)** <br> - Tìm hiểu sự thay đổi của ranh giới bảo mật khi AI Agent được cấp quyền truy cập cloud environment <br> - Thiết kế identity, permission theo nguyên tắc least privilege, operating boundaries và guardrails trước khi cấp quyền <br> - Sử dụng Agent có quyền read-only để review AWS demo account và ghi lại các nhận định dựa trên evidence <br> - Hoàn thành artifact **Agentic Cloud Review** và thực hành kỹ năng **Agent Supervisor** | 05/09/2026 | 05/09/2026 | <https://docs.cloudthinker.io/learn/workshops/prove-it/day-03-prove-it-arena> |

### Kết quả đạt được trong tuần 10:

* Phân biệt được giao tiếp đồng bộ và bất đồng bộ.
* Hiểu các thành phần chính của Amazon SQS và Amazon SNS.
* Phân biệt được Standard queue và FIFO queue.
* Giải thích được mô hình publish/subscribe.
* Hiểu cách SNS và SQS giảm sự phụ thuộc giữa các thành phần ứng dụng.
