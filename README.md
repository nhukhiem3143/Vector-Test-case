# 📢 SCRIPT THUYẾT TRÌNH – BGE-M3 TEST SUITE OPTIMIZATION
### Nguyễn Như Khiêm – K225480106030 | GVHD: TS. Nguyễn Tuấn Linh

---

## 🎯 SLIDE 1 – TRANG BÌA

> *"Kính thưa Thầy, em xin phép bắt đầu phần thuyết trình của mình.
> Đề tài của em thuộc môn Đảm bảo Chất lượng và Kiểm thử Phần mềm, với tiêu đề:
> **'Vector hóa tài liệu kiểm thử để tối ưu hóa Test Suite'** —
> ứng dụng mô hình BGE-M3 Embedding vào bài toán phân tích và tối ưu bộ kịch bản kiểm thử cho hệ thống đăng nhập người dùng."*

---

## 🗂️ SLIDE 2 – NỘI DUNG TRÌNH BÀY

> *"Phần thuyết trình của em được chia thành 7 phần chính.
> Em sẽ bắt đầu từ lý do chọn đề tài, sau đó đi vào cơ sở lý thuyết về vector hóa và Cosine Similarity,
> rồi giới thiệu mô hình BGE-M3, trình bày bài toán thực tế, pipeline 6 bước em xây dựng,
> phân tích kết quả từ 4 biểu đồ, và kết thúc bằng kết luận cùng định hướng mở rộng."*

---

## 🔴 SLIDE 3 – LÝ DO CHỌN ĐỀ TÀI

> *"Vấn đề xuất phát từ thực tế: trong các dự án phần mềm lớn, các bộ Test Suite được viết thủ công
> thường tích lũy rất nhiều Test Case trùng lặp về mặt ngữ nghĩa — nghĩa là hai test case kiểm tra
> cùng một tình huống nhưng dùng cách diễn đạt khác nhau, khiến mắt thường rất khó phát hiện.
>
> Điều này gây ra ba hậu quả: lãng phí tài nguyên thực thi, phân bố nhãn mất cân bằng,
> và thiếu liên kết rõ ràng giữa Test Case với Requirements.
>
> Giải pháp em đề xuất là dùng mô hình AI — cụ thể là BGE-M3 — để chuyển văn bản test case thành
> vector 1024 chiều và đo lường độ tương đồng ngữ nghĩa một cách tự động, chính xác.
>
> Hệ thống áp dụng là Login System — một bài toán xác thực người dùng điển hình,
> đủ phức tạp để kiểm chứng phương pháp này trong thực tế."*

---

## 📚 SLIDE 4 – CƠ SỞ LÝ THUYẾT

> *"Trước khi vào mô hình, em muốn điểm qua hành trình phát triển của các phương pháp vector hóa văn bản.
>
> Thế hệ đầu tiên là **Bag of Words và TF-IDF** — chỉ đơn giản đếm tần suất từ, không nắm được
> ngữ nghĩa ngữ cảnh. Ví dụ, 'đăng nhập thất bại' và 'xác thực không thành công' sẽ bị coi là
> hoàn toàn khác nhau.
>
> Thế hệ tiếp theo là **Word2Vec và Doc2Vec** — học từ ngữ cảnh lân cận, bắt đầu nắm được
> ngữ nghĩa cục bộ.
>
> **BERT** với cơ chế Self-Attention đã mang đến bước đột phá: hiểu ngữ nghĩa toàn câu theo
> ngữ cảnh hai chiều.
>
> Và đỉnh cao hiện tại là **BGE-M3** — đa ngôn ngữ, 1024 chiều, đặc biệt phù hợp với
> văn bản kỹ thuật chuyên ngành QA bằng tiếng Việt.
>
> Về phép đo, em sử dụng **Cosine Similarity** — đo góc giữa hai vector trong không gian,
> với giá trị từ -1 đến 1. Ngưỡng 0.85 trở lên được coi là hai Test Case tương đồng về ngữ nghĩa."*

