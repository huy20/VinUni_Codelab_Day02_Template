# Lab 02 — Problem Scan & Quick Cards (Vin Smart Future)

**Nhóm:** VinSmart Future — AI Product Scoping Team  
**Đơn vị:** Vin Smart Future (Vingroup)  
**Người thực hiện:** [Tên thành viên]  
**Ngày nộp:** September 2026

---

## 🏛️ Bối cảnh

Tôi là AI Engineer tại **Vin Smart Future**, đơn vị công nghệ thống nhất của Vingroup. Nhiệm vụ của tôi là tìm kiếm, scoping và phân tích độ khả thi các giải pháp AI để tối ưu hóa vận hành tại các công ty thành viên: VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl.

Trong buổi Lab hôm nay, nhóm tập trung vào mảng **Xanh SM (GSM)** — hệ thống xe taxi điện thông minh với hơn 5.000 xe VF5/VFe34 đang vận hành tại Hà Nội, TP.HCM và các tỉnh phía Bắc.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup, ghi lại ít nhất 5 bottleneck thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày.
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên.
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn.
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn.

### 📝 List bài toán đã xác định:

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán | Ước tính tổn thất |
|---|----------------------------------|------|---------------------|-------------------|
| 1 | **Xanh SM** | Lặp lại | So khớp và phân bổ lại cuốc xe khi khách hàng yêu cầu thay đổi điểm đến giữa chừng. Điều phối viên phải đọc tin nhắn, tra GPS mới, re-route tay → ~120 req/hora tại Hà Nội. | ~2 phút/lượt × 120req = 240 giờ/ngày lãng phí |
| 2 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công các phản hồi khẩn cấp từ tài xế về sự cố sạc pin hoặc hết pin thực địa (mất 15-20 min/lượt). Tra cứu GPS + trạm sạc + soạn tin nhắn hoàn toàn thủ công. | ~80 sự cố/ngày × 15phút = 20 giờ làm việc/lãng phí mỗi ngày |
| 3 | **VinFast** | Lặp lại | So khớp hóa đơn sạc điện và đối chiếu số liệu trạm sạc đối tác hằng tuần (hàng nghìn giao dịch). Nhân viên tài chính phải mở 10+ tab Excel, cross-reference manual. | ~3 nhân viên × 5 ngày/tuần × 4 giờ/ngày = 60 giờ/tuần |
| 4 | **Vinhomes** | AI-upgrade | Hệ thống phân loại và route tự động các phản ánh/khiếu nại của cư dân trên App Vinhomes Resident. Phản hồi cồng kềnh, mất 12 tiếng để response trung bình. | ~200 khiếu nại/tuần bị xử lý chậm, NPS giảm 15% |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ mất quá nhiều thời gian viết tóm tắt hồ sơ xuất viện (discharge summary) — mất 20-30 phút/bệnh nhân, bác sĩ phàn nàn vì quá tải ca đêm. | ~50 bệnh nhân xuất viện/ngày × 25phút = 20 giờ/bác sĩ/tuần |
| 6 | **Xanh SM** | Tốn thời gian | Phân tích lý do khách hàng hủy chuyến từ cuộc gọi ghi âm và ghi chú của tài xế để tìm pattern lỗi hệ thống thống kê theo tháng. | ~400 cuốc hủy/tháng → không phân tích được root cause kịp thời |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn **top 3 bài toán** từ danh sách SCAN để đánh giá nhanh độ khả thi AI.

---

