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
- Ký hiệu Nx: Khung nét đứt biểu diễn 1 tầng đại diện, được xếp chồng lặp lại N = 6 lần liên tiếp.
- Encoder: Nhận toàn bộ câu nguồn cùng lúc, qua N tầng để xuất ra ma trận biểu diễn ngữ cảnh H_enc [B, n, d_model].
- Decoder: Hộp Masked Attention ở đáy chỉ nhìn các từ đích đã sinh; hộp Cross-Attention ở giữa lấy Q từ Decoder và nhân H_enc của Encoder với W_K, W_V để trích xuất K, V.
- Add & Norm: Mọi Sub-layer đều có đường nối tắt cộng đầu vào ngay trước nó với đầu ra rồi chuẩn hóa LayerNorm, giữ cố định kích thước [B, T, d_model].
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
- Vai trò W_Q, W_K, W_V: Embedding X chỉ đặt các từ đồng nghĩa gần nhau; chiếu qua W_Q, W_K giúp xoay những từ có liên hệ ngữ cảnh trong câu về góc nhỏ (cos lớn) trong không gian Q-K.
- Tích vô hướng QK^T: q_m · k_n^T = ||q_m|| ||k_n|| cos(θ) đo độ liên quan giữa mọi cặp từ [T × T]; góc càng nhỏ -> cos càng lớn -> qua Softmax cho Attention Weight càng cao.
- Chia √d_k: Tích vô hướng d_k chiều làm phương sai tăng lên d_k khiến Softmax bão hòa (đạo hàm -> 0); chia √d_k giữ phương sai ở mức 1.
-->

---
transition: slide-left
---

# Cơ chế Hoạt động của Scaled Dot-Product Attention

<div class="relative w-[860px] h-[365px] mx-auto mt-2 select-none font-mono text-xs">

<!-- Lớp SVG chứa toàn bộ các đường mũi tên gióng toạ độ chính xác -->
<svg width="860" height="365" viewBox="0 0 860 365" class="absolute inset-0 pointer-events-none overflow-visible">
<!-- 1. Mũi tên từ X (cạnh trái) vòng lên K^T: × W_K -->
<polyline points="40,288 14,288 14,42 114,42" fill="none" stroke="#d97706" stroke-width="1.8" />
<polygon points="122,42 113,38 113,46" fill="#d97706" />

<!-- 2. Mũi tên từ X (đỉnh) đi thẳng lên đáy Q: × W_Q -->
<line x1="72" y1="240" x2="72" y2="196" stroke="#2563eb" stroke-width="1.8" />
<polygon points="72,189 68,197 76,197" fill="#2563eb" />

<!-- 3. Mũi tên từ X (cạnh phải) đi ngang sang phải rồi chỉ thẳng lên đáy Ma trận V: × W_V -->
<polyline points="104,288 626,288 626,236" fill="none" stroke="#0891b2" stroke-width="1.8" />
<polygon points="626,229 622,237 630,237" fill="#0891b2" />

<!-- 4. Mũi tên giữa QK^T và Softmax -->
<line x1="252" y1="134" x2="314" y2="134" stroke="#6b7280" stroke-width="1.8" />
<polygon points="322,134 313,130 313,138" fill="#6b7280" />

<!-- 5. Mũi tên chẻ 3 nhánh từ Hàng 1 của Softmax (x=450, y=98) sang 3 ô cột Row 1 (x=504) -->
<line x1="452" y1="98" x2="496" y2="98" stroke="#2563eb" stroke-width="2" />
<polygon points="503,98 495,94 495,102" fill="#2563eb" />
<line x1="474" y1="98" x2="474" y2="210" stroke="#2563eb" stroke-width="2" />
<circle cx="474" cy="98" r="3" fill="#2563eb" />
<line x1="474" y1="154" x2="496" y2="154" stroke="#2563eb" stroke-width="2" />
<polygon points="503,154 495,150 495,158" fill="#2563eb" />
<line x1="474" y1="210" x2="496" y2="210" stroke="#2563eb" stroke-width="2" />
<polygon points="503,210 495,206 495,214" fill="#2563eb" />

