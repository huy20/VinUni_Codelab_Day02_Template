# AI Log & Reflection — Lab 02: AI Product Scoping

**Tên:** [Tên thành viên]  
**Nhóm:** VinSmart Future — AI Product Scoping Team  
**Ngày:** September 2026  

---

## 📝 Phần 1: AI đã giúp gì? (What Helped)

### 1. Brainstorming Problem List (Phase 1 SCAN)

Khi bắt đầu Phase 1, tôi chưa biết nên tập trung vào subsidiary nào. Tôi đã dùng AI để brainstorm với prompt:

> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng Xanh SM. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

**Kết quả hữu ích:**
- AI gợi ý được 5 problem areas rất sát thực tế vận hành Xanh SM, bao gồm cả case "xử lý sự cố sạc pin" — cái mà nhóm sau đó chọn làm deep-dive
- AI đưa ra được các con số estimate hợp lý: "~80 sự cố/ngày", "~20 giờ lãng phí mỗi ngày" — những con số này tôi cross-check với dữ liệu từ anh điều phối viên từng train, khá khớp
- Giúp áp dụng đúng 4 lenses (Repetitive, Time-consuming, AI-upgrade, Stakeholder Pain) cho từng problem

### 2. Stress-Testing Quick Cards (Phase 2)

Tôi dùng AI đóng vai CFO + Trưởng phòng Vận hành để stress-test card bài toán:

> *"Đây là một thẻ bài toán vận hành đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành khắt khe, chỉ ra 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết tốt hơn AI."*

**Phản hồi giá trị:**
- AI chỉ ra rằng metric "giảm từ 15 phút → dưới 3 phút" cần baseline rõ ràng — hiện tại chưa có data đo thời gian xử lý hiện tại chính xác
- Gợi ý thêm competitive analysis: so sánh với cách VinFast đang solve类似问题 ở trạm sạc
- Challenge đáng giá: "Tại sao không dùng Rule-based router thay LLM? Rule-based rẻ hơn, predictable hơn cho vấn đề định tuyến đơn giản này"

### 3. Draft Workflow Diagrams (Phase 3 DEEP-DIVE)

AI hỗ trợ vẽ ASCII workflow diagrams cho cả current-state và future-state flows. Khi tôi mô tả quy trình 5 bước, AI tự động tạo sơ đồ với ký hiệu bottleneck (🔴), handoff (🔄), đánh dấu AI steps (🔵) và human steps (🟢) — tiết kiệm khoảng 30 phút so với vẽ tay.

---

## ⚠️ Phần 2: AI sai ở đâu? (Where AI Was Wrong)

### 1. Gợi ý quá phức tạp — Multi-Agent Systems khi không cần thiết

**Tình huống:** Khi hỏi AI về architecture options, nó liên tục gợi ý multi-agent systems ("nên dùng 2 agents: one for GPS retrieval, one for station matching, one for message generation").

**Tại sao sai:** Trong bối cảnh lab này, solution cần đơn giản và có boundary rõ ràng. Multi-agent adds complexity không cần thiết, tăng latency, khó debug, và quan trọng nhất — **tăng rủi ro an toàn** khi mỗi agent có thể hoạt động semi-autonomously.

**Cách tôi sửa:** Refine prompt để explicitly yêu cầu: *"Hãy phân tích tại sao LLM Feature (không phải Agent) phù hợp hơn cho use case này, với constraint là cần strict HITL boundaries."* Khi đó AI mới đưa ra phân tích AI-Fit matrix đúng đắn.

### 2. Đưa ra metrics thiếu cơ sở

**Tình huống:** AI đề xuất metric "tăng satisfaction score từ 75% lên 95%" nhưng không có baseline survey nào được thực hiện trước đó. Con số này hoàn toàn invent.

**Tại sao sai:** Metric trong AI product scoping cần dựa trên observable data, không phải target mong muốn. Một metric "vô hình dung" thì không thể đo success/failure.

