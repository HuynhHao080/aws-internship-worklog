---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Trong phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# Policy Assistant
## Truy xuất chính sách không máy chủ và trả lời câu hỏi hỗ trợ bởi AI

### 1. Tóm tắt điều hành / Executive Summary

Policy Assistant là một Minimum Viable Product (MVP) không máy chủ để truy xuất và tìm kiếm chính sách tổ chức. Nó được xây dựng với AWS SAM, Python 3.12, Amazon API Gateway HTTP API (v2), AWS Lambda và Amazon DynamoDB, được triển khai trong vùng `ap-southeast-1`.

MVP hiện tại hiển thị hai điểm cuối API:
- `GET /policies/{policy_id}` — truy xuất một chính sách theo ID chính xác.
- `GET /policies?keyword=xxx` — tìm kiếm từ khóa không phân biệt hoa thường trên tiêu đề, danh mục, nội dung và từ khóa.

Hướng phát triển đề xuất của dự án là thêm khả năng trả lời câu hỏi hỗ trợ bởi AI có căn cứ nguồn, cho phép người dùng đặt câu hỏi bằng ngôn ngữ tự nhiên về các chính sách và nhận câu trả lời kèm theo trích dẫn nguồn. **Amazon Bedrock, RAG (Retrieval-Augmented Generation), nhúng vector và tìm kiếm vector KHÔNG được triển khai hiện tại.** Chúng được đánh giá là các cải tiến trong tương lai chỉ được lên kế hoạch.

Đề xuất này mô tả kiến trúc hiện tại đã xác minh, kiến trúc mục tiêu được lên kế hoạch, lộ trình phát triển 25 tuần, giả định ngân sách, đánh giá rủi ro và kết quả kỳ vọng. Tất cả các 주장 về triển khai hiện tại đều được hỗ trợ bởi mã kho lưu trữ, cấu hình hoặc kết quả kiểm tra đã thực thi.

### 2. Tuyên bố vấn đề / Problem Statement

#### Quy trình truy xuất hiện tại

Người dùng tương tác với Policy Assistant thông qua hai thao tác API:

1. **Tra cứu theo ID chính xác** — `GET /policies/{policy_id}` thực hiện DynamoDB `GetItem` bằng khóa phân vùng `policy_id`. Nó trả về một tài liệu chính sách nếu tồn tại, hoặc phản hồi 404.

2. **Tìm kiếm từ khóa** — `GET /policies?keyword=xxx` thực hiện DynamoDB `Scan` với `FilterExpression` trên bốn trường chữ thường (`title_lower`, `category_lower`, `content_lower`, `keywords_lower`). Phân trang được xử lý qua `LastEvaluatedKey`.

#### Các hạn chế đã xác minh

Các hạn chế sau được hỗ trợ bởi kiểm tra mã:

- **Tìm kiếm dựa trên Scan** — Tìm kiếm từ khóa đọc toàn bộ bảng (tối đa 1 MB mỗi yêu cầu). Điều này phù hợp với tập dữ liệu MVP nhỏ (hiện tại 3 chính sách mẫu) nhưng không mở rộng cho kho chính sách lớn. (`policy_assistant/app.py`, dòng 128–164; nhận xét `template.yaml` dòng 38–39)
- **Không có tìm kiếm ngữ nghĩa** — Khớp từ khóa sử dụng `contains()` trên các trường chữ thường. Nó không thể xử lý từ đồng nghĩa, khái niệm liên quan hoặc câu hỏi ngôn ngữ tự nhiên. (`policy_assistant/app.py`, dòng 132–135)
- **Không có xác thực hoặc phân quyền** — API được truy cập công khai. CORS được giới hạn cho `localhost:3000` và `127.0.0.1:3000`. Không có khóa API, nhóm người dùng Cognito, hoặc Lambda authorizer được cấu hình. (`template.yaml`, dòng 72–82)
- **Không có trích dẫn nguồn cho câu trả lời AI** — Không có thành phần AI nào trong triển khai hiện tại, do đó không có cơ chế trích dẫn hoặc tham chiếu tồn tại. (Không tìm thấy mã Bedrock, RAG hoặc kho vector trong kho lưu trữ)
- **Không có giám sát hoặc cảnh báo** — CloudWatch Logs được cấu hình với thời gian lưu trữ 30 ngày, nhưng không có CloudWatch Alarms, bảng điều khiển hoặc truy vết X-Ray nào có mặt. (`template.yaml`, dòng 21–25)
- **Không có pipeline CI/CD** — Triển khai qua `sam deploy --guided` (thủ công). Không có CodePipeline, CodeBuild hoặc quy trình làm việc GitHub Actions nào tồn tại. (`samconfig.toml`; không có tệp CI/CD nào trong kho lưu trữ)

