---
transition: slide-left
---

# Tổng thể Workflow Kiến trúc Transformer

Kiến trúc Encoder-Decoder loại bỏ hoàn toàn phép đệ quy, xếp chồng $N$ tầng đồng nhất ở mỗi nhánh với cấu hình tiêu chuẩn $N=6, d_{model}=512$ theo Vaswani et al., 2017:

<div class="grid grid-cols-3 gap-4 mt-2 items-center">
<div class="col-span-1">
<ArchitectureMinimap active="all" />
</div>

<div class="col-span-2 space-y-2.5 text-[11px]">
<div class="p-2.5 rounded-lg border border-blue-500/25 bg-blue-500/5">

<div class="font-semibold text-xs text-blue-700 dark:text-blue-300 mb-1 [&>p]:!m-0">

1\. Luồng xử lý tại Khối Encoder qua $N = 6$ tầng đồng nhất

</div>

* Tiếp nhận tổng của Token Embedding và Positional Encoding, xử lý song song toàn bộ chuỗi nguồn qua hai Sub-layer: **Multi-Head Self-Attention** và **Position-wise Feed-Forward Network**.
* Trích xuất ma trận biểu diễn ngữ cảnh hai chiều làm nguồn thông tin cho Decoder.

</div>

<div class="p-2.5 rounded-lg border border-purple-500/25 bg-purple-500/5">

<div class="font-semibold text-xs text-purple-700 dark:text-purple-300 mb-1 [&>p]:!m-0">

2\. Luồng xử lý tại Khối Decoder qua $N = 6$ tầng đồng nhất

</div>

* **Masked Multi-Head Attention:** Che các token tương lai nhằm bảo toàn tính nhân quả.
* **Cross-Attention:** Lấy Query từ tầng Decoder phía dưới, kết hợp với Key và Value xuất ra từ khối Encoder.
* **Feed-Forward & Đầu ra:** Qua mạng truyền thẳng, tầng Linear và hàm Softmax để dự đoán token kế tiếp.

</div>

<div class="p-2 rounded-lg bg-emerald-500/10 border border-emerald-500/30 text-emerald-800 dark:text-emerald-300 font-mono text-[10px] text-center">
Mọi Sub-layer đều qua Add & Norm: LayerNorm(x + Sublayer(x)) — Shape [B, T, d_model]
</div>
</div>
</div>

<!--
[Thời lượng: 1p | Phương pháp 1/5: Tổng thể Workflow]
- Mục tiêu vận hành: Theo dõi trọn vẹn đường đi của một tensor từ lúc vào nhánh Encoder bên trái cho tới khi xuất ra xác suất từ vựng ở đỉnh nhánh Decoder bên phải trên sơ đồ.
- Cơ chế sinh & Ánh xạ thực thể <-> Sơ đồ kiến trúc:
  1. Nhánh trái (Encoder): Toàn bộ n token của câu nguồn đi vào cùng một lúc dưới dạng ma trận [B, n, 512], được cộng thêm thông tin vị trí Pos Enc rồi đi xuyên qua 6 tầng liên tiếp. Tại mỗi tầng, khối Multi-Head Attention cho phép mọi từ trong câu nguồn đối chiếu trực tiếp với nhau để làm giàu ngữ cảnh, sau đó khối Feed-Forward biến đổi phi tuyến độc lập trên từng từ. Đầu ra ở đỉnh Encoder là bộ biểu diễn ngữ cảnh K, V có kích thước giữ nguyên [B, n, 512].
  2. Nhánh phải (Decoder): Nhận các token đích đã sinh ở các bước trước, đi qua Masked Attention (chỉ nhìn về quá khứ), rồi tiến lên khối Cross-Attention ở giữa. Tại đây, mũi tên xanh nối từ đỉnh Encoder sang chính là nơi Decoder dùng trạng thái hiện tại của mình làm câu truy vấn Query để rút trích thông tin K, V từ câu nguồn.
  3. Tính bất biến kích thước: Mọi khối con đều có đường nối tắt cộng trực tiếp đầu vào x với đầu ra Sublayer(x) rồi chuẩn hóa LayerNorm, giữ cố định hình dạng [B, T, 512] giúp xếp chồng 6 tầng mà không bị suy giảm đạo hàm.
- Khoảng trống & Cầu nối sang Trang 5: Ngay ở đáy sơ đồ, trước khi vào tầng đầu tiên, tại sao bắt buộc phải có vòng tròn dấu cộng Pos Enc? Điều gì xảy ra nếu đưa thẳng Token Embedding vào khối Self-Attention?
-->

---
transition: slide-left
---

# Thành phần Chính: Positional Encoding

