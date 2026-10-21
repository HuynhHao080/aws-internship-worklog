---
title: "Event 2"
date: 2026-08-08
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---


# Bài thu hoạch “AWS FCAJ Agent Forge - Deepdive Day 2”

### Mục Đích Của Sự Kiện

- Tổng quan về Agent Core
- Các thành phần cốt lõi của Agent: Memory, Observability, Evaluation, Policy & Security, Tools & Capability
- Các dịch vụ hỗ trợ như Browser (duyệt web/search), Code Interpreter (môi trường thực thi code), và Payment (tích hợp thanh toán)
- Thực hành Lab

### Danh Sách Diễn Giả

- **NGhia Tran** - Các thành phần cốt lõi của Agent
- **Anh Pham** -Thực Hành lab

### Nội Dung Nổi Bật

#### Đưa ra các kiến thức về AI agent

- Cách tính context window, các Agent Core memory: Hiểu rõ giới hạn context window của các mô hình (ví dụ: Claude có thể lên tới 1  triệu token) để thấy sự cần thiết của Memory trong việc lưu trữ và cá nhân hóa trải nghiệm người dùng.
- Cơ chế lưu trữ và trích xuất của Bộ nhớ ngắn hạn (Short-term) & dài hạn (Long-term): Bộ nhớ ngắn hạn lưu tin nhắn thô (raw) 
 đồng bộ để hội thoại liền mạch, bộ nhớ dài hạn trích xuất thông tin quan trọng một cách bất đồng bộ để lưu dạng vector
 với 4 chiến lược chính: Summary, User Preference, Semantic và Episodic.
- Phân vùng bộ nhớ bằng Namespace và Bảo mật phân quyền: Sử dụng cấu trúc Namespace (dạng thư mục) để phân mảnh dữ liệu giúp tìm kiếm nhanh và tiết kiệm token. Thiết lập bảo mật thông qua IAM policy và cơ chế kiểm soát chi tiết phạm vi truy cập (access scoping) giữa các agent.

#### Trụ Cột Giám Sát và Đánh Giá Hệ Thống Agentic AI

- **Observability (Khả năng quan sát)**: Hoạt động theo triết lý "You cannot fix what you cannot see. "Tích hợp OpenTelemetry để thu thập dữ liệu qua 3 trụ cột: Logs (lỗi/trạng thái hệ thống), Traces (luồng di chuyển của hội thoại), và Metrics (latency, chi phí token).
- **Evaluation (Đánh giá năng lực):** Giúp phát hiện lỗi ảo tưởng (hallucination), lập luận sai (faulty reasoning) dẫn đến gọi sai công cụ. Cung cấp 13 bộ đánh giá tích hợp (built-in evaluators) chạy theo cơ chế On-demand (khi dev) hoặc Online (real-time trên production).

#### Các Dịch Vụ Hỗ Trợ Đắc Lực (Capabilities)

- **Cedar Policy & Security**: Kiểm soát hành vi của Agent bằng ngôn ngữ Cedar (ví dụ: thiết lập quy tắc tự động duyệt refund dưới $100). Hỗ trợ can thiệp phê duyệt của con người (human-in-the-loop) khi vượt ngưỡng
- **Browser & Code Interpreter (Sandbox)**: Tạo môi trường giả lập an toàn để Agent tự động duyệt web/tìm kiếm thông tin
 và tự viết/chạy thử nghiệm code mà không ảnh hưởng tới hệ thống thật của người dùng
- **Payment & Registry**: Tích hợp thanh toán trực tiếp qua Stripe/Coinbase. Thiết lập Registry để đăng ký và tái sử dụng công cụ/API giữa các bộ phận trong doanh nghiệp
- **Agent Harness & Knowledge Base**:  Khởi tạo nhanh agent tối giản chỉ với model, system prompt và tool. Kết nối RAG với dữ liệu nội bộ của doanh nghiệp (S3, SharePoint, OneDrive),

### Những Gì Học Được

#### Tư Duy Thiết Kế

- **Tư duy kỹ thuật hệ thống (System Engineering)**: Nhận thức được việc xây dựng AI Agent cho doanh nghiệp thực tế thì công nghệ AI lõi chỉ chiếm 6%, trong khi 93% còn lại là kỹ thuật phần mềm (software engineering) như xử lý bộ nhớ, tích hợp công cụ, giám sát và bảo mật.
- **Personalization & Responsibility**: Tầm quan trọng của việc cá nhân hóa trải nghiệm qua bộ nhớ lâu dài, song song với việc tuân thủ các quy tắc bảo mật dữ liệu nhạy cảm của khách hàng và sử dụng AI có trách nhiệm.

#### Kiến Trúc Kỹ Thuật

- **Kết hợp Memory hợp lý**: Để tối ưu chi phí và tránh tràn context window, cần kết hợp khéo léo giữa việc truy xuất một phần bộ nhớ ngắn hạn và các key insights từ bộ nhớ dài hạn.
- **Giám sát bằng AI Judge**: Hiểu cách vận hành của cơ chế dùng một mô hình AI thông minh khác làm "trọng tài" (LLM Judge) để đánh giá từng bước lập luận của Agent theo các tiêu chuẩn công nghiệp.

### Ứng Dụng Vào Công Việc

