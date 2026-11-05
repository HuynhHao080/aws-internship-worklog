---
title: "Bản thảo 1 — ownership ngoài JWT"
date: 2026-10-21
weight: 4
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

# Đăng nhập đúng chưa đủ để được đọc tài liệu

**Bản thảo kỹ thuật dựa trên mã và kiểm thử local Policy Assistant hiện tại. Chưa đăng; cần sinh viên tự rà soát, đăng và bổ sung URL/ngày thật.** Không ghi thành công việc quá khứ hoặc kết quả AWS thật.

HTTP API có thể chặn JWT Cognito sai nhưng ứng dụng vẫn lộ tài liệu nếu chỉ nhận doc ID rồi trả dữ liệu. ID là định danh, không phải quyền. Policy Assistant tách hai mô hình: danh mục chính sách dùng chung cho người đã đăng nhập, còn metadata/file tài liệu thuộc từng chủ sở hữu.

Identity tin cậy lấy từ `sub` trong authorizer claims của API Gateway. API không lấy owner do client tự khai và tự chọn key lưu trữ. `_owned_document` so owner đã lưu với subject hiện tại. Tài liệu không tồn tại và tài liệu người khác đều trả 404 để không tiết lộ sự tồn tại. Record cũ thiếu owner bị từ chối, không tự gán. Query library ByOwner và cursor phân trang cũng gắn với subject; chỉ bảo vệ detail route sẽ chưa chặn dò danh sách.

Frontend xóa state riêng khi logout/401 để Bob dùng cùng browser không nhìn state Alice. Tuy vậy, ẩn UI chưa đủ: API phải chặn khi Bob nhập thủ công ID đã biết của Alice. URL storage ký sẵn là bearer capability ngắn hạn, ai giữ cũng có thể dùng đến khi hết hạn. Không lưu trong log/ảnh. S3 không tự kiểm JWT Cognito cho request ký sẵn này.

Bằng chứng có ý nghĩa là test phủ định ownership trong `tests/unit/test_documents.py` và `tests/frontend/local.e2e.cjs`: Alice upload/download; Bob có library rỗng, nhận 404 cho ID Alice. Test dùng handler thật với moto/identity giả local, **không** kiểm chữ ký Cognito hoặc IAM/CORS đang deploy. Sau khi được phép triển khai, sinh viên cần lặp lại tất cả document endpoint bằng hai tài khoản Cognito thật, JWT thiếu/sai/hết hạn và cursor bị sửa.

Thiết kế không cấp credentials S3 tổng quát cho browser, phân biệt service IAM role với quyền user. Giới hạn còn có capability chia sẻ được và onboarding do quản trị cấp. Tham khảo [JWT authorizer HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html), ma trận nghiệm thu ở Workshop. Câu hỏi bảo vệ: identity tin cậy từ đâu, vì sao trả 404, vì sao CORS không bảo vệ ownership?
