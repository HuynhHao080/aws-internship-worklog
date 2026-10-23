---
title: "5.7 Policy Assistant — workshop hoàn chỉnh"
date: 2026-10-21
weight: 7
---

## Bàn giao local D4 — 22/10/2026

B4: **19 PASS**; B5 ban đầu: **FAIL**; B5-R7: **10 PASS**. D3-A: **BLOCKED_PENDING_OWNER_OPERATIONAL_CONFIRMATION**. Checklist D3-A2 đã chuẩn bị, chưa có câu trả lời của chủ dự án; chưa disable/sign-out hoặc reset credential. Public push/Pages và cleanup vẫn NO-GO, cần đáp ứng gate và phê duyệt riêng. D4 dùng evidence đã lưu, không health check AWS hoặc browser test mới. Tài liệu trong app repo local: `docs/d4-final-readiness-review.md`, `docs/d4-internship-project-summary.md`, `docs/d3a2-owner-operational-confirmation.md`; không giả định đã publish.


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

# Tra cứu chính sách có xác thực và xác minh PDF riêng tư

Đối chiếu mã/IaC local ngày 21/10/2026. Baseline ứng dụng `80c4eef`, Worklog `846d2ff`, cộng thay đổi local chưa commit. Không coi baseline đã chứa toàn bộ sửa đổi. Workshop này dành cho `HuynhHao080/aws-policy-assistant`. Các mục 5.1–5.6 S3/VPC Endpoint là bài lab riêng, không phải dependency ứng dụng. Nghiệm thu AWS thật vẫn chưa thực hiện.

## 1. Kết quả và điều kiện chuẩn bị

Ứng dụng có đăng nhập Cognito trực tiếp, tra cứu ID/keyword chính sách dùng chung, thư viện tài liệu riêng theo chủ và upload PDF → kiểm byte bất đồng bộ → download so hash. Chưa có AI/OCR/RAG/parser PDF đầy đủ/quét mã độc. `processed` nghĩa là phiên bản nộp đã qua kiểm header/kích thước và được tính SHA-256.

Cần Git, Python 3.12, Node.js 22+, npm, AWS CLI v2, SAM CLI, Hugo 0.167.0. Browser test Windows dùng Edge có sẵn; Linux chạy `npx playwright install --with-deps chromium`. AWS sau này cần sandbox đã duyệt, credentials CLI ngắn hạn và quyền triển khai do chủ tài khoản xét. Đổi mật khẩu demo từng nằm trong Git trước khi dùng; xóa chuỗi code không thu hồi mật khẩu. Không chụp bí mật.

Chạy tại **gốc repository ứng dụng**, kiểm môi trường sẵn có trước khi tạo lại:

```powershell
uv venv --python 3.12 .venv
uv pip install --python .venv/Scripts/python.exe -r policy_assistant/requirements.txt -r tests/requirements.txt
npm ci
.venv/Scripts/python.exe -m scripts.verify
.venv/Scripts/python.exe -m scripts.demo_server
```

Hoặc `pwsh -File scripts/start-demo.ps1`. Linux: `python3.12 -m venv .venv`, `.venv/bin/python -m pip install -r policy_assistant/requirements.txt -r tests/requirements.txt`, rồi thay interpreter bằng `.venv/bin/python`. Browser test tự mở server tạm; demo thủ công thường in `http://127.0.0.1:8080`. Giữ nhãn **OFFLINE**. Alice/Bob là định danh giả không mật khẩu; AWS được giả lập và dữ liệu mất khi tắt. Đây không phải link demo triển khai.

## 2. Kiến trúc thật và luồng dữ liệu

| Thành phần | Vai trò thực tế | Lý do / đánh đổi |
|---|---|---|
| CloudFront, frontend S3, OAC | UI HTTPS, origin private | Managed hosting; cần phối hợp cấu hình/cache |
| Cognito Lite, HTTP API | Login/challenge/refresh; gateway kiểm issuer/audience JWT | Identity managed; quản trị cấp user, MFA mặc định OFF |
| API Lambda Python 3.12 | Validation, ownership, tìm kiếm, document API | Không VM chạy chờ; SDK role riêng |
| DynamoDB PoliciesTable | Chính sách chung cho người đã xác thực | On-demand GetItem/Scan giới hạn; không phải full-text lớn |
| DynamoDB DocumentMetadata | Owner/state/version/attempt, index ByOwner/ByStatus | Conditional write/query giới hạn; index eventual consistent |
| S3 document private versioning | PDF đúng phiên bản, signed POST/GET | Chuyển file trực tiếp; capability tạm thời ai giữ cũng dùng được |
| SQS Standard, worker, DLQ | Kiểm hash/header/size; retry và giữ việc lỗi | At-least-once; conditional attempt ngăn ghi lỗi thời |
| EventBridge rule, recovery Lambda | Gửi lại pending đến hạn mỗi 2 phút | Sửa khoảng trống metadata → queue, không atomic liên dịch vụ |
| CloudWatch, SNS | JSON log 30 ngày, dashboard, 5 alarm, email tùy chọn xác nhận | Baseline vận hành; chưa chứng minh giao nhận thật |

