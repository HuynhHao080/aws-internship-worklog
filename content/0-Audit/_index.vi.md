---
title: "Đối soát và minh chứng còn thiếu"
date: 2026-10-21
weight: 0
---

<!-- policy-assistant-managed:begin -->
## Bản chính — cập nhật kỹ thuật 04/11/2026

**Phần cập nhật này mô tả bản chính [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). Các phần 21–23/10 phía dưới là lịch sử của môi trường test khác.** Không lấy kết quả cũ làm nghiệm thu bản chính.

- Hiện có đăng ký công khai, thư viện PDF riêng, trích xuất/OCR, hỏi đáp có nguồn, lịch sử, quản trị chính sách, yêu cầu hỗ trợ, cấu hình shop và đánh giá câu trả lời. Nội dung shop vẫn là **DEMO**, chưa có đơn vị vận hành phê duyệt chính sách thật.
- Báo cáo release ngày 04/11: **251 Python PASS, 11 live SKIP; 54 frontend PASS; 15 kiểm tra API/metadata/dọn dữ liệu PASS**. Frontend dùng DOM giả lập; kiểm tra API chất lượng dùng dữ liệu tổng hợp và hai tài khoản tạm, không gọi model. Không phải bằng chứng kiểm thử hình ảnh trình duyệt, độ chính xác AI hoặc CI mới.
- [Chương bản chính, hướng dẫn và giới hạn]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) có bảng chức năng, kịch bản demo, đầu vào chi phí và bằng chứng đã lọc. Bản cập nhật worklog này được chuẩn bị local; chưa khẳng định đã xuất bản lên GitHub Pages.
- Ngày 04/11, chủ dự án cho biết chưa thực hiện các mục được hỏi: ba blog đã đăng, minh chứng office/sự kiện, sơ đồ tự vẽ và Pricing Calculator. Giữ trạng thái **CHỜ**; không suy diễn điều đó thành chưa làm toàn bộ nhật ký cũ. Ba bài kỹ thuật 3.4–3.6 vẫn là bản thảo cần tự rà soát và đăng.

Nội dung mới có AI hỗ trợ; sinh viên cần đọc, tự thực hiện và giải thích trước khi nộp. Giữ nguyên các lần FAIL, PASS, CI và ảnh lịch sử theo đúng ngày/môi trường. Dự toán 6,63 USD và budget 20 USD trong phần cũ không mô tả đầy đủ bản AI/OCR hiện tại.

### Tài liệu lịch sử bên dưới
<!-- policy-assistant-managed:end -->

# Trạng thái hồ sơ FCAJ

Đọc các trang con **Đối soát Daily Worklog tuần 1–12** và **Minh chứng sinh viên cần bổ sung**. Dòng lịch sử, ảnh nguồn hoặc nội dung mẫu chưa được xem là hoạt động đã xác minh. Ba blog kỹ thuật mới là bản thảo chưa đăng. Cần tự vẽ và giải thích kiến trúc, bổ sung chuyên cần thật và nghiệm thu demo AWS sau khi duyệt triển khai.

Sản phẩm và Workshop chính là **Policy Assistant**. Giữ bài lab S3/VPC và nhật ký IoT cũ thành nội dung học tập độc lập. Dự toán COST có nguồn/ngày/giả định cụ thể trong Workshop 5.7; không có Calculator share link đã tạo.
