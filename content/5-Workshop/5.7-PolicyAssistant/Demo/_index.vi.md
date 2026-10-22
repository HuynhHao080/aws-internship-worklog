---
title: "Demo — 5 đến 7 phút"
date: 2026-10-21
weight: 3
---

> Cập nhật bằng chứng 21/10/2026: B1 stack/B2 frontend/B3 users đã hoàn tất; B4 API/SDK 19 PASS, browser thực tế NOT_RUN. Xem chương AWS E2E. Các đoạn pending/not-deployed dưới đây là kế hoạch/lịch sử trước B4, không phải trạng thái hiện hành. Không chạy lại poison hoặc demo có writes khi chưa được duyệt tiếp.

# Demo và bảo vệ 5–7 phút

**Chỉ dùng nhãn AWS thật sau khi deploy/kiểm thử thực sự đạt. Hiện candidate mới chưa triển khai.** Nếu dùng local phải giữ nhãn OFFLINE. Không dùng stack cũ để khẳng định đã triển khai release mới.

| Thời gian | Thao tác / lời giải thích | Bằng chứng |
|---|---|---|
| 0:00–0:40 | Bài toán tra chính sách chung và thư viện tài liệu riêng; kiểm byte/hash PDF, không AI/OCR/RAG | UI, môi trường/revision đúng |
| 0:40–1:40 | Sơ đồ chính bạn vẽ: Cognito/API/Lambda, hai bảng, S3/version, SQS/worker/DLQ/recovery, CloudWatch | File chỉnh sửa/ngày, icon AWS chính thức |
| 1:40–3:10 | Alice login trực tiếp, POL-001/refund, upload PDF tổng hợp nhỏ, processed/download/library | State/hash gốc-worker-download khớp; quay sau khi xóa ô mật khẩu |
| 3:10–4:10 | Logout, Bob login, library riêng và ID Alice trả 404 | UI/HTTP thật, giải thích JWT khác ownership |
| 4:10–5:10 | Evidence đã chạy: từ chối quá size/sai định dạng, retry/DLQ; không chờ queue ngay giữa bài nói ngắn | Ngày/query window/mapping; phân biệt direct Invoke mixed event và redrive SQS |
| 5:10–6:10 | Dashboard/alarm nguyên trạng thái, gross 6,63 USD theo giả định và giới hạn chi phí | Bảng giá có ngày, tài nguyên thật; stack cũ/thuế/usage vượt tải tính riêng |
| 6:10–6:40 | Kết quả đo và giới hạn Scan/eventual index/enqueue-recovery/byte check | Ma trận thực tế, phần chưa đạt rõ ràng |

Không quay Network, Authorization, mật khẩu, auth response đầy đủ, signed URL/POST field hoặc tài liệu thật của người khác. Dùng file tổng hợp và evidence an toàn.

## Câu hỏi bảo vệ

| Câu hỏi | Ý cần tự giải thích |
|---|---|
| Vì sao HTTP API/Lambda/DynamoDB? | Managed cho tải nhỏ, không VM chờ; GetItem hiệu quả, Scan tốn theo dữ liệu đọc trước lọc và có giới hạn |
| JWT kiểm ở đâu? | Gateway issuer/audience, Lambda nhận sub authorizer; unit/direct Invoke không chứng minh chữ ký JWT |
| Vì sao Bob 404? | Owner check ở backend, list/cursor cũng gắn owner; CORS/ẩn nút không cấp quyền |
| S3 có kiểm Cognito? | Không; signed capability tạm, IAM/policy ràng buộc; ai giữ capability có thể dùng đến hết hạn |
| POST/versioning để làm gì? | Chặn key/MIME/size tại storage, chốt version để overwrite không thay byte đã nộp; header không phải quét mã độc/parser |
| Retry/trùng có làm sai state? | At-least-once, conditional state/attempt, partial failure, số lần nhận/DLQ; phân biệt input sai vĩnh viễn và lỗi dịch vụ tạm |
| Metadata → SQS atomic không? | Không; index đến hạn + recovery sửa crash gap và tạo duplicate an toàn; outage kéo dài cần operator |
| Chứng minh partial failure thế nào? | Nêu chính xác direct Lambda mixed event với dependency thật và SQS BatchSize 1 redrive riêng; không nhận là natural mixed batch |
| Errors bằng 0 có nghĩa không lỗi? | Không; partial response/caught HTTP 500 có thể invocation thành công; xem HTTP/log/queue/DLQ |
| Test tách hệ thống cũ ra sao? | Stack/bảng prefix/bucket/user riêng, data ARN giới hạn, không migrate hoặc sửa tài nguyên đang có |
| 6,63 USD gồm gì? | Một tháng tải giả định gồm monitoring, trước ưu đãi/thuế; stack cũ, extras và usage thật tách riêng |
| Rollback/cleanup? | Dừng rollout lỗi, phục hồi artifact/config tương thích đã duyệt; giữ data, chỉ xóa theo inventory và chủ sở hữu đồng ý |

## Tự vẽ kiến trúc

Tải [icon AWS chính thức](https://aws.amazon.com/architecture/icons/), đọc `docs/architecture-self-draw.md`. Chính bạn vẽ từng box ứng với resource thật; ghi protocol/identity/data từng mũi tên; vùng global/regional/auth/owner/capability; đường retry/DLQ/recovery. Giữ file chỉnh sửa/ngày và tự luyện giải thích không đọc câu trả lời AI. Mermaid/PNG tham khảo của trợ lý không được nộp làm thiết kế của sinh viên. Chỉ chèn endpoint/sơ đồ thật khi đã có.
