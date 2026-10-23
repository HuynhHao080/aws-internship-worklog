---
title: "AWS E2E — bằng chứng thực tế"
date: 2026-10-21
weight: 5
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

> **B5-R local remediation:** đã xác nhận local Edge CSP connect-src chặn global S3 hostname. Helper sửa một dòng virtual addressing; regional POST/GET và hash pipeline local PASS. 129 backend/17 frontend/3 browser fixture/1 full-stack local và SAM lint/build PASS;9 integration skip=NOT_RUN. Không đổi CSP/CORS/IAM/template/frontend, chưa upload artifact hoặc deploy; B5 AWS vẫn FAIL. Root cause AWS gốc LIKELY vì không giữ exact violation; local mechanism CONFIRMED. Cần duyệt riêng artifact/change set/deploy/rerun; cleanup C riêng.

> **Cập nhật22/10/2026: B5 browser FAIL/STOPPED**, 2PASS/1FAIL/7NOT_RUN. Edge thật login/search TestA PASS; upload UI lỗi sau session200, trước S3 upload. Nguyên nhân khả dĩ CSP regional-only/global S3 signing host; chưa capture exact violation. Một orphan metadata uploading, không có S3 object; main0/QueueAge OK, DLQ1/ALARM. Không fix/deploy/rerun/cleanup; chờ duyệt riêng. B4 API/SDK PASS bên dưới là snapshot trước B5, không thay thế browser failure. Không có screenshot browser success.

# AWS E2E API/SDK — 21/10/2026

**19 PASS / 0 FAIL / 0 BLOCKED / 1 NOT_RUN (browser thật)**. Cửa sổ 16:47:11–16:57:34 UTC; follow-up alarms 16:59:27 UTC. Account …0098, ap-southeast-1, stack policy-assistant-fcaj-test. B1 tạo 45 tài nguyên; B2 publish bốn static assets; B3 tạo đúng hai users tổng hợp. B4 chạy một lần, tiếp tục theo dõi cùng tiến trình sau gián đoạn, không gửi thêm poison.

| Phạm vi | Kết quả |
|---|---|
| Cognito | Hai identities đăng nhập; sai password bị từ chối; JWT thiếu/sai HTTP401; không đổi email_verified=false |
| API | Health/lookup/search/pagination/input/CORS PASS; seed ba policy tổng hợp |
| Documents | Hai PDF qua presigned POST → submit → SQS worker → status → download; ba hash gốc/worker/download bằng nhau |
| Ownership | Sáu cross-user status/download/submit HTTP404, thư viện không lộ tài liệu user kia |
| Validation | 10 MiB accepted, +1 byte rejected; MIME/key tampering rejected; non-PDF INVALID_PDF/download409 |
| Version/idempotency | Ghi phiên bản mới vẫn tải frozen original; resubmit giữ processed |
| Partial failure | Direct Lambda event, chỉ record lỗi xuất hiện trong batchItemFailures; healthy record đã processed nên chứng minh idempotent acknowledgement |
| Retry/DLQ | Một poison, ba main receives và một DLQ read, marker/message ID khớp; không delete/purge hoặc đổi visibility config |
| Logs | 158 events, không thấy secret theo exact values/patterns trong cửa sổ; không xuất raw logs |
| Browser CloudFront | **NOT_RUN**; tests browser local không thay thế AWS browser acceptance |

Mapping BatchSize=1; direct mixed event không chứng minh natural multi-record SQS batch. Upload thực tế là **POST**, không phải PUT. Sản phẩm kiểm size/header/hash, không full PDF parser, malware scan, OCR/AI/RAG.

DLQ nhận message lúc 16:57:30 UTC. Lần đầu DLQ alarm còn OK; follow-up ghi **DLQ ALARM và QueueAge ALARM**, ba alarms còn lại OK. Đây là fault injection có chủ đích; không reset alarm hoặc xóa message để ép OK. Metrics có độ trễ: không dùng empty/zero series để phủ nhận queue attributes. Lambda Errors=0 không đồng nghĩa không retry khi handler trả partial failures.

Budget vẫn COST/MONTHLY/20 USD, toàn account, UnblendedCost loại Credit/Refund, ba cảnh báo, không Actions. Snapshot sau test Actual5,479/Forecast8,539 USD có độ trễ Billing, không là chi phí riêng B4. Email Budget: Active(legacy), verification0/1 theo Console trước; delivery chưa xác minh. Budget không hard cap.

Nguồn local: application repo `docs/aws-test-e2e-b4-result.md`, `docs/evidence/approval-b4-e2e.json`, `approval-b4-alarm-followup.json`. Chưa commit/push/publish website; không có screenshot/browser minh chứng để bịa thêm. Credentials DPAPI riêng, không public.

**GO nghiệm thu backend/API-SDK; browser acceptance vẫn thiếu. DỪNG sau B4.** Giữ dữ liệu tổng hợp, bốn metadata sessions, ba policy fixtures, một DLQ message để chờ cleanup được duyệt riêng. PoliciesTable, DocumentMetadata, DocumentsBucket và HTTPS policy có Retain; không tháo policy khi bucket còn. Mọi sửa/deploy/cleanup tiếp theo cần phê duyệt độc lập. Chuyên cần/sự kiện/sơ đồ sinh viên tự vẽ vẫn cần minh chứng thật.