## Thẻ bài toán tiêu biểu #1 — Xanh SM Xử lý sự cố sạc pin thực địa ⭐ (Được chọn làm Deep-Dive)

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo cáo sự cố sạc pin / hết pin    │
│ giữa đường cần điều phối cứu hộ hoặc trạm sạc gần nhất.     │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (chờ đợi, lo lắng),                     │
│              Điều phối viên (quá tải, stress vào giờ cao    │
│              điểm)                                          │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi tổng đài điều vận → Báo hết pin             │
│      ⏱ 2 phút                                               │
│      In: Cuộc gọi thoại                                     │
│   2. Điều phối viên tra cứu định vị GPS xe trên bản đồ       │
│      nội bộ                                                   │
│      ⏱ 2 phút                                               │
│      In: Biển số xe → Out: Toạ độ GPS                       │
│   3. Tra cứu thủ công các trạm sạc VinFast còn trụ trống      │
│      trên Dashboard                                           │
│      ⏱ 5 phút 🔴 BOTTLENECK                                 │
│      In: Vị trí GPS, loại xe → Out: Danh sách trạm gần nhất  │
│   4. Viết tin nhắn chỉ dẫn/đường đi chi tiết gửi qua          │
│      App xanhSM                                              │
│      ⏱ 5 phút 🔴 BOTTLENECK                                 │
│      In: Trạm sạc → Out: Tin nhắn raw text                  │
│   5. Liên hệ đội xe cứu hộ nếu xe đã cạn kiệt pin (< 5%)     │
│      ⏱ 1 phút                                               │
│      In: Cảnh báo pin thấp → Out: dispatch_mobile_charger    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                             │
│   Bước 3 + 4 (⏱ 10 phút/lượt — chiếm 67% tổng thời gian)    │
│                                                           │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 3: Auto-pull vị trí GPS + query API trạm sạc trống    │
│   Bước 4: LLM tự động soạn tin nhắn hướng dẫn               │
│                                                          │
│ Đo thành công bằng gì (Metric có số)?                        │
│   • Giảm thời gian xử lý sự cố từ 15 phút → dưới 3 phút      │
│     (tăng tốc 5×)                                          │
│   • Tỉ lệ hướng dẫn đúng địa điểm + đúng loại trụ sạc: >98%  │
│   • Giảm workload điều phối viên: -70% thao tác tay         │
│                                                          │
│ Quick Architecture: [ ] No AI  [x] LLM Feature  [ ] Agent   │
└─────────────────────────────────────────────────────────────┘
```

---

## Thẻ bài toán #2 — VinFast Chẩn đoán lỗi xe từ mô tả tiếng Việt

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Khách hàng mô tả triệu chứng hư hỏng xe bằng       │
│ tiếng Việt tự nhiên, hệ thống cần phân loại mã lỗi kỹ        │
│ thuật ban đầu để tư vấn sửa chữa phù hợp.                    │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau? Khách hàng (không hiểu kỹ thuật),              │
│              Kỹ thuật viên BSX (phải hỏi lại nhiều lần)     │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Khách hàng gọi Hotline 1900 XXX → Mô tả symptoms       │
│      ⏱ 5 phút (khách nói, telesales nghe, ghi chép)         │
│   2. Telesales phỏng vấn thêm → Hỏi chi tiết từng bộ phận   │
│      ⏱ 8 phút                                               │
│   3. Ghi nhận symptom code → Chuyển cho BSX gần nhất        │
│      ⏱ 2 phút (manual entry)                                │
│   4. BSX kiểm tra + xác nhận diagnosis chính xác             │
│      ⏱ 10 phút (tại garage)                                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                             │
│   Bước 2: Phỏng vấn lại Symptoms — Telesales không có kiến   │
│   thức kỹ thuật → hỏi sai câu, bỏ sót symptom → Diagnostic   │
│   delay = Khách hàng chờ 15-20 phút trước khi được BSX xem  │
│   ⏱ 8 phút/buzz call, error rate ~25%                      │
│                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 1→2: LLM tiếp nhận mô tả tiếng Việt → Tự động phân   │
│   loại symptom → Gợi ý symptom code chuẩn → Gợi ý BSX phù  │
│   hợp theo chuyên môn                                      │
│                                                          │
│ Đo thành công bằng gì (Metric có số)?                        │
│   • Giảm thời gian pre-diagnosis từ 13 phút → dưới 2 phút   │
│   • Tăng准确率 symptom coding từ 75% → 95%                 │
│   • Giảm tỷ lệ chuyển tay wrong-specialist từ 25% → <5%    │
│                                                          │
│ Quick Architecture: [ ] No AI  [x] LLM Feature  [ ] Agent   │
└─────────────────────────────────────────────────────────────┘
```

---