<!-- 6. Đường gộp 3 hàng v_1, v_2, v_3 sang phải vào vector tổng hợp z_1 -->
<polyline points="688,98 708,98 708,210 688,210" fill="none" stroke="#4b5563" stroke-width="1.8" />
<line x1="688" y1="154" x2="726" y2="154" stroke="#4b5563" stroke-width="1.8" />
<polygon points="733,154 725,150 725,158" fill="#4b5563" />
</svg>

<!-- Nhãn trên 3 mũi tên chiếu W_Q, W_K, W_V -->
<div class="absolute left-[38px] top-[24px] px-1.5 bg-white dark:bg-[#121212] text-[10px] font-bold text-amber-600 dark:text-amber-400">
× W_K
</div>
<div class="absolute left-[78px] top-[207px] text-[10px] font-bold text-blue-600 dark:text-blue-400">
× W_Q
</div>
<div class="absolute left-[335px] top-[270px] px-1.5 bg-white dark:bg-[#121212] text-[10px] font-bold text-cyan-600 dark:text-cyan-400">
× W_V
</div>

<!-- Khối Đầu vào X (3 hàng ngang x_1, x_2, x_3) -->
<div class="absolute left-[40px] top-[240px] w-[64px] border border-gray-600 dark:border-gray-300 divide-y divide-gray-600 dark:divide-gray-300 bg-gray-500/10">
<div class="h-8 flex items-center justify-center font-bold text-gray-800 dark:text-gray-200">x₁</div>
<div class="h-8 flex items-center justify-center text-gray-600 dark:text-gray-400">x₂</div>
<div class="h-8 flex items-center justify-center text-gray-600 dark:text-gray-400">x₃</div>
</div>
<div class="absolute left-[28px] top-[340px] w-[88px] text-center text-[10px] font-sans text-gray-500">
Đầu vào X
</div>

<!-- Nhãn góc K^T / Q -->
<div class="absolute left-[88px] top-[46px] text-[10px] text-gray-400 text-right leading-tight">
<div>Kᵀ</div>
<div>Q</div>
</div>

<!-- Ma trận K^T: 3 cột dọc phía trên QK^T -->
<div class="absolute left-[124px] top-[20px] w-8 h-11 border border-amber-600 dark:border-amber-400 bg-amber-500/20 flex items-center justify-center font-bold text-amber-800 dark:text-amber-200">
k₁
</div>
<div class="absolute left-[164px] top-[20px] w-8 h-11 border border-amber-600 dark:border-amber-400 bg-amber-500/20 flex items-center justify-center font-bold text-amber-800 dark:text-amber-200">
k₂
</div>
<div class="absolute left-[204px] top-[20px] w-8 h-11 border border-amber-600 dark:border-amber-400 bg-amber-500/20 flex items-center justify-center font-bold text-amber-800 dark:text-amber-200">
k₃
</div>
<div class="absolute left-[124px] top-[64px] w-8 text-center text-[10px] text-gray-400">↓</div>
<div class="absolute left-[164px] top-[64px] w-8 text-center text-[10px] text-gray-400">↓</div>
<div class="absolute left-[204px] top-[64px] w-8 text-center text-[10px] text-gray-400">↓</div>

<!-- Ma trận Q: 3 hàng ngang bên trái QK^T -->
<div class="absolute left-[40px] top-[80px] w-[64px] border border-blue-600 dark:border-blue-400 divide-y divide-blue-600 dark:divide-blue-400">
<div class="h-9 bg-blue-500/30 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">q₁</div>
<div class="h-9 bg-blue-500/10 flex items-center justify-center text-blue-700/70 dark:text-blue-300/70">q₂</div>
<div class="h-9 bg-blue-500/10 flex items-center justify-center text-blue-700/70 dark:text-blue-300/70">q₃</div>
</div>
<div class="absolute left-[106px] top-[89px] text-[10px] text-gray-400">→</div>
<div class="absolute left-[106px] top-[125px] text-[10px] text-gray-400">→</div>
<div class="absolute left-[106px] top-[161px] text-[10px] text-gray-400">→</div>