---

## 🤖 SLIDE 5 – MÔ HÌNH BGE-M3

> *"BGE-M3 là viết tắt của BAAI/bge-m3, được phát triển bởi Beijing Academy of AI.
>
> Kiến trúc bên trong là XLM-RoBERTa kết hợp cơ chế Self-Attention — một họ mô hình
> transformer đa ngôn ngữ cực mạnh.
>
> Mỗi đoạn văn bản khi đưa vào mô hình sẽ được tokenize, đi qua 12 lớp Self-Attention,
> rồi pooling và normalize để cho ra một vector 1024 chiều — đã chuẩn hóa về độ dài 1.
>
> Lý do em chọn BGE-M3 thay vì các mô hình khác là vì nó hỗ trợ hơn 100 ngôn ngữ
> bao gồm tiếng Việt kỹ thuật, và đặc biệt phù hợp với văn bản chuyên ngành QA —
> nơi nhiều thuật ngữ rất đặc thù."*

---

## 🏢 SLIDE 6 – BÀI TOÁN THỰC TẾ

> *"Bài toán cụ thể: Một đội QA đang phát triển hệ thống xác thực người dùng.
> Bộ Test Suite hiện có 20 Test Case thủ công cần được phân tích tối ưu.
>
> Dữ liệu đầu vào gồm 8 Requirements — từ REQ-001 đến REQ-008 —
> bao phủ các nhóm chức năng: Authentication, Validation, Security, Recovery, Session và Audit.
>
> 20 Test Case được phân bố theo priority: 8 High, 7 Medium, và 5 Low.
> Về nhãn: 11 Test Case có nhãn 0 (Fail/Skip) và 9 Test Case nhãn 1 (Pass),
> cho thấy phân bố khá cân bằng."*

---

## ⚙️ SLIDE 7 – PIPELINE 6 BƯỚC

> *"Em xây dựng pipeline gồm 6 bước liên tiếp:
>
> **Bước 1** – Thu thập và tiền xử lý: đọc dữ liệu JSON, kết hợp title, description, steps,
> và expected result thành một chuỗi văn bản đầu vào duy nhất cho mỗi TC.
>
> **Bước 2** – Vector hóa BGE-M3: encode toàn bộ văn bản thành vector 1024 chiều
> với normalize bằng True để chuẩn hóa.
>
> **Bước 3** – Tính Cosine Similarity: xây dựng ma trận 20×20 thể hiện độ tương đồng
> giữa từng cặp Test Case.
>
> **Bước 4** – Traceability Matrix: ma trận 8×20 đánh giá mức độ bao phủ của từng
> Requirement theo ngưỡng 0.70.
>
> **Bước 5** – t-SNE Visualization: giảm chiều từ 1024D xuống 2D để trực quan hóa
> phân cụm ngữ nghĩa.
>
> **Bước 6** – Tối ưu Test Suite: đề xuất loại bỏ các TC similarity ≥ 0.90,
> xem xét hợp nhất các cặp từ 0.85 đến 0.90."*

---

## 💻 CHUYỂN SANG CODE – GIẢI THÍCH CÁC HÀM CHÍNH

> *"Bây giờ em xin chuyển sang phần demo code để Thầy thấy pipeline này được triển khai như thế nào."*

---

### 🔧 CELL 3 – Import & Cài đặt

> *"Cell đầu tiên là phần import thư viện. Các thư viện chính gồm:
> - `numpy` và `pandas` để xử lý ma trận và dữ liệu dạng bảng
> - `cosine_similarity` từ sklearn để tính độ tương đồng
> - `TSNE` để giảm chiều không gian vector
> - `seaborn` và `matplotlib` để vẽ biểu đồ
>
> Kết quả: hệ thống xác nhận import thành công với numpy 2.4.6 và pandas 3.0.3."*

