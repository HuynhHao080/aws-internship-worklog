---
title: "COST — giá AWS chính thức"
date: 2026-10-21
weight: 1
---

<!-- policy-assistant-managed:begin -->
## Bản chính — cập nhật tài liệu 06/11/2026

**Model của bản chính:** Global Claude Haiku 5.5. Ứng dụng xếp hạng từ khóa trên văn bản PDF và chính sách. Các báo cáo kiểm thử có ngày bên dưới ghi lại từng bản đã nghiệm thu. [Bằng chứng chuyển model](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/haiku55-migration.md).

**[Bộ hồ sơ nộp]({{< relref "/5-Workshop/5.7-PolicyAssistant/Submission" >}})**

**Bản chính là [Policy Assistant](https://djreo5eptvwk5.cloudfront.net/). Các phần tháng 10 bên dưới là lịch sử của môi trường test khác.**

- Đọc [đề xuất hiện tại]({{< relref "/2-Proposal/CurrentSolution" >}}) để xem bài toán, phạm vi, kiến trúc cần tự vẽ, kế hoạch chốt, đầu vào COST và rủi ro. [Chương bản chính]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}) giữ danh mục và bằng chứng qua từng release.
- Đã có đăng ký công khai, PDF riêng/text/OCR, hỏi đáp có nguồn, lịch sử, draft/publish/hiệu lực chính sách, thông tin shop, đánh giá tổng hợp và hỗ trợ có consent. [Phối hợp hỗ trợ]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}) bổ sung nhận việc/bàn giao/ghi chú nội bộ; không tự tạo quyền nhân viên hạn chế hoặc quyền đọc PDF khách.
- Snapshot nghiệm thu hỗ trợ ngày 06/11: **322 Python đạt, 11 live bỏ qua; 67 frontend đạt; 20 HTTP checks đạt**, ba tài khoản QA và yêu cầu thử đã dọn. 40 tệp frontend và 6 Lambda khớp artifact. Timestamp UTC 05/11 tương ứng 06/11 Việt Nam; không phải lần chạy mới khi mở trang. Không gọi model/PDF/OCR/email trong đợt HTTP này. [Báo cáo](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/support-team-verification.md), [CI ứng dụng](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/42072920395).
- Nội dung shop vẫn **DEMO**; còn chờ chính sách thật được phê duyệt, browser/accuracy/tải/restore/email theo phạm vi. Budget 50 USD chỉ cảnh báo; dự toán 6,63 USD/budget 20 USD bên dưới thuộc môi trường cũ, không dùng làm giá bản hiện tại.
- Ba blog đã đăng, minh chứng office/sự kiện, sơ đồ tự vẽ, Pricing Calculator và video còn chờ evidence cá nhân. Bản thảo kỹ thuật không phải bài đã đăng. Không sửa ngày thực tập, daily worklog hoặc tạo ảnh/điểm thay sinh viên.

Nội dung có AI hỗ trợ; sinh viên phải tự rà soát, thực hiện và giải thích. Kết quả DOM không phải visual browser; artifact/CI không phải accuracy hoặc SLA. Giữ nguyên mọi FAIL/PASS và ảnh lịch sử đúng môi trường.

### Tài liệu lịch sử bên dưới
<!-- policy-assistant-managed:end -->

## Dự toán bản chính · 12/11/2026

**Kịch bản cơ sở 7.10 USD/tháng, kịch bản tăng tải 11.97 USD/tháng.** Đây là mô hình giả định 30 ngày dùng đơn giá công khai trước miễn phí, credits và thuế. Các số dưới đây dùng để nhập AWS Pricing Calculator và đối chiếu với CloudWatch khi có tải thật. Tài nguyên vùng tính theo Singapore, CloudFront dùng một kịch bản edge châu Âu cho PriceClass_100. [AWS Price List API](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_pricing_GetProducts.html).

