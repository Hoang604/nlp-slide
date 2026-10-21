---
transition: slide-left
---

# Multi-Head Attention & Causal Masking

<div class="mt-2.5">

<CompareCols leftTitle="Multi-Head Attention" rightTitle="Causal Masking tại Decoder" leftColor="purple" rightColor="rose" leftBadge="h = 8 Heads" rightBadge="Autoregressive">
<template #left>

Chiếu $Q, K, V$ vào $h$ không gian con độc lập, với cấu hình tiêu chuẩn $h = 8$ và $d_k = d_v = d_{model} / h = 64$ theo Vaswani et al., 2017:

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W^O
$$

$$
\text{head}_i = \text{Attention}(Q W_i^Q, K W_i^K, V W_i^V)
$$

* Cho phép mô hình đồng thời nắm bắt nhiều mối quan hệ ngữ pháp và ngữ nghĩa tại các vị trí khác nhau.
* Tổng chi phí tính toán tương đương với Single-Head Attention trên toàn bộ chiều $d_{model}$.

</template>

<template #right>

Trong quá trình sinh chuỗi tại Decoder, vị trí $i$ chỉ được phép truy cập các vị trí tiền nhiệm $j \le i$:

$$
S_{i,j} = \begin{cases} \frac{q_i \cdot k_j}{\sqrt{d_k}} & \text{khi } j \le i \\ -\infty & \text{khi } j > i \end{cases}
$$

* Gán giá trị $-\infty$ vào vùng tam giác trên của ma trận trọng số trước khi qua hàm Softmax để triệt tiêu xác suất $e^{-\infty} = 0$.
* Ngăn rò rỉ thông tin tương lai và cho phép huấn luyện song song toàn bộ chuỗi đích thông qua Teacher Forcing.

</template>
</CompareCols>

</div>

<div class="mt-3 flex items-center justify-between text-[11px] px-3 py-2 rounded-lg border border-gray-200 dark:border-gray-800 bg-gray-50/50 dark:bg-gray-900/50">
<TensorBadge name="Input" shape="B × T × 512" color="blue" />
<span class="font-mono text-gray-500">➔ Split h = 8 ➔</span>
<TensorBadge name="Parallel Heads" shape="B × 8 × T × 64" color="purple" />
<span class="font-mono text-gray-500">➔ Concat & W^O ➔</span>
<TensorBadge name="Output" shape="B × T × 512" color="emerald" />
</div>

<!--
[Thời lượng: 1p | Phương pháp 4/5: Multi-Head Attention & Causal Masking]
- Mục tiêu vận hành: Giải thích cách Transformer mở rộng khả năng biểu diễn đa quan hệ mà không tăng chi phí tính toán, đồng thời bảo toàn tính nhân quả ở nhánh Decoder.
- Cơ chế sinh & Ánh xạ thực thể <-> Ký hiệu:
  1. Multi-Head Attention (Cột trái): Thay vì thực hiện 1 phép Attention duy nhất trên vector 512 chiều (khi đó một trọng số lớn sẽ lấn át toàn bộ phép trung bình), mô hình chia 512 chiều thành h = 8 nhánh độc lập, mỗi nhánh chỉ có số chiều d_k = 512 / 8 = 64 <-> Tensor [B, 8, T, 64]. Nhánh 1 có thể tập trung trọng số từ "it" vào danh từ "animal", trong khi nhánh 2 tập trung từ "it" vào tính từ "tired". Sau khi 8 nhánh tính xong song song, ta ghép nối 8 vector 64 chiều lại thành [B, T, 512] rồi nhân ma trận W^O để trộn thông tin <-> Công thức Concat(head_1, ..., head_h) W^O. Vì mỗi nhánh giảm số chiều đi 8 lần nên tổng số phép nhân ma trận của cả 8 nhánh bằng đúng 1 nhánh 512 chiều.
  2. Causal Masking (Cột phải): Khi huấn luyện Decoder trên câu đích "Ngôn ngữ tự nhiên", ta đưa cả 4 từ vào cùng lúc để tính song song. Nhưng khi dự đoán từ ở vị trí i = 2 ("ngữ"), mô hình không được phép nhìn thấy vị trí j = 3 ("tự") và j = 4 ("nhiên"). Do đó, ngay sau khi tính bảng điểm QK^T / căn(d_k), mọi ô nằm phía trên đường chéo chính (j > i) bị cộng thêm âm vô cùng <-> S_{i,j} = - vô cùng. Khi đi qua hàm Softmax, e mũ âm vô cùng bằng đúng 0, khóa chặt mọi thông tin tương lai.