#### Các giả định cần đánh giá

Sau đây là các giả định, không phải là sự thật đã xác minh:
- Khối lượng chính sách sẽ phát triển vượt quá quy mô MVP nơi Scan vẫn chấp nhận được.
- Người dùng sẽ cần trả lời câu hỏi ngôn ngữ tự nhiên thay vì tra cứu theo ID chính xác hoặc từ khóa.
- Trích dẫn nguồn sẽ được yêu cầu cho câu trả lời do AI tạo ra.

Các giả định này cần được xác minh với các bên liên quan trước khi cam kết con đường cải tiến RAG.

### 3. Kiến trúc giải pháp / Solution Architecture

#### Kiến trúc hiện tại (đã xác minh)

Kiến trúc hiện tại bao gồm các tài nguyên đã xác minh được định nghĩa trong `template.yaml` và triển khai trong `policy_assistant/app.py`:

```
[Client Applications]
    Web Frontend (localhost:3000)
    curl / API Client
         |
         | HTTPS GET
         v
[AWS Cloud (ap-southeast-1)]
    API Gateway HTTP API
    (PolicyAssistantApi, Stage: Prod)
         |
         | Invoke AWS_PROXY
         v
    Lambda Function
    (PolicyAssistantFunction, Python 3.12, thời gian chờ 10 s)
         |
         | GetItem / Scan
         v
    DynamoDB Table
    (PolicyAssistantPolicies, PK: policy_id, PAY_PER_REQUEST)
         |
         | Logs JSON
         v
    CloudWatch Logs
    (/aws/lambda/PolicyAssistantFunction-..., lưu trữ 30 ngày)
         ^
         | Assumes / Allows
    IAM Role
    (PolicyAssistantFunctionRole: dynamodb:GetItem, dynamodb:Scan)
```

**Chú thích:** Tất cả các thành phần ở trên là **Hiện tại** (đã triển khai và xác minh trong kho lưu trữ).

#### Kiến trúc mục tiêu được đề xuất (cải tiến được lên kế hoạch)

Kiến trúc mục tiêu thêm các thành phần được lên kế hoạch cho trả lời câu hỏi hỗ trợ bởi AI. Các thành phần được lên kế hoạch **chưa được triển khai** và được đánh dấu tương ứng:

```
[Client Applications]
    Web Frontend (được lên kế hoạch)
    curl / API Client
         |
         | HTTPS GET
         v
[AWS Cloud (ap-southeast-1)]
    API Gateway HTTP API
    (PolicyAssistantApi, Stage: Prod)
         |
         | Invoke AWS_PROXY
         v
    Lambda Function
    (PolicyAssistantFunction, Python 3.12)
         |
         | GetItem / Scan (hiện tại)
         | Gọi Bedrock (được lên kế hoạch)
         v
    DynamoDB Table
    (PolicyAssistantPolicies, PK: policy_id)
         |
         | Tìm kiếm vector / truy xuất (được lên kế hoạch)
         v
    [Được lên kế hoạch: Amazon Bedrock Knowledge Base]
    [Được lên kế hoạch: Mô hình nhúng]
    [Được lên kế hoạch: Chỉ mục vector]
         |
         | Logs JSON
         v
    CloudWatch Logs
         ^
         | Assumes / Allows
    IAM Role
    (Vai trò ít quyền nhất; được lên kế hoạch: thêm quyền Cognito, Bedrock)
```