<div class="grid grid-cols-2 gap-4 mt-2 items-center">
<div>

<InsightBox type="warning" title="Đặc tính Permutation Invariance">

Toán tử Self-Attention xử lý các phần tử đầu vào như một tập hợp không có thứ tự:

$$
\text{Attention}(P \cdot X) = P \cdot \text{Attention}(X)
$$

với mọi ma trận hoán vị $P$. Nếu không bổ sung thông tin vị trí, mô hình không thể phân biệt thứ tự từ trong câu.

</InsightBox>

<div class="mt-2.5 p-2.5 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50 text-[11px] text-gray-700 dark:text-gray-300">

Vector vị trí được xác định qua các hàm lượng giác có tần số biến thiên theo chiều đặc trưng $i$:

$$
PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right), \quad PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$

</div>

</div>

<div class="space-y-3">
<EmbeddingSum token="Word Embedding" tokenDim="d_model" pos="Positional Encoding" posDim="d_model"
note="Phép cộng từng phần tử bảo toàn kích thước không gian d_model, không làm tăng số lượng tham số của mô hình." />

<div class="p-2.5 rounded-lg border border-blue-500/20 bg-blue-500/5 text-[11px] text-gray-700 dark:text-gray-300 leading-relaxed">

**Tính chất dịch chuyển tuyến tính:** Với mọi khoảng cách cố định $k$, vector $PE_{pos+k}$ có thể biểu diễn dưới dạng một phép biến đổi tuyến tính của $PE_{pos}$, giúp mô hình học quan hệ vị trí tương đối giữa các token.

</div>
</div>
</div>

<!--
[Thời lượng: 1p | Phương pháp 2/5: Positional Encoding]
- Mục tiêu vận hành: Giải thích tại sao việc bỏ cấu trúc tuần tự làm mất thông tin thứ tự từ và cách mã hóa vị trí bằng hàm sin/cos khắc phục điều đó mà không tốn thêm tham số.
- Cơ chế sinh & Ánh xạ thực thể <-> Ký hiệu:
  1. Vấn đề bất biến hoán vị: Xét hai câu có nghĩa trái ngược nhau do đảo vị trí: "chó cắn người" và "người cắn chó". Vì Self-Attention chỉ tính tích vô hướng giữa mọi cặp vector độc lập với vị trí đứng, nếu ta đổi chỗ dòng 1 và dòng 3 của ma trận đầu vào X thì kết quả đầu ra của từng từ vẫn giữ nguyên giá trị, chỉ đổi chỗ dòng tương ứng <-> Attention(P * X) = P * Attention(X). Mô hình hoàn toàn mù về thứ tự từ.
  2. Cơ chế khắc phục: Tại mỗi vị trí pos (0, 1, 2,...), ta tạo một vector 512 chiều mà mỗi chiều chẵn 2i lấy giá trị sin và mỗi chiều lẻ 2i+1 lấy giá trị cos với bước sóng tăng dần từ 2*pi đến 10000 * 2*pi <-> Công thức PE_(pos, 2i) và PE_(pos, 2i+1).
  3. Tại sao dùng sin/cos và phép cộng: Nhờ công thức lượng giác sin(a+b) = sin(a)cos(b) + cos(a)sin(b), vector vị trí tại pos+k luôn biểu diễn được bằng một phép nhân ma trận cố định từ vector tại pos, giúp mô hình dễ dàng nhận biết "cách nhau k từ". Khi cộng trực tiếp vào Word Embedding 512 chiều, tổng thu được vẫn giữ nguyên kích thước 512 chiều thay vì phình lên 1024 chiều như phép ghép nối.
- Khoảng trống & Cầu nối sang Trang 6: Khi mỗi vector đầu vào X [B, T, 512] đã mang cả ngữ nghĩa lẫn vị trí, khối Self-Attention bên trong tính toán như thế nào để một từ rút trích được thông tin từ các từ liên quan?
-->

---
transition: slide-left
---

# Thành phần Chính: Scaled Dot-Product Attention

Chiếu tuyến tính đầu vào $X \in \mathbb{R}^{B \times T \times d_{model}}$ thành ba ma trận $Q = XW_Q$, $K = XW_K$, $V = XW_V$:

<div class="grid grid-cols-3 gap-2.5 mt-2">
<div class="p-2 rounded-lg border border-blue-500/30 bg-blue-500/10 text-[10.5px]">
<div class="flex items-center justify-between">
<span class="font-semibold text-blue-700 dark:text-blue-300">Query</span>
<TensorBadge shape="B × T × d_k" color="blue" />
</div>
<p class="text-gray-700 dark:text-gray-300 mt-1">Vector đại diện cho vị trí hiện tại đang tìm kiếm thông tin ngữ cảnh.</p>
</div>