<!-- Bảng lưới 3x3: QK^T -->
<div class="absolute left-[120px] top-[80px] border border-gray-500 dark:border-gray-400 divide-y divide-gray-500 dark:divide-gray-400">
<div class="grid grid-cols-3 divide-x divide-gray-500 dark:divide-gray-400">
<div class="w-10 h-9 bg-purple-600/85"></div>
<div class="w-10 h-9 bg-purple-500/50"></div>
<div class="w-10 h-9 bg-purple-400/20"></div>
</div>
<div class="grid grid-cols-3 divide-x divide-gray-500 dark:divide-gray-400">
<div class="w-10 h-9 bg-purple-500/35"></div>
<div class="w-10 h-9 bg-purple-600/70"></div>
<div class="w-10 h-9 bg-purple-400/25"></div>
</div>
<div class="grid grid-cols-3 divide-x divide-gray-500 dark:divide-gray-400">
<div class="w-10 h-9 bg-purple-400/20"></div>
<div class="w-10 h-9 bg-purple-500/40"></div>
<div class="w-10 h-9 bg-purple-600/75"></div>
</div>
</div>
<div class="absolute left-[120px] top-[194px] w-[120px] text-center text-[10px] font-sans text-gray-500">
Q · Kᵀ
</div>

<!-- Nhãn bước chia căn d_k và Softmax -->
<div class="absolute left-[252px] top-[112px] w-[70px] text-center text-[10px] font-sans font-semibold text-gray-600 dark:text-gray-300">
÷ √d_k
</div>
<div class="absolute left-[252px] top-[140px] w-[70px] text-center text-[10px] font-sans font-semibold text-blue-600 dark:text-blue-400">
Softmax
</div>

<!-- Bảng lưới 3x3: Softmax(QK^T / √d_k) -->
<div class="absolute left-[315px] top-[56px] w-[150px] text-center text-[10px] font-sans text-gray-500">
Softmax(QKᵀ / √d_k)
</div>
<div class="absolute left-[330px] top-[80px] border border-gray-500 dark:border-gray-400 divide-y divide-gray-500 dark:divide-gray-400">
<!-- Hàng 1 đóng khung đậm -->
<div class="grid grid-cols-3 divide-x-2 divide-blue-600 dark:divide-blue-400 ring-2 ring-blue-600 dark:ring-blue-400 relative z-10">
<div class="w-10 h-9 bg-blue-600"></div>
<div class="w-10 h-9 bg-blue-500/45"></div>
<div class="w-10 h-9 bg-blue-500/12"></div>
</div>
<!-- Hàng 2 -->
<div class="grid grid-cols-3 divide-x divide-gray-500 dark:divide-gray-400 opacity-40">
<div class="w-10 h-9 bg-blue-500/30"></div>
<div class="w-10 h-9 bg-blue-600/80"></div>
<div class="w-10 h-9 bg-blue-500/15"></div>
</div>
<!-- Hàng 3 -->
<div class="grid grid-cols-3 divide-x divide-gray-500 dark:divide-gray-400 opacity-40">
<div class="w-10 h-9 bg-blue-500/15"></div>
<div class="w-10 h-9 bg-blue-500/35"></div>
<div class="w-10 h-9 bg-blue-600/85"></div>
</div>
</div>

<!-- Cụm 3: Cột Row 1 × Ma trận V (3 hàng v_1, v_2, v_3) -->
<div class="absolute left-[500px] top-[56px] w-[44px] text-center text-[10px] font-sans text-gray-500">
Row 1
</div>
<div class="absolute left-[564px] top-[56px] w-[124px] text-center text-[10px] font-sans text-gray-500">
Ma trận V
</div>

<!-- Hàng 1: Ô 1 (Đậm) × v_1 -->
<div class="absolute left-[504px] top-[80px] w-9 h-9 border-2 border-blue-600 dark:border-blue-400 bg-blue-600"></div>
<div class="absolute left-[540px] top-[80px] w-6 h-9 flex items-center justify-center font-bold text-gray-500">×</div>
<div class="absolute left-[564px] top-[80px] w-[124px] h-9 border border-gray-600 dark:border-gray-300 bg-cyan-500 flex items-center justify-center font-bold text-white">
v₁
</div>

<div class="absolute left-[564px] top-[116px] w-[124px] h-5 flex items-center justify-center font-bold text-gray-500">+</div>

