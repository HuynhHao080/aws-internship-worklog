---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## Workshop cá nhân: AWS Policy Assistant

[Đọc hướng dẫn Policy Assistant](5.7-policyassistant/): kiến trúc, triển khai, kiểm thử, monitoring, COST và cleanup có kiểm soát. Evidence đã lưu: **B4 19 PASS; B5 FAIL lịch sử; B5-R7 browser 10 PASS**. D3-A đã containment legacy identity; D11 publish lịch sử sanitized với CI đầu FAIL; D15 CI 19/19 steps PASS. Không tuyên bố production-ready.

Môi trường AWS test vẫn duy trì, chưa cleanup và chưa chốt ngày kết thúc demo. Kiến trúc tự vẽ, chuyên cần và minh chứng cá nhân chưa đủ vẫn PENDING; không dùng template để thay thế.

## Tài liệu tham khảo FCAJ: S3/VPC Endpoint

Các mục 5.1–5.6 bên dưới chỉ giữ ghi chú khái niệm và liên kết tới [template FCAJ](https://workshop-sample.awsfcaj.com/5-workshop/), không sao chép nguyên walkthrough hoặc tái bản 80 ảnh mẫu. Chúng không phải kiến trúc Policy Assistant và không chứng minh sinh viên đã chạy lab. Ảnh gốc được bảo toàn local; quyền đăng lại chưa xác minh nên bị loại khỏi output website.

- [Tham khảo: Khái niệm VPC endpoint](5.1-workshop-overview/)
- [Tham khảo: Điều kiện chuẩn bị lab](5.2-prerequiste/)
- [Tham khảo: Truy cập S3 từ VPC](5.3-s3-vpc/)
- [Tham khảo: Truy cập S3 từ môi trường hybrid](5.4-s3-onprem/)
- [Tham khảo: Ranh giới endpoint policy](5.5-policy/)
- [Tham khảo: Lập kế hoạch dọn dẹp lab tham khảo](5.6-cleanup/)
