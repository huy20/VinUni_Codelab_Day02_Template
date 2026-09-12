# Phase 6 — AI Log & Reflection

## 1. Tôi đã sử dụng AI như thế nào?

Trong bài lab này, tôi sử dụng AI như một thought-partner để đọc yêu cầu bài tập, làm rõ mối liên hệ giữa các phase và chuẩn hóa các ý tưởng ban đầu thành deliverable đúng cấu trúc.

Ban đầu, tôi ghi lại bốn ý tưởng ngắn về Vinmec trong `nhap.md`: quản lý hồ sơ bệnh án, quy trình khám bệnh, giải thích bệnh án cho người không có chuyên môn và theo dõi tình trạng bệnh nhân. Tôi yêu cầu AI bổ sung một vấn đề để đủ năm vấn đề và chọn ba vấn đề phù hợp nhất để viết Quick Problem Cards.

AI đã hỗ trợ tôi:

- Làm rõ rằng Phase 1 có thể khảo sát nhiều vấn đề, Phase 2 chọn ba vấn đề và Phase 3 mới chọn một vấn đề để phân tích sâu.
- Chuyển các ý tưởng còn rộng thành pain point có actor, workflow và bottleneck cụ thể hơn.
- Đề xuất vấn đề thứ năm: phân loại nhu cầu đặt lịch khám ban đầu từ mô tả triệu chứng của bệnh nhân.
- So sánh năm vấn đề và chọn ba bài toán có đầu vào ngôn ngữ tự nhiên phù hợp với LLM.
- Bổ sung metric, kiến trúc sơ bộ, Human-in-the-loop và operational boundary cho từng Quick Problem Card.
- Giải thích cách chuẩn hóa bản nháp thành các file mà autograder yêu cầu.

## 2. AI đã giúp ích ở điểm nào?

Điểm hữu ích nhất là AI giúp biến những cụm từ chung như “quy trình khám bệnh” thành một bài toán có thể đánh giá. Ví dụ, thay vì chỉ nói “giải thích bệnh án”, Quick Problem Card xác định rõ actor là bác sĩ, điều dưỡng và bệnh nhân; bottleneck là bước chọn lọc và diễn giải thuật ngữ chuyên môn; giải pháp là LLM tạo bản nháp để nhân viên y tế kiểm duyệt.

AI cũng giúp tôi nhận ra rằng trong lĩnh vực y tế, mục tiêu không chỉ là giảm thời gian. Hệ thống còn phải có ranh giới an toàn: không tự chẩn đoán, không thay đổi thuốc, không che giấu cảnh báo và không gửi kết quả khi chưa được người có chuyên môn phê duyệt.

## 3. AI có thể sai hoặc gây hiểu nhầm ở đâu?

AI đề xuất các con số như thời gian xử lý 10–15 phút, độ chính xác 95% hoặc recall cảnh báo 98%. Những con số này không đến từ dữ liệu vận hành thực tế của Vinmec. Nếu trình bày chúng như số liệu đã đo, nội dung sẽ thiếu trung thực và có thể trở thành hallucination.

AI cũng có thể mô tả quy trình bệnh viện theo giả định chung, trong khi quy trình thực tế có thể khác giữa chuyên khoa, loại xét nghiệm và cơ sở Vinmec. Ví dụ, vai trò của phòng xét nghiệm, bác sĩ chỉ định và điều dưỡng cần được xác nhận với người hiểu nghiệp vụ.

Ngoài ra, một LLM có thể diễn giải sai thuật ngữ, bỏ sót chi tiết quan trọng hoặc tạo thêm thông tin không tồn tại trong hồ sơ. Vì vậy, không thể xem kết quả của AI là tư vấn y tế chính thức.

## 4. Tôi đã sửa và thu hẹp kết quả của AI như thế nào?

Tôi giữ các con số dưới dạng **mục tiêu giả định phục vụ bài lab** và ghi rõ rằng chúng phải được kiểm chứng bằng dữ liệu nội bộ. Tôi không coi các số liệu này là thống kê thực tế của Vinmec.

Tôi thu hẹp vai trò của AI từ “giải thích hoặc tư vấn cho bệnh nhân” thành “tạo bản nháp hỗ trợ nhân viên y tế”. Với các trường hợp dữ liệu thiếu, mâu thuẫn, có dấu hiệu nguy cấp hoặc độ tin cậy thấp, hệ thống phải chuyển cho bác sĩ hoặc điều dưỡng thay vì tự xử lý.

Tôi cũng chọn kiến trúc kết hợp thay vì giao toàn bộ quyết định cho LLM:

- LLM xử lý và tóm tắt ngôn ngữ tự nhiên.
- Rule-based system kiểm tra các cảnh báo rõ ràng.
- Con người kiểm duyệt quyết định có ảnh hưởng đến bệnh nhân.

## 5. Bài học rút ra

AI hữu ích nhất ở giai đoạn brainstorm, phản biện và chuẩn hóa ý tưởng. Tuy nhiên, AI không thay thế kiến thức nghiệp vụ hoặc dữ liệu thực tế. Đặc biệt trong y tế, một ý tưởng chỉ có giá trị khi xác định rõ phạm vi, metric có thể kiểm chứng, dữ liệu được bảo vệ, cơ chế Human-in-the-loop và fallback an toàn.

Sau quá trình này, tôi hiểu rằng cần bắt đầu từ vấn đề vận hành rồi mới chọn công nghệ. Việc một bài toán có thể dùng LLM không có nghĩa là AI được phép tự đưa ra quyết định y tế.
