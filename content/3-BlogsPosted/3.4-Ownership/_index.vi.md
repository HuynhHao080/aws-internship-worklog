---
title: "Bản thảo 1 — ownership ngoài JWT"
date: 2026-10-21
weight: 4
---

# Đăng nhập đúng chưa đủ để được đọc tài liệu

**Bản thảo kỹ thuật dựa trên mã và kiểm thử local Policy Assistant hiện tại. Chưa đăng; cần sinh viên tự rà soát, đăng và bổ sung URL/ngày thật.** Không ghi thành công việc quá khứ hoặc kết quả AWS thật.

HTTP API có thể chặn JWT Cognito sai nhưng ứng dụng vẫn lộ tài liệu nếu chỉ nhận doc ID rồi trả dữ liệu. ID là định danh, không phải quyền. Policy Assistant tách hai mô hình: danh mục chính sách dùng chung cho người đã đăng nhập, còn metadata/file tài liệu thuộc từng chủ sở hữu.

Identity tin cậy lấy từ `sub` trong authorizer claims của API Gateway. API không lấy owner do client tự khai và tự chọn key lưu trữ. `_owned_document` so owner đã lưu với subject hiện tại. Tài liệu không tồn tại và tài liệu người khác đều trả 404 để không tiết lộ sự tồn tại. Record cũ thiếu owner bị từ chối, không tự gán. Query library ByOwner và cursor phân trang cũng gắn với subject; chỉ bảo vệ detail route sẽ chưa chặn dò danh sách.

Frontend xóa state riêng khi logout/401 để Bob dùng cùng browser không nhìn state Alice. Tuy vậy, ẩn UI chưa đủ: API phải chặn khi Bob nhập thủ công ID đã biết của Alice. URL storage ký sẵn là bearer capability ngắn hạn, ai giữ cũng có thể dùng đến khi hết hạn. Không lưu trong log/ảnh. S3 không tự kiểm JWT Cognito cho request ký sẵn này.

Bằng chứng có ý nghĩa là test phủ định ownership trong `tests/unit/test_documents.py` và `tests/frontend/local.e2e.cjs`: Alice upload/download; Bob có library rỗng, nhận 404 cho ID Alice. Test dùng handler thật với moto/identity giả local, **không** kiểm chữ ký Cognito hoặc IAM/CORS đang deploy. Sau khi được phép triển khai, sinh viên cần lặp lại tất cả document endpoint bằng hai tài khoản Cognito thật, JWT thiếu/sai/hết hạn và cursor bị sửa.

Thiết kế không cấp credentials S3 tổng quát cho browser, phân biệt service IAM role với quyền user. Giới hạn còn có capability chia sẻ được và onboarding do quản trị cấp. Tham khảo [JWT authorizer HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html), ma trận nghiệm thu ở Workshop. Câu hỏi bảo vệ: identity tin cậy từ đâu, vì sao trả 404, vì sao CORS không bảo vệ ownership?