**Chú thích:**
- **Hiện tại** — đã triển khai và xác minh trong kho lưu trữ.
- **Được lên kế hoạch** — cải tiến trong tương lai được đề xuất, chưa được triển khai.

#### Dịch vụ AWS được sử dụng (hiện tại vs được lên kế hoạch)

| Dịch vụ | Hiện tại | Được lên kế hoạch | Bằng chứng |
|---------|---------|-------------------|------------|
| AWS Lambda | ✅ Đã triển khai | — | `template.yaml`, `policy_assistant/app.py` |
| Amazon API Gateway HTTP API | ✅ Đã triển khai | — | `template.yaml` |
| Amazon DynamoDB | ✅ Đã triển khai | — | `template.yaml`, `sample_data/policies.json` |
| Amazon CloudWatch Logs | ✅ Đã triển khai | — | `template.yaml` |
| AWS IAM | ✅ Đã triển khai (vai trò Lambda chỉ) | Được lên kế hoạch (Cognito, mở rộng ít quyền nhất) | `template.yaml` |
| Amazon Bedrock | — | Được lên kế hoạch (RAG, trả lời câu hỏi) | Không tìm thấy trong kho lưu trữ |
| Amazon Cognito | — | Được lên kế hoạch (xác thực/phân quyền) | Không tìm thấy trong kho lưu trữ |
| AWS Secrets Manager | — | Được lên kế hoạch (quản lý thông tin xác thực) | Không tìm thấy trong kho lưu trữ |
| AWS KMS | — | Được lên kế hoạch (mã hóa dữ liệu khi lưu trữ) | Không tìm thấy trong kho lưu trữ |
| Amazon OpenSearch | — | Được lên kế hoạch (thay thế Scan cho tìm kiếm) | Không tìm thấy trong kho lưu trữ |

### 4. Triển khai kỹ thuật / Technical Implementation

#### Triển khai hiện tại (đã xác minh)

**Các điểm cuối API:**

| Điểm cuối | Phương thức | Mô tả | Trạng thái |
|-----------|-------------|-------|------------|
| `/policies/{policy_id}` | GET | Truy xuất chính sách theo ID chính xác bằng DynamoDB `GetItem` | ĐÃ TRIỂN KHAI — ĐÃ XÁC MINH |
| `/policies` | GET | Tìm kiếm từ khóa không phân biệt hoa thường bằng DynamoDB `Scan` | ĐÃ TRIỂN KHAI — ĐÃ XÁC MINH |

**Mô hình dữ liệu (`sample_data/policies.json`):**

Mỗi mục chính sách chứa:
- `policy_id` (Chuỗi, khóa phân vùng)
- `title`, `category`, `content` (Chuỗi)
- `keywords` (Danh sách Chuỗi)
- `title_lower`, `category_lower`, `content_lower`, `keywords_lower` (bản sao chữ thường cho tìm kiếm không phân biệt hoa thường)

Ba chính sách mẫu được bao gồm: POL-001 (Chính sách Hoàn Tiền Khách Hàng), POL-002 (Chính sách Bảo Mật), POL-003 (Điều Khoản Dịch Vụ).

**Hành vi Lambda (`policy_assistant/app.py`):**

- Đọc `POLICIES_TABLE_NAME` từ biến môi trường (mặc định là `PolicyAssistantPolicies`).
- Loại bỏ tiền tố giai đoạn API Gateway từ đường dẫn yêu cầu để định tuyến.
- Trả về `405 Method Not Allowed` cho các phương thức không phải GET.
- Trả về `400 Bad Request` cho `policy_id` hoặc `keyword` bị thiếu hoặc trống.
- Trả về `404 Not Found` cho chính sách không tồn tại hoặc điểm cuối không hợp lệ.
- Trả về `500 Internal Server Error` cho `ClientError` của DynamoDB hoặc ngoại lệ không mong đợi.
- Tất cả phản hồi bao gồm tiêu đề CORS (`Access-Control-Allow-Origin: http://localhost:3000`).
- Ghi nhật ký định dạng JSON qua CloudWatch Logs.

