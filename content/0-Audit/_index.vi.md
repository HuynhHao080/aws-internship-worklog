---
title: "Đối soát và minh chứng còn thiếu"
date: 2026-10-21
weight: 0
---

<!-- policy-assistant-managed:begin -->
## Bản chính — cập nhật tài liệu 06/11/2026

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

# Trạng thái hồ sơ FCAJ

Đọc các trang con **Đối soát Daily Worklog tuần 1–12** và **Minh chứng sinh viên cần bổ sung**. Dòng lịch sử, ảnh nguồn hoặc nội dung mẫu chưa được xem là hoạt động đã xác minh. Ba blog kỹ thuật mới là bản thảo chưa đăng. Cần tự vẽ và giải thích kiến trúc, bổ sung chuyên cần thật và nghiệm thu demo AWS sau khi duyệt triển khai.

Sản phẩm và Workshop chính là **Policy Assistant**. Giữ bài lab S3/VPC và nhật ký IoT cũ thành nội dung học tập độc lập. Dự toán COST có nguồn/ngày/giả định cụ thể trong Workshop 5.7; không có Calculator share link đã tạo.
