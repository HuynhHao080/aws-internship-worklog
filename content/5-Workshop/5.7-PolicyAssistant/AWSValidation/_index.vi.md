---
title: "AWS E2E — bằng chứng thực tế"
date: 2026-10-21
weight: 5
---

## Hiện trạng D1 —22/10/2026

**B4 API/SDK19PASS; B5 browserFAIL lịch sử; B5-R6 API-only UPDATE_COMPLETE; B5-R7 browser10PASS. C0:45resources/4Retain. GO demo test có giới hạn, NO-GO production.** D1 chỉ rà soát local, không chạy AWS/E2E hoặc publish website.

DLQ còn khoảng1diagnostic/ALARM; SNS alarm topic0subscriptions. Budget20USD toàn account không hard cap; email delivery chưa chứng minh. Cleanup chưa thực hiện. **Ngày kết thúc duy trì môi trường chưa được chủ dự án chốt.** Credentials cũ trong Git history cần rotation/revocation được xác minh trước public push.

Browser fix là virtual S3 addressing cho regional POST/GET, không mở IAM/CSP/CORS. R7 dùng Edge thật,2PDF/1.218bytes,3login/3logout,27requests; không phải load benchmark. Deployed API dùng ZIP mới, Worker/Recovery giữ ZIP cũ. Source SAM shared CodeUri không phải replay của API-only candidate.

Evidence nằm trong application repo local: docs/aws-browser-b5r7-result.md, docs/cleanup-c0-inventory-and-acceptance.md, docs/d1-handover-review.md; chưa giả định đã commit/push. Ảnh local/mocks không thay evidence AWS. Sinh viên tự vẽ, kiểm tra và giải thích architecture bằng icon AWS chính thức; không nhận sơ đồ tham khảo AI là thiết kế do mình vẽ.

### Các snapshot trước đây — giữ lịch sử, không phải trạng thái hiện hành

Các đoạn pending/chưa deploy/browser incomplete bên dưới được supersede bởi trạng thái D1 bên trên; B5FAIL không bị sửa thành PASS.



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
