---
title: "Bản thảo 3 — xử lý lỗi SQS trung thực"
date: 2026-10-21
weight: 6
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

# Invocation Lambda thành công vẫn có thể chứa message thất bại

**Bản thảo chưa đăng, dựa trên sửa worker và kiểm thử local ngày 21/10/2026. Chưa thực hành DLQ trên AWS thật.**

SQS Standard yêu cầu xử lý trùng và lỗi tạm thời. Bắt exception rồi return bình thường có thể ACK message chưa xử lý thành công. Throw cho toàn batch lại khiến record tốt chạy lại. Policy Assistant xử lý độc lập từng record, trả ID lỗi trong `batchItemFailures`; SAM event mapping bật rõ `ReportBatchItemFailures`.

PDF sai vĩnh viễn được ghi failed rồi ACK có chủ đích: retry cùng byte bất biến không sửa được đầu vào. Lỗi storage/database tạm thời trả batch failure. Lần nhận thứ ba cố ghi PROCESSING_FAILED rồi vẫn báo record lỗi để redrive DLQ. Nếu gặp lại trạng thái failed của đúng attempt, worker không được ACK âm thầm. Event thiếu message ID bị xem là không hợp lệ, không tự bịa định danh.

Idempotency dùng cả state và attempt. Record processed trùng là no-op; message attempt cũ không được ghi đè lần retry mới. Conditional terminal write bảo vệ ngay cả khi owner retry và worker chạy đồng thời. Chốt version S3 giúp retry cùng byte. Ghi metadata và gửi queue là hai thao tác; recovery theo lịch query pending đến hạn rồi gửi lại cùng attempt. Giảm khoảng trống do crash nhưng không biến hai ghi thành atomic.

Regression gồm batch có một record lỗi/một tốt, đúng ID lỗi trả về, failed state không nuốt lỗi, message trùng/cũ và version chốt. Moto kiểm transition local, không chứng minh visibility timeout, receive count, xóa message hoặc DLQ redrive AWS. IaC hiện BatchSize 1, visibility 180 giây, worker timeout 30 giây và maxReceiveCount 3. Hỗ trợ batch hỗn hợp không đòi tăng batch ngay bây giờ.

Giám sát phải khớp protocol: partial failure có thể khiến Lambda Errors vẫn 0. Dashboard bổ sung SQS age/backlog/inflight/DLQ cạnh worker/recovery; log chỉ lớp lỗi an toàn, không SDK message thô. Người vận hành sửa nguyên nhân trước retry/redrive có duyệt, không purge tùy tiện. Xem [partial batch handling của AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html). Cần giải thích lỗi đầu vào vĩnh viễn, lỗi dịch vụ tạm thời, duplicate và giới hạn ghi failed khi database outage.