**Cách tôi sửa:** Cross-check lại với dữ liệu thực tế của Xanh SM: số incident logs, average handling time measurement từ hệ thống ticketing — từ đó derive metric thực sự measurable như "giảm từ 15min → 3min based on observed manual process timing".

### 3. Không nhận ra operational constraints địa phương

**Tình huống:** AI đề xuất các station recommendations không考虑到 fact that nhiều trạm sạc VinFast ở ngoại thành Hà Nội đã full capacity vào giờ cao điểm. Model gợi ý station cách xa nhưng không check availability real-time.

**Tại sao sai:** Solution design cần grounded in reality của operations environment, không chỉ dựa trên API hypothetical.

**Cách tôi sửa:** Thêm constraint vào SYSTEM_PROMPT: station recommendation chỉ valid nếu có ≥ 2 trụ trống AND distance ≤ max_safe_distance cho battery level hiện tại. Đồng thời thêm fallback mechanism khi tất cả stations trong radius đều full.

---

## 🔧 Phần 3: Cách tôi khắc phục (How I Corrected)

### 1. Iterative Prompt Refinement

Thay vì accept kết quả AI lần đầu tiên, tôi áp dụng pattern **refinement loop**:
- First pass: AI output → identify weakness
- Second pass: Add specific constraints/context to prompt
- Third pass: Verify against adversarial scenarios

Ví dụ điển hình: System prompt ban đầu chỉ nói "don't recommend far station when low battery". Sau stress-test với adversarial input (social engineering attempt), tôi refine thành: *"ABSOLUTE SAFETY RULE — CRITICAL THRESHOLD (<5% PIN): If battery < 5%, do NOT recommend ANY station beyond 5km..."* — enforce-level từ "should" sang "TUYỆT ĐỐI KHÔNG" để model hiểu mức độ nghiêm trọng.

### 2. Cross-Verification with Real Operations

Mỗi con số, mỗi step timing trong deep-dive report đều được cross-verify với ít nhất một nguồn:
- Thời gian per-step: tham khảo từ interview với dispatcher tại trung tâm Hà Nội
- Số lượng incidents/day: đối chiếu với GSM monthly ops report公开 data
- Technical constraints: check official VinFast Charging API documentation cho station types

### 3. Simplification Discipline

Khi AI push toward complex solutions, tôi giữ nguyên principle: **"Problem First, AI Second"** — được nhấn mạnh trong Inspiration Kit:
> *Đừng cố tìm bài toán phức tạp chỉ để dùng "Multi-Agent". Một giải pháp rule-based hoặc LLM feature đơn giản mang lại giá trị cao luôn được điểm tối đa.*

Nguyên tắc này giúp group stay focused: LLM Feature cho Zinc SM battery dispatcher, không phải agentic system phức tạp.

---

## 🎯 Phần 4: Bài học rút ra (Key Takeaways)

### 1. Prompt Engineering = Boundary Design

Học được rằng viết system prompt không chỉ là "describe what the model should do" — mà quan trọng hơn là **"define what it must NOT do."** Operational boundaries ([DRAFT_ONLY], <5% threshold) là thứ quyết định safety của整个 solution. Good prompts are defensive by design.

### 2. AI là Thought Partner, Không phải Oracle

AI có thể generate ideas rất nhanh, nhưng cần luôn cross-check:
- Metrics cần evidence-base, không phải intuition
- Solutions cần scoped to complexity of problem, not trendiness
- Assumptions need validation before being treated as facts

AI accelerates exploration but doesn't replace judgment.

### 3. Simple > Complex trong Production AI

The biggest lesson: trong thực tế sản xuất, **đơn giản và có boundary rõ ràng luôn thắng complexity.** Một LLM Feature solution với strict HITL layers, [DRAFT_ONLY] enforcement, và conditional routing cho critical thresholds sẽ reliably hơn một multi-agent system dù "smart" hơn về mặt academic. ROI matters more than architectural elegance.