---

### 🔧 CELL 5 – Tải dữ liệu

```python
with open('test-case.json', 'r', encoding='utf-8') as f:
    data = json.load(f)

requirements = data['requirements']
test_cases   = data['test_cases']
traceability = data['traceability_matrix']
```

> *"Hàm `json.load()` đọc file test-case.json vào bộ nhớ.
> Em tách ra 3 thành phần: requirements (8 yêu cầu), test_cases (20 kịch bản),
> và traceability_matrix (ánh xạ thủ công REQ ↔ TC).
>
> Output cho thấy: 8 Requirements và 20 Test Cases được tải thành công.
> Priority distribution: 8 High, 7 Medium, 5 Low.
> Label: 11 non-critical và 9 critical — tỷ lệ 9/11."*

---

### 🔧 CELL 7 – Tải mô hình BGE-M3

```python
from sentence_transformers import SentenceTransformer
model = SentenceTransformer('BAAI/bge-m3')
print(f"Kích thước embedding: {model.get_sentence_embedding_dimension()} chiều")
```

> *"Hàm `SentenceTransformer()` tải mô hình BGE-M3 từ HuggingFace về bộ nhớ.
> Phương thức `get_sentence_embedding_dimension()` trả về kích thước vector đầu ra.
> Kết quả xác nhận: **1024 chiều** — đúng như kiến trúc của mô hình."*

---

### 🔧 CELL 8 – Vector hóa Requirements & Test Cases

```python
req_texts = [f"[Yêu cầu] {r['title']}: {r['description']}" for r in requirements]
tc_texts  = [f"[Test Case] {t['title']}: {t['description']} Bước: {t['steps']} ..."
             for t in test_cases]

req_embeddings = model.encode(req_texts, normalize_embeddings=True, batch_size=8)
tc_embeddings  = model.encode(tc_texts,  normalize_embeddings=True, batch_size=8)
```

> *"Đây là bước quan trọng nhất. Em chuẩn bị văn bản đầu vào bằng cách ghép
> title + description + steps + expected result của mỗi TC thành một chuỗi dài,
> cho phép mô hình hiểu được ngữ cảnh đầy đủ.
>
> Hàm `model.encode()` chuyển từng chuỗi văn bản thành vector.
> Tham số `normalize_embeddings=True` đảm bảo độ dài vector là 1 — chuẩn hóa L2 —
> giúp Cosine Similarity hoạt động chính xác hơn.
> `batch_size=8` xử lý 8 văn bản cùng lúc để tối ưu bộ nhớ.
>
> Output: Requirements shape (8, 1024) và Test Cases shape (20, 1024) —
> tức là 8 vector cho 8 REQ, 20 vector cho 20 TC, mỗi vector 1024 chiều."*

---

### 🔧 CELL 10 – Ma trận Cosine Similarity (TC × TC)

```python
tc_sim_matrix = cosine_similarity(tc_embeddings)
tc_sim_matrix = np.clip(tc_sim_matrix, -1.0, 1.0)

sns.heatmap(tc_sim_matrix, annot=True, fmt='.2f', cmap='RdYlGn',
            vmin=0.3, vmax=1.0)
```

> *"Hàm `cosine_similarity()` tính ma trận 20×20, trong đó ô [i][j] là độ tương đồng
> giữa TC thứ i và TC thứ j.
> `np.clip()` đảm bảo giá trị luôn trong khoảng [-1, 1] do sai số tính toán số thực.
>
> Biểu đồ heatmap dùng màu xanh đậm cho similarity cao, màu cam trung bình,
> màu đỏ cho similarity thấp.
> Đường chéo chính luôn có giá trị 1.0 — mỗi TC tương đồng hoàn toàn với chính nó.
>
> Nhìn vào biểu đồ, ta thấy phần lớn các ô ngoài đường chéo có màu đỏ đến cam —
> cho thấy hầu hết TC là độc lập với nhau về mặt ngữ nghĩa.
> Chỉ có một số cụm nhỏ có màu xanh — đó là các TC cùng nhóm chức năng."*

