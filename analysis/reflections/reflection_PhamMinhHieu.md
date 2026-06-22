# Individual Reflection — Lab 18

**Tên:** Phạm Minh Hiếu  
**Module phụ trách:** M1, M2, M3, M4, M5

---

## 1. Đóng góp kỹ thuật

- Module đã implement: M1 (Chunking), M2 (Search), M3 (Rerank), M4 (Eval), M5 (Enrichment)
- Các hàm/class chính đã viết:
  - `chunk_semantic`, `chunk_hierarchical`, `chunk_structure_aware`
  - `segment_vietnamese`, `BM25Search`, `DenseSearch`, `reciprocal_rank_fusion`
  - `CrossEncoderReranker`
  - `evaluate_ragas`, `failure_analysis`
  - `summarize_chunk`, `generate_hypothesis_questions`, `contextual_prepend`, `extract_metadata`, `_enrich_single_call`
- Số tests pass: 37/37

## 2. Kiến thức học được

- Khái niệm mới nhất: Chunking phân cấp (Hierarchical parent-child) lưu trữ mối quan hệ cha-con và Reciprocal Rank Fusion (RRF) để kết hợp kết quả tìm kiếm từ nhiều nguồn.
- Điều bất ngờ nhất: Reranking đóng vai trò cực kỳ quan trọng trong việc lọc bớt nhiễu từ các văn bản không liên quan trước khi gửi tới LLM.
- Kết nối với bài giảng: Áp dụng trực tiếp các khái niệm về Hybrid Search, Reranking, RAGAS metrics từ slide bài giảng Lab 18.

## 3. Khó khăn & Cách giải quyết

- Khó khăn lớn nhất: Tải mô hình reranker quá lớn trong môi trường mạng yếu, dẫn đến test suite bị timeout.
- Cách giải quyết: Cấu hình sử dụng mô hình siêu nhẹ `cross-encoder/ms-marco-MiniLM-L-6-v2` chỉ khoảng 80MB. Đồng thời bổ sung fallback cho Qdrant in-memory khi Docker daemon không hoạt động.
- Thời gian debug: 15 phút.

## 4. Nếu làm lại

- Sẽ làm khác điều gì: Sử dụng các thư viện OCR tốt hơn để xử lý các file PDF scan ảnh bị bỏ qua trong thư mục `data/`.
- Module nào muốn thử tiếp: Module 5 (Enrichment) - muốn thử nghiệm tích hợp sâu hơn metadata tự động để tối ưu hóa bộ lọc tìm kiếm.

## 5. Tự đánh giá

| Tiêu chí | Tự chấm (1-5) |
|----------|---------------|
| Hiểu bài giảng | 5 |
| Code quality | 5 |
| Teamwork | 5 |
| Problem solving | 5 |