<div class="p-2 rounded-lg border border-amber-500/30 bg-amber-500/10 text-[10.5px]">
<div class="flex items-center justify-between">
<span class="font-semibold text-amber-700 dark:text-amber-300">Key</span>
<TensorBadge shape="B × T × d_k" color="amber" />
</div>
<p class="text-gray-700 dark:text-gray-300 mt-1">Vector đặc trưng của từng vị trí dùng để đối sánh độ tương đồng với Query.</p>
</div>

<div class="p-2 rounded-lg border border-emerald-500/30 bg-emerald-500/10 text-[10.5px]">
<div class="flex items-center justify-between">
<span class="font-semibold text-emerald-700 dark:text-emerald-300">Value</span>
<TensorBadge shape="B × T × d_v" color="emerald" />
</div>
<p class="text-gray-700 dark:text-gray-300 mt-1">Vector mang nội dung ngữ nghĩa được tổng hợp theo trọng số phân phối.</p>
</div>
</div>

<div class="mt-2.5">
<MathCard title="Scaled Dot-Product Attention" badge="Core Formula" color="blue"
  intuition="Tích vô hướng QK^T đo độ tương đồng giữa mọi cặp token tạo ma trận [B, T, T], qua Softmax chuyển thành phân phối xác suất rồi nhân với Value."
  caveat="Khi d_k lớn, phương sai tích vô hướng tăng theo d_k làm Softmax rơi vào vùng bão hòa có đạo hàm tiến về 0. Chia cho căn bậc hai của d_k giúp chuẩn hóa phương sai về 1.">

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V
$$

</MathCard>
</div>

<!--
[Thời lượng: 1p | Phương pháp 3/5: Scaled Dot-Product Attention]
- Mục tiêu vận hành: Dẫn dắt từng phép toán bên trong công thức Attention và chứng minh lý do bắt buộc phải chia cho căn bậc hai của d_k.
- Cơ chế sinh & Ánh xạ thực thể <-> Ký hiệu:
  1. Tại sao phải tách thành 3 vector Q, K, V từ cùng một đầu vào X: Trong câu "The animal didn't cross the street because it was tired", khi xét từ "it", nhu cầu đi tìm danh từ mà nó thay thế (đóng vai trò Query) khác với đặc trưng nhận diện của bản thân nó khi từ khác tìm đến (đóng vai trò Key) và nội dung ngữ nghĩa nó đóng góp (đóng vai trò Value). Ba ma trận trọng số W_Q, W_K, W_V chiếu X vào ba vai trò riêng biệt này.
  2. Luồng tính toán 4 bước:
     - Bước 1: Lấy Q nhân với chuyển vị của K <-> Q K^T. Phép toán này lấy tích vô hướng của mọi cặp từ trong câu cùng lúc, sinh ra bảng điểm tương đồng kích thước [T, T]. Ví dụ dòng của từ "it" nhân với cột của từ "animal" cho điểm số cao.
     - Bước 2: Chia bảng điểm cho căn bậc hai của d_k. Tại sao? Giả sử mỗi phần tử trong q và k có trung bình bằng 0, phương sai bằng 1. Khi cộng dồn d_k = 64 tích lại, phương sai của tích vô hướng vọt lên bằng 64 (độ lệch chuẩn bằng 8), nghĩa là xuất hiện các số rất lớn như +20 hoặc -20. Khi đưa vào hàm mũ của Softmax, từ có điểm lớn nhất sẽ chiếm xấp xỉ 100% xác suất còn các từ khác về 0, làm đạo hàm triệt tiêu. Chia cho căn(64) = 8 kéo phương sai về lại đúng bằng 1.
     - Bước 3 & 4: Qua hàm Softmax để các điểm trên mỗi dòng có tổng bằng 1 (ví dụ "it" phân bổ 0.88 cho "animal", 0.05 cho "tired",...), rồi nhân với ma trận V để cộng gộp các vector Value theo đúng tỷ lệ đó.
- Khoảng trống & Cầu nối sang Trang 7: Một phép Scaled Dot-Product Attention đơn lẻ chỉ tạo ra một phân phối trọng số duy nhất cho mỗi từ. Nhưng nếu từ "it" vừa cần liên kết ngữ nghĩa với chủ ngữ "animal", vừa cần liên kết ngữ pháp với tính từ "tired" ở cuối câu thì sao? Và làm sao ngăn Decoder nhìn trộm từ tương lai khi nhân toàn bộ ma trận QK^T?
-->