<!-- Hàng 2: Ô 2 (Vừa) × v_2 -->
<div class="absolute left-[504px] top-[136px] w-9 h-9 border-2 border-blue-600 dark:border-blue-400 bg-blue-500/45"></div>
<div class="absolute left-[540px] top-[136px] w-6 h-9 flex items-center justify-center font-bold text-gray-500">×</div>
<div class="absolute left-[564px] top-[136px] w-[124px] h-9 border border-gray-500 dark:border-gray-400 bg-amber-500/45 flex items-center justify-center font-bold text-gray-900 dark:text-white">
v₂
</div>

<div class="absolute left-[564px] top-[172px] w-[124px] h-5 flex items-center justify-center font-bold text-gray-500">+</div>

<!-- Hàng 3: Ô 3 (Nhạt) × v_3 -->
<div class="absolute left-[504px] top-[192px] w-9 h-9 border-2 border-blue-600 dark:border-blue-400 bg-blue-500/12"></div>
<div class="absolute left-[540px] top-[192px] w-6 h-9 flex items-center justify-center font-bold text-gray-500">×</div>
<div class="absolute left-[564px] top-[192px] w-[124px] h-9 border border-gray-400 dark:border-gray-600 bg-rose-500/15 flex items-center justify-center font-bold text-gray-500 dark:text-gray-400">
v₃
</div>

<!-- Vector tổng hợp z_1 đặt ở bên phải -->
<div class="absolute left-[734px] top-[114px] w-[118px] text-center text-[10px] font-sans font-semibold text-gray-600 dark:text-gray-300">
Vector tổng hợp
</div>
<div class="absolute left-[734px] top-[136px] w-[118px] h-9 border-2 border-gray-800 dark:border-white overflow-hidden flex items-center justify-center">
<div class="absolute inset-0 flex flex-col">
<div class="bg-cyan-500" style="height: 65%"></div>
<div class="bg-amber-500" style="height: 25%"></div>
<div class="bg-rose-500" style="height: 10%"></div>
</div>
<span class="relative z-10 font-bold text-white drop-shadow">z₁ (Q, K, V)</span>
</div>

</div>

<!--
- Chiếu tuyến tính (X -> Q, K, V): W_Q, W_K xoay các từ có quan hệ ngữ pháp/ngữ cảnh trong câu về gần nhau (góc nhỏ, cos lớn), thay vì chỉ gần nhau giữa các từ đồng nghĩa như ở tầng Embedding X.
- Nhân Q · K^T: Tích vô hướng q_i · k_j = ||q_i|| ||k_j|| cos(θ). Hai từ càng liên quan thì góc θ càng nhỏ -> cos(θ) lớn -> điểm tương đồng cao.
- Chia √d_k & Softmax: Chuẩn hóa phương sai về 1 rồi đưa qua hàm mũ Softmax để chuyển điểm tương đồng thành phân phối trọng số Attention Weight (từ liên quan nhận weight lớn).
- Nhân Ma trận V: Lấy trọng số hàng 1 nhân từng vector v_1, v_2, v_3 rồi cộng lại thành vector ngữ cảnh z_1.
-->

---
transition: slide-left
---

# Thành phần Chính: Positional Encoding

<div class="relative w-[860px] h-[370px] mx-auto mt-1 select-none font-mono text-xs">

<!-- Hàng 1: Công thức Toán học đặt trực tiếp ở đỉnh (Không dùng hộp bo tròn) -->
<div class="absolute left-[0px] top-[0px] w-[360px] text-center">
<div class="text-[10px] font-sans font-bold text-blue-700 dark:text-blue-300 uppercase tracking-wider mb-0.5">
1. Tích vô hướng Mù vị trí & Phép cộng PE
</div>
<div class="text-xs text-gray-800 dark:text-gray-200">

$Q K^T \implies Z_0 = X + PE$

</div>
</div>

<div class="absolute left-[370px] top-[0px] w-[490px] text-center">
<div class="text-[10px] font-sans font-bold text-amber-700 dark:text-amber-300 uppercase tracking-wider mb-0.5">
2. Hàm Lượng giác Đa tần số & Dịch chuyển Tuyến tính
</div>
<div class="text-xs text-gray-800 dark:text-gray-200">

