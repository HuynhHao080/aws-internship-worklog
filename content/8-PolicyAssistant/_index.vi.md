---
title: "Policy Assistant — Trạng thái dự án"
date: 2026-10-21
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

## Hiện trạng D1 —22/10/2026

**B4 API/SDK19PASS; B5 browserFAIL lịch sử; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO demo test có giới hạn, NO-GO production.** D1 chỉ rà soát local, không chạy AWS/E2E hoặc publish website.

DLQ còn khoảng1diagnostic/ALARM; SNS alarm topic0subscriptions. Budget20USD toàn account không hard cap; email delivery chưa chứng minh. Cleanup chưa thực hiện. **Ngày kết thúc duy trì môi trường chưa được chủ dự án chốt.** Credentials cũ trong Git history cần rotation/revocation được xác minh trước public push.

Browser fix là virtual S3 addressing cho regional POST/GET, không mở IAM/CSP/CORS. R7 dùng Edge thật,2PDF/1.218bytes,3login/3logout,27requests; không phải load benchmark. Deployed API dùng ZIP mới, Worker/Recovery giữ ZIP cũ. Source SAM shared CodeUri không phải replay của API-only candidate.

Evidence nằm trong application repo local: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md; chưa giả định đã commit/push. Ảnh local/mocks không thay evidence AWS. Sinh viên tự vẽ, kiểm tra và giải thích architecture bằng icon AWS chính thức; không nhận sơ đồ tham khảo AI là thiết kế do mình vẽ.

### Các snapshot trước đây — giữ lịch sử, không phải trạng thái hiện hành

Các đoạn pending/chưa deploy/browser incomplete bên dưới được supersede bởi trạng thái D1 bên trên; B5FAIL không bị sửa thành PASS.



> Cập nhật bằng chứng 21/10/2026: B1 stack/B2 frontend/B3 users đã hoàn tất; B4 API/SDK 19 PASS, browser thực tế NOT_RUN. Xem chương AWS E2E. Các đoạn pending/not-deployed dưới đây là kế hoạch/lịch sử trước B4, không phải trạng thái hiện hành. Không chạy lại poison hoặc demo có writes khi chưa được duyệt tiếp.

# Policy Assistant — tiến độ có minh chứng

Cập nhật 21/10/2026. Các sửa đổi dưới đây hiện ở working tree local; chưa push hoặc triển khai AWS trong đợt hoàn thiện này.

## Phạm vi

Tra cứu chính sách theo ID, tìm kiếm từ khóa và tiếp nhận PDF theo chủ sở hữu. Không có Bedrock, RAG, OCR hoặc trích xuất nội dung chính sách từ PDF. `processed` nghĩa worker đã kiểm tra byte PDF và tính SHA-256.

## Mốc bàn giao

