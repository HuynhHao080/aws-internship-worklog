---
title: "Bản thảo 2 — upload S3 giới hạn và version bất biến"
date: 2026-10-21
weight: 5
---

# Giới hạn upload trước khi dữ liệu đi vào ứng dụng

**Bản thảo chưa đăng, ngày 21/10/2026. Dựa trên sửa mã và regression local thật; chưa nghiệm thu policy trên S3 thật.**

Upload session cũ trả presigned PUT. Submit chỉ kiểm size/MIME sau khi object đã vào S3. File quá lớn dù bị ứng dụng từ chối vẫn có thể chiếm storage và phát sinh phí. Kiểm size ở browser không ngăn client bị sửa.

Session mới dùng `generate_presigned_post`: bucket/key do server chọn, `Content-Type: application/pdf` chính xác, success status 201, hạn 300 giây, `content-length-range` từ 5 byte đến 10 MiB. Frontend thêm các field trả về vào FormData, file ở cuối, để browser tự tạo multipart boundary. Không chuyển Cognito Authorization sang S3. Chữ ký/field là thông tin truy cập tạm thời, không đưa vào minh chứng báo cáo.

Submit vẫn kiểm object đã lưu. Nhãn MIME ký đúng chưa chứng minh byte PDF; worker kiểm header/size. Phạm vi này không phải parser đầy đủ hoặc quét mã độc. Submit chốt S3 version ID và attempt ID. Dùng lại capability còn hạn có thể tạo current version mới, nhưng không thay được version đã nộp. Download cũng ký đúng version, để browser so SHA-256 gốc, worker và byte tải về.

Test giải mã POST policy để kiểm key/MIME và điều kiện byte range. Regression frontend kiểm field trước file, không gửi credentials hoặc tự đặt multipart header. Browser local chạy FormData qua handler backend thật và adapter storage giả lập, rồi so byte tải. Moto/local kiểm contract và hành vi ứng dụng; chỉ thử S3 thật mới chứng minh AWS từ chối file quá lớn, hết hạn hoặc policy bị sửa. Cần payload tổng hợp có giới hạn và chủ tài khoản cho phép trước thử thật.

Hệ quả vận hành: phải phối hợp protocol backend, CORS POST S3 và frontend cache. Client PUT cũ cần reload hoặc rollback đồng bộ. Retention version ảnh hưởng COST: expiry current tạo delete marker, bản cũ có thể vẫn lưu tiếp. Xem [điều kiện POST policy S3](https://docs.aws.amazon.com/AmazonS3/latest/API/sigv4-HTTPPOSTConstructPolicy.html), giả định COST của Workshop. Khi bảo vệ cần giải thích validation ở đâu, overwrite nghĩa gì khi versioning và `processed` không bảo đảm điều gì.
