---
title: "Bản thảo 1 — ownership ngoài JWT"
date: 2026-10-21
weight: 4
---

<!-- policy-assistant-managed:begin -->
## Bản chính — cập nhật tài liệu 06/11/2026

**Model của bản chính:** Global Claude Haiku 5.5. Ứng dụng xếp hạng từ khóa trên văn bản PDF và chính sách. Các báo cáo kiểm thử có ngày bên dưới ghi lại từng bản đã nghiệm thu. [Bằng chứng chuyển model](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/haiku55-migration.md).

**[Bộ hồ sơ nộp]({{< relref "/5-Workshop/5.7-PolicyAssistant/Submission" >}})**

**Bản chính là [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). Các phần tháng 10 bên dưới là lịch sử của môi trường test khác.**

- Đọc [đề xuất hiện tại]({{< relref "/2-Proposal/CurrentSolution" >}}) để xem bài toán, phạm vi, kiến trúc cần tự vẽ, kế hoạch chốt, đầu vào COST và rủi ro. [Chương bản chính]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) giữ danh mục và bằng chứng qua từng release.
- Đã có đăng ký công khai, PDF riêng/text/OCR, hỏi đáp có nguồn, lịch sử, draft/publish/hiệu lực chính sách, thông tin shop, đánh giá tổng hợp và hỗ trợ có consent. [Phối hợp hỗ trợ]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}) bổ sung nhận việc/bàn giao/ghi chú nội bộ; không tự tạo quyền nhân viên hạn chế hoặc quyền đọc PDF khách.
- Snapshot runtime gần nhất: **322 Python đạt, 11 live bỏ qua; 67 frontend đạt; 20 HTTP checks đạt**, ba tài khoản QA và yêu cầu thử đã dọn. 40 tệp frontend và 6 Lambda khớp artifact. Timestamp UTC 05/11 tương ứng 06/11 Việt Nam; không phải lần chạy mới khi mở trang. Không gọi model/PDF/OCR/email trong đợt HTTP này. [Báo cáo](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/support-team-verification.md), [CI ứng dụng](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42072920395).
- Nội dung shop vẫn **DEMO**; còn chờ chính sách thật được phê duyệt, browser/accuracy/tải/restore/email theo phạm vi. Budget 50 USD chỉ cảnh báo; dự toán 6,63 USD/budget 20 USD bên dưới thuộc môi trường cũ, không dùng làm giá bản hiện tại.
- Ba blog đã đăng, minh chứng office/sự kiện, sơ đồ tự vẽ, Pricing Calculator và video còn chờ evidence cá nhân. Bản thảo kỹ thuật không phải bài đã đăng. Không sửa ngày thực tập, daily worklog hoặc tạo ảnh/điểm thay sinh viên.

Nội dung có AI hỗ trợ; sinh viên phải tự rà soát, thực hiện và giải thích. Kết quả DOM không phải visual browser; artifact/CI không phải accuracy hoặc SLA. Giữ nguyên mọi FAIL/PASS và ảnh lịch sử đúng môi trường.

### Tài liệu lịch sử bên dưới
<!-- policy-assistant-managed:end -->

# Đăng nhập đúng chưa đủ để được đọc tài liệu

**Bản thảo kỹ thuật dựa trên mã và kiểm thử local Policy Assistant hiện tại. Chưa đăng; cần sinh viên tự rà soát, đăng và bổ sung URL/ngày thật.** Không ghi thành công việc quá khứ hoặc kết quả AWS thật.

HTTP API có thể chặn JWT Cognito sai nhưng ứng dụng vẫn lộ tài liệu nếu chỉ nhận doc ID rồi trả dữ liệu. ID là định danh, không phải quyền. Policy Assistant tách hai mô hình: danh mục chính sách dùng chung cho người đã đăng nhập, còn metadata/file tài liệu thuộc từng chủ sở hữu.

Identity tin cậy lấy từ `sub` trong authorizer claims của API Gateway. API không lấy owner do client tự khai và tự chọn key lưu trữ. `_owned_document` so owner đã lưu với subject hiện tại. Tài liệu không tồn tại và tài liệu người khác đều trả 404 để không tiết lộ sự tồn tại. Record cũ thiếu owner bị từ chối, không tự gán. Query library ByOwner và cursor phân trang cũng gắn với subject; chỉ bảo vệ detail route sẽ chưa chặn dò danh sách.

Frontend xóa state riêng khi logout/401 để Bob dùng cùng browser không nhìn state Alice. Tuy vậy, ẩn UI chưa đủ: API phải chặn khi Bob nhập thủ công ID đã biết của Alice. URL storage ký sẵn là bearer capability ngắn hạn, ai giữ cũng có thể dùng đến khi hết hạn. Không lưu trong log/ảnh. S3 không tự kiểm JWT Cognito cho request ký sẵn này.

Bằng chứng có ý nghĩa là test phủ định ownership trong `tests/unit/test_documents.py` và `tests/frontend/local.e2e.cjs`: Alice upload/download; Bob có library rỗng, nhận 404 cho ID Alice. Test dùng handler thật với moto/identity giả local, **không** kiểm chữ ký Cognito hoặc IAM/CORS đang deploy. Sau khi được phép triển khai, sinh viên cần lặp lại tất cả document endpoint bằng hai tài khoản Cognito thật, JWT thiếu/sai/hết hạn và cursor bị sửa.

Thiết kế không cấp credentials S3 tổng quát cho browser, phân biệt service IAM role với quyền user. Giới hạn còn có capability chia sẻ được và onboarding do quản trị cấp. Tham khảo [JWT authorizer HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html), ma trận nghiệm thu ở Workshop. Câu hỏi bảo vệ: identity tin cậy từ đâu, vì sao trả 404, vì sao CORS không bảo vệ ownership?
