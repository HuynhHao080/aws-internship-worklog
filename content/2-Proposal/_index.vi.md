---
title: "Bản đề xuất"
date: 2026-10-21
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Hiện trạng D1 —22/10/2026

**B4 API/SDK19PASS; B5 browserFAIL lịch sử; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO demo test có giới hạn, NO-GO production.** D1 chỉ rà soát local, không chạy AWS/E2E hoặc publish website.

DLQ còn khoảng1diagnostic/ALARM; SNS alarm topic0subscriptions. Budget20USD toàn account không hard cap; email delivery chưa chứng minh. Cleanup chưa thực hiện. **Ngày kết thúc duy trì môi trường chưa được chủ dự án chốt.** Credentials cũ trong Git history cần rotation/revocation được xác minh trước public push.

Browser fix là virtual S3 addressing cho regional POST/GET, không mở IAM/CSP/CORS. R7 dùng Edge thật,2PDF/1.218bytes,3login/3logout,27requests; không phải load benchmark. Deployed API dùng ZIP mới, Worker/Recovery giữ ZIP cũ. Source SAM shared CodeUri không phải replay của API-only candidate.

Evidence nằm trong application repo local: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md; chưa giả định đã commit/push. Ảnh local/mocks không thay evidence AWS. Sinh viên tự vẽ, kiểm tra và giải thích architecture bằng icon AWS chính thức; không nhận sơ đồ tham khảo AI là thiết kế do mình vẽ.

### Các snapshot trước đây — giữ lịch sử, không phải trạng thái hiện hành

Các đoạn pending/chưa deploy/browser incomplete bên dưới được supersede bởi trạng thái D1 bên trên; B5FAIL không bị sửa thành PASS.



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
