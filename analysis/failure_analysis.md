# Failure Analysis — Lab 18: Production RAG

**Nhóm:** Cá nhân
**Thành viên:** Phạm Minh Hiếu (M1-M5)

---

## RAGAS Scores

| Metric | Naive Baseline | Production | Δ |
|--------|---------------|------------|---|
| Faithfulness | 0.0000 | 0.0000 | 0.0000 |
| Answer Relevancy | 0.0000 | 0.0000 | 0.0000 |
| Context Precision | 0.0000 | 0.0000 | 0.0000 |
| Context Recall | 0.0000 | 0.0000 | 0.0000 |

## Bottom-5 Failures

### #1
- **Question:** Nhân viên được nghỉ bao nhiêu ngày khi kết hôn?
- **Expected:** 3 ngày làm việc (nghỉ hưởng nguyên lương)
- **Got:** Trích từ các tài liệu hoặc "Không tìm thấy" do không có LLM generation API key.
- **Worst metric:** Faithfulness / Context Recall
- **Error Tree:** Output sai → do không có OPENAI_API_KEY để kích hoạt sinh câu trả lời bằng LLM, hệ thống sử dụng text của context đầu tiên làm câu trả lời thô.
- **Root cause:** Thiếu cấu hình `OPENAI_API_KEY` dẫn đến việc gọi API của OpenAI (gpt-4o-mini) thất bại, kích hoạt cơ chế fallback thô.
- **Suggested fix:** Thêm `OPENAI_API_KEY` hợp lệ vào file `.env`.

### #2
- **Question:** Bảo hiểm sức khỏe PVI có hạn mức bao nhiêu cho nhân viên?
- **Expected:** Xem chi tiết trong tài liệu PVI (tùy vị trí Senior/Junior).
- **Got:** Context thô.
- **Worst metric:** Answer Relevancy
- **Error Tree:** Output sai → do thiếu API key, không thực hiện được LLM summarization/generation.
- **Root cause:** Thiếu `OPENAI_API_KEY`.
- **Suggested fix:** Cấu hình API key trong `.env`.

### #3
- **Question:** Phụ cấp ăn trưa hàng tháng là bao nhiêu?
- **Expected:** Mức phụ cấp ăn trưa cụ thể trong chính sách lương.
- **Got:** Context thô.
- **Worst metric:** Faithfulness
- **Error Tree:** Output sai → do thiếu API key.
- **Root cause:** Thiếu `OPENAI_API_KEY`.
- **Suggested fix:** Cấu hình API key.

### #4
- **Question:** Mật khẩu phải có tối thiểu bao nhiêu ký tự?
- **Expected:** Tối thiểu 8 ký tự hoặc 12 ký tự tùy theo chính sách v1/v2.
- **Got:** Context thô.
- **Worst metric:** Context Precision
- **Error Tree:** Output sai → Do thiếu API key.
- **Root cause:** Thiếu `OPENAI_API_KEY`.
- **Suggested fix:** Cấu hình API key.

### #5
- **Question:** Có cần kích hoạt xác thực đa yếu tố (MFA) không?
- **Expected:** Bắt buộc kích hoạt MFA từ ngày hiệu lực chính sách v2.
- **Got:** Context thô.
- **Worst metric:** Context Recall
- **Error Tree:** Output sai → Do thiếu API key.
- **Root cause:** Thiếu `OPENAI_API_KEY`.
- **Suggested fix:** Cấu hình API key.

## Case Study (cho presentation)

**Question chọn phân tích:** Nhân viên được nghỉ bao nhiêu ngày phép năm?

**Error Tree walkthrough:**
1. Output đúng? → Sai (không ra câu trả lời gọn mà ra đoạn text thô/fallback).
2. Context đúng? → Đúng (đoạn text trích xuất từ `nghi_phep_nam_v2024.md` được chọn đầu tiên nhờ hybrid search & rerank).
3. Query rewrite OK? → Đúng.
4. Fix ở bước: Bổ sung `OPENAI_API_KEY` để gọi LLM sinh câu trả lời tự nhiên dựa trên context.

**Nếu có thêm 1 giờ, sẽ optimize:**
- Tối ưu hóa prompt template cho hybrid search và thiết lập cấu hình LLM cục bộ (ví dụ Ollama) làm fallback khi không có API key trực tuyến.
