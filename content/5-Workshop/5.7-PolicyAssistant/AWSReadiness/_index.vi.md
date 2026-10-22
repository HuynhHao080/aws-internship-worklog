---
title: "Trạng thái AWS sau B4"
date: 2026-10-21
weight: 2
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

B1/B2/B3 đã hoàn tất; B4 API/SDK thật 19 PASS, browser 1 NOT_RUN. Candidate 45 Add đã được execute đúng một lần; stack test CREATE_COMPLETE. Không tiếp tục ghi candidate AVAILABLE hoặc kết nối BLOCKED từ lịch sử. Chi tiết trong chương AWS E2E bên cạnh.

Bốn Retain: PoliciesTable, DocumentMetadata, DocumentsBucket, HTTPS bucket policy. DLQ/QueueAge ALARM từ poison có chủ đích; message còn nguyên. Không cleanup hoặc sửa để ép PASS. Không ảnh hưởng stack cũ; metadata cũ không đổi, không có full drift scan. IAM không runtime Allow Resource *, ba log grants test-only. Budget20 USD không hard cap; email delivery chưa xác minh. Mọi thay đổi sau B4 cần duyệt riêng.