- [AWS Policy Assistant](https://github.com/HuynhHao080/aws-policy-assistant): `80c4eef`.
- [AWS Internship Worklog](https://github.com/HuynhHao080/aws-internship-worklog): `846d2ff`.
- Hai commit trên là mốc trước sửa đổi local, chưa chứa những thay đổi chưa push.

## Kiến trúc trong mã nguồn hiện tại

Frontend local → Cognito JWT / API Gateway HTTP API → Lambda Python 3.12 → DynamoDB Policies và Document Metadata. PDF nằm trong S3 private versioned. Submit chốt phiên bản S3 và gửi SQS. Worker xác minh PDF, tính SHA-256 và cập nhật metadata; lỗi được retry rồi chuyển DLQ theo cấu hình. CloudWatch có log và alarm.

## Công việc đã thực hiện

- Gắn upload session với JWT sub; kiểm tra chủ sở hữu ở submit/status/download. Record cũ không có owner không tự được gán cho người dùng.
- Submit dùng key do server lưu và chốt version ID. Tải xuống đúng bản đã xử lý.
- Worker không nuốt lỗi; message thành công xử lý lặp lại không reset trạng thái.
- Sửa cursor phân trang để không bỏ sót kết quả; kiểm tra hình dạng cursor đầu vào.
- Frontend polling, xử lý HTTP lỗi, tải PDF và đối chiếu SHA-256 file gốc/worker/bản tải về.
- Thay script có mật khẩu hardcode bằng nhập mật khẩu tương tác, không in JWT hoặc presigned URL.
- Bổ sung kiểm thử phân quyền hai user, version S3, retry, phân trang và trình duyệt.

## Kết quả đã kiểm chứng local

- Python 3.12.10: **120 unit tests pass**, dịch vụ AWS giả lập bằng moto.
- Frontend: **17 tests pass** trên Node; **2 browser scenarios pass** trên Edge headless, gồm upload/download, polling, lỗi 401/500, SHA-256 và mobile không tràn ngang.
- Một kịch bản dùng HTTP fixture; kịch bản full-stack đi qua backend Lambda thật và AWS/authentication giả lập bằng moto. Đây không phải bằng chứng E2E AWS thật.
- 9 integration tests được skip có chủ đích khi chưa bật kiểm thử live.
- Kết quả build/lint và ảnh minh chứng nằm trong `docs/final-verification.md` và `docs/evidence/` của ứng dụng local.

## Cần hoàn tất trước khi nộp

1. Đổi các mật khẩu từng bị commit. Xóa literal khỏi mã mới không xóa lịch sử hoặc vô hiệu hóa mật khẩu cũ.
2. Review thay đổi và change set; chỉ triển khai khi được cho phép. Sau đó kiểm chứng JWT, hai user, S3 CORS, worker/DLQ và SHA-256 trên AWS.
3. Lưu evidence đã che bí mật, gắn commit/ngày/môi trường rồi cập nhật trạng thái thực tế.
4. Xác nhận worklog, blog, tự đánh giá và feedback từ trải nghiệm thật. Không thay hoạt động IoT thành Policy Assistant.
5. Chốt chi phí theo usage và trình bày giới hạn: Scan, enqueue không transactional, giới hạn recovery khi quá tải/sự cố metadata, retention S3 và hosting chưa nghiệm thu live.

Workshop S3/VPC Endpoint là nội dung học tập độc lập. Bedrock/RAG chỉ là hướng phát triển có điều kiện.

## Hoàn thiện sản phẩm MVP v1

- Đăng nhập Cognito trực tiếp; xử lý đổi mật khẩu lần đầu, MFA challenge, refresh và đăng xuất. Tài khoản do quản trị viên cấp; chưa triển khai các thay đổi này lên AWS.
- Thư viện PDF có tên file, phân trang theo owner và kiểm tra lỗi/khôi phục theo Document ID.
- Worker có failed state và attempt ID; retry cũ không thể ghi đè lần xử lý mới. Recovery theo lịch truy vấn tối đa 20 pending đến hạn mỗi lượt, bù trường hợp ghi metadata xong nhưng chưa gửi được queue.
- SAM định nghĩa hosting private S3/CloudFront HTTPS/OAC/security headers, SNS alarm tùy chọn email và các index phục vụ truy vấn. Nâng cấp bảng cũ cần hai bước tạo GSI theo runbook.
- Chạy `python -m scripts.demo_server` để dùng toàn bộ ứng dụng offline; `python -m scripts.verify` để nghiệm thu; `python -m scripts.build_release` để tạo gói bàn giao ZIP và manifest SHA-256.

![Ứng dụng full-stack local](/aws-internship-worklog/images/policy-assistant/full-stack-desktop.png)

*Minh chứng local: trình duyệt → backend thật → AWS/authentication giả lập. Không phải ảnh triển khai AWS; không chứa token hoặc URL ký số.*


## Đợt P0 và hồ sơ FCAJ mới

Signed POST giới hạn key/MIME/5 byte–10 MiB; JSON/base64/ID validation; log không chứa request/SDK message thô; partial batch SQS; TLS-only bucket; queue SSE; Cognito Lite; HeadObject 403 có kiểm soát. Có mypy/lint/cú pháp JS; seed không ghi đè dữ liệu đã có. Kết quả local hiện tại: 120 backend, 17 frontend, hai suite browser; SAM lint/build đạt. AWS integration 9 skip, không tính PASS.

Workshop chính ở 5.7 có hướng dẫn tái lập, monitoring/troubleshooting/cleanup; COST kiểm giá AWS 21/10/2026 khoảng 6,6306 USD/tháng gross theo giả định, không phải hóa đơn. Website build 124 EN/123 VI outputs; kiểm toàn bộ 125 HTML và 11.638 tham chiếu nội bộ không lỗi. Còn ba cảnh báo deprecation theme. Có ba blog bản thảo Việt/Anh (3.4–3.6), chưa đăng. Đối soát Daily tuần 1–12 giữ nguyên lịch sử; thiếu xác nhận ngày/reference hoạt động thật, 10 office/3 event, tự đánh giá/feedback và sơ đồ sinh viên tự vẽ. Không tự tạo bằng chứng hoặc deploy/push.
