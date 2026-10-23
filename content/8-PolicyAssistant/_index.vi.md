---
title: "Policy Assistant — Trạng thái dự án"
date: 2026-10-21
weight: 8
chapter: false
pre: " <b> 8. </b> "
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
