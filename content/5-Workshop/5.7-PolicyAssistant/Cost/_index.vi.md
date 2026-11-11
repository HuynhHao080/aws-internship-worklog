---
title: "COST — giá AWS chính thức"
date: 2026-10-21
weight: 1
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

# Dự toán và dọn dẹp

**Singapore ap-southeast-1, USD, 30 ngày. Kiểm giá AWS ngày 21/10/2026. Khoảng 6,6306 USD/tháng trước Free Tier, credits và thuế.** Đây là mô hình giả định, không phải hóa đơn hoặc benchmark. Kịch bản tăng tải 10 lần khoảng **34,7081 USD/tháng**, giữ lịch recovery và baseline alarm/dashboard/SNS. Chưa tạo AWS Pricing Calculator estimate hoặc link chia sẻ; không khẳng định đã có. Nhập thông số bên dưới tại [AWS Pricing Calculator](https://calculator.aws/), lưu estimate thật rồi bổ sung URL/export.

## Giả định và thông số Calculator

100 MAU Cognito Lite; 100.000 HTTP API request (20.000 keyword Scan, 77.000 lookup, 3.000 document/library/status). 500 upload PDF 2 MiB; 1.000 download 2 MiB, 2,1 GB egress Internet trực tiếp gồm phản hồi API. CloudFront 20.000 HTTPS request và 5 GB outbound, tính theo edge châu Âu do PriceClass_100; edge thực tế làm thay đổi giá. Không tính trùng S3 → CloudFront thành egress trực tiếp. Upload ingress không mất phí.

Lambda x86: API 100.000 × 0,2 s × 0,125 GB; worker 500 × 1 s × 0,25 GB; recovery 21.600 × 0,1 s × 0,125 GB. Tổng 122.100 invocation và 2.895 GB-second. Duration là giả định, phải thay bằng CloudWatch đo thật. EventBridge scheduled rule cũ chạy mỗi 2 phút, không phải Scheduler; đã tính Lambda đích, không mua workload custom event/Scheduler.

DynamoDB on-demand: 1.050.000 RRU, 10.000 WRU gồm ghi GSI; 0,1 GB bảng + index, 0,05 GB base bật PITR. Scan đọc trước khi lọc, nên tập chính sách lớn/polling có thể tăng phí. S3 Standard: 25 GB mọi version document và 0,05 GB frontend; 1.000 POST/PUT/LIST, 5.000 GET/HEAD. SQS Standard: 300.000 request gồm idle polling/retry, payload dưới 64 KiB. SNS dự phòng 100 publish/100 email, chỉ khi có người nhận xác nhận. CloudWatch 1 GB ingest, 1 GB retained log, 5 alarm và 1 dashboard. Không SMS/advanced security/M2M.

AI/Bedrock/OCR/OpenSearch: **0 vì chưa triển khai**. VPC/NAT/EC2/RDS: **0 vì template không có**. Không thêm dịch vụ để tăng số lượng.

## Chi phí theo dịch vụ

Giá trong bảng là USD cho mỗi đơn vị; số thập phân dùng dấu chấm để nhập Calculator chính xác. Snapshot `docs/cost/aws-rates-2026-10-21.json` trong repo ứng dụng lưu SKU, unit/tier, effective/publication date, URL và SHA-256 danh mục. `python -m scripts.estimate_cost` tái tạo bảng và tổng.

| Component | Monthly quantity | USD/unit | USD/month |
|---|---:|---:|---:|
| Lambda requests | 122100 Request | 0.0000002000 | 0.024420 |
| Lambda x86 GB-seconds | 2895 Lambda-GB-Second | 0.0000166667 | 0.048250 |
| HTTP API requests | 100000 Requests | 0.0000012500 | 0.125000 |
| DynamoDB read request units | 1050000 ReadRequestUnits | 0.0000001425 | 0.149625 |
| DynamoDB write request units | 10000 WriteRequestUnits | 0.0000007100 | 0.007100 |
| DynamoDB storage | 0.1 GB-Mo | 0.2850000000 | 0.028500 |
| DynamoDB PITR | 0.05 GB-Mo | 0.2280000000 | 0.011400 |
| S3 Standard storage including versions | 25.05 GB-Mo | 0.0250000000 | 0.626250 |
| S3 POST PUT LIST requests | 1000 Requests | 0.0000050000 | 0.005000 |
| S3 GET HEAD requests | 5000 Requests | 0.0000004000 | 0.002000 |
| SQS Standard requests including idle polls | 300000 Requests | 0.0000004000 | 0.120000 |
| SNS publish | 100 Requests | 0.0000005000 | 0.000050 |
| SNS email notifications | 100 Notifications | 0.0000200000 | 0.002000 |
| CloudWatch log ingestion | 1 GB | 0.7000000000 | 0.700000 |
| CloudWatch log storage | 1 GB-Mo | 0.0300000000 | 0.030000 |
| CloudWatch standard alarms | 5 Alarms | 0.1000000000 | 0.500000 |
| CloudWatch dashboard | 1 Dashboards | 3.0000000000 | 3.000000 |
| Cognito Lite monthly active users | 100 CognitoUserPoolsMAU | 0.0055000000 | 0.550000 |
| CloudFront HTTPS Europe | 20000 Requests | 0.0000012000 | 0.024000 |
| CloudFront egress Europe GB | 5 GB | 0.0850000000 | 0.425000 |
| Singapore direct internet egress GB | 2.1 GB | 0.1200000000 | 0.252000 |
| **Total before allowances/credits/tax** | | | **6.6306** |


## Trước và sau ưu đãi

Mô hình áp giá biên có phí cho toàn bộ quantity để thể hiện ngân sách trước ưu đãi. DynamoDB storage/SNS có bậc đầu giá 0 trong catalogue; bảng cố ý dùng giá biên có phí cho toàn lượng. Lambda/Cognito/CloudFront/SQS/CloudWatch cũng có thể có allowance. Kiểm đúng tài khoản, eligibility, mức dùng chung và credit còn lại trong Billing; không mặc định hóa đơn 0. Chưa tính VAT, tỷ giá, support, tên miền, SMS tùy chọn hoặc truy vấn metric ngoài quy mô console demo. Calculator có thể tự áp allowance, cần giữ cả gross/net.

## Tối ưu, cảnh báo và retention

Chi phí tăng do version S3 tồn trữ, Scan, download, polling/retry và log thừa. Budget thực tế `policy-assistant-account-monthly` là **20 USD/tháng toàn account**, không lọc region/service/tag, dùng UnblendedCost và loại Credit/Refund. Ba cảnh báo: Actual 80% (16 USD), Actual 100% (20 USD), Forecasted 100% (20 USD). Không Budget Actions; giữ nguyên Budget 5 USD hiện có.

Budget test **10 USD/tháng theo tag là PENDING**, chưa được coi là hoạt động vì cost-allocation tags chưa xác minh đủ điều kiện. Budget 20 USD không phải hard spending cap, không tự dừng dịch vụ, không phải credits được cấp thêm; Billing/forecast có độ trễ và email delivery chưa được chứng minh. Không ghi email recipient lên website. D17 chỉ cập nhật tài liệu, không tạo/sửa Budget hoặc đọc Billing mới.

Môi trường AWS test vẫn duy trì; **chưa cleanup, chưa có ngày kết thúc được chủ dự án phê duyệt**. Snapshot C0/chi phí lịch sử không phải hóa đơn hiện tại. [Giá Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/pricing/) là nguồn tham khảo khi duyệt thay đổi sau này; không tự bật actions/reports.

IaC giữ log 30 ngày, DLQ 14 ngày; S3 current expiry 365 ngày và noncurrent expiry thêm 365 ngày từ lúc thành bản cũ. Expiry current trong versioning tạo delete marker, byte có thể còn gần thêm một năm. Không nói dữ liệu xóa hết sau 365 ngày. Không tự rút retention dữ liệu đang dùng.

Sau khi chủ sở hữu cho phép, export bằng chứng cần giữ, kiểm CloudFormation Retain, S3 version/delete marker, DynamoDB backup/PITR, log group, CloudFront và SNS. `sam delete` không bảo đảm dọn bucket còn dữ liệu/resource Retain. Không recursive delete tùy tiện. Ghi inventory còn lại và kiểm Billing sau cleanup. Quy trình duyệt/rollback ở `docs/deployment-review.md`; các bước này chưa thực hiện.

## Nguồn chính thức

Giá lấy từ [AWS Price List bulk API](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/price-changes.html), ngày kiểm 21/10/2026; snapshot ghi đầy đủ URL/tier/ngày công bố mỗi dòng. Các trang đối chiếu: [Lambda](https://aws.amazon.com/lambda/pricing/), [API Gateway](https://aws.amazon.com/api-gateway/pricing/), [DynamoDB](https://aws.amazon.com/dynamodb/pricing/on-demand/), [S3](https://aws.amazon.com/s3/pricing/), [SQS](https://aws.amazon.com/sqs/pricing/), [SNS](https://aws.amazon.com/sns/pricing/), [CloudWatch](https://aws.amazon.com/cloudwatch/pricing/), [Cognito](https://aws.amazon.com/cognito/pricing/), [CloudFront](https://aws.amazon.com/cloudfront/pricing/), [EventBridge](https://aws.amazon.com/eventbridge/pricing/). Cần cập nhật giá trước lần triển khai muộn hơn.
