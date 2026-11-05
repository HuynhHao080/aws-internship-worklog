---
title: "Bản thảo 2 — upload S3 giới hạn và version bất biến"
date: 2026-10-21
weight: 5
---

<!-- policy-assistant-managed:begin -->
## Bản chính — cập nhật tài liệu 06/11/2026

**Bản chính là [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). Các phần tháng 10 bên dưới là lịch sử của môi trường test khác.**

- Đọc [đề xuất hiện tại]({{< relref "/2-Proposal/CurrentSolution" >}}) để xem bài toán, phạm vi, kiến trúc cần tự vẽ, kế hoạch chốt, đầu vào COST và rủi ro. [Chương bản chính]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) giữ danh mục và bằng chứng qua từng release.
- Đã có đăng ký công khai, PDF riêng/text/OCR, hỏi đáp có nguồn, lịch sử, draft/publish/hiệu lực chính sách, thông tin shop, đánh giá tổng hợp và hỗ trợ có consent. [Phối hợp hỗ trợ]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}) bổ sung nhận việc/bàn giao/ghi chú nội bộ; không tự tạo quyền nhân viên hạn chế hoặc quyền đọc PDF khách.
- Snapshot runtime gần nhất: **322 Python đạt, 11 live bỏ qua; 67 frontend đạt; 20 HTTP checks đạt**, ba tài khoản QA và yêu cầu thử đã dọn. 40 tệp frontend và 6 Lambda khớp artifact. Timestamp UTC 05/11 tương ứng 06/11 Việt Nam; không phải lần chạy mới khi mở trang. Không gọi model/PDF/OCR/email trong đợt HTTP này. [Báo cáo](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/support-team-verification.md), [CI ứng dụng](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42072920395).
- Nội dung shop vẫn **DEMO**; còn chờ chính sách thật được phê duyệt, browser/accuracy/tải/restore/email theo phạm vi. Budget 50 USD chỉ cảnh báo; dự toán 6,63 USD/budget 20 USD bên dưới thuộc môi trường cũ, không dùng làm giá bản hiện tại.
- Ba blog đã đăng, minh chứng office/sự kiện, sơ đồ tự vẽ, Pricing Calculator và video còn chờ evidence cá nhân. Bản thảo kỹ thuật không phải bài đã đăng. Không sửa ngày thực tập, daily worklog hoặc tạo ảnh/điểm thay sinh viên.

Nội dung có AI hỗ trợ; sinh viên phải tự rà soát, thực hiện và giải thích. Kết quả DOM không phải visual browser; artifact/CI không phải accuracy hoặc SLA. Giữ nguyên mọi FAIL/PASS và ảnh lịch sử đúng môi trường.

### Tài liệu lịch sử bên dưới
<!-- policy-assistant-managed:end -->

# Giới hạn upload trước khi dữ liệu đi vào ứng dụng

**Bản thảo chưa đăng, ngày 21/10/2026. Dựa trên sửa mã và regression local thật; chưa nghiệm thu policy trên S3 thật.**

Upload session cũ trả presigned PUT. Submit chỉ kiểm size/MIME sau khi object đã vào S3. File quá lớn dù bị ứng dụng từ chối vẫn có thể chiếm storage và phát sinh phí. Kiểm size ở browser không ngăn client bị sửa.

Session mới dùng `generate_presigned_post`: bucket/key do server chọn, `Content-Type: application/pdf` chính xác, success status 201, hạn 300 giây, `content-length-range` từ 5 byte đến 10 MiB. Frontend thêm các field trả về vào FormData, file ở cuối, để browser tự tạo multipart boundary. Không chuyển Cognito Authorization sang S3. Chữ ký/field là thông tin truy cập tạm thời, không đưa vào minh chứng báo cáo.

Submit vẫn kiểm object đã lưu. Nhãn MIME ký đúng chưa chứng minh byte PDF; worker kiểm header/size. Phạm vi này không phải parser đầy đủ hoặc quét mã độc. Submit chốt S3 version ID và attempt ID. Dùng lại capability còn hạn có thể tạo current version mới, nhưng không thay được version đã nộp. Download cũng ký đúng version, để browser so SHA-256 gốc, worker và byte tải về.

Test giải mã POST policy để kiểm key/MIME và điều kiện byte range. Regression frontend kiểm field trước file, không gửi credentials hoặc tự đặt multipart header. Browser local chạy FormData qua handler backend thật và adapter storage giả lập, rồi so byte tải. Moto/local kiểm contract và hành vi ứng dụng; chỉ thử S3 thật mới chứng minh AWS từ chối file quá lớn, hết hạn hoặc policy bị sửa. Cần payload tổng hợp có giới hạn và chủ tài khoản cho phép trước thử thật.

Hệ quả vận hành: phải phối hợp protocol backend, CORS POST S3 và frontend cache. Client PUT cũ cần reload hoặc rollback đồng bộ. Retention version ảnh hưởng COST: expiry current tạo delete marker, bản cũ có thể vẫn lưu tiếp. Xem [điều kiện POST policy S3](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html), giả định COST của Workshop. Khi bảo vệ cần giải thích validation ở đâu, overwrite nghĩa gì khi versioning và `processed` không bảo đảm điều gì.