---

### 🔧 CELL 11 – Phát hiện trùng lặp

```python
THRESHOLD_DUPLICATE = 0.85

for i in range(len(tc_ids)):
    for j in range(i+1, len(tc_ids)):
        sim = tc_sim_matrix[i][j]
        if sim >= THRESHOLD_DUPLICATE:
            duplicates.append({...})
```

> *"Thuật toán duyệt toàn bộ 190 cặp TC — tức C(20,2) — và ghi nhận các cặp có similarity từ 0.85 trở lên.
>
> **Kết quả thực tế từ output:**
> Phát hiện 4 cặp có nguy cơ trùng lặp:
>
> 1. TC-012 vs TC-013 — similarity **0.9003** — cùng về 2FA nhưng khác OTP đúng/sai
> 2. TC-003 vs TC-020 — similarity **0.8895** — khóa tài khoản vs mở khóa sau chờ
> 3. TC-006 vs TC-007 — similarity **0.8659** — đặt lại mật khẩu và link hết hạn
> 4. TC-001 vs TC-002 — similarity **0.8534** — đăng nhập thành công vs thất bại
>
> Điều thú vị là dù similarity cao, các TC này vẫn kiểm tra **hai kịch bản đối lập** —
> điều này cho thấy BGE-M3 nhận ra ngữ nghĩa cùng nhóm chức năng,
> nhưng tester vẫn cần xét thêm về nghiệp vụ trước khi quyết định loại bỏ."*

---

### 🔧 CELL 13 – Traceability Matrix (REQ × TC)

```python
req_tc_sim = cosine_similarity(req_embeddings, tc_embeddings)

sns.heatmap(req_tc_sim, cmap='Blues', vmin=0.3, vmax=1.0,
            annot=True, fmt='.2f')
```

> *"Hàm `cosine_similarity(req_embeddings, tc_embeddings)` tính ma trận 8×20 —
> mỗi ô thể hiện mức độ liên quan ngữ nghĩa giữa 1 Requirement và 1 Test Case.
>
> Biểu đồ dùng màu xanh dương — càng đậm càng liên quan nhiều.
>
> Kết quả từ CELL 14 với ngưỡng 0.70 cho thấy:
> - Tất cả 8/8 Requirements đều có ít nhất 1 TC bao phủ — **100% coverage**
> - BGE còn phát hiện thêm TC liên quan ngoài ma trận thủ công.
>   Ví dụ: REQ-001 thủ công có 3 TC nhưng BGE tìm thêm được 1 TC nữa (+1 extra).
>   REQ-004 Recovery cũng được phát hiện thêm 1 TC ngữ nghĩa liên quan mà QA bỏ sót."*

---

### 🔧 CELL 16 – t-SNE Visualization

```python
tsne = TSNE(n_components=2, random_state=42, perplexity=5,
            max_iter=1000, learning_rate='auto', init='pca')
embeddings_2d = tsne.fit_transform(all_embeddings)
```

> *"t-SNE là thuật toán giảm chiều không tuyến tính, giữ nguyên cấu trúc cụm cục bộ.
> Em đưa cả 28 điểm (8 REQ + 20 TC) vào cùng một không gian 2D để so sánh trực quan.
>
> Tham số `perplexity=5` phù hợp với dataset nhỏ (thường đặt khoảng sqrt(N)).
> `init='pca'` giúp khởi tạo ổn định hơn cho dataset nhỏ.
>
> Trên biểu đồ:
> - **Ngôi sao tím** là Requirements
> - **Điểm xanh lá** là TC Critical (label=1)
> - **Điểm đỏ** là TC Non-Critical (label=0)
>
> Nhận xét: TC nằm **gần ngôi sao REQ** tương ứng đang bao phủ tốt yêu cầu đó.
> Các cụm hình thành theo nhóm chức năng — Security TC tụ về một phía,
> Session TC về phía khác — chứng tỏ BGE-M3 nắm được ngữ nghĩa chuyên ngành rất tốt."*

