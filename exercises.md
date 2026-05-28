# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, phản hồi rất ổn định, nhất quán và có xu hướng lặp lại cùng một câu trả lời mỗi lần gọi — mô hình chọn token có xác suất cao nhất. Khi tăng temperature lên 0.5 và 1.0, phản hồi trở nên đa dạng và sáng tạo hơn, đưa ra các sự thật khác nhau với cách diễn đạt phong phú hơn. Ở temperature = 1.5, phản hồi bắt đầu trở nên lan man, đôi khi thiếu mạch lạc hoặc chứa thông tin không chính xác do mô hình chọn các token có xác suất thấp hơn.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature khoảng 0.0 đến 0.3 cho chatbot hỗ trợ khách hàng. Lý do là vì chatbot hỗ trợ khách hàng cần cung cấp thông tin chính xác, nhất quán và đáng tin cậy. Temperature thấp đảm bảo mô hình luôn chọn các câu trả lời có xác suất cao nhất, giảm thiểu rủi ro đưa ra thông tin sai lệch hoặc câu trả lời không liên quan — điều rất quan trọng khi xử lý các vấn đề của khách hàng.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> Tổng số lần gọi mỗi ngày: 10.000 × 3 = 30.000 lần. Mỗi lần ~350 token input và ~350 token output.
>
> **GPT-4o:** Chi phí = 30.000 × (350 × $5.00 + 350 × $20.00) / 1.000.000 = 30.000 × $0.00875 = **$262.50/ngày**
>
> **GPT-4o-mini:** Chi phí = 30.000 × (350 × $0.15 + 350 × $0.60) / 1.000.000 = 30.000 × $0.0002625 = **$7.875/ngày**
>
> GPT-4o đắt hơn GPT-4o-mini khoảng **33.3 lần** ($262.50 / $7.875 ≈ 33.3x).

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> **GPT-4o xứng đáng:** Trong các ứng dụng phân tích pháp lý hoặc y tế, nơi yêu cầu suy luận phức tạp, hiểu ngữ cảnh sâu và độ chính xác cao. Sai sót trong các lĩnh vực này có thể gây hậu quả nghiêm trọng, nên chất lượng phản hồi vượt trội của GPT-4o hoàn toàn xứng đáng với chi phí cao hơn.
>
> **GPT-4o-mini tốt hơn:** Trong các tác vụ đơn giản như phân loại email, tóm tắt nội dung ngắn, trả lời FAQ, hoặc chatbot hỗ trợ khách hàng cơ bản. Những tác vụ này không đòi hỏi suy luận phức tạp, và GPT-4o-mini có thể xử lý tốt với chi phí thấp hơn 33 lần — rất phù hợp cho các ứng dụng có lượng truy cập lớn.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng chatbot tương tác trực tiếp với người dùng, đặc biệt khi phản hồi dài (như viết bài, giải thích chi tiết, hoặc sinh code). Khi người dùng phải chờ 5-10 giây để nhận toàn bộ phản hồi, trải nghiệm sẽ rất tệ — streaming cho phép hiển thị từng phần ngay lập tức, tạo cảm giác mô hình đang "suy nghĩ và trả lời" giống như con người, giảm đáng kể perceived latency. Ngược lại, non-streaming phù hợp hơn trong các trường hợp xử lý hàng loạt (batch processing), pipeline tự động, hoặc khi cần toàn bộ phản hồi trước khi xử lý tiếp (ví dụ: phân tích JSON từ output, gọi API tiếp theo dựa trên kết quả). Non-streaming cũng đơn giản hơn về mặt triển khai và dễ xử lý lỗi hơn, phù hợp cho các tác vụ backend không cần tương tác real-time với người dùng.


## Danh Sách Kiểm Tra Nộp Bài
- [x] Tất cả tests pass: `pytest tests/ -v`
- [x] `call_openai` đã triển khai và kiểm thử
- [x] `call_openai_mini` đã triển khai và kiểm thử
- [x] `compare_models` đã triển khai và kiểm thử
- [x] `streaming_chatbot` đã triển khai và kiểm thử
- [x] `retry_with_backoff` đã triển khai và kiểm thử
- [x] `batch_compare` đã triển khai và kiểm thử
- [x] `format_comparison_table` đã triển khai và kiểm thử
- [x] `exercises.md` đã điền đầy đủ
- [x] Sao chép bài làm vào folder `solution` và đặt tên theo quy định
