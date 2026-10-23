---
title: "Trạng thái AWS sau B4"
date: 2026-10-21
weight: 2
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

B1/B2/B3 đã hoàn tất; B4 API/SDK thật 19 PASS, browser 1 NOT_RUN. Candidate 45 Add đã được execute đúng một lần; stack test CREATE_COMPLETE. Không tiếp tục ghi candidate AVAILABLE hoặc kết nối BLOCKED từ lịch sử. Chi tiết trong chương AWS E2E bên cạnh.

Bốn Retain: PoliciesTable, DocumentMetadata, DocumentsBucket, HTTPS bucket policy. DLQ/QueueAge ALARM từ poison có chủ đích; message còn nguyên. Không cleanup hoặc sửa để ép PASS. Không ảnh hưởng stack cũ; metadata cũ không đổi, không có full drift scan. IAM không runtime Allow Resource *, ba log grants test-only. Budget20 USD không hard cap; email delivery chưa xác minh. Mọi thay đổi sau B4 cần duyệt riêng.