Browser tải UI CloudFront, đăng nhập Cognito. HTTP API kiểm JWT, Lambda lấy `sub` từ authorizer đã tin cậy, không lấy owner từ body. `/health` chỉ là liveness public. Chính sách dùng chung; tài liệu riêng. API tự chọn key/tạo session của chủ, ký POST fields ràng buộc key/MIME PDF và **5 byte–10 MiB**. Browser thêm đủ field rồi file cuối, không gửi Cognito Authorization hoặc tự đặt multipart header.

Submit kiểm metadata S3, chốt version/attempt, ghi pending rồi gửi SQS. Recovery query pending đến hạn nếu crash giữa hai ghi. Worker chỉ đọc version đã chốt, cập nhật có điều kiện đúng attempt. Message đã thành công/trùng hoặc attempt cũ không đổi sai dữ liệu. PDF sai chuyển failed vĩnh viễn và ACK có chủ đích. Lỗi tạm từng record trả `batchItemFailures`; event mapping bật `ReportBatchItemFailures`. Timeout worker 30 s, visibility 180 s, tối đa nhận 3 lần, DLQ giữ 14 ngày. Browser polling, xin GET đúng version và so SHA-256 gốc/worker/download trước báo thành công.

**Thiếu sơ đồ do sinh viên tự vẽ.** Tải [icon AWS chính thức](https://aws.amazon.com/architecture/icons/), tự vẽ và giữ file chỉnh sửa/lịch sử. `docs/architecture-self-draw.md` có resource ID, từng kết nối, trust boundary, quyết định thiết kế và câu hỏi bảo vệ. Vẽ Internet/browser, edge global, dịch vụ regional ap-southeast-1, xác thực API, phân quyền owner, SDK role, capability S3, retry/DLQ/recovery/monitoring. Template không có VPC/NAT/AI. Mermaid/PNG tham khảo do trợ lý tạo không chứng minh kiến trúc tự thiết kế. Sau khi hoàn thành hãy chèn sơ đồ, tác giả và ngày tại đây.

Đánh giá Well-Architected: Security—ownership, IAM giới hạn, bucket private/mã hóa/TLS, upload giới hạn; Reliability—version bất biến, conditional attempt, partial batch, DLQ/recovery; Operations—verifier/runbook/log/alarm; Cost—on-demand, giả định có ngày và mọi version tính phí. Performance—Scan vẫn chỉ phù hợp tập nhỏ. Sustainability—tránh tài nguyên/artifact dư thừa. Chưa đo latency, khôi phục và restore trên AWS. Không phải chứng nhận AWS.

## 3. Chuẩn bị triển khai ở local

Giữ thay đổi của người dùng. Đọc `git status --short`, `git diff -- template.yaml`, `docs/deployment-review.md`, `docs/operations.md` và trang COST. Chạy từ gốc app:

```powershell
$env:PATH = "$PWD/.venv/Scripts;" + $env:PATH
sam validate --lint
sam build
.venv/Scripts/python.exe -m scripts.seed_policies
.venv/Scripts/python.exe -m scripts.estimate_cost
```

Seed mặc định preview, không gọi AWS. Verifier gỡ biến opt-in live test và ghi `docs/evidence/offline-acceptance.json`. Skip không phải PASS. Kiểm tra local không tạo change set AWS.

## 4. Triển khai chỉ khi chủ tài khoản cho phép

Đây là **hướng dẫn thao tác tương lai**, chưa thực hiện. Tự kiểm CLI profile/account qua `aws sts get-caller-identity`, region ap-southeast-1; không công khai định danh/credentials. Xem chi phí, IAM, replace/delete, retention, rollback trước.

```powershell
sam deploy --guided --stack-name policy-assistant --region ap-southeast-1 --no-execute-changeset
```

Lệnh này vẫn upload artifact/tạo change set nên cần ủy quyền ghi AWS. Đọc change set thật, chỉ thực thi change set đã duyệt bằng console sau khi được phép. Bảng mới dùng `EnableOwnerIndex=true`. Bảng metadata cũ chưa có GSI: lần đầu false thêm ByStatus, chờ ACTIVE; lần sau true bằng cập nhật duyệt riêng thêm ByOwner. **Không chuyển false khi đã có ByOwner.** Library chưa dùng được giữa hai bước. Không xóa/tạo lại bảng hoặc bucket chứa dữ liệu.

Đọc output PolicyAssistantApiUrl, CognitoUserPoolId, CognitoClientId, FrontendUrl, FrontendBucketName, FrontendDistributionId, rồi:

```powershell
.venv/Scripts/python.exe -m scripts.export_frontend_config --stack policy-assistant --region ap-southeast-1
.venv/Scripts/python.exe -m scripts.seed_policies --stack policy-assistant --region ap-southeast-1 --apply
```

Export chỉ đọc AWS, ghi API URL/region/client ID công khai. Seed chỉ tạo ID mẫu còn thiếu vào PoliciesTableName của stack, giữ dữ liệu đang có. Chủ tài khoản phải cho phép ghi và xác nhận đúng account. Qua Cognito console tạo hai user test do quản trị viên cấp; thống nhất mật khẩu riêng, thử NEW_PASSWORD_REQUIRED, refresh/logout. Client không secret, hỗ trợ USER_PASSWORD_AUTH/REFRESH_TOKEN_AUTH. Không nhập mật khẩu vào CLI argument hoặc quay màn hình chứa nó.

Chỉ publish bốn file `index.html`, `app.js`, `styles.css`, `config.json` từ frontend/ vào bucket output. Ví dụ `aws s3 cp frontend/app.js s3://<FrontendBucketName>/app.js`, lặp cho bốn file. Thay placeholder bằng output thật đã kiểm; không chạy nguyên dấu ngoặc nhọn. Không sync gốc repo. Invalidate bằng `aws cloudfront create-invalidation --distribution-id <FrontendDistributionId> --paths /index.html /app.js /styles.css /config.json`. Publish cũng cần cho phép. Mở FrontendUrl và nghiệm thu thật.

API CORS gồm localhost:3000, 127.0.0.1:3000, LocalFrontendOrigin (mặc định localhost:8080), frontend HTTPS sinh ra. S3 document chỉ LocalFrontendOrigin/frontend HTTPS, POST/GET. Nếu serve origin khác phải cấu hình rõ; CORS không thay phân quyền. Phối hợp backend, S3 POST CORS và frontend; client PUT cũ trong cache cần reload. Giữ artifact/config trước đó để rollback.

## 5. Kiểm thử, demo và bằng chứng

| Thao tác | Kết quả cần có | Bằng chứng local | AWS |
|---|---|---|---|
| Alice login, tra POL-001/keyword | Kết quả danh mục đúng | Browser với handler local thật | Chờ Cognito/API thật |
| POST PDF, submit, poll, download | processed; hash gốc/worker/download khớp | Policy/unit + byte browser tải | Chờ S3/SQS thật |
| Bob list/đọc/submit ID Alice | Không thấy tài liệu; route riêng 404 | Unit/browser ownership | Chờ hai user thật |
| JSON/ID/MIME/size/key sai | 400/404/409, không ghi trái quyền | Unit tham số hóa | Chờ ràng buộc storage thật |
| Batch hỗn hợp có lỗi tạm | Chỉ ID lỗi; record kia thành công | Regression worker/mapping | Chờ retry/redrive |
| Byte sai, message trùng/attempt cũ | Failed đầu vào hoặc no-op an toàn | Unit state/version | Chờ thử thật có kiểm soát |
| HTTP lỗi/poll timeout/hash sai | Không lưu file hoặc báo thành công giả | Frontend test | Chỉ xác nhận local tương ứng |

`scripts.verify` chạy lint Python nhóm lỗi nghiêm trọng, mypy app.py, cú pháp JS, unit/frontend, hai suite browser và SAM lint/build; không khẳng định toàn repo strict typed. Integration AWS cần opt-in, không được tính thành PASS thật khi skip. Với AWS ghi revision/diff, ngày/region, expected/actual, hash và cửa sổ thời gian CloudWatch. Thử JWT thiếu/sai/hết hạn, CORS, signed POST hết hạn/sửa field, ghi đè sau submit, trùng/retry/recovery/DLQ. Gây lỗi phải có kế hoạch an toàn được duyệt riêng. Không chụp Authorization, signed URL/field, mật khẩu hoặc Network body.

Demo 5–7 phút: bài toán/phạm vi → sơ đồ tự vẽ → login/search/upload/hash/library → từ chối user khác → bằng chứng queue/log/metric → COST/giới hạn. Xem `docs/demo-script.md`. Demo offline hữu ích nhưng chưa chứng minh sản phẩm triển khai AWS.

## 6. Giám sát và xử lý sự cố

Dashboard tách counter API, worker/recovery, queue/DLQ visible/inflight/oldest age và duration Lambda trung bình. Năm alarm: API Errors, API Duration tối đa, recovery Errors, queue age, DLQ visible. Email SNS tùy chọn cần AlertEmail và người nhận xác nhận; công việc local chưa gửi email.

Lỗi từng record có thể không tăng Lambda Errors; xem log an toàn `Worker record failed` cùng age/backlog/DLQ. API bắt lỗi trả 500 cũng có thể không tăng Errors; cần xem HTTP/log an toàn. Đây là baseline, chưa phải SLO production.

| Hiện tượng | Kiểm tra và xử lý |
|---|---|
| 401 | Issuer/audience/client/region/token hết hạn; login/refresh, không tắt auth |
| Document 404 | Chủ tài khoản/ID; không sửa owner_sub để vượt quyền |
| Upload 403/CORS | Origin, fields/key/MIME/hạn; session mới, file cuối, không JWT/header multipart tự đặt |
| Submit 409/503 | Object/state/versioning/queue; retry ID pending, PDF sai cần upload mới |
| Library thiếu dòng mới | GSI ACTIVE/eventual consistency, nâng cấp hai bước; không tạo lại bảng |
| Pending kéo dài | Age/log worker/recovery/DLQ; sửa dependency, không purge tùy tiện |
| Hash sai | Dừng lưu/báo thành công, kiểm version/byte; không bỏ so sánh |
| UI upload cũ | Bốn file deploy/cache CloudFront, phối hợp rollout/invalidate |

DLQ: kiểm lỗi/attempt an toàn, sửa dependency rồi chọn retry do owner hoặc redrive có duyệt. Attempt cũ không ghi đè state mới. Lỗi database có thể ngăn ghi failed; không chỉ dựa UI.

## 7. COST, budget và dọn dẹp

Đọc trang con **COST** để xem giá chính thức có ngày, quantity từng dịch vụ, bảng trước ưu đãi, kịch bản tăng tải và đầu vào Calculator. Chưa có Calculator estimate/link thật. Chỉ tạo budget sau duyệt; công việc local không gửi thông báo.

Retention hiện tại: log 30 ngày, DLQ 14 ngày, S3 current expiry 365 ngày và noncurrent expiry thêm 365 ngày từ khi thành bản cũ. Byte versioning có thể còn khoảng 730 ngày; không nói mọi dữ liệu xóa sau 365 ngày. Kiểm retained S3/DynamoDB, version/delete marker/backup/log/CloudFront/SNS sau teardown. `sam delete` có thể để lại resource Retain hoặc bucket không rỗng. Cần chủ đồng ý export/xóa; không recursive delete tùy tiện. Ghi inventory còn lại và kiểm Billing sau cleanup được duyệt. Runbook rollback/retention ở `docs/deployment-review.md`.

## 8. Build website báo cáo

Tại repo Worklog chạy `hugo --destination <thư-mục-output-tạm>` bằng Hugo 0.167.0; sau đó tại repo app chạy `python -m scripts.check_worklog_links --site-root <thư-mục-output-tạm>`. Chỉ kiểm link local sinh ra, không chứng minh publish hoặc mọi site bên ngoài. Trước nộp cần sinh viên bổ sung ngày thực tập/daily thật, chuyên cần/sự kiện, link blog đã đăng và sơ đồ tự vẽ.


## Cập nhật AWS thật ngày 21/10/2026

Đã kiểm account/region/stack cũ chỉ đọc và tạo change set test riêng chưa execute. Đọc trang con **Triển khai AWS test — chờ duyệt** và **Demo — 5 đến 7 phút**. Chưa có kết quả E2E hoặc endpoint AWS của release mới; không ghi PASS khi chưa chạy.


Bản hardening hiện tại: 128 backend tests, 17 frontend tests và hai browser suites local đạt. Candidate 45 tài nguyên CHƯA EXECUTE; kiểm AWS/artifact mới bị chặn do kết nối. Xem [trạng thái AWS](AWSReadiness/).
