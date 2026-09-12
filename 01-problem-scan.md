# 01 — Problem Scan & Quick Cards (Vin Smart Future)

> **Nộp bài tập cá nhân/nhóm — Phase 1 (SCAN) & Phase 2 (QUICK-ASSESS).**
> Mảng được chọn: 🚗 **VinFast — Dịch vụ hậu mãi & Chẩn đoán sức khỏe pin (Battery SoH Diagnostics).**

---

## 🏛️ Bối cảnh

Tôi là **AI Product Engineer** tại **Vin Smart Future** (Vingroup), phối hợp với Khối Dịch vụ Hậu mãi của **VinFast** để tìm cơ hội tối ưu vận hành bằng AI. Qua quan sát xưởng dịch vụ, tôi nhận thấy các kỹ thuật viên pin đang xử lý nhiều báo cáo sức khỏe pin (SoH) thủ công, tốn thời gian và thiếu nhất quán, gây rò rỉ hiệu suất và giảm trải nghiệm khách hàng.

---

## 🔍 Phase 1 — SCAN (4 Lenses)

Quét bằng 4 lăng kính: **Lặp lại**, **Tốn thời gian**, **AI có thể tốt hơn**, **Pain từ người khác**.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | VinFast | Lặp lại | Thẩm định & phân loại sơ bộ yêu cầu bảo hành từ mô tả tiếng Việt + ảnh lỗi của khách (~1.200 claim/ngày, 12 phút/lượt). |
| 2 | VinFast | Tốn thời gian | Tổng hợp báo cáo sức khỏe pin (SoH) & khuyến nghị bảo trì cho xe vào xưởng (~25 phút/xe). |
| 3 | VinFast | AI có thể tốt hơn | Hotline/CSKH hậu mãi trả lời FAQ lặp lại (sạc, quãng đường, OTA, bảo hành); 50–60% cuộc gọi là FAQ, tắc nghẽn giờ cao điểm. |
| 4 | VinFast | Pain từ người khác | Tra cứu & đặt phụ tùng đúng mã theo dòng xe (VF5/VFe34/VF8) và phiên bản; nhân viên kho đặt sai mã khiến xe chờ phụ tùng. |
| 5 | VinFast | Tốn thời gian + Pain từ người khác | Điều phối hỗ trợ cạn pin thực địa: chủ xe báo pin dưới ngưỡng, điều phối viên tra trạm sạc trống & soạn tin chỉ dẫn, hoặc điều xe sạc pin di động (~15 phút/lượt, 60–80 ca/ngày). |

> Các con số là **ước lượng định hướng**, cần thay bằng baseline đo thực tế trước khi đưa vào Problem Statement.

---

## 🃏 Phase 2 — QUICK-ASSESS (3 Quick Problem Cards)

Top 3 bài toán tiềm năng nhất: **#1 (Bảo hành), #2 (Báo cáo SoH), #5 (Hỗ trợ cạn pin thực địa).**

### Card #1 — VinFast Thẩm định yêu cầu bảo hành

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại & thẩm định sơ bộ yêu cầu bảo   │
│ hành từ mô tả tiếng Việt + ảnh lỗi của khách hàng.          │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ (Service Advisor)       │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách gửi mô tả + ảnh qua App/Call center              │
│   ──> 2. Cố vấn đọc & phân loại lỗi                         │
│   ──> 3. Tra điều kiện bảo hành theo dòng xe                │
│   ──> 4. Soạn phản hồi/đề xuất                              │
│   ──> 5. Chuyển xưởng xếp lịch                              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3 (⏱ 12 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (phân loại lỗi + đối chiếu điều kiện, draft kết quả)        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian thẩm định từ 12 phút ──> dưới 2 phút;        │
│ tỉ lệ phân loại đúng đạt ≥95%.                              │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

### Card #2 — VinFast Báo cáo sức khỏe pin (SoH) — ĐƯỢC CHỌN DEEP-DIVE

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tổng hợp báo cáo sức khỏe pin (SoH) và    │
│ khuyến nghị bảo trì sau khi test xe vào xưởng.              │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên pin                      │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cắm máy đọc dữ liệu BMS                                │
│   ──> 2. Xuất dữ liệu thô (cell, nhiệt độ, chu kỳ)          │
│   ──> 3. Phân tích SoH & cell bất thường                    │
│   ──> 4. Viết nhận xét + khuyến nghị bảo trì                │
│   ──> 5. In/gửi báo cáo cho khách                           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 25 phút/xe)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 4                │
│ (sinh nhận xét & khuyến nghị từ dữ liệu có cấu trúc)        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian soạn báo cáo từ 25 phút ──> dưới 5 phút;     │
│ 100% báo cáo đủ các trường bắt buộc.                        │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

### Card #5 — VinFast Hỗ trợ cạn pin thực địa

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán (1 câu): Hỗ trợ chủ xe VinFast cạn pin thực địa:   │
│ tra trạm sạc trống gần nhất & soạn tin chỉ dẫn, hoặc điều   │
│ xe sạc pin di động khi pin ở mức nguy hiểm.                 │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên hỗ trợ (Roadside        │
│ Support Dispatcher); chủ xe (chờ đợi, nguy cơ kẹt đường)     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận cuộc gọi/ticket cạn pin                           │
│   ──> 2. Lấy toạ độ GPS của xe                              │
│   ──> 3. Tra trạm sạc VinFast còn trụ & đúng cổng sạc       │
│   ──> 4. Soạn tin nhắn chỉ dẫn gửi qua App cho chủ xe       │
│   ──> 5. Điều xe sạc pin di động nếu pin < 5%               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3-4 (⏱ 10 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4              │
│ (auto-pull GPS → tra trạm → draft tin chỉ dẫn)              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý từ 15 phút ──> dưới 3 phút;            │
│ 98% chỉ đúng trạm và đúng loại cổng sạc.                    │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết định chọn bài toán Deep-Dive

Nhóm chọn **Card #2 — Báo cáo sức khỏe pin (SoH)** để Deep-Dive.

**Lý do:** Bài toán có scope hẹp, dữ liệu BMS **có cấu trúc** nên LLM Feature xử lý tốt; metric định lượng rõ (25 phút → dưới 5 phút; 100% báo cáo đủ trường); và rủi ro được kiểm soát vì AI chỉ **tạo nháp**, kỹ thuật viên duyệt trước khi gửi khách (HITL).

**Lý do loại các card khác:**
* **Card #1 (Bảo hành):** Rủi ro tài chính – pháp lý cao nếu AI quyết định thay người; cần thêm dữ liệu và rule-based router trước.
* **Card #5 (Cứu hộ cạn pin):** Đòi hỏi tích hợp API định vị/trạm sạc real-time 24/7, khó chứng minh trọn vẹn trong scope lab.
