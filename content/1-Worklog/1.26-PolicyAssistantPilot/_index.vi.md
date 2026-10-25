---
title: "AWS Policy Assistant — Nghiệm thu AI Pilot"
date: 2026-10-25
weight: 26
chapter: false
pre: " <b> 1.26. </b> "
---

## Mục tiêu

Hoàn thiện pilot hỏi đáp chính sách có citation trên AWS TEST. Đây là mốc dự án ngày 25/10/2026, không phải xác nhận hoàn thành tuần 26 hoặc toàn bộ chương trình thực tập. Trạng thái bên dưới dựa trên evidence đã lưu, không phải kiểm tra hạ tầng live tại thời điểm người đọc mở trang.

## Kiến trúc và chức năng

CloudFront phân phối frontend từ S3 private. Cognito quản lý session; API Gateway xác minh JWT trước khi gọi Lambda. DynamoDB lưu policy, metadata tài liệu theo owner và AI quota/idempotency ledger. Upload PDF dùng presigned S3, SQS, worker, DLQ và recovery schedule; kiểm tra PDF chỉ là bytes/header/size/hash, không OCR, malware scanning hoặc trích xuất policy cho AI.

Luồng AI: Cognito → API Gateway → AI Lambda riêng → Bedrock Nova Lite Converse → server validation → answer/citation trên UI. Model `apac.amazon.nova-lite-v1:0` dùng APAC profile từ Singapore; dữ liệu mẫu có thể xử lý ở Singapore, Sydney, Tokyo, Seoul, Osaka hoặc Mumbai theo scope đã duyệt. Không đưa JWT, định danh người dùng, credentials hoặc PDF cá nhân vào model.

Policy lookup, keyword search, upload/status/download và documents library đã được triển khai. Owner xác nhận lookup POL-001, một kết quả refund, ba tài liệu và refresh giữ trạng thái. Upload không được chạy lại trong đợt AI acceptance cuối. Native AI UI chỉ có một câu hỏi cố định, không ô nhập tự do và không tự gọi model khi mở trang.

## Timeline thực tế

| Thời điểm | Công việc/kết quả | Nguồn evidence |
| --- | --- | --- |
| 24/10/2026 | Triển khai AI infrastructure trên TEST với feature OFF; tiếp tục các bước nghiệm thu ứng dụng riêng | CloudFormation và read-only deployment evidence; không coi deploy xong là full acceptance |
| 25/10/2026 | Backend AI-ON giới hạn: thêm Bedrock policy và đổi environment của AI Lambda | Saved deployment/config verification; không dùng simulation thay inference evidence |
| 25/10/2026 | Một TestA API acceptance được owner thực hiện | Response/citation PASS_BY_OWNER, bounded AWS audit PASS |
| 25/10/2026 | Release frontend bốn static files, config-last; xác minh integrity/cache/CSP | Saved release/static checks |
| 25/10/2026 | Owner xác nhận visibility và answer/source sau một UI click | LIVE_UI_ACCEPTANCE_PASS_BY_OWNER, offline source/hash comparison và bounded AWS audit |
| Trước activation acceptance | CloudWatch synthetic alarm → SNS → Gmail | END_TO_END_DELIVERY_PASS_BY_OWNER và AWS action Succeeded; không lặp test |

TestA/TestB chỉ là nhãn vai trò kiểm thử, không phải username hoặc credentials. Scope duy nhất là **TestA-only, POL-001 và câu hỏi mẫu về thời hạn hoàn tiền**. TestB không có quyền AI và không thấy panel theo owner observation.

## Nghiệm thu live và usage

Answer hiển thị đúng thời hạn 30 ngày từ ngày mua cho sản phẩm lỗi, hư hỏng hoặc không đúng mô tả. Citation là POL-001, `/policies/POL-001` và approved hash. Owner-transcribed answer khớp exact source excerpt/hash khi đối chiếu offline; không suy thành mọi câu trả lời hoàn tiền đều được chấp nhận.

API/helper và native UI là hai requests khác nhau, mỗi cửa sổ có một ModelAttempts và usage **588 input /85 output tokens**. Actual Errors/Fallbacks/QuotaDenied datapoints bằng 0 trong hai cửa sổ. Tổng scoped 1176/170 không phải tổng account. Admission đầu có một idempotency record và năm counters bằng 1; lần UI có UUID riêng, minute counters mới và shared day/month counters tăng lên 2. Không reset ledger/marker để chạy lại.

## Giới hạn và sai lệch giữ nguyên

- Quota user/global 2/phút, 5/ngày; global 100/tháng theo UTC, reserve atomically. Không retry/fallback/streaming; timeout vẫn giữ reservation.
- Body <=2048 bytes; system <=512; messages <=512; tool config <=768; output <=200 tokens. CountTokens chưa được chứng minh hoạt động. Byte caps/hậu kiểm usage không bảo đảm hard <=6000 actual input tokens trước inference; owner đã duyệt policy bounded-input riêng.
- Forced tool `submit_policy_answer` được validate phía server: schema, policy, exact quote, answer evidence và usage; citation chỉ tạo sau PASS. Không thực thi external tool/toolResult.
- **UUID → Lambda INFERRED; provider request ID UNKNOWN.** Đây là bounded audit, không distributed tracing đầy đủ. Screenshot contents do owner cung cấp, không independently inspect ảnh gốc trong lượt tổng hợp cuối.
- **TestB batch-control FAIL; thủ tục TestB cũ INCONCLUSIVE.** Owner chấp nhận denial security evidence với documented exception; không retroactive approval và không retry để làm đẹp kết quả.
- UI click được owner bổ sung vào timeline sau đó và đã xảy ra trước approval riêng cho UI inference; functional acceptance không xóa procedural deviation.
- OFF rollback đã chuẩn bị có điều kiện nhưng **chưa live-test**; không hủy invocation đang chạy. Ẩn frontend không thay backend kill switch.
- **Budget alert 20 USD/tháng không phải hard spending cap**; model và infrastructure có chi phí riêng, billing/alerts có thể trễ.

## Bài học và bước tiếp theo

HTTP 200 chưa đủ để PASS: cần output contract, answer evidence và server citation validation. IAM simulation không thay runtime authorization; missing datapoints không bằng zero. Release phải pin bytes/hash và giữ rollback baseline. One-shot/session/batch controls cũng cần evidence provenance.

Hoàn tất handover và lưu sanitized evidence trước log retention expiry. Giữ narrow pilot; request mới hoặc mở rộng users/corpus/free-text/tracing/production cần review và approval riêng. Không publish credentials, raw logs, account identifiers, private paths hoặc vận hành lại helper cũ.

Repository dự án: [AWS Policy Assistant](https://github.com/HuynhHao080/aws-policy-assistant). Deployed pilot có local integration chưa được publish vào main; cập nhật tài liệu không đồng nghĩa source main đã đủ tái tạo deployment. Chưa production-ready.