- **Áp dụng Bedrock Agent Core Memory** Triển khai cơ chế lưu trữ lịch sử chat của dự án hiện tại với cấu hình retention (ví dụ: lưu 7 ngày), phân mảnh dữ liệu khách hàng theo Namespace nhằm tối ưu hóa chi phí token và tăng độ chính xác khi tìm kiếm.
- **Tích hợp AWS CloudWatch / Observability**: Thiết lập các chỉ số giám sát độ trễ (latency), chi phí token tiêu thụ của từng model để kịp thời điều chỉnh system prompt.
- **Thử nghiệm mô hình Nova Micro**: Cấu hình chuyển đổi các tác vụ đơn giản từ Claude sang dòng model thế hệ mới Amazon Nova Micro của AWS để tăng tốc độ phản hồi và tiết kiệm ngân sách chạy thử nghiệm.
- **Thiết lập sandbox an toàn**: Thử nghiệm tích hợp Code Interpreter giả lập để cho phép Agent tự động thực hiện các phép toán phức tạp hoặc phân tích dữ liệu một cách biệt lập.

### Trải nghiệm trong event

Tham gia workshop **“ AWS FCAJ Agent Forge - Deepdive Day 2”** là một trải nghiệm rất bổ ích,  cung cấp lượng kiến thức cực kỳ lớn ở cấp độ chuyên sâu (L300 - Advance), các bài giảng lý thuyết của diễn giả giúp người nghe dễ dàng hình dung cách hệ thống vận hành bên dưới (under the hood) trước khi các công cụ của Agent Core tự động hóa và tối giản hóa chúng.
#### Học hỏi từ các diễn giả có chuyên môn cao
- Các diễn giả đến từ AWS đã chia sẻ sử dụng Kiro IDE với cơ chế file steering định hướng ngữ cảnh, dùng UV để quản lý môi trường và triển khai trực tiếp tài nguyên AWS bằng Agent Core CLI / CDK.

#### Trải nghiệm kỹ thuật thực tế
- Tiếp cận kiến trúc **modular của Amazon Bedrock Agent Core**, giúp hình dung cách phân rã hệ thống Agentic AI thành các dịch vụ độc lập để dễ dàng mở rộng và quản lý.
- Học cách thiết lập và vận hành hệ thống **Memory** đa lớp. Phân biệt rõ sự khác nhau giữa **Short-term memory** hoạt động đồng bộ (synchronous) để duy trì mạch hội thoại, và **Long-term memory** hoạt động bất đồng bộ (asynchronous) để trích xuất tri thức lâu dài dưới dạng vector.
- Hiểu sâu về cách sử dụng cấu trúc **Namespace** (định dạng cây thư mục) trong bộ nhớ dài . Cơ chế này giúp cô lập không gian lưu trữ của từng actor/session, thu hẹp phạm vi tìm kiếm tương đồng (similarity search) để tiết kiệm token và tối ưu hóa chi phí.
- Nắm bắt phương pháp giám sát hệ thống thông qua **3 trụ cột của Observability (Logs, Traces, Metrics)** dựa trên giao thức **OpenTelemetry**. Điều này giúp đo lường trực quan độ trễ (latency), chi phí và lượng token tiêu thụ của từng mô hình.

#### Ứng dụng công cụ hiện đại
- Trực tiếp làm quen với trình soạn thảo code hỗ trợ AI là **Kiro IDE**, tìm hiểu cơ chế steering document để định hướng ngữ cảnh dự án một cách nhanh chóng và chính xác.
- Thực hành sử dụng UV để quản lý và cài đặt các gói phụ thuộc (dependencies) trong môi trường biệt lập, cùng với **Agent Core CLI và CDK** để tự động hóa quy trình đóng gói và triển khai tài nguyên lên AWS thông qua CloudFormation.
- Học cách cấu hình và chuyển đổi mô hình mặc định sang dòng mô hình thế hệ mới **Amazon Nova Micro**. Việc này giúp tăng đáng kể tốc độ phản hồi và tiết kiệm tối đa chi phí khi chạy thử nghiệm.

#### Kết nối và trao đổi
- Cơ hội kết nối và học hỏi trực tiếp trong cộng đồng **First Cloud and AI (FCAJ)**, được chia sẻ các kịch bản triển khai thực tế từ các Solution Architects giàu kinh nghiệm của AWS tại Việt Nam.
- Biết thêm góc nhìn thực tế đắt giá từ các chuyên gia: Trong một hệ thống Agentic AI thực tế cấp doanh nghiệp, phần công nghệ lõi AI chỉ chiếm 6%, trong khi 93% còn lại thuộc về kỹ thuật phần mềm (software engineering) như xử lý bộ nhớ, tích hợp công cụ, giám sát và bảo mật.

#### Bài học rút ra
- Việc xây dựng AI Agent cho doanh nghiệp đòi hỏi tư duy bảo mật đa lớp nghiêm ngặt thông qua chính sách kiểm soát chi tiết phạm vi truy cập **(Access Scoping)** và ngôn ngữ **Cedar Policy**, nhằm tránh rò rỉ dữ liệu nhạy cảm giữa các Agent
- Quá trình kiểm chứng năng lực Agent (Evaluation) với 13 bộ đánh giá tích hợp (built-in evaluators)chia làm 3 cấp độ (Session, Trace, Span), kết hợp cơ chế LLM Judge, là giải pháp cốt lõi để phát hiện lỗi ảo tưởng (hallucination) một cách có hệ thống trước khi đưa hệ thống lên production.
- Việc dịch chuyển hệ thống Agentic AI lên các nền tảng serverless đám mây như Bedrock Agent Core giúp doanh nghiệp đạt được độ tin cậy cao về cả **Security** và **Scalability** với chi phí tối ưu theo lượng sử dụng thực tế

#### Một số hình ảnh khi tham gia sự kiện
* Thêm các hình ảnh của các bạn tại đây
> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi thay đổi cách tư duy về thiết kế ứng dụng, hiện đại hóa hệ thống và phối hợp hiệu quả hơn giữa các team.
