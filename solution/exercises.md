# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)
> *temperature thấp thì phản hồi ổn định, dễ đoán; càng cao thì càng đa dạng nhưng dễ lan man hoặc kém mạch lạc. Thường khoảng 1.2–1.8 bắt đầu thấy kém nhất quán rõ hơn.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
> *Contract/legal: 0.0 đến 0.2 để chính xác, ít sáng tạo. Slogan quảng cáo: khoảng 0.8 đến 1.2 để sáng tạo hơn.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
> *
Mỗi ngày có 20,000 người dùng × 2 lần gọi × 500 token đầu ra = 20,000,000 token output/ngày.
Model lớn gpt-4o: 20,000,000 / 1000 × 0.010 = khoảng 200 USD/ngày.
Model nhỏ gpt-4o-mini: 20,000,000 / 1000 × 0.0006 = khoảng 12 USD/ngày.
Model lớn xứng đáng khi cần độ chính xác cao, ví dụ tư vấn pháp lý, y tế, hoặc bài toán phức tạp.
Model nhỏ phù hợp cho FAQ, tóm tắt ngắn, chatbot nội bộ, hoặc tác vụ cần tiết kiệm chi phí.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:
- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)
> *Prompt “nhà thơ” thường cho câu trả lời giàu hình ảnh, mềm mại, ít thuật ngữ và dễ hiểu hơn.
Prompt “kỹ sư phần mềm senior” cho câu trả lời chính xác hơn, có cấu trúc rõ, và kỹ thuật hơn.
System prompt điều khiển được giọng văn, mức độ chi tiết, độ kỹ thuật và kiểu ví dụ được dùng.
*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
> *Với một đoạn văn mẫu 146 từ, ước lượng theo số từ / 0.75 là khoảng 194.67 token, còn count_tokens cho ra 180 token.
Chênh lệch khoảng 7.5%.
Nếu chỉ dùng ước lượng thô, mình sẽ có xu hướng dự toán thừa trong ví dụ này; với tiếng Việt, tách token không khớp trực tiếp với số từ nên đếm thật bằng tiktoken vẫn đáng tin hơn.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
> *Chatbot văn bản và trợ lý giọng nói hưởng lợi nhiều nhất từ streaming vì người dùng thấy phản hồi ngay, giảm cảm giác chờ.
Trợ lý giọng nói còn cần streaming để đọc từng phần trả lời theo thời gian thực. Pipeline dịch tài liệu chạy ngầm ban đêm không cần streaming nhiều vì không có người dùng theo dõi tức thì.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
> *Exponential backoff giúp các client không retry cùng lúc, giảm áp lực lên server khi hệ thống đang quá tải.
So với delay cố định, nó tránh tình trạng hàng nghìn request cùng dội lại theo nhịp giống nhau. Jitter thêm độ trễ ngẫu nhiên để các client không đồng loạt retry cùng lúc, giảm “thundering herd”.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
> *Prompt mẫu: “Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt.”
Nếu bỏ “thân thiện”, giọng trợ lý sẽ khô và ít gần gũi hơn.
Nếu bỏ “trả lời ngắn gọn”, câu trả lời sẽ dài hơn và có thể lan man hơn.
Nếu bỏ “bằng tiếng Việt”, trợ lý có thể trả lời sang tiếng Anh hoặc pha ngôn ngữ.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
> *Một tình huống dễ sai là người dùng hỏi nhiều lượt liên tiếp, rồi đến lượt thứ 5 hỏi lại một điều đã nhắc từ đầu như tên biến, yêu cầu format, hoặc mục tiêu persona. Vì chỉ giữ 4 lượt cuối, trợ lý có thể quên chi tiết ban đầu và trả lời lệch ngữ cảnh. Cách khắc phục tốt là tóm tắt các lượt cũ thành một summary ngắn, hoặc lưu riêng các thông tin quan trọng như persona, ràng buộc, và quyết định chính.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
