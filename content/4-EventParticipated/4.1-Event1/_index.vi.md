---
title: "Prove It: Agentic Cloud Investigation Series"
date: 2026-09-19
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch “Prove It: Agentic Cloud Investigation Series”

### Thông tin sự kiện

- **Đơn vị tổ chức:** CloudThinker × First Cloud AI Journey (FCAJ)
- **Thời gian:** Ngày 05, 12 và 19/09/2026 · 09:00–12:00
- **Địa điểm:** Văn phòng AWS, tầng 26, Bitexco Financial Tower, số 2 đường Hải Triều, phường Sài Gòn, Thành phố Hồ Chí Minh
- **Vai trò:** Người tham dự và thành viên **Team HCM15 (Tyrex)**
- **Hình thức:** Chuỗi ba workshop thực hành trên AWS và Kubernetes demo environment có quyền read-only

### Mục tiêu sự kiện

- Học cách quản trị AI Agent trước khi cấp quyền truy cập cloud environment.
- Điều tra cloud incident bằng cách kiểm tra competing hypotheses và thu thập evidence có thể truy nguồn.
- Thực hành làm việc với AI Agent dưới identity, permission, guardrail và operating boundary rõ ràng.
- Rèn luyện khả năng bảo vệ kết luận kỹ thuật trong điều kiện giới hạn thời gian.
- Hoàn thành ba portfolio artifact: Agentic Cloud Review, Incident Report và Investigation Dossier.

### Nguyên tắc của chương trình

Chương trình nhấn mạnh nguyên tắc **evidence over confidence**. Người tham dự không cung cấp AWS credential cá nhân và không thay đổi infrastructure. Các demo environment chỉ có quyền read-only: nhóm được inspect, analyze, investigate và recommend nhưng không được execute remediation.

Mỗi buổi được tổ chức theo cấu trúc **LEARN → PRACTICE → SHARE**, kết hợp kiến thức hướng dẫn, điều tra thực hành và chia sẻ kết quả.

### Day 01 — GOVERN · Observe Before You Act

**Thứ Bảy, ngày 05/09/2026**

#### Hoạt động chính

- Tìm hiểu sự thay đổi của ranh giới bảo mật khi AI Agent được cấp quyền truy cập cloud environment.
- Xác định identity của Agent và thiết kế permission theo nguyên tắc least privilege.
- Xác định operating boundary và guardrail trước khi cấp quyền.
- Sử dụng Agent có quyền read-only để review AWS demo account mà không thay đổi tài nguyên.
- Ghi lại các quan sát, rủi ro và hành động đề xuất dựa trên evidence.

#### Kết quả

- Hiểu vì sao phải thiết kế cơ chế quản trị Agent trước khi cấp quyền truy cập.
- Phân biệt quyền quan sát read-only với quyền có thể thay đổi tài nguyên.
- Thực hành giám sát Agent thay vì mặc định chấp nhận kết quả của Agent.
- Hoàn thành artifact **Agentic Cloud Review** và phát triển kỹ năng **Agent Supervisor**.