$PE_{(pos, 2i)} = \sin\!\left(\frac{pos}{10000^{2i/d_{model}}}\right), \quad PE_{(pos, 2i+1)} = \cos\!\left(\frac{pos}{10000^{2i/d_{model}}}\right)$

</div>
</div>

<!-- Lớp SVG: Mũi tên chỉ dẫn, Đồ thị sóng đa tần số và Chu kỳ lặp lại -->
<svg width="860" height="370" viewBox="0 0 860 370" class="absolute inset-0 pointer-events-none overflow-visible">
<!-- Mũi tên ngang biểu diễn chiều tăng bước sóng trên đỉnh ma trận PE -->
<line x1="124" y1="74" x2="292" y2="74" stroke="#d97706" stroke-width="1.5" />
<polygon points="298,74 290,70 290,78" fill="#d97706" />

<!-- Mũi tên từ công thức (mẫu số 10000^(2i/d)) chỉ xuống khu vực giải phẫu bên phải -->
<polyline points="546,61 546,72 446,72 446,86" fill="none" stroke="#d97706" stroke-width="1.6" />
<polygon points="446,92 442,84 450,84" fill="#d97706" />

<!-- Khung đồ thị Sóng 1: i = 0 (Mẫu số = 1, tần số cao, chu kỳ ngắn 2π) -->
<line x1="530" y1="122" x2="848" y2="122" stroke="#9ca3af" stroke-width="1" stroke-dasharray="3 3" />
<path d="M 530 122 Q 545 96, 560 122 T 590 122 T 620 122 T 650 122 T 680 122 T 710 122 T 740 122 T 770 122 T 800 122 T 830 122" fill="none" stroke="#2563eb" stroke-width="2" />
<!-- Đánh dấu 1 chu kỳ 2π -->
<line x1="530" y1="96" x2="530" y2="144" stroke="#2563eb" stroke-width="1.2" />
<line x1="590" y1="96" x2="590" y2="144" stroke="#2563eb" stroke-width="1.2" />
<line x1="530" y1="98" x2="590" y2="98" stroke="#2563eb" stroke-width="1.2" />

<!-- Khung đồ thị Sóng 2: 2i → d_model (Mẫu số = Base, tần số thấp, chu kỳ cực dài 2π · Base) -->
<line x1="530" y1="186" x2="848" y2="186" stroke="#9ca3af" stroke-width="1" stroke-dasharray="3 3" />
<!-- 1 chu kỳ dài từ x=530 đến x=770, sau x=770 lặp lại chu kỳ gây trùng vị trí (màu đỏ) -->
<path d="M 530 186 Q 590 154, 650 186 T 770 186" fill="none" stroke="#d97706" stroke-width="2.2" />
<path d="M 770 186 Q 810 160, 848 180" fill="none" stroke="#e11d48" stroke-width="2.2" stroke-dasharray="4 2" />
<line x1="530" y1="158" x2="530" y2="210" stroke="#d97706" stroke-width="1.2" />
<line x1="770" y1="158" x2="770" y2="210" stroke="#e11d48" stroke-width="1.5" />
<line x1="530" y1="160" x2="770" y2="160" stroke="#d97706" stroke-width="1.2" />

<!-- Đường phân cách ngang giữa phần Đồ thị Sóng và phần Context Window Scaling -->
<line x1="434" y1="228" x2="856" y2="228" stroke="#d1d5db" stroke-width="1.2" />
</svg>

<!-- CỤM TRÁI: Ma trận X + Ma trận PE = Ma trận Đầu vào Z_0 (6 hàng pos = 0..5) -->
<!-- Nhãn trục bước sóng trên đỉnh PE -->
<div class="absolute left-[120px] top-[56px] w-[180px] flex justify-between text-[9px] font-sans font-semibold text-amber-700 dark:text-amber-300">
<span>i = 0 (÷1)</span>
<span>2i → d (÷10000)</span>
</div>