**Tài sản kiểm tra:**

- `tests/unit/test_handler.py` — 21 bài kiểm tra đơn sử dụng `moto` để giả lập DynamoDB. **Tất cả 21 bài kiểm tra đều PASSED** (đã xác minh bằng cách thực thi).
- `tests/integration/test_api_gateway.py` — Kiểm tra tích hợp yêu cầu stack đã triển khai (bị bỏ qua nếu không có `AWS_SAM_STACK_NAME`).
- `test_event.json` — Sự kiện API Gateway HTTP API v2 mẫu cho gọi cục bộ.

**Cấu hình cơ sở hạ tầng:**

- `template.yaml` — Mẫu AWS SAM định nghĩa Lambda, API Gateway HTTP API, bảng DynamoDB, nhóm CloudWatch Logs và vai trò IAM.
- `samconfig.toml` — Cấu hình SAM CLI (tên stack: `policy-assistant`, vùng: `ap-southeast-1`, khả năng: `CAPABILITY_IAM`).

#### Các cải tiến được lên kế hoạch (chưa được triển khai)

Các cải tiến sau đây là **được lên kế hoạch** và không phải là phần của triển khai hiện tại:

1. **Xác thực API và tài liệu OpenAPI** — Thêm xác thực yêu cầu ở cấp độ API Gateway; xuất bản đặc tả OpenAPI 3.0.
2. **Tối ưu hóa tìm kiếm và đánh giá truy xuất** — Thay thế hoặc bổ sung DynamoDB Scan bằng GSI hoặc OpenSearch; đánh giá độ liên quan và độ hồi phục của truy xuất.
3. **Bảo mật và IAM ít quyền nhất** — tinh chỉnh chính sách IAM; thêm chính sách dựa trên tài nguyên khi thích hợp.
4. **Xác thực/phân quyền** — Thêm Amazon Cognito hoặc khóa API API Gateway / Lambda authorizer.
5. **Giám sát và cảnh báo** — Thêm CloudWatch Alarms, bảng điều khiển và tùy chọn truy vết X-Ray.
6. **CI/CD và xác thực triển khai** — Thêm GitHub Actions hoặc AWS CodePipeline/CodeBuild cho kiểm tra và triển khai tự động.
7. **Triển khai giao diện người dùng tùy chọn** — Thêm giao diện web cho tìm kiếm chính sách (hiện tại được nhắc trong README nhưng không có tệp nào tồn tại).
8. **Khả năng và triển khai Bedrock/RAG** — Đánh giá Amazon Bedrock cho trả lời câu hỏi có căn cứ nguồn; triển khai trích dẫn nguồn, xử lý bằng chứng không đủ và các biện pháp bảo vệ chống châm chọc nhắc lệnh.
9. **Đánh giá AI** — Thiết lập các chỉ số cho độ chính xác câu trả lời, chất lượng trích dẫn và tỷ lệ từ chối cho câu hỏi không có câu trả lời.

**Không bao giờ trộn lẫn các cải tiến được lên kế hoạch với danh sách tính năng hiện tại.** Triển khai hiện tại chỉ chứa hai điểm cuối GET, lưu trữ DynamoDB, CloudWatch Logs và vai trò thực thi IAM.

### 5. Lộ trình và các mốc / Timeline & Milestones

Lộ trình 25 tuần sau đây là **lộ trình được đề xuất** cho phát triển Policy Assistant. Nó **không được rút ra** từ các mục nhập Worklog lịch sử. Tuần 1–13 trong Worklog hiện tại mô tả các hoạt động học AWS chung; Tuần 14–25 mô tả các hoạt động IoT Weather Platform. Không phải là lịch sử phát triển Policy Assistant.

