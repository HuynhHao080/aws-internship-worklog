---
title: "Bộ hồ sơ nộp Policy Assistant"
weight: 0
---


**Model của bản chính:** Global Claude Haiku 5.5. Ứng dụng xếp hạng từ khóa trên văn bản PDF và chính sách. Các báo cáo kiểm thử có ngày bên dưới ghi lại từng bản đã nghiệm thu. [Bằng chứng chuyển model](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/haiku55-migration.md).
## Bốn đường dẫn cần mở khi trình bày

1. [Ứng dụng chạy trên AWS](https://djreo5eptvwk5.cloudfront.net/) dùng bộ chính sách DEMO cho shop phụ kiện.
2. [Proposal Việt/Anh]({{< relref "/2-Proposal/CurrentSolution" >}}) mô tả bài toán, phạm vi, kiến trúc, rủi ro và đầu vào dự toán.
3. [Mã nguồn và README](https://github.com/HuynhHao080/aws-policy-assistant) có template AWS SAM, kiểm thử và hướng dẫn vận hành.
4. [Báo cáo nghiệm thu triển khai](https://github.com/HuynhHao080/aws-policy-assistant/blob/6225d5c66ed5f114be1e3cf004df937ab0e58f67/docs/HUONG-DAN-PHOI-HOP-HO-TRO.md) hướng dẫn thao tác. [Bằng chứng API và artifact]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}).

## Bản mã nguồn và bằng chứng dùng khi nộp

- Hồ sơ dùng commit ứng dụng **c25e278**. CloudFormation báo `UPDATE_COMPLETE`, cấu hình model `global.anthropic.claude-haiku-5-5`. Bản giao diện cùng commit đã phát hành qua CloudFront. [Mã nguồn tại commit](https://github.com/HuynhHao080/aws-policy-assistant/tree/c25e27829b15251206b54982bfcd7b7274a1a88e).
- Bản này đạt **93 kiểm thử giao diện**, gồm quay lại đúng mục và phục hồi khi phiên đăng nhập hết hạn. Ba trang Cognito đăng nhập, đăng ký và quên mật khẩu đã tải nhận diện mới. Kiểm tra HTTP chỉ xác nhận hiển thị form. Giữ riêng kết quả mô phỏng, API live đã ghi ngày và demo do sinh viên thực hiện. [CI ứng dụng](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/43180702320) · [Bản bàn giao]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}).

## Bài thực hành trên máy cá nhân

Clone repo ứng dụng, cài Python 3.12 và Node.js 22. Mở PowerShell tại thư mục repo mới.

```powershell
git switch --detach c25e27829b15251206b54982bfcd7b7274a1a88e
py -3.12 -m venv .venv
& ./.venv/Scripts/python.exe -m pip install -r policy_assistant/requirements.txt -r tests/requirements.txt
Remove-Item Env:AWS_SAM_STACK_NAME, Env:POLICY_ASSISTANT_ID_TOKEN -ErrorAction SilentlyContinue
& ./.venv/Scripts/python.exe -m pytest tests -q
npm ci --prefix frontend --ignore-scripts
npm run build --prefix frontend
npm test --prefix frontend
```

Kiểm mã thoát sau mỗi lệnh. Các bài Python dùng dịch vụ giả lập và bài frontend dùng DOM giả lập. Có thể mở bản giao diện dựng cục bộ để xem bố cục. Cấu hình mẫu để trống kết nối Cognito. [Hướng dẫn tái lập](https://github.com/HuynhHao080/aws-policy-assistant/tree/main/docs/reproducibility).

## Kịch bản demo khoảng 4 phút

| Thời gian | Thao tác và điểm cần giải thích |
|---|---|
| 0:00–0:40 | Mở app, nêu bài toán và phạm vi shop DEMO. Giải thích sơ đồ tự vẽ từ trình duyệt qua CloudFront, Cognito và API. |
| 0:40–1:40 | Mở chính sách bảo hành và chọn **Hỏi từ chính sách này**. Hỏi thời hạn bảo hành củ sạc và ai chịu phí gửi. Đối chiếu từng ý với đoạn trích và ghi lại cách agent xử lý dữ kiện còn thiếu. |
| 1:40–2:50 | Gửi yêu cầu hỗ trợ với dữ liệu giả. Quản trị nhận việc và thêm ghi chú. Quay về tài khoản khách để đối chiếu phần hội thoại thấy được. |
| 2:50–4:00 | Nêu cách kiểm quyền sở hữu PDF, revision khi hai người xử lý, hàng đợi và dọn dữ liệu. Mở báo cáo, chỉ rõ số liệu nào do bạn chạy. |

Khi quay, che token, URL tải lên có chữ ký và thông tin tài khoản. Nếu không chạy được một bước, ghi nguyên nhân và trình bày bằng chứng đã có với đúng ngày và phạm vi. Sau demo, dọn dữ liệu thử qua giao diện tài khoản hoặc chức năng xóa tương ứng.

## Những mục cần chốt trước khi nộp

Ưu tiên sơ đồ kiến trúc tự vẽ và phần trình bày workshop. Theo barem, cả hai là điều kiện để được tính điểm workshop. Dùng biểu tượng AWS chính thức và tập giải thích ranh giới quyền, luồng dữ liệu và lý do chọn dịch vụ. [Tiêu chí chấm FCAJ](https://hcm-rules.awsfcaj.com/5-scoring/).

| Mục | Việc của người nộp |
|---|---|
| Kiến trúc | Tự vẽ luồng trên sơ đồ AWS và tập giải thích ranh giới quyền, Singapore và định tuyến Global, lỗi và retry. [Phiếu rà soát](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/fcaj/ARCHITECTURE-REVIEW-WORKSHEET.md). |
| COST | Nhập giả định của Proposal vào Calculator, lưu tổng tiền, ngày lập và link hoặc bản export. Ngân sách mục tiêu 30–50 USD/tháng. [Đầu vào dự toán]({{< relref "/2-Proposal/CurrentSolution" >}}). |
| Demo | Tự thực hiện, lưu video hoặc ảnh minh chứng và thời điểm kiểm tra. |
| Worklog và minh chứng | Kiểm lại nhật ký, các bài blog đã đăng và minh chứng sự kiện, điền link thực tế. [Trang rà soát]({{< relref "/0-Audit/student-evidence" >}}). |

Tài liệu kỹ thuật này có AI hỗ trợ. Khi nộp, rà soát nội dung, ghi rõ phần bạn thực hiện và dùng [quy định workshop FCAJ](https://hcm-rules.awsfcaj.com/3-project/) làm tài liệu đối chiếu.