| Hạng mục | Đơn vị | Cơ sở | Tăng tải | USD/đơn vị | SKU hoặc nguồn |
|---|---|---:|---:|---:|---|
| API Gateway HTTP API | request | 20000 | 40000 | .00000125 | HTX6VZU3YCMMNRM5 |
| Lambda | request | 64700 | 87700 | .0000002 | 36D3E7396ZKUSWWW |
| Lambda x86 | GB-s | 5945 | 10720 | .0000166667 | 67ZV6RMYY72YSRDC |
| DynamoDB on-demand | RRU | 250000 | 750000 | .0000001425 | XF4WN9CQUFPSY9SQ |
| DynamoDB on-demand | WRU | 50000 | 150000 | .00000071 | PZTVP859DVDCGT8X |
| DynamoDB Standard | GB-month | .10 | .30 | .285 | YCMS8QPJ7DRVHAXD |
| DynamoDB PITR | GB-month | .10 | .30 | .228 | GSSDTS2TPQ4RHW4R |
| S3 Standard | GB-month | 2 | 6 | .025 | M6MTARCQFUQBQ2V3 |
| S3 PUT / COPY / POST / LIST | request | 2000 | 6000 | .000005 | VHRJWKRHEA6VHEFY |
| S3 GET and other | request | 5000 | 15000 | .0000004 | T469UNPYX78434EG |
| API / S3 → Internet | GB | 1 | 3 | .12 | SDHP4R7WGBVJPQPY |
| CloudFront HTTPS (Europe) | request | 10000 | 30000 | .0000012 | YA5QZD7XG45UGHCH |
| CloudFront egress (Europe) | GB | 1 | 3 | .085 | CloudFront |
| SQS Standard | request | 300000 | 600000 | .0000004 | PVF7MDBNRK7V9S5H |
| Cognito Lite | MAU | 50 | 150 | .0055 | CJUNATY8K3J8WJ5U |
| CloudWatch Logs ingest | GB | 1 | 3 | .70 | ZEKCE7VGMSM5XZPW |
| CloudWatch Logs retained | GB-month | .5 | 1.5 | .03 | RKCBFFXUWXG8KUQY |
| CloudWatch standard alarm | alarm-month | 9 | 9 | .10 | ERS2PGV5BX4XH4WQ |
| CloudWatch dashboard | dashboard-month | 1 | 1 | 3 | CloudWatch |
| SNS API | request | 100 | 300 | .0000005 | UNWF5P8PEVEH2AZ2 |
| SNS email | delivery | 100 | 300 | .00002 | SNS |
| Claude Haiku 5.5 Global input | MTok | 8 | 16 | .10 | 82DP2ZQEH38W5NDX |
| Claude Haiku 5.5 Global output | MTok | .9 | 1.8 | .50 | 58YZ69WTV49A9DVA |
| Textract AsyncTextPagesProcessed | page | 200 | 600 | .0015 | VDM7FRY6BHABK2PJ |

- Lambda gồm 20.000/40.000 lượt API ở 512 MiB, với 1.000/2.000 lượt AI giả định 3 giây và các lượt còn lại 0,25 giây. Cộng 43.200 lượt recovery ở 256 MiB × 0,15 giây và 1.500/4.500 lượt nền ở 512 MiB × 0,6 giây. Lịch recovery giữ nguyên trong hai kịch bản. Các thời lượng là giả định.
- DynamoDB tính RRU/WRU đã quy đổi cho Scan, giao dịch, index và retry. Dung lượng S3 bao gồm PDF, phiên bản, vùng chờ, văn bản trích xuất và tệp web. Egress trực tiếp của API/PDF tách khỏi lưu lượng CloudFront. SQS gồm polling và retry.
- Bản AWS có chín alarm. Dự toán tính một dashboard, log theo GB và SNS thông báo email. Chi phí Lambda của quy tắc recovery đã tính ở dòng Lambda. Budget 50 USD là ngưỡng cảnh báo. Thuế, hỗ trợ trả phí và tải phát sinh ngoài kịch bản chưa được tính.
- Khi tạo estimate trong [AWS Pricing Calculator](https://calculator.aws/), dùng đúng đơn vị ở bảng. Nếu không chọn được model/đơn vị OCR, cộng hai dòng token và dòng OCR bên ngoài, kiểm tra tránh tính trùng. [Bản đối chiếu AWS Pricing Calculator](https://calculator.aws/#/estimate?id=edb8950a7263f4b391e23fb3565f807c70be5781) hiển thị **2,55 USD/tháng** cho hai hạng mục: 2,25 USD cho 1.800 lượt Claude Haiku 5.5 Global và 0,30 USD cho 200 trang Textract. Calculator yêu cầu số nguyên ở hai ô lượt/phút và giờ/ngày. Bản đối chiếu dùng 1 lượt/phút trong 1 giờ/ngày, 8.000 token vào và 900 token ra mỗi lượt. Bảng 24 hạng mục phía trên dùng giả định tải riêng và vẫn chờ bản xuất Calculator đầy đủ.

Nguồn công khai cho các dòng không dùng SKU gồm [CloudFront](https://aws.amazon.com/cloudfront/pricing/pay-as-you-go/), [CloudWatch](https://aws.amazon.com/cloudwatch/pricing/), [SNS](https://aws.amazon.com/sns/faqs/) và [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets/pricing/).

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
