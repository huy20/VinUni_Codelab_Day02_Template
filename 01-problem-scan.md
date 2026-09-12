# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **Vinhomes** | Lặp lại | Tự động định tuyến phiếu yêu cầu sửa chữa (Work Order) từ App cư dân theo ma trận phân loại sự cố và bảng phân ca trực kỹ thuật tòa nhà. |
| 2 | **Vinmec** | Tốn thời gian | Trợ lý trích xuất dữ liệu lâm sàng từ hồ sơ bệnh án điện tử (EMR) để dự thảo tóm tắt xuất viện (Discharge Summary) cho bác sĩ điều trị duyệt. |
| 3 | **VinFast** | AI có thể tốt hơn | Tự động phân loại mô tả lỗi xe bằng tiếng Việt thông thường của chủ xe (âm thanh lạ, rung lắc, cảnh báo màn hình) thành mã lỗi kỹ thuật sơ bộ (DTC triage) trước khi xe vào xưởng dịch vụ. |
| 4 | **Xanh SM** | Pain từ người khác | Tự động phát hiện và cảnh báo điểm đón/trả khách bị kẹt (điểm cấm dừng đỗ, rào chắn công trình) dựa trên phân tích phản hồi tin nhắn/ghi chú của tài xế theo thời gian thực. |
| 5 | **Vinpearl** | Tốn thời gian | Trích xuất thông tin từ email yêu cầu đặt phòng đoàn (Group Booking RFP) của công ty lữ hành để kiểm tra quỹ phòng và dự thảo báo giá chính sách linh hoạt. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card):
* **Card #1:** Vinhomes — Định tuyến phiếu kỹ thuật cư dân theo ca trực (Rule-based Matrix Routing).
* **Card #2:** Vinmec — Soạn thảo tóm tắt hồ sơ xuất viện (Discharge Summary Generator).
* **Card #3:** VinFast — Triage mã lỗi sơ bộ từ mô tả âm thanh / hiện tượng bất thường của chủ xe.

---

### 🎴 Thẻ 1: Vinhomes — Rule-based Work Order Dispatching

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động định tuyến phiếu yêu cầu kỹ thuật  │
│ cư dân dựa trên form chọn danh mục và ma trận ca trực tòa nhà.│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên CSKH Vinhomes (quá tải),│
│ Kỹ thuật viên tòa nhà (chậm nhận việc), Cư dân (chờ lâu).   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân chọn loại sự cố trên dropdown App Resident      │
│   ──> 2. Điều phối viên mở file Excel tra cứu ca trực tòa nhà│
│   ──> 3. Tạo Work Order trên ERP và gán ID kỹ thuật viên    │
│   ──> 4. Gửi SMS/Push notification thông báo cho kỹ thuật   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 6 phút/phiếu)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? KHÔNG DÙNG AI!        │
│ (Dùng Rule Engine: Lookup Matrix [Tòa + Loại sự cố + Ca trực]│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Thời gian gán vé giảm từ 6 phút ──> 0 giây (Real-time);   │
│    Độ chính xác phân công đạt 100%, Chi phí LLM API = 0 VNĐ" │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> **💡 Lý giải kỹ thuật vì sao chọn RULE thay vì LLM:**
> Dữ liệu đầu vào từ App cư dân đã được chuẩn hóa qua Form/Dropdown (Tòa nhà, Tầng, Loại sự cố: Điện/Nước/Thang máy). Ma trận phân ca trực là quy tắc tất định (deterministic). Sử dụng Rule Engine / SQL Lookup giải quyết bài toán với độ chính xác 100%, độ trễ 0s, không tốn chi phí token và loại bỏ hoàn toàn rủi ro ảo giác (hallucination) phân công nhầm người. Dùng LLM ở đây là "Over-engineering".

---

### 🎴 Thẻ 2: Vinmec — Discharge Summary Generator

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Trích xuất lịch sử khám, xét nghiệm và toa│
│ thuốc để dự thảo bản tóm tắt hồ sơ xuất viện cho bệnh nhân. │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị & Bệnh nhân xuất viện. │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Mở nhiều tab bệnh án điện tử (xét nghiệm, mổ, thuốc)   │
│   ──> 2. Đọc và tổng hợp thủ công diễn tiến bệnh 3-7 ngày   │
│   ──> 3. Gõ văn bản tóm tắt xuất viện và dặn dò sau ra viện │
│   ──> 4. Ký duyệt và in hồ sơ trao cho bệnh nhân            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 20 phút/ca)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3            │
│ (Trích xuất dữ liệu có cấu trúc từ EMR -> Draft tóm tắt)    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Giảm thời gian soạn hồ sơ từ 25 phút ──> dưới 5 phút;    │
│    100% tóm tắt bắt buộc có Bác sĩ kiểm tra và ký số (HITL)"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

### 🎴 Thẻ 3: VinFast — Voice/Text Diagnostic Triage

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại mô tả hiện tượng lạ trên xe điện│
│ thành mã nhóm chẩn đoán ban đầu để đặt lịch xưởng tối ưu.   │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ xưởng VinFast, Chủ xe VF│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Lắng nghe khách mô tả qua hotline/app (tiếng lục cục..)│
│   ──> 2. Cố vấn dịch vụ phỏng đoán hệ thống gặp sự cố       │
│   ──> 3. Tra cứu lịch trống cầu nâng và phụ tùng dự trữ     │
│   ──> 4. Hẹn khách mang xe đến xưởng kiểm tra trực tiếp     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 1 & 2 (⏱ 10 phút/cuộc)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & 2            │
│ (Phân tích ngôn ngữ tự nhiên -> Map với sổ tay kỹ thuật VF) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│   "Độ chính xác phân loại ban đầu (gầm/pin/phần mềm) >= 90%;│
│    Giảm 40% thời gian chờ tiếp nhận tại xưởng dịch vụ"      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

---

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*
