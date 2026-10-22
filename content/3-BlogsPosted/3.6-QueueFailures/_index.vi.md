---
title: "Bản thảo 3 — xử lý lỗi SQS trung thực"
date: 2026-10-21
weight: 6
---

# Invocation Lambda thành công vẫn có thể chứa message thất bại

**Bản thảo chưa đăng, dựa trên sửa worker và kiểm thử local ngày 21/10/2026. Chưa thực hành DLQ trên AWS thật.**

SQS Standard yêu cầu xử lý trùng và lỗi tạm thời. Bắt exception rồi return bình thường có thể ACK message chưa xử lý thành công. Throw cho toàn batch lại khiến record tốt chạy lại. Policy Assistant xử lý độc lập từng record, trả ID lỗi trong `batchItemFailures`; SAM event mapping bật rõ `ReportBatchItemFailures`.

PDF sai vĩnh viễn được ghi failed rồi ACK có chủ đích: retry cùng byte bất biến không sửa được đầu vào. Lỗi storage/database tạm thời trả batch failure. Lần nhận thứ ba cố ghi PROCESSING_FAILED rồi vẫn báo record lỗi để redrive DLQ. Nếu gặp lại trạng thái failed của đúng attempt, worker không được ACK âm thầm. Event thiếu message ID bị xem là không hợp lệ, không tự bịa định danh.

Idempotency dùng cả state và attempt. Record processed trùng là no-op; message attempt cũ không được ghi đè lần retry mới. Conditional terminal write bảo vệ ngay cả khi owner retry và worker chạy đồng thời. Chốt version S3 giúp retry cùng byte. Ghi metadata và gửi queue là hai thao tác; recovery theo lịch query pending đến hạn rồi gửi lại cùng attempt. Giảm khoảng trống do crash nhưng không biến hai ghi thành atomic.

Regression gồm batch có một record lỗi/một tốt, đúng ID lỗi trả về, failed state không nuốt lỗi, message trùng/cũ và version chốt. Moto kiểm transition local, không chứng minh visibility timeout, receive count, xóa message hoặc DLQ redrive AWS. IaC hiện BatchSize 1, visibility 180 giây, worker timeout 30 giây và maxReceiveCount 3. Hỗ trợ batch hỗn hợp không đòi tăng batch ngay bây giờ.

Giám sát phải khớp protocol: partial failure có thể khiến Lambda Errors vẫn 0. Dashboard bổ sung SQS age/backlog/inflight/DLQ cạnh worker/recovery; log chỉ lớp lỗi an toàn, không SDK message thô. Người vận hành sửa nguyên nhân trước retry/redrive có duyệt, không purge tùy tiện. Xem [partial batch handling của AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html). Cần giải thích lỗi đầu vào vĩnh viễn, lỗi dịch vụ tạm thời, duplicate và giới hạn ghi failed khi database outage.