<!-- Header cột của PE: thể hiện rõ cặp sin/cos và tần số giảm dần -->
<div class="absolute left-[120px] top-[80px] w-[180px] grid grid-cols-4 text-center text-[8.5px] text-gray-500 font-mono">
<span>sin(i=0)</span>
<span>cos(i=0)</span>
<span>sin(i=k)</span>
<span>cos(→d)</span>
</div>

<!-- Cột chỉ số hàng pos = 0..5 -->
<div class="absolute left-[0px] top-[100px] w-[36px] divide-y divide-transparent text-[10px] text-gray-400 text-right pr-1.5">
<div class="h-9 flex items-center justify-end">pos=0</div>
<div class="h-9 flex items-center justify-end">pos=1</div>
<div class="h-9 flex items-center justify-end">pos=2</div>
<div class="h-9 flex items-center justify-end">pos=3</div>
<div class="h-9 flex items-center justify-end">pos=4</div>
<div class="h-9 flex items-center justify-end">pos=5</div>
</div>

<!-- Ma trận X (6 hàng ngang x_0..x_5) -->
<div class="absolute left-[38px] top-[100px] w-[54px] border border-blue-600 dark:border-blue-400 divide-y divide-blue-600 dark:divide-blue-400">
<div class="h-9 bg-blue-500/25 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">x₀</div>
<div class="h-9 bg-blue-500/20 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">x₁</div>
<div class="h-9 bg-blue-500/25 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">x₂</div>
<div class="h-9 bg-blue-500/15 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">x₃</div>
<div class="h-9 bg-blue-500/25 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">x₄</div>
<div class="h-9 bg-blue-500/20 flex items-center justify-center font-bold text-blue-900 dark:text-blue-100">x₅</div>
</div>
<div class="absolute left-[30px] top-[326px] w-[70px] text-center text-[10px] font-sans text-gray-500">
Token X [T×d]
</div>

<!-- Dấu + -->
<div class="absolute left-[96px] top-[196px] w-[20px] text-center text-base font-bold text-gray-500">+</div>

<!-- Ma trận PE [6 hàng pos=0..5 × 4 cột]: Độ đậm nhạt tính chính xác theo (sin + 1)/2 và (cos + 1)/2 -->
<!-- Cột 1: sin(pos)   -> 0.50 (sin 0 = 0), 0.92, 0.96 (đỉnh +1), 0.57 (qua 0), 0.12, 0.05 (đáy -1) -->
<!-- Cột 2: cos(pos)   -> 1.00 (cos 0 = 1), 0.77, 0.29 (qua 0), 0.05 (đáy -1), 0.18, 0.64 (đi lên) -->
<!-- Cột 3: sin(pos/2) -> 0.50 (sin 0 = 0), 0.74, 0.92, 1.00 (đỉnh uốn +1), 0.95 (vòng xuống), 0.80 -->
<!-- Cột 4: cos(pos/10000) -> 1.00 (cos 0 = 1), 0.99, 0.99, 0.98, 0.98, 0.97 (Gần như bất biến ở đầu câu) -->
<div class="absolute left-[120px] top-[100px] border border-amber-600 dark:border-amber-400 divide-y divide-amber-600/60 dark:divide-amber-400/60 bg-white dark:bg-gray-900">
<!-- pos = 0 -->
<div class="grid grid-cols-4 divide-x divide-amber-600/60 dark:divide-amber-400/60">
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.50"></div>
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 1.00"></div>
<div class="w-[45px] h-9 bg-amber-500" style="opacity: 0.50"></div>
<div class="w-[45px] h-9 bg-amber-600" style="opacity: 1.00"></div>
</div>
<!-- pos = 1 -->
<div class="grid grid-cols-4 divide-x divide-amber-600/60 dark:divide-amber-400/60">
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.92"></div>
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.77"></div>
<div class="w-[45px] h-9 bg-amber-500" style="opacity: 0.74"></div>
<div class="w-[45px] h-9 bg-amber-600" style="opacity: 0.99"></div>
</div>
<!-- pos = 2 -->
<div class="grid grid-cols-4 divide-x divide-amber-600/60 dark:divide-amber-400/60">
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.96"></div>
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.29"></div>
<div class="w-[45px] h-9 bg-amber-500" style="opacity: 0.92"></div>
<div class="w-[45px] h-9 bg-amber-600" style="opacity: 0.99"></div>
</div>
<!-- pos = 3 -->
<div class="grid grid-cols-4 divide-x divide-amber-600/60 dark:divide-amber-400/60">
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.57"></div>
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.05"></div>
<div class="w-[45px] h-9 bg-amber-500" style="opacity: 1.00"></div>
<div class="w-[45px] h-9 bg-amber-600" style="opacity: 0.98"></div>
</div>
<!-- pos = 4 -->
<div class="grid grid-cols-4 divide-x divide-amber-600/60 dark:divide-amber-400/60">
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.12"></div>
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.18"></div>
<div class="w-[45px] h-9 bg-amber-500" style="opacity: 0.95"></div>
<div class="w-[45px] h-9 bg-amber-600" style="opacity: 0.98"></div>
</div>
<!-- pos = 5 -->
<div class="grid grid-cols-4 divide-x divide-amber-600/60 dark:divide-amber-400/60">
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.05"></div>
<div class="w-[45px] h-9 bg-blue-600" style="opacity: 0.64"></div>
<div class="w-[45px] h-9 bg-amber-500" style="opacity: 0.80"></div>
<div class="w-[45px] h-9 bg-amber-600" style="opacity: 0.97"></div>
</div>
</div>
<div class="absolute left-[120px] top-[326px] w-[180px] text-center text-[10px] font-sans text-gray-500">
PE [T×d]  (PE<sub>pos+k</sub> = M<sub>k</sub> · PE<sub>pos</sub>)
</div>