**Tài liệu:** [Day 01 — GOVERN](https://docs.cloudthinker.io/learn/workshops/prove-it/day-01-govern)

### Day 02 — INVESTIGATE · From Signal to Root Cause

**Thứ Bảy, ngày 12/09/2026**

#### Hoạt động chính

- Điều tra một cloud environment đã được cố ý cấu hình sai.
- Challenge câu trả lời đầu tiên của Agent thay vì chấp nhận ngay giải thích ban đầu.
- Xây dựng competing hypotheses và kiểm tra từng giả thuyết bằng evidence thu thập từ môi trường.
- Ghi claim, source và evidence ủng hộ hoặc phản bác vào Evidence Table.
- Xây dựng Causal Graph để kết nối các triệu chứng quan sát được với nguyên nhân gốc.
- Tổng hợp quá trình điều tra thành Incident Report.

#### Kết quả

- Cải thiện khả năng phân biệt observation, hypothesis, evidence và conclusion.
- Thực hành loại bỏ claim không có bằng chứng và truy ngược mỗi kết luận về source.
- Hoàn thành artifact **Incident Report** và phát triển kỹ năng **Agentic Incident Investigator**.
- Team HCM15 (Tyrex) đạt **93,6/100 điểm** cho bài nộp Day 02.

**Tài liệu:** [Day 02 — INVESTIGATE](https://docs.cloudthinker.io/learn/workshops/prove-it/day-02-investigate)

### Day 03 — OPERATE · Prove It Arena

**Thứ Bảy, ngày 19/09/2026**

#### Hoạt động chính

- Thi đấu Prove It Arena với vai trò thành viên Team HCM15 (Tyrex).
- Tiếp nhận một cloud environment hoàn toàn mới và thực hiện rapid review.
- Xây dựng Environment Map gồm các tài nguyên, dependency, ranh giới truy cập và rủi ro tiềm ẩn.
- Phản ứng với incident inject trong điều kiện giới hạn thời gian.
- Phân chia nhiệm vụ vận hành Agent, giữ giả thuyết đối lập, thu thập evidence, phân tích quan hệ nhân quả và chuẩn bị dossier.
- Thu thập evidence kèm source và timestamp, xây dựng Causal Graph và hoàn thiện Investigation Dossier.
- Bảo vệ kết luận của nhóm trước hội đồng giám khảo trong phần Final Defence.

#### Kết quả

- Áp dụng toàn bộ quy trình điều tra: rapid review, xây dựng giả thuyết, thu thập evidence, causal reasoning, ra quyết định và bảo vệ kết luận.
- Hoàn thành artifact **Investigation Dossier** và thực hành kỹ năng **Agentic Cloud Operator**.
- Kết quả riêng của Day 03 chưa đạt kỳ vọng, cho thấy nhóm cần cải thiện việc phân chia vai trò, quản lý thời gian, kiểm chứng evidence và bảo vệ kỹ thuật.

**Tài liệu:** [Day 03 — Prove It Arena](https://docs.cloudthinker.io/learn/workshops/prove-it/day-03-prove-it-arena)

### Tiêu chí chấm điểm Prove It Arena

- Agent governance và operating boundaries: **15 điểm**
- Environment review và risk prioritization: **20 điểm**
- Incident investigation và causal reasoning: **30 điểm**
- Decision và remediation quality: **20 điểm**
- Evidence defence và communication: **15 điểm**

Mỗi hint được sử dụng sẽ bị trừ hai điểm và claim không thể truy nguồn sẽ không được tính điểm. Kết luận insufficient evidence vẫn có thể được ghi nhận nếu được bảo vệ hợp lý.

### Kết quả toàn bộ series

- Hoàn thành cả ba giai đoạn GOVERN, INVESTIGATE và OPERATE.
- Hoàn thành ba portfolio artifact: Agentic Cloud Review, Incident Report và Investigation Dossier.
- Phát triển ba kỹ năng Agent Supervisor, Agentic Incident Investigator và Agentic Cloud Operator.
- Dựa trên kết quả tổng hợp của cả ba ngày, **Team HCM15 (Tyrex)** nhận giải **“Nhóm HCM có bài thi ấn tượng nhất.”**

### Bài học rút ra

- AI Agent phải hoạt động trong identity, permission scope và guardrail được xác định rõ ràng.
- Câu trả lời đầu tiên của Agent chỉ là một hypothesis, chưa phải conclusion đã được kiểm chứng.
- Mỗi claim quan trọng cần có evidence và source có thể truy ngược.
- Competing hypotheses giúp giảm confirmation bias trong quá trình điều tra incident.
- Làm việc nhóm hiệu quả cần phân vai rõ ràng cho việc vận hành Agent, ghi evidence, giữ giả thuyết đối lập, phân tích quan hệ nhân quả và viết báo cáo.
- Kết luận hợp lý về mặt kỹ thuật vẫn chưa đủ nếu nhóm không thể trình bày và bảo vệ rõ ràng.

### Trải nghiệm tại sự kiện

Chuỗi workshop mang lại góc nhìn thực tế về cách AI Agent hỗ trợ cloud operation mà không thay thế khả năng phán đoán của con người. Bài học quan trọng nhất là không xem confidence là đủ và luôn yêu cầu evidence trước khi chấp nhận một kết luận. Quá trình làm việc cùng Team HCM15 (Tyrex) cũng cho thấy vai trò của việc phân công rõ ràng, quản lý thời gian và giao tiếp trong một cuộc điều tra.

Mặc dù kết quả Day 03 chưa đạt kỳ vọng, trải nghiệm này đã chỉ ra những điểm yếu cụ thể để nhóm cải thiện trong các bài thực hành incident response sau này. Việc hoàn thành toàn bộ series giúp kết nối cloud governance, investigation và operation thành một quy trình thống nhất dựa trên evidence.

### Hình ảnh sự kiện

#### Check-in tại văn phòng AWS

![Người tham dự check-in tại sự kiện Prove It](../../../Image/Event%201/6C7A3010.jpg)

#### Team HCM15 (Tyrex) thực hiện bài điều tra

![Team HCM15 Tyrex tập trung thực hiện bài điều tra](../../../Image/Event%201/6C7A3113.jpg)

#### Team HCM15 (Tyrex) tại không gian làm việc AWS

![Team HCM15 Tyrex trong workshop Prove It tại văn phòng AWS](../../../Image/Event%201/6C7A3197.jpg)

#### Nhận phần ghi nhận từ ban tổ chức

![Các thành viên nhận quà từ ban tổ chức Prove It](../../../Image/Event%201/6C7A3326.jpg)

![Team HCM15 Tyrex nhận quà tại Prove It Arena](../../../Image/Event%201/6C7A3331.jpg)

#### Quà lưu niệm sau khi hoàn thành series

![Team HCM15 Tyrex cùng quà tặng CloudThinker và AWS](../../../Image/Event%201/6C7A3353.jpg)

#### Ảnh tập thể Prove It Arena

![Người tham dự và ban tổ chức trong phần tổng kết Prove It Arena](../../../Image/Event%201/6C7A3383.jpg)