| Giai đoạn | Khoảng tuần | Hoạt động được lên kế hoạch | Kết quả dự kiến | Tiêu chí chấp nhận có thể xác minh |
|---------|-------------|---------------------------|-----------------|-----------------------------------|
| 1. Yêu cầu và đánh giá cơ sở | 1–2 | Xem xét kho lưu trữ hiện tại; ghi lại tính năng đã xác minh; xác định khoảng trống; phỏng vấn bên liên quan | Tài liệu yêu cầu; báo cáo phân tích khoảng trống | Tất cả tính năng hiện tại được liệt kê với tham chiếu bằng chứng; phân tích khoảng trống được phê duyệt |
| 2. Nền tảng không máy chủ AWS | 3–5 | Xem xét mẫu SAM, vai trò IAM, thiết kế DynamoDB; xác thực cú pháp mẫu | `template.yaml` đã xác thực; xem xét IAM ít quyền nhất | `sam validate` thành công (khi SAM CLI có sẵn); quyền IAM được giới hạn ở ARN bảng |
| 3. Triển khai API và dữ liệu | 6–8 | Triển khai và kiểm tra đơn cả hai điểm cuối; thêm phân trang; xác thực phản hồi lỗi | `policy_assistant/app.py` đã cập nhật; kiểm tra đơn | 21+ bài kiểm tra đơn pass; tất cả trường hợp lỗi trả về mã trạng thái chính xác |
| 4. Kiểm tra và bằng chứng kiểm tra | 9–10 | Mở rộng phạm vi kiểm tra đơn; tạo thiết bị kiểm tra tích hợp; ghi lại thực thi kiểm tra | Báo cáo kiểm tra; chỉ số độ bao phủ | Kiểm tra đơn pass trong môi trường giống CI; kiểm tra tích hợp được ghi lại |
| 5. Tài liệu cơ sở hạ tầng | 11–12 | Tạo thư mục `docs/` với ARCHITECTURE.md, TEST_REPORTING.md, CLOUDWATCH_OPERATIONS.md | Các tệp tài liệu | Các liên kết trong README giải quyết được; sơ đồ khớp với tài nguyên `template.yaml` |
| 6. Đánh giá tìm kiếm | 13–14 | Đánh giá GSI so với OpenSearch so với Bedrock cho tìm kiếm; ghi lại các đánh đổi | Báo cáo đánh giá tìm kiếm | Khuyến nghị được ghi lại với lý do chi phí và hiệu suất |
| 7. Bảo mật và quan sát | 15–16 | Thêm CloudWatch Alarms; đánh giá Cognito/khóa API; xem xét quyền IAM | Cảnh báo đã cấu hình; xem xét bảo mật | Các cảnh báo được định nghĩa cho tỷ lệ lỗi và độ trễ; quyền IAM đã xem xét |
| 8. Tự động hóa triển khai | 17–18 | Thêm pipeline CI/CD (GitHub Actions hoặc CodePipeline); ghi lại thủ册 triển khai | Cấu hình pipeline; thủ册 triển khai | Pipeline chạy kiểm tra đơn trên pull request; triển khai được ghi lại |
| 9. Đánh giá khả năng RAG | 19–20 | Đánh giá Amazon Bedrock cho trả lời câu hỏi có căn cứ nguồn; nguyên mẫu truy xuất và tạo câu trả lời | Báo cáo khả năng; nguyên mẫu tùy chọn | Nguyên mẫu trả lời câu hỏi với trích dẫn nguồn; hành vi từ chối cho câu hỏi không có câu trả lời |
| 10. Nguyên mẫu RAG tùy chọn | 21–22 | Triển khai Amazon Bedrock Knowledge Base hoặc tương đương; thêm các biện pháp bảo vệ chống châm chọc nhắc lệnh | Nguyên mẫu RAG (nếu được phê duyệt) | Nguyên mẫu thể hiện câu trả lời có căn cứ nguồn với trích dẫn |
| 11. Handover cuối cùng | 23–25 | Tài liệu cuối cùng; chuyển giao kiến thức; chuẩn bị phát hành | Gói phát hành; ghi chú chuyển giao | Tất cả tài liệu hoàn tất; mã đã xem xét; gán thẻ phát hành |

### 6. Ước tính ngân sách / Budget Estimation

#### Cách tiếp cận mô hình chi phí

