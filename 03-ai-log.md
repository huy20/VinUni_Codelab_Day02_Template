# 03 — AI Log & Reflection (Vin Smart Future)

> **Nộp bài cá nhân — Nhật ký tương tác AI trong Lab 02.**
> Bài toán Deep-Dive được chọn: **VinFast — Báo cáo sức khỏe pin (SoH)**.
> Công cụ đã dùng: Trợ lý AI (LLM) làm thought-partner trong quá trình scan, scoping và viết prompt prototype.

---

## 1. Bối cảnh sử dụng AI

Trong buổi Lab, tôi dùng AI như một **thought-partner** ở ba giai đoạn: (1) brainstorm 5 pain point vận hành cho VinFast theo 4 lenses, (2) phản biện các thẻ bài toán để chọn ra bài toán Deep-Dive, và (3) hỗ trợ thiết kế ranh giới an toàn (operational boundary) cho prompt prototype.

---

## 2. AI đã giúp tôi những gì

* **Gợi ý bài toán nhanh:** AI đề xuất nhiều quy trình thủ công thực tế của VinFast (thẩm định bảo hành, **báo cáo SoH pin**, CSKH FAQ, tra cứu phụ tùng, hỗ trợ cạn pin thực địa) và gắn nhãn lens cho từng bài toán, giúp tôi không bị "trắng ý tưởng".
* **Phản biện đa góc nhìn:** Khi tôi yêu cầu đóng vai CFO và Trưởng phòng Vận hành khắt khe, AI chỉ ra rằng bài toán bảo hành (Card #1) có rủi ro tài chính – pháp lý cao, còn bài toán cứu hộ (Card #5) đòi hỏi tích hợp real-time 24/7; nhờ đó nhóm chọn **Card #2 — Báo cáo SoH** (dữ liệu có cấu trúc, AI chỉ draft, metric đo được ngay).
* **Gợi ý cấu trúc ranh giới an toàn:** AI giúp tôi hình dung tag `[DRAFT_ONLY]` (chống auto-send báo cáo cho khách) và ngưỡng cảnh báo pin nguy hiểm dựa trên SoH/độ lệch điện áp cell.
* **Tăng tốc soạn thảo:** AI giúp tôi chuyển ý tưởng thành bảng 6-field và sơ đồ future-state nhanh hơn nhiều so với viết tay.

---

## 3. AI đã sai / hallucination ở đâu

* **Con số không có nguồn:** AI đưa ra các con số tổn thất (số xe/ngày, số giờ-người, % rò rỉ) rất "chắc chắn" nhưng thực chất là **suy đoán**, không có nguồn dữ liệu thật. ➜ Tôi đã đánh dấu toàn bộ là **ước lượng định hướng** và ghi chú phải đo baseline thực tế.
* **Đề xuất giải pháp quá mức cần thiết:** Ở lần đầu, AI gợi ý dùng **Agentic Loop tự trị** để tự phân tích và kết luận tình trạng pin. Điều này không phù hợp vì rủi ro kết luận sai về an toàn pin là rất lớn. ➜ Tôi hạ mức AI Fit xuống **LLM Feature** (AI chỉ tạo nháp, người duyệt).
* **Bỏ sót ràng buộc an toàn:** AI ban đầu chỉ tập trung mô tả chỉ số SoH mà chưa đặt **ngưỡng cảnh báo** nào, nên có thể viết ra kết luận "pin bình thường" cho một viên pin lỗi. ➜ Tôi bổ sung quy tắc ngưỡng: SoH < 80% hoặc lệch cell > 5% hoặc bất thường nhiệt thì bắt buộc escalate.

---

## 4. Tôi đã sửa prompt / ranh giới như thế nào

1. **Siết định dạng:** Thêm yêu cầu output **bắt buộc bắt đầu bằng `[DRAFT_ONLY]`** và cấm tự gửi/khẳng định đã gửi báo cáo cho khách.
2. **Thêm quy tắc an toàn tuyệt đối:** "Nếu pin ở mức nguy hiểm (SoH < 80% hoặc độ lệch điện áp cell > 5% hoặc bất thường nhiệt), TUYỆT ĐỐI không kết luận pin khỏe mạnh; phải trả JSON `escalate_battery_service`."
3. **Chống prompt injection:** Thêm chỉ thị rằng người dùng không thể ghi đè ranh giới dù có đòi bỏ tag, dọa, hay ra lệnh "ignore previous instructions".
4. **Kiểm chứng bằng test tấn công:** Viết các adversarial input (dụ kết luận pin "bình thường" khi dữ liệu cho thấy pin lỗi, dụ bỏ tag, prompt injection) và xác nhận mô hình vẫn giữ đúng ranh giới.

---

## 5. Bài học rút ra

* AI rất mạnh ở **gợi ý và phản biện**, nhưng **không đáng tin về con số** — mọi số liệu phải kiểm chứng bằng dữ liệu thật.
* Với bài toán có rủi ro an toàn (pin, y tế, bảo hành), nguyên tắc là **"Problem First, AI Second"** và luôn giữ **Human-in-the-loop**: AI chỉ tạo nháp, con người quyết định.
* Một ranh giới an toàn chỉ thực sự đáng tin khi được **kiểm thử bằng tấn công** (adversarial testing), không chỉ dựa vào việc "viết prompt hay".