<!-- Dấu = -->
<div class="absolute left-[304px] top-[196px] w-[20px] text-center text-base font-bold text-gray-500">=</div>

<!-- Ma trận Đầu vào Z_0 = X + PE -->
<div class="absolute left-[328px] top-[100px] w-[68px] border border-emerald-600 dark:border-emerald-400 divide-y divide-emerald-600 dark:divide-emerald-400">
<div class="h-9 bg-emerald-500/30 flex items-center justify-center font-bold text-emerald-950 dark:text-emerald-100">x₀+PE₀</div>
<div class="h-9 bg-emerald-500/20 flex items-center justify-center font-bold text-emerald-950 dark:text-emerald-100">x₁+PE₁</div>
<div class="h-9 bg-emerald-500/30 flex items-center justify-center font-bold text-emerald-950 dark:text-emerald-100">x₂+PE₂</div>
<div class="h-9 bg-emerald-500/20 flex items-center justify-center font-bold text-emerald-950 dark:text-emerald-100">x₃+PE₃</div>
<div class="h-9 bg-emerald-500/30 flex items-center justify-center font-bold text-emerald-950 dark:text-emerald-100">x₄+PE₄</div>
<div class="h-9 bg-emerald-500/20 flex items-center justify-center font-bold text-emerald-950 dark:text-emerald-100">x₅+PE₅</div>
</div>
<div class="absolute left-[322px] top-[326px] w-[80px] text-center text-[10px] font-sans text-gray-500">
Input Z₀ [T×d]
</div>

<!-- CỤM PHẢI TRÊN: Giải phẫu Bước sóng λ_i = 2π · Base^(2i/d) -->
<!-- Nhánh 1: i = 0 -->
<div class="absolute left-[434px] top-[98px] w-[92px]">
<div class="text-[10px] font-bold text-blue-600 dark:text-blue-400">i = 0</div>
<div class="text-[9px] font-sans text-gray-500">Mẫu số = 1</div>
<div class="text-[9px] font-sans text-blue-600 dark:text-blue-400 font-semibold">λ₀ = 2π ≈ 6.3</div>
</div>
<div class="absolute left-[534px] top-[82px] text-[9px] font-sans font-bold text-blue-600 dark:text-blue-400">
1 vòng = 2π
</div>

