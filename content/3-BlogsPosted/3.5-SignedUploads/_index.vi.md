---
title: "Bản thảo 2 — upload S3 giới hạn và version bất biến"
date: 2026-10-21
weight: 5
---

<!-- policy-assistant-managed:begin -->
## Bản chính — cập nhật kỹ thuật 04/11/2026

**Phần cập nhật này mô tả bản chính [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). Các phần 21–23/10 phía dưới là lịch sử của môi trường test khác.** Không lấy kết quả cũ làm nghiệm thu bản chính.

- Hiện có đăng ký công khai, thư viện PDF riêng, trích xuất/OCR, hỏi đáp có nguồn, lịch sử, quản trị chính sách, yêu cầu hỗ trợ, cấu hình shop và đánh giá câu trả lời. Nội dung shop vẫn là **DEMO**, chưa có đơn vị vận hành phê duyệt chính sách thật.
- Báo cáo release ngày 04/11: **251 Python PASS, 11 live SKIP; 54 frontend PASS; 15 kiểm tra API/metadata/dọn dữ liệu PASS**. Frontend dùng DOM giả lập; kiểm tra API chất lượng dùng dữ liệu tổng hợp và hai tài khoản tạm, không gọi model. Không phải bằng chứng kiểm thử hình ảnh trình duyệt, độ chính xác AI hoặc CI mới.
- [Chương bản chính, hướng dẫn và giới hạn]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) có bảng chức năng, kịch bản demo, đầu vào chi phí và bằng chứng đã lọc. Ngày 05/11, chủ dự án cho phép công bố: [CI ứng dụng đạt](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42050386735), [worklog đã lên GitHub Pages](https://github.com/HuynhHao080/aws-internship-worklog/actions/runs/42049972955). Frontend AWS đã xuất bản lại; 31 file và 6 Lambda khớp hash, backend không cần thay đổi artifact. Các ghi chú “local/chưa publish” bên dưới là snapshot trước lần công bố này.
- Ngày 04/11, chủ dự án cho biết chưa thực hiện các mục được hỏi: ba blog đã đăng, minh chứng office/sự kiện, sơ đồ tự vẽ và Pricing Calculator. Giữ trạng thái **CHỜ**; không suy diễn điều đó thành chưa làm toàn bộ nhật ký cũ. Ba bài kỹ thuật 3.4–3.6 vẫn là bản thảo cần tự rà soát và đăng.

Nội dung mới có AI hỗ trợ; sinh viên cần đọc, tự thực hiện và giải thích trước khi nộp. Giữ nguyên các lần FAIL, PASS, CI và ảnh lịch sử theo đúng ngày/môi trường. Dự toán 6,63 USD và budget 20 USD trong phần cũ không mô tả đầy đủ bản AI/OCR hiện tại.

### Tài liệu lịch sử bên dưới
<!-- policy-assistant-managed:end -->

# Giới hạn upload trước khi dữ liệu đi vào ứng dụng

**Bản thảo chưa đăng, ngày 21/10/2026. Dựa trên sửa mã và regression local thật; chưa nghiệm thu policy trên S3 thật.**

Upload session cũ trả presigned PUT. Submit chỉ kiểm size/MIME sau khi object đã vào S3. File quá lớn dù bị ứng dụng từ chối vẫn có thể chiếm storage và phát sinh phí. Kiểm size ở browser không ngăn client bị sửa.

Session mới dùng `generate_presigned_post`: bucket/key do server chọn, `Content-Type: application/pdf` chính xác, success status 201, hạn 300 giây, `content-length-range` từ 5 byte đến 10 MiB. Frontend thêm các field trả về vào FormData, file ở cuối, để browser tự tạo multipart boundary. Không chuyển Cognito Authorization sang S3. Chữ ký/field là thông tin truy cập tạm thời, không đưa vào minh chứng báo cáo.

Submit vẫn kiểm object đã lưu. Nhãn MIME ký đúng chưa chứng minh byte PDF; worker kiểm header/size. Phạm vi này không phải parser đầy đủ hoặc quét mã độc. Submit chốt S3 version ID và attempt ID. Dùng lại capability còn hạn có thể tạo current version mới, nhưng không thay được version đã nộp. Download cũng ký đúng version, để browser so SHA-256 gốc, worker và byte tải về.

Test giải mã POST policy để kiểm key/MIME và điều kiện byte range. Regression frontend kiểm field trước file, không gửi credentials hoặc tự đặt multipart header. Browser local chạy FormData qua handler backend thật và adapter storage giả lập, rồi so byte tải. Moto/local kiểm contract và hành vi ứng dụng; chỉ thử S3 thật mới chứng minh AWS từ chối file quá lớn, hết hạn hoặc policy bị sửa. Cần payload tổng hợp có giới hạn và chủ tài khoản cho phép trước thử thật.

Hệ quả vận hành: phải phối hợp protocol backend, CORS POST S3 và frontend cache. Client PUT cũ cần reload hoặc rollback đồng bộ. Retention version ảnh hưởng COST: expiry current tạo delete marker, bản cũ có thể vẫn lưu tiếp. Xem [điều kiện POST policy S3](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html), giả định COST của Workshop. Khi bảo vệ cần giải thích validation ở đâu, overwrite nghĩa gì khi versioning và `processed` không bảo đảm điều gì.