- Khoảng trống & Cầu nối sang Trang tiếp theo: Sau khi đi qua khối Attention, từng vector từ được biến đổi phi tuyến và nối tắt qua các Sub-layer bên trong một tầng Encoder và Decoder như thế nào?
-->

---
transition: slide-left
---

# Cấu trúc Bên trong Tầng Encoder & Decoder

Mỗi tầng thứ $l$ gồm các Sub-layer nối tiếp nhau, mỗi Sub-layer đều có đường nối tắt và chuẩn hóa riêng biệt:

<div class="mt-2.5">

<CompareCols leftTitle="Luồng Sub-layer & Add & Norm" rightTitle="Position-wise Feed-Forward Network" leftColor="emerald" rightColor="blue" leftBadge="Residual Flow" rightBadge="d_ff = 2048">
<template #left>

Đường nối tắt chỉ bắc cầu qua **từng Sub-layer một**, lấy đầu vào ngay trước Sub-layer đó cộng với đầu ra của chính nó:

$$
h_l = \text{LayerNorm}\Big(z_{l-1} + \text{MHA}(z_{l-1})\Big)
$$

$$
z_l = \text{LayerNorm}\Big(h_l + \text{FFN}(h_l)\Big)
$$

* Tại Decoder, xen giữa `Masked MHA` và `FFN` có thêm một Sub-layer `Cross-Attention` nhận $K, V$ từ đỉnh Encoder.
* Đạo hàm truyền trực tiếp qua toán tử cộng $+ z_{l-1}$ và $+ h_l$, ngăn suy giảm tín hiệu khi xếp chồng $N$ tầng.

</template>

<template #right>

Sau khi `Attention` trộn thông tin ngữ cảnh giữa các từ, khối `FFN` áp dụng độc lập và giống hệt nhau lên từng vị trí token:

$$
\text{FFN}(x) = \max(0, x W_1 + b_1) W_2 + b_2
$$

* Gồm 2 phép biến đổi tuyến tính với hàm kích hoạt phi tuyến `ReLU` ở giữa.
* Mở rộng chiều đặc trưng từ $d_{model} = 512$ lên không gian ẩn $d_{ff} = 2048$ rồi nén về lại $d_{model} = 512$ theo cấu hình chuẩn của Vaswani et al., 2017.

</template>
</CompareCols>

</div>

<div class="mt-3 flex items-center justify-between text-[11px] px-3 py-2 rounded-lg border border-gray-200 dark:border-gray-800 bg-gray-50/50 dark:bg-gray-900/50">
<TensorBadge name="Input" shape="B × T × 512" color="blue" />
<span class="font-mono text-gray-500">➔ Linear W₁ + ReLU ➔</span>
<TensorBadge name="FFN Hidden" shape="B × T × 2048" color="amber" />
<span class="font-mono text-gray-500">➔ Linear W₂ + Add & Norm ➔</span>
<TensorBadge name="Output" shape="B × T × 512" color="emerald" />
</div>

<!--
[Thời lượng: 1p | Cấu trúc bên trong Tầng Encoder & Decoder]
- Mục tiêu vận hành: Làm rõ đường đi của tensor qua từng Sub-layer bên trong 1 tầng Encoder/Decoder và vai trò của khối Position-wise Feed-Forward Network.
- Cơ chế sinh & Ánh xạ thực thể <-> Ký hiệu:
  1. Cột trái (Luồng Sub-layer & Add & Norm): Trả lời câu hỏi x trong LayerNorm(x + Sublayer(x)) lấy từ đâu. Đầu vào của tầng thứ l là z_{l-1}. Khi đi qua Sub-layer 1 (Multi-Head Attention), đường nối tắt lấy chính z_{l-1} cộng với kết quả MHA(z_{l-1}) rồi chuẩn hóa tạo ra h_l. Tiếp theo, h_l này trở thành đầu vào cho Sub-layer 2 (Feed Forward): đường nối tắt thứ hai lấy h_l cộng với FFN(h_l) rồi chuẩn hóa để xuất ra z_l đưa lên tầng l+1.
  2. Cột phải (Position-wise Feed-Forward Network): Nếu Attention làm nhiệm vụ thu thập và trộn thông tin giữa các từ khác nhau trong câu, thì FFN là mạng MLP 2 tầng xử lý riêng từng vector từ (Position-wise) để trích xuất đặc trưng phi tuyến: chiếu từ 512 chiều lên 2048 chiều, qua hàm ReLU lọc bỏ phần âm, rồi chiếu ngược về 512 chiều.
-->