---

### 🔧 CELL 18 – Tối ưu Test Suite

```python
# TC-013 bị đề xuất loại bỏ vì sim(TC-012, TC-013) = 0.9003 >= 0.90
to_remove.add(tc_ids[j])
```

> *"Bước cuối cùng: áp dụng hai ngưỡng để tối ưu.
>
> Ngưỡng loại bỏ 0.90: chỉ có cặp TC-012 và TC-013 vượt qua.
> TC-013 được đề xuất loại bỏ — giữ lại TC-012 vì nó là TC đứng trước.
>
> Ngưỡng xem xét hợp nhất 0.85–0.90: có 3 cặp.
> Đây là gợi ý để tester xem xét thêm — không tự động loại bỏ vì cần phán đoán nghiệp vụ.
>
> Thời gian embedding trước lọc: **1.065 giây** — sau lọc: **0.527 giây**,
> tương đương giảm 50% chi phí encode cho bộ test tối ưu."*

---

### 🔧 CELL 22 – Báo cáo tổng kết

> *"Output cuối cùng từ notebook là bảng tổng kết so sánh trước và sau tối ưu:
>
> | Tiêu chí | Trước | Sau | Thay đổi |
> |---|---|---|---|
> | Tổng Test Case | 20 | 19 | -1 TC |
> | TC cần loại bỏ | 1 | 0 | -1 TC |
> | Độ bao phủ | 8/8 | 8/8 | Không đổi |
> | Thời gian embed | 1.065s | 0.527s | -0.537s |
>
> 4 điểm nổi bật:
> ✅ Giảm 5% TC — tiết kiệm chi phí thực thi
> ✅ Duy trì 100% độ bao phủ Requirements
> ✅ Tự động phát hiện 4 cặp TC có nguy cơ dư thừa
> ✅ BGE-M3 nhận diện ngữ nghĩa chuyên ngành QA tiếng Việt chính xác"*

---

## 📊 SLIDE 15 – KẾT QUẢ SO SÁNH (Biểu đồ optimization_comparison.png)

> *"Biểu đồ so sánh 3 panel cho thấy toàn cảnh kết quả.
>
> Panel trái: Số lượng TC giảm từ 20 xuống 19 — giảm 5%.
> Panel giữa: Độ bao phủ Requirements vẫn giữ nguyên 100% — 8/8 — không bị ảnh hưởng.
> Panel phải: Thời gian thực thi giảm đáng kể khi bộ test được tinh gọn.
>
> Điều này chứng minh rằng pipeline BGE-M3 có thể cải thiện hiệu quả mà không đánh đổi chất lượng bao phủ."*

---

## 📑 SLIDE 16 – PHÂN TÍCH ĐỘ BAO PHỦ

> *"Nhìn chi tiết hơn vào ma trận truy vết:
> BGE-M3 không chỉ xác nhận 14 liên kết thủ công đã có, mà còn phát hiện thêm các liên kết tiềm năng.
> Ví dụ: REQ-001 ban đầu được liên kết thủ công với 3 TC, nhưng BGE tìm thêm được TC thứ 4
> có ngữ nghĩa liên quan — TC mà QA engineer đã bỏ sót trong quá trình ánh xạ thủ công."*

---

## ⚠️ SLIDE 17 – HẠN CHẾ & MỞ RỘNG

