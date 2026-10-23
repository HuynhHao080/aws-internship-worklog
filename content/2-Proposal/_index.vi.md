---
title: "Bản đề xuất"
date: 2026-10-21
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Hiện trạng đã xác minh — 23/10/2026

**B4 API/SDK: 19 PASS; B5 browser: FAIL lịch sử; B5-R7 browser: 10 PASS. GO demo test trong phạm vi đã nghiệm thu; NO-GO production.** B4 còn một ca browser NOT_RUN tại thời điểm B4; B5-R7 là đợt browser riêng, không sửa kết quả lịch sử thành PASS.

Legacy Cognito identity đã **disabled và global sign-out (D3-A)**. Không đổi mật khẩu trong containment; không khẳng định mọi JWT kiểm offline hết hiệu lực tức thì hoặc đã loại trừ truy cập trái phép. **D11 đã publish lịch sử ứng dụng sanitized; CI đầu tiên FAIL**. **D15 đã publish bản sửa test CI: [GitHub Actions 19/19 steps PASS](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39901988136), 0 FAIL/0 SKIPPED**. Đây là 19 bước CI, khác 19 ca AWS API/SDK B4. [Run FAIL D11](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/39899173855) vẫn là bằng chứng lịch sử; D17 chỉ sửa tài liệu, không deploy hoặc chạy lại AWS E2E.