Phần này cung cấp một **mô hình chi phí trong suốt** thay vì các tổng số bị phát minh. Chi phí hàng tháng thực tế phụ thuộc vào khối lượng sử dụng chưa được biết.

#### Các danh mục chi phí

| Danh mục | MVP hiện tại | Tùy chọn (được lên kế hoạch) |
|----------|-------------|---------------------------|
| Amazon API Gateway HTTP API | Trả theo yêu cầu | — |
| AWS Lambda | Trả theo yêu cầu + GB-giây | — |
| Amazon DynamoDB | Yêu cầu (PAY_PER_REQUEST) | — |
| Amazon CloudWatch Logs | Trả theo GB nhập/lưu trữ | — |
| Amazon Bedrock | — | Trả theo lần gọi mô hình / mỗi token |
| Amazon Cognito | — | Người dùng hoạt động hàng tháng (MAU) |
| AWS KMS | — | Mỗi khóa + mỗi yêu cầu |
| AWS Secrets Manager | — | Mỗi bí mật + mỗi lệnh gọi API |
| Amazon OpenSearch | — | Mỗi giờ phiên bản + lưu trữ |

#### Các giả định cần thiết cho ước tính hàng tháng

Các đầu vào sau đây là **không có sẵn** và phải được cung cấp trước khi có thể tính toán ước tính số:

- Khối lượng yêu cầu hàng tháng (API Gateway + lời gọi Lambda)
- Thời gian thực thi Lambda trung bình và phân bổ bộ nhớ
- Kích thước bảng DynamoDB và sử dụng khả năng đọc/ghi
- Khối lượng nhập CloudWatch Logs và thời gian lưu trữ
- Lần gọi mô hình Bedrock và số lượng token (nếu triển khai RAG)
- Người dùng hoạt động hàng tháng Cognito (nếu thêm xác thực)
- Khối lượng truyền dữ liệu ra
- Gói hỗ trợ AWS (nếu có)

#### Cơ sở MVP hiện tại so với chi phí AI/RAG tùy chọn

- **Cơ sở MVP hiện tại:** Lambda, API Gateway HTTP API, DynamoDB theo yêu cầu, CloudWatch Logs. Các dịch vụ này có mức miễn phí có thể bao phủ việc sử dụng phát triển ở mức thấp. Chi phí thực tế phụ thuộc vào khối lượng yêu cầu.
- **Chi phí AI/RAG tùy chọn:** Lần gọi mô hình Amazon Bedrock, lưu trữ vector và tạo nhúng. Những chi phí này phụ thuộc vào sử dụng và không thể ước tính mà không có khối lượng token và lựa chọn mô hình.

**Không có con số chi phí hàng tháng nào được nêu trong đề xuất này** vì các đầu vào sử dụng cần thiết không có sẵn. Ước tính đang chờ thu thập các giả định được liệt kê ở trên.

### 7. Đánh giá rủi ro / Risk Assessment