<!-- Nhánh 2: 2i → d_model -->
<div class="absolute left-[434px] top-[162px] w-[92px]">
<div class="text-[10px] font-bold text-amber-600 dark:text-amber-400">2i → d<sub>model</sub></div>
<div class="text-[9px] font-sans text-gray-500">Mẫu = Base</div>
<div class="text-[9px] font-sans text-amber-600 dark:text-amber-400 font-semibold">λ<sub>max</sub> = 2π·Base</div>
</div>
<div class="absolute left-[610px] top-[144px] text-[9px] font-sans font-bold text-amber-600 dark:text-amber-400">
1 vòng tối đa = 2π · Base
</div>
<div class="absolute left-[774px] top-[144px] text-[9px] font-sans font-bold text-rose-600 dark:text-rose-400">
Lặp lại chu kỳ
</div>
<div class="absolute left-[742px] top-[208px] text-[9px] font-mono text-rose-600 dark:text-rose-400 font-bold">
pos = 2π·Base
</div>

<!-- CỤM PHẢI DƯỚI: Thanh đối chiếu Cơ số Base (10.000 vs 500.000 - 1.000.000+) theo Context Window -->
<div class="absolute left-[434px] top-[236px] text-[10px] font-sans font-bold text-gray-700 dark:text-gray-200 tracking-wide">
MỞ RỘNG CƠ SỐ BASE THEO CONTEXT WINDOW (λ<sub>max</sub> = 2π · Base)
</div>

<!-- Thanh 1: Base = 10.000 -->
<div class="absolute left-[434px] top-[260px] w-[115px]">
<div class="text-[10px] font-bold text-amber-700 dark:text-amber-300">Base = 10.000</div>
<div class="text-[9px] font-sans text-gray-500">λ<sub>max</sub> ≈ 62.8K</div>
</div>
<div class="absolute left-[554px] top-[262px] w-[302px] h-7 border border-gray-500 flex overflow-hidden text-[10px] font-sans font-semibold">
<div class="w-[46%] bg-amber-500/30 border-r border-amber-600 flex items-center justify-center text-amber-950 dark:text-amber-100">
1 chu kỳ (≤ 62.8K)
</div>
<div class="w-[54%] bg-rose-500/25 flex items-center justify-center text-rose-800 dark:text-rose-200">
> 62.8K: Lặp chu kỳ (Trùng vị trí)
</div>
</div>

<!-- Thanh 2: Base = 500.000 - 1.000.000+ -->
<div class="absolute left-[434px] top-[304px] w-[115px]">
<div class="text-[10px] font-bold text-emerald-700 dark:text-emerald-300">Base = 500K – 1M+</div>
<div class="text-[9px] font-sans text-gray-500">λ<sub>max</sub> ≈ 3.1M – 6.3M</div>
</div>
<div class="absolute left-[554px] top-[306px] w-[302px] h-7 border border-emerald-600 flex overflow-hidden text-[10px] font-sans font-semibold">
<div class="w-full bg-emerald-500/25 flex items-center justify-center text-emerald-950 dark:text-emerald-100">
1 chu kỳ kéo giãn (Bao trọn Context 128K – 1M+)
</div>
</div>

</div>

<!--
- Tích vô hướng QK^T mù vị trí & Phép cộng PE: W_Q, W_K xoay các từ liên quan về góc nhỏ (cos lớn -> Attention Weight cao), nhưng phép nhân QK^T chỉ phụ thuộc nội dung từ mà không chứa biến vị trí pos -> phải cộng Z_0 = X + PE ngay ở đầu vào.
- Hệ trục Ma trận PE (0-indexed): Hàng là vị trí từ (pos = 0..T-1), cột là chiều vector (i = 0..d/2-1). Tại pos = 0: cột chẵn sin(0) = 0, cột lẻ cos(0) = 1. Ghép cặp sin/cos giúp PE_(pos+k) = M_k · PE_pos (học được khoảng cách tương đối k).
- Đa tần số theo chiều i: i = 0 (mẫu = 1) có chu kỳ ngắn 2π ≈ 6.3 token (như kim giây, phân biệt từ sát nhau); 2i → d_model (mẫu = Base) có chu kỳ dài 2π·Base (như kim giờ).
- Mở rộng Base theo Context Window: Base = 10000 có chu kỳ tối đa 2π·10000 ≈ 62.8K token; vượt ngưỡng này sóng lặp lại gây trùng vị trí. Các LLM ngữ cảnh dài (128K - 1M+) phải tăng Base lên 500K - 1M+ để 1 chu kỳ bao trọn Context Window.
-->
