# Phase 1 — SCAN: Các bài toán tại Vinmec

> Các con số trong tài liệu này là mục tiêu giả định phục vụ bài lab, cần được kiểm chứng bằng dữ liệu vận hành thực tế trước khi triển khai.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Vinmec | Lặp lại | **Quản lý hồ sơ bệnh án:** Nhân viên phải tổng hợp và đối chiếu thông tin bệnh nhân từ nhiều nguồn như phiếu khám, kết quả xét nghiệm, đơn thuốc và ghi chú của bác sĩ. Việc nhập liệu lặp lại dễ gây thiếu hoặc trùng thông tin. |
| 2 | Vinmec | Tốn thời gian | **Quy trình tiếp nhận và khám bệnh:** Nhân viên phải kiểm tra thông tin, xác nhận lịch, hướng dẫn bệnh nhân và chuyển thông tin giữa các bộ phận. Các bước thủ công có thể làm tăng thời gian chờ và tạo điểm nghẽn vào giờ cao điểm. |
| 3 | Vinmec | AI có thể tốt hơn | **Giải thích trọng tâm bệnh án cho bệnh nhân:** Bệnh án và kết quả khám chứa nhiều thuật ngữ chuyên môn. Bác sĩ hoặc điều dưỡng mất thời gian diễn giải lại, trong khi bệnh nhân vẫn có thể hiểu sai hướng dẫn điều trị. |
| 4 | Vinmec | Pain từ người khác | **Theo dõi tình trạng bệnh nhân sau khám hoặc xuất viện:** Nhân viên phải gọi điện, đọc phản hồi và ghi nhận triệu chứng thủ công. Những dấu hiệu bất thường có nguy cơ được phát hiện muộn khi số lượng bệnh nhân lớn. |
| 5 | Vinmec | AI có thể tốt hơn | **Phân loại nhu cầu đặt lịch khám ban đầu:** Bệnh nhân thường mô tả triệu chứng bằng ngôn ngữ tự nhiên và không biết nên chọn chuyên khoa nào. Nhân viên phải hỏi lại nhiều lần trước khi chuyển yêu cầu đến đúng chuyên khoa. |

## Ba vấn đề được chọn để Quick-Assess

1. Giải thích trọng tâm bệnh án cho bệnh nhân.
2. Theo dõi tình trạng bệnh nhân sau khám hoặc xuất viện.
3. Phân loại nhu cầu đặt lịch khám ban đầu.

Ba vấn đề này có đầu vào ngôn ngữ tự nhiên phù hợp với LLM, có thể đo hiệu quả bằng thời gian và tỷ lệ xử lý chính xác, đồng thời có thể thiết kế bước con người phê duyệt để kiểm soát rủi ro y tế.

# Phase 2 — QUICK-ASSESS

## Quick Problem Card #1 — Giải thích trọng tâm bệnh án

**Bài toán (1 câu):** Nhân viên y tế mất nhiều thời gian chuyển nội dung bệnh án có thuật ngữ chuyên môn thành bản giải thích dễ hiểu cho bệnh nhân.

**Công ty thành viên:** Vinmec

**Ai đang đau (Actor)?** Bác sĩ, điều dưỡng và bệnh nhân/người nhà bệnh nhân.

**Workflow thủ công hiện tại:**

1. Phòng xét nghiệm hoàn tất ghi chú khám, kết quả xét nghiệm và chỉ định.
2. Bác sĩ chỉ định xét nghiệm (bác sĩ khám ban đầu) đọc lại toàn bộ hồ sơ liên quan.
3. Bác sĩ chọn các nội dung quan trọng và diễn giải bằng ngôn ngữ phổ thông.
4. Bệnh nhân hỏi lại những thuật ngữ hoặc hướng dẫn chưa hiểu.
5. Bác sĩ xác nhận và giải thích bổ sung.

**Bước tốn thời gian/lỗi nhất:** Chọn lọc và diễn giải thông tin chuyên môn ở bước 3; giả định mất khoảng 10–15 phút cho một hồ sơ.

**AI có thể hỗ trợ ở đâu?** LLM đọc nội dung đã được cho phép, tạo bản tóm tắt bằng ngôn ngữ dễ hiểu và đánh dấu những phần cần bác sĩ kiểm tra. AI chỉ tạo bản nháp, không đưa ra chẩn đoán mới và không thay đổi đơn thuốc.

**Metric thành công đề xuất:**

- Giảm thời gian tạo bản giải thích từ khoảng 10–15 phút xuống dưới 3 phút/hồ sơ.
- 100% bản tóm tắt phải được bác sĩ hoặc điều dưỡng phê duyệt trước khi gửi.
- Đạt ít nhất 95% thông tin trọng yếu đúng với hồ sơ nguồn trên bộ dữ liệu kiểm thử nội bộ.

**Quick Architecture:** LLM Feature kết hợp Human-in-the-loop.

**Operational Boundary:** Không tự chẩn đoán, không đề xuất hoặc thay đổi thuốc, không che giấu cảnh báo y tế và không gửi nội dung cho bệnh nhân khi chưa được nhân viên y tế duyệt.