> *"Về hạn chế, em nhận thấy 4 điểm:
> Thứ nhất, dataset vẫn còn nhỏ — 20 TC và 8 REQ — cần kiểm chứng ở quy mô 200–1000 TC.
> Thứ hai, quyết định cuối cùng vẫn cần xem xét thủ công của QA engineer.
> Thứ ba, hệ thống chưa tự cập nhật real-time khi có TC mới.
> Thứ tư, mô hình 1024 chiều tiêu thụ bộ nhớ lớn, cần tối ưu cho môi trường sản xuất.
>
> Về định hướng mở rộng, em thấy 4 hướng tiềm năng:
> Tích hợp CI/CD với Jenkins hoặc GitHub Actions để tự động phân tích mỗi khi có Pull Request.
> Fine-tuning BGE-M3 trên corpus tiếng Việt chuyên ngành kiểm thử phần mềm.
> Áp dụng RAG để tự động sinh TC bổ sung cho vùng bao phủ còn thiếu.
> Và xây dựng benchmark chuẩn hóa để so sánh các mô hình embedding trong bài toán QA."*

---

## ✅ SLIDE 18 – KẾT LUẬN

> *"Tóm lại, đề tài đã đạt được 4 mục tiêu chính:
>
> Một: Áp dụng thành công BGE-M3 — một hướng ứng dụng AI/NLP mới trong lĩnh vực QA phần mềm.
>
> Hai: Tự động hóa phân tích 190 cặp TC trong chưa đến 2 giây —
> thay thế hoàn toàn quá trình kiểm tra thủ công.
>
> Ba: Xác nhận 8/8 yêu cầu được bao phủ 100%, không có TC dư thừa tuyệt đối —
> bộ test được thiết kế tốt.
>
> Bốn: Trực quan hóa không gian ngữ nghĩa bằng t-SNE giúp QA engineer
> có cái nhìn tổng thể về mối quan hệ giữa Requirements và Test Cases.
>
> Em xin kết thúc phần thuyết trình tại đây. Kính mời Thầy đặt câu hỏi ạ."*

---

## 🎓 SLIDE 19 – LỜI CẢM ƠN

> *"Em xin chân thành cảm ơn Thầy Nguyễn Tuấn Linh đã dành thời gian theo dõi và hướng dẫn em
> trong suốt quá trình thực hiện đề tài này.
> Em rất mong nhận được nhận xét của Thầy để em có thể hoàn thiện hơn.
> Xin kính chúc Thầy dồi dào sức khỏe và thành công!"*

---

## 💡 GỢI Ý KHI HỎI ĐÁP

**Câu hỏi có thể gặp:**

**Q: Tại sao chọn ngưỡng 0.85 thay vì 0.80 hay 0.90?**
> *"Ngưỡng 0.85 là mức cân bằng giữa độ chính xác và độ nhạy. Nếu giảm xuống 0.80,
> sẽ có nhiều cảnh báo false positive — các TC thực ra khác nhau nhưng bị đánh dấu trùng.
> Nếu tăng lên 0.90, ta sẽ bỏ sót nhiều trường hợp cần xem xét.
> 0.85 được chọn dựa trên thực nghiệm và phổ biến trong các nghiên cứu về duplicate detection."*

**Q: BGE-M3 khác gì so với BERT thông thường?**
> *"Điểm khác biệt chính là BGE-M3 hỗ trợ đa ngôn ngữ với 100+ ngôn ngữ bao gồm tiếng Việt,
> kích thước vector lớn hơn — 1024 so với 768 của BERT base —
> và được fine-tune đặc biệt cho bài toán semantic similarity và retrieval,
> phù hợp hơn với bài toán của em."*

**Q: Tại sao t-SNE mà không dùng PCA hay UMAP?**
> *"PCA là giảm chiều tuyến tính, không giữ được cấu trúc cụm cục bộ.
> t-SNE tốt hơn cho trực quan hóa vì nó giữ nguyên quan hệ láng giềng gần.
> UMAP cũng là lựa chọn tốt và nhanh hơn, nhưng t-SNE là lựa chọn chuẩn
> cho trực quan hóa embedding trong nghiên cứu NLP."*