| Rủi ro | Tác động | Chiến lược giảm thiểu |
|--------|---------|----------------------|
| Hiệu suất và khả năng mở rộng tìm kiếm | DynamoDB Scan đọc toàn bộ bảng; giới hạn 1 MB mỗi yêu cầu; phân trang cần thiết | Đánh giá GSI hoặc OpenSearch cho tập dữ liệu lớn hơn; ghi lại hạn chế của Scan |
| Nội dung chính sách không chính xác hoặc lỗi thời | DynamoDB không có lược đồ; không có xác thực nội dung hoặc phiên bản | Thêm xác thực lược đồ trong Lambda; triển khai quy trình đánh giá nội dung |
| Câu trả lời AI không được hỗ trợ | Không có thành phần AI hiện tại; RAG trong tương lai có thể ảo giác | Triển khai trích dẫn nguồn; hành vi từ chối cho câu hỏi không có câu trả lời; đánh giá con người |
| Rò rỉ dữ liệu nhạy cảm và khoảng trống phân quyền | API công khai; không có xác thực được cấu hình | Thêm Cognito hoặc khóa API; IAM ít quyền nhất; đánh giá mã hóa khi lưu trữ |
| Châm chọc nhắc lệnh | Không áp dụng cho MVP hiện tại; liên quan nếu RAG được thêm | Làm sạch đầu vào; rào cản nhắc lệnh hệ thống; xác thực đầu ra |
| Chi phí AI không kiểm soát | Không áp dụng cho MVP hiện tại; liên quan nếu Bedrock được thêm | Cảnh báo ngân sách; giám sát sử dụng; lựa chọn mô hình dựa trên chi phí/hiệu suất |
| Sự cố triển khai và sai lệch cấu hình | Triển khai thủ công `sam deploy`; không có CI/CD | Thêm pipeline CI/CD; xem xét cơ sở hạ tầng dưới dạng mã; tập hợp thay đổi |
| Bằng chứng kiểm tra không đủ | 21 bài kiểm tra đơn pass; kiểm tra tích hợp yêu cầu stack đã triển khai | Mở rộng phạm vi kiểm tra; ghi lại thực thi kiểm tra; tích hợp CI |
| Sự không nhất quán giữa Worklog và dự án thực tế | Tuần 14–25 mô tả IoT Weather Platform, không phải Policy Assistant | Duy trì tài liệu kiểm toán riêng; gán nhãn rõ ràng lộ trình được đề xuất |
| Mở rộng phạm vi | Các cải tiến được đề xuất có thể vượt quá khả năng thực hiện của kỳ thực tập | Lộ trình có cổng giai đoạn; tiêu chí chấp nhận cho mỗi giai đoạn; phê duyệt bên liên quan |

### 8. Kết quả kỳ vọng / Expected Outcomes

#### Các khả năng hiện tại được hỗ trợ bởi bằng chứng

- `GET /policies/{policy_id}` trả về một chính sách theo ID chính xác (21 bài kiểm tra đơn pass).
- `GET /policies?keyword=xxx` thực hiện tìm kiếm từ khóa không phân biệt hoa thường (21 bài kiểm tra đơn pass).
- Phản hồi lỗi trả về 400, 404, 405 hoặc 500 với phần thân JSON (21 bài kiểm tra đơn pass).
- Tiêu đề CORS có mặt trên các phản hồi (21 bài kiểm tra đơn pass).
- Cơ sở hạ tầng được định nghĩa trong AWS SAM (`template.yaml`, `samconfig.toml`).
- Bảng DynamoDB `PolicyAssistantPolicies` với khóa phân vùng `policy_id` được định nghĩa.
- Nhóm CloudWatch Logs với thời gian lưu trữ 30 ngày và định dạng JSON được định nghĩa.
- Vai trò IAM cấp quyền `dynamodb:GetItem` và `dynamodb:Scan` trên ARN bảng chỉ.

#### Kết quả dự kiến sau công việc được lên kế hoạch

- Cơ sở hạ tầng đã được ghi lại và xác thực khớp với `template.yaml`.
- Phạm vi kiểm tra được mở rộng với tích hợp CI.
- Khuyến nghị tối ưu hóa tìm kiếm (GSI, OpenSearch hoặc Bedrock).
- Xem xét bảo mật với IAM ít quyền nhất và xác thực tùy chọn.
- Giám sát với CloudWatch Alarms.
- Pipeline tự động hóa triển khai.
- Báo cáo khả năng RAG; nguyên mẫu tùy chọn với trích dẫn nguồn.

#### Tiêu chí chấp nhận để khai báo dự án thành công

- Tất cả bài kiểm tra đơn pass trong môi trường sạch.
- Cơ sở hạ tầng triển khai qua `sam deploy` mà không cần can thiệp thủ công.
- Các điểm cuối API hiện tại trả về mã trạng thái đúng cho tất cả trường hợp đã ghi lại.
- Các cải tiến được lên kế hoạch được gán nhãn rõ ràng là được lên kế hoạch hoặc đã triển khai với bằng chứng.
- Không có số liệu, chi phí hoặc 주장 triển khai bị phát minh xuất hiện trong tài liệu.