## Thẻ bài toán #3 — Vinhomes Phân loại & Điều hướng phản ánh cư dân

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Phân loại tự động các khiếu nại/phản ánh của cư    │
│ dân gửi qua App Vinhomes Resident → Route đến đúng ban quản  │
│ lý, tổ bảo trì tương ứng.                                    │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân (phản hồi mất 12 tiếng),               │
│              Ban Quản Lý (thủ công categorize 200+ tickets/tuần)│
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi complaint qua App Vinhomes Resident         │
│      ⏱ Instant (submissions)                               │
│   2. BQL nhận ticket → Đọc nội dung                          │
│      ⏱ 15 phút/ticket                                       │
│   3. Tự分类 category: nước/máy lạnh/ồn ào/cộng đồng/...     │
│      ⏱ 5 phút                                                │
│   4. Route đến đúng tổ/ban quản lý trong tòa nhà            │
│      ⏱ 3 phút                                                │
│   5. Draft reply → Gửi cư dân                                │
│      ⏱ 10 phút (soạn template-based)                        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                             │
│   Bước 3 + 5: Categorize + Draft reply — repetitive, dễ nhầm│
│   category (ví dụ "tiếng ồn" vs "công trình xây dựng"),     │
│   draft reply thiếu empathy → cư dân不满意 → escalation      │
│   ⏱ 15 phút/ticket, error rate ~30% on wrong routing        │
│                                                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 3: LLM auto-classify complaint → Category + Priority │
│   Bước 5: LLM draft personalized response → BQL approve     │
│                                                          │
│ Đo thành成功 bằng gì (Metric có số)?                        │
│   • Giảm mean response time từ 12 tiếng → under 4 tiếng     │
│   • Tăng accuracy complaint classification từ 70% → 95%    │
│   • Reduce human review time per ticket from 15min → 3min   │
│                                                          │
│ Quick Architecture: [ ] No AI  [x] LLM Feature  [ ] Agent   │
└─────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn của nhóm

## Bài toán được chọn cho DEEP-DIVE: **Card #1 — Xanh SM Xử lý sự cố sạc pin thực địa**

### Lý do lựa chọn:
- **Tác động doanh thu cao nhất:** 80 sự cố/ngày × 15 phút = 20 giờ lãng phí mỗi ngày, trực tiếp ảnh hưởng đến khả năng đón khách của tài xế (~15% revenue leakage từ xe không hoạt động)
- **Rủi ro an toàn:** Pin yếu + đề xuất trạm xa = xe hết giữa đường → nguy hiểm cho tài xế
- **Dữ liệu sẵn có:** Có sẵn API định vị xe, API trạm sạc VinFast → không cần gom dữ liệu mới
- **Scope nhỏ, impact lớn:** Giải pháp LLM Feature đơn giản nhưng giảm 80% thời gian xử lý

### Lý do loại trừ các thẻ khác:
- **Card #2 (VinFast Symptom Coding):** Cần nhiều data training để achieve high accuracy (>95%), rủi ro misdiagnosis liên quan sức khỏe — cần PoC dài hạn hơn.
- **Card #3 (Vinhomes Complaint Routing):** Đã có rule-based classifier cơ bản đang hoạt động, AI upgrade sẽ incremental gain (~20% improvement) — chưa đủ compelling để invest ngay.

---

## 🏁 AI Readiness Checklist

1. ✅ **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
   - Có: Log cuộc gọi sự cố 6 tháng gần nhất, historical station availability data, GPS trajectory logs từ 500+ xeVF5/VFe34
2. ✅ **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   - Có: Mọi output đều qua `[DRAFT_ONLY]` tag → điều phối viên phê duyệt trước khi gửi. Fallback: Dispatcher rewrite thủ công như cũ.
3. ✅ **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
   - Có: Team điều vận Xanh SM đã phàn nàn về overwork vào giờ cao điểm → rất ủng hộ solution giúp giảm 70% manual work.

## Quyết định cuối cùng: **[ ] GO (Bắt đầu xây dựng Prototype)**

**Justification:** Bài toán cụ thể, có metric rõ ràng (15min → 3min), giải pháp công nghệ đơn giản (LLM Feature), ranh giới an toàn được kiểm soát chặt chẽ qua [DRAFT_ONLY] tag + HITL. ROI cao: 20 giờ/lãng phí/ngày được khôi phục, giảm 15% revenue leakage.