Ứng dụng demo test: [https://d3ataim7dfsl7t.cloudfront.net/](https://d3ataim7dfsl7t.cloudfront.net/). Source public hiện tại: [a044936](https://github.com/HuynhHao080/aws-policy-assistant/tree/a04493682a417761f534954a1f8d38cc75d8a87b). Source/CI mới không đồng nghĩa một lần triển khai AWS mới; API dùng artifact sửa regional virtual-hosted S3 từ B5-R6, Worker/Recovery giữ artifact cũ. Không mở rộng IAM/CSP/CORS. Local/mocked screenshots vẫn phải ghi đúng nhãn.

Budget thực tế **20 USD/tháng toàn account** (Actual 80% = 16 USD, Actual 100% và Forecasted 100% = 20 USD); **Budget test 10 USD theo tag: PENDING**. Budget không phải hard spending cap, không tự dừng tài nguyên; email delivery chưa có bằng chứng. Dự toán test khoảng 6,6306 USD/tháng là mô hình tải giả định, không phải hóa đơn hoặc forecast AWS hiện tại.

AWS test stack vẫn duy trì, **chưa cleanup; chủ dự án chưa phê duyệt ngày kết thúc demo/cleanup**. Snapshot C0 ghi 45 resources/4 Retain, khoảng 1 diagnostic message trong DLQ/ALARM và SNS topic 0 subscriptions; đây không phải số liệu AWS được đọc mới hôm nay. Retain có thể tiếp tục tính phí. Chưa production-ready.

Evidence public: [B4](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-test-e2e-b4-result.md), [B5-R7](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/aws-browser-b5r7-result.md), [C0](https://github.com/HuynhHao080/aws-policy-assistant/blob/a04493682a417761f534954a1f8d38cc75d8a87b/docs/cleanup-c0-inventory-and-acceptance.md). Containment dùng hồ sơ D3-A đã lọc thông tin; không đưa username, mật khẩu hoặc token vào site. Git rewrite không xóa được các clones/caches cũ. Sơ đồ kiến trúc và minh chứng chuyên cần vẫn cần chính sinh viên xác nhận, không tạo minh chứng thay thế.

### Các snapshot cũ — lịch sử, không phải trạng thái hiện tại

Các đoạn pending/not-deployed/browser-incomplete hoặc yêu cầu security/publication cũ dưới đây là kế hoạch/snapshot trước các đợt tương ứng. Dùng phần hiện trạng phía trên khi báo cáo; giữ nguyên B5 FAIL lịch sử. Website Worklog chưa được publish trong D17.

# Policy Assistant — đề xuất hoàn thiện MVP

## Bài toán và trạng thái

Xây dựng ứng dụng tra cứu chính sách và tiếp nhận PDF có phân quyền, có bằng chứng kiểm thử cho FCAJ. Mã local hiện có policy lookup/search, Cognito JWT, document API, S3 versioning, SQS worker, DynamoDB metadata, frontend và CI kiểm thử. Sửa đổi chưa push hoặc triển khai. Xem mục **Policy Assistant** để đọc bằng chứng và giới hạn cập nhật.

## Giải pháp và ưu tiên

Giữ kiến trúc SAM: HTTP API, Lambda Python 3.12, Cognito, hai bảng DynamoDB, S3 private versioned, SQS/DLQ và CloudWatch. Gắn upload session với JWT sub, chốt version S3 và xác minh byte PDF/SHA-256. Không coi `processed` là OCR hoặc trích xuất nội dung.

1. Bảo mật: chủ sở hữu, credential trong lịch sử Git, kiểm tra đầu vào.
2. Tính đúng đắn: retry, idempotency, version và cursor.
3. Demo: polling, HTTP lỗi, tải file và đối chiếu hash.
4. Evidence: tests và build/lint local; tách kiểm chứng AWS thật sau khi được phép.
5. Hồ sơ: tài liệu khớp code, nhật ký có bằng chứng và kịch bản trình bày.

## Tiêu chí nghiệm thu

Tests backend/frontend/browser và build/lint pass trong môi trường được ghi nhận. Sau khi được phép triển khai, kiểm chứng JWT, phân quyền hai người dùng, CORS, worker/DLQ và SHA-256 trên AWS. Tài liệu ghi đúng commit/ngày/môi trường, không có bí mật hoặc số liệu bịa đặt. Ghi rõ giới hạn Scan, enqueue không transactional, pending khi lỗi kéo dài, retention và hosting local.

## AI/RAG và ngân sách

Bedrock, embeddings, semantic search và câu trả lời có trích dẫn chưa triển khai; chỉ xem xét sau khi có nhu cầu, quyền truy cập, dataset đánh giá và ngân sách. Đã bổ sung kịch bản giả định có thông số công khai: 6,6306 USD/tháng trước ưu đãi, giá AWS kiểm 21/10/2026; xem COST trong Workshop 5.7. Chưa phải lượng dùng đo thật hoặc estimate Calculator đã tạo. Nhật ký IoT và workshop S3/VPC Endpoint là nội dung độc lập, không tự coi là kết quả Policy Assistant.

## Sản phẩm bàn giao MVP v1

Ứng dụng có đăng nhập trực tiếp, thư viện tài liệu, upload/worker/download có hash và trạng thái lỗi, recovery theo lịch, demo full-stack offline, công cụ nghiệm thu/đóng gói và cấu hình hosting HTTPS. Mã và bằng chứng local đã được bổ sung; CloudFront/Cognito/IAM/DLQ vẫn cần kiểm chứng trên AWS sau khi được phép. Xem mục Policy Assistant và runbook ứng dụng để biết giới hạn và cách nâng cấp hai GSI trên bảng cũ.


## Phạm vi, thời gian và rủi ro

Phạm vi chốt: policy lookup/search, login và thư viện PDF riêng có xác minh byte/hash; không AI/OCR hoặc tự đăng ký public. Tiêu chí demo gồm luồng thành công, từ chối user khác, không báo thành công khi timeout/hash sai và xử lý lỗi SQS rõ ràng.

Timeline bên dưới là kế hoạch tổ chức 12 tuần để sinh viên đối chiếu, **không ghi thành đã thực hiện trong quá khứ**: tuần 1–2 xác nhận bài toán/tài khoản/ngân sách; 3–4 catalogue/API/IaC; 5–6 identity/ownership/upload; 7–8 worker/version/recovery; 9–10 UI/test/monitoring; 11 chuẩn bị demo AWS sau duyệt; 12 tự vẽ kiến trúc, đối soát nhật ký/blog/minh chứng và luyện trình bày. Ngày thực tế cần sinh viên xác nhận.

Rủi ro: credentials cũ chưa thu hồi; AWS chưa nghiệm thu; GSI cũ cần hai bước; client cache cần phối hợp POST rollout; Scan/retained version tăng phí; DLQ/database outage cần operator; thiếu minh chứng làm mất điểm dù code tốt. Có cost model khoảng 6,6306 USD/tháng gross, stress 10× khoảng 34,7081 USD; không tự thêm VPC/NAT/AI. Tự vẽ kiến trúc theo worksheet, không nộp sơ đồ AI làm thiết kế của sinh viên.
