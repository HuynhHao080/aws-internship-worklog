---
title: "Tham khảo: Khái niệm VPC endpoint"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

> **Tài liệu tham khảo FCAJ — không phải Workshop cá nhân hoặc minh chứng đã thực hành.** Trang này là ghi chú khái niệm được biên soạn lại để chỉ dẫn tới nguồn, không tái bản hướng dẫn và ảnh mẫu.

Gateway endpoint kết nối route table trong VPC tới S3; interface endpoint sử dụng network interface của PrivateLink. Lab tham khảo minh họa mạng hybrid, còn Policy Assistant dùng kiến trúc serverless riêng không có VPC/NAT.

## Nguồn và phạm vi

- [Trang tham khảo trong template FCAJ](https://workshop-sample.awsfcaj.com/5-workshop/5.1-workshop-overview/) (nguồn tiếng Anh).
- [Workshop S3/VPC liên quan của AWS Study Group](https://000111.awsstudygroup.com/).
- Ảnh template được giữ trong bản gốc local nhưng không đưa vào output website thử nghiệm; quyền đăng lại chưa được xác minh. Dẫn nguồn không thay thế quyền sử dụng.
- Ngày trong front matter là dữ liệu lịch sử của template, không phải ngày sinh viên thực hành. Không có kết quả AWS mới được tạo hoặc suy diễn tại đây.

[Đọc Workshop cá nhân AWS Policy Assistant](../5.7-policyassistant/). Kiến trúc tự vẽ và minh chứng sinh viên còn thiếu tiếp tục ở trạng thái PENDING.