---

## Quick Problem Card #2 — Theo dõi bệnh nhân sau khám hoặc xuất viện

**Bài toán (1 câu):** Việc đọc và phân loại phản hồi sau khám thủ công có thể khiến nhân viên y tế phát hiện chậm các dấu hiệu cần can thiệp sớm.

**Công ty thành viên:** Vinmec

**Ai đang đau (Actor)?** Điều dưỡng phụ trách theo dõi, bác sĩ điều trị và bệnh nhân sau khám/xuất viện.

**Workflow thủ công hiện tại:**

1. Hệ thống hoặc điều dưỡng gửi câu hỏi theo dõi cho bệnh nhân.
2. Bệnh nhân mô tả triệu chứng và tình trạng dùng thuốc.
3. Điều dưỡng đọc từng phản hồi và nhập thông tin vào hệ thống.
4. Điều dưỡng xác định mức ưu tiên và chuyển trường hợp đáng lo cho bác sĩ.
5. Bác sĩ xem lại và quyết định gọi bệnh nhân, hẹn khám hoặc hướng dẫn tiếp theo.

**Bước tốn thời gian/lỗi nhất:** Đọc, ghi nhận và phân loại phản hồi ở bước 3–4; giả định mất khoảng 5–8 phút cho mỗi phản hồi và dễ bị chậm khi lượng phản hồi tăng cao.

**AI có thể hỗ trợ ở đâu?** LLM trích xuất triệu chứng, thời điểm xuất hiện và mức độ được bệnh nhân mô tả; rule-based system nhận diện từ khóa nguy cấp để ưu tiên chuyển nhân viên y tế.

**Metric thành công đề xuất:**

- Ít nhất 90% phản hồi được phân loại trong vòng 1 phút.
- Recall đối với nhóm tình huống cảnh báo đạt ít nhất 98% trên bộ dữ liệu kiểm thử đã gắn nhãn.
- Giảm ít nhất 50% thời gian điều dưỡng dành cho việc đọc và nhập lại phản hồi.

**Quick Architecture:** LLM Feature kết hợp rule-based cảnh báo và Human-in-the-loop.

**Operational Boundary:** AI không tự kết luận tình trạng bệnh và không tự đưa ra hướng điều trị. Trường hợp có dấu hiệu nguy cấp, dữ liệu thiếu hoặc độ tin cậy thấp phải được chuyển ngay cho nhân viên y tế; hệ thống phải hiển thị hướng dẫn liên hệ cấp cứu theo quy trình đã được Vinmec phê duyệt.

---

## Quick Problem Card #3 — Phân loại nhu cầu đặt lịch khám ban đầu

**Bài toán (1 câu):** Nhân viên tư vấn mất thời gian hỏi lại khi bệnh nhân không biết chọn chuyên khoa phù hợp từ mô tả triệu chứng ban đầu.

**Công ty thành viên:** Vinmec

**Ai đang đau (Actor)?** Bệnh nhân và nhân viên tổng đài/đặt lịch.

**Workflow thủ công hiện tại:**

1. Bệnh nhân gửi yêu cầu đặt lịch và mô tả triệu chứng.
2. Nhân viên đọc nội dung và hỏi thêm thông tin còn thiếu.
3. Nhân viên tra cứu phạm vi tiếp nhận của các chuyên khoa.
4. Nhân viên đề xuất chuyên khoa và khung giờ phù hợp.
5. Bệnh nhân xác nhận để hoàn tất lịch hẹn.

**Bước tốn thời gian/lỗi nhất:** Khai thác thêm thông tin và lựa chọn chuyên khoa ở bước 2–3; giả định mất khoảng 5–10 phút cho một yêu cầu không rõ ràng.

**AI có thể hỗ trợ ở đâu?** LLM tóm tắt nhu cầu, hỏi các câu hỏi chuẩn hóa còn thiếu và đề xuất nhóm chuyên khoa để nhân viên xác nhận. Rule-based system ưu tiên các từ khóa cảnh báo khẩn cấp.

**Metric thành công đề xuất:**

- Giảm thời gian xử lý yêu cầu đặt lịch từ khoảng 5–10 phút xuống dưới 2 phút.
- Ít nhất 90% đề xuất chuyên khoa khớp với lựa chọn đã được nhân viên y tế xác nhận trên tập kiểm thử.
- 100% trường hợp có dấu hiệu cảnh báo được chuyển sang quy trình xử lý khẩn cấp thay vì tiếp tục đặt lịch thông thường.

**Quick Architecture:** LLM Feature kết hợp rule-based triage và Human-in-the-loop.

**Operational Boundary:** Đây chỉ là hỗ trợ điều hướng lịch khám, không phải chẩn đoán. AI không được khẳng định bệnh, kê thuốc hoặc trấn an rằng bệnh nhân an toàn. Nhân viên y tế phải xác nhận đề xuất; trường hợp khẩn cấp, mơ hồ hoặc ngoài phạm vi phải chuyển cho người phụ trách.
