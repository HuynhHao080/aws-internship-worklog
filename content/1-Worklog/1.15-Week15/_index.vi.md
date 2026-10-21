---
title: "Worklog Tuần 15"
date: 2026-10-11
weight: 15
chapter: false
pre: " <b> 1.15. </b> "
---

### Mục tiêu tuần 15:

* Triển khai VPC và nền tảng networking cho IoT Platform.
* Tạo CloudFormation stack cho VPC, subnet, security group, VPC endpoint.
* Thiết lập cross-account role cho CI/CD pipeline.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo VPC CloudFormation stack: VPC, 2 public subnet, 2 private subnet trên 2 AZ <br> - Cấu hình route table, Internet Gateway, NAT Gateway | 05/10/2026 | 05/10/2026 | <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html> |
| 3 | - Thêm VPC Endpoint (Gateway) cho S3 và DynamoDB <br> - Cấu hình Security Group cho Lambda, IoT Core, ALB | 06/10/2026 | 06/10/2026 | <https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html> |
| 4 | - Tạo IAM role cho Lambda execution, IoT Core rule action, Kinesis access <br> - Áp dụng policy least-privilege với condition key | 07/10/2026 | 07/10/2026 | <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html> |
| 5 | - Thiết lập CloudFormation nested stack cho infrastructure modular <br> - Thêm parameter cho environment (dev/staging/prod) | 08/10/2026 | 08/10/2026 | <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-nested-stacks.html> |
| 6 | - Deploy infrastructure lên dev account qua CloudFormation <br> - Kiểm tra kết nối: Lambda → DynamoDB, Lambda → S3 qua VPC endpoint <br> - Vẽ sơ đồ kiến trúc mạng | 09/10/2026 | 09/10/2026 | |

### Kết quả đạt được trong tuần 15:

* **PLANNED**: Deploy VPC với public/private subnet trên 2 AZ.
* **PLANNED**: Cấu hình VPC Gateway Endpoint cho S3 và DynamoDB (không đi qua NAT).
* **PLANNED**: Tạo IAM role least-privilege cho tất cả thành phần dịch vụ.
* **PLANNED**: Triển khai nested CloudFormation stack với parameter environment.
* **TODO**: Chạy test network reachability và ghi chép vấn đề (nếu có).