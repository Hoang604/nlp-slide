---
transition: slide-left
---

# Giới thiệu Bài toán Sequence Transduction

Bài toán ánh xạ chuỗi đầu vào $X = (x_1, x_2, \dots, x_n)$ sang chuỗi đầu ra $Y = (y_1, y_2, \dots, y_m)$ có độ dài biến thiên trong dịch máy và xử lý ngôn ngữ tự nhiên:

<div class="mt-3">

<CompareCols leftTitle="Biểu diễn Dữ liệu Input" rightTitle="Đặc tả Kết quả Output" leftColor="blue" rightColor="emerald" leftBadge="Source Sequence" rightBadge="Target Sequence">
<template #left>

* Chuỗi văn bản nguồn được phân tách thành $n$ token rời rạc thuộc không gian từ vựng kích thước $V$.
* Mỗi token $x_t$ được ánh xạ qua ma trận Token Embedding thành vector đặc trưng liên tục có số chiều $d_{model}$.
* Toàn bộ lô dữ liệu đầu vào được biểu diễn dưới dạng tensor 3 chiều:

$$
X \in \mathbb{R}^{B \times n \times d_{model}}
$$

</template>

<template #right>

* Đầu ra là chuỗi token đích $Y = (y_1, y_2, \dots, y_m)$ được sinh tuần tự theo cơ chế Autoregressive.
* Tại mỗi bước $t$, tầng Linear và hàm Softmax ước lượng phân phối xác suất trên toàn bộ tập từ vựng $\mathbb{R}^{V}$:

$$
P(Y \mid X) = \prod_{t=1}^{m} P(y_t \mid y_1, \dots, y_{t-1}, X)
$$

</template>
</CompareCols>

</div>

<div class="mt-3 flex justify-between items-center text-[11px] px-3 py-2 rounded-lg border border-gray-200 dark:border-gray-800 bg-gray-50/60 dark:bg-gray-900/60">
<TensorBadge name="Input Tensor" shape="B × n × d_model" color="blue" />
<span class="font-mono text-gray-500">➔ Mô hình Sequence-to-Sequence ➔</span>
<TensorBadge name="Output Distribution" shape="B × m × V" color="emerald" />
</div>

<!--
- Input: Chuỗi n token rời rạc được ánh xạ qua Token Embedding thành tensor [B, n, d_model].
- Output: Độ dài đích m khác n nên mô hình sinh từng từ theo cơ chế Autoregressive.
- Công thức tích xác suất: Xác suất xuất hiện cả chuỗi Y bằng tích xác suất từng từ y_t khi đã biết các từ cụ thể y_1..y_{t-1} đã chọn trước đó và câu nguồn X.
- Tầng Linear chiếu vector ẩn d_model sang kích thước từ điển V, qua Softmax chuẩn hóa thành phân phối xác suất.
-->

---
transition: slide-left
---

# Các Kiến trúc Mô hình hóa Chuỗi Tiền nhiệm

Các phương pháp tiếp cận bài toán Sequence-to-Sequence trước kiến trúc Transformer:

<div class="grid grid-cols-3 gap-3 mt-3">
<div class="p-2.5 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50">

<div class="font-semibold text-xs text-blue-600 dark:text-blue-400">Vanilla RNN</div>

<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 leading-relaxed">

Cập nhật trạng thái ẩn tuần tự theo từng bước thời gian:

$$
h_t = \tanh(W x_t + U h_{t-1})
$$

Gặp hiện tượng Gradient Vanishing khi chiều dài chuỗi $n$ lớn.

</div>

</div>

<div class="p-2.5 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50">

<div class="font-semibold text-xs text-purple-600 dark:text-purple-400">LSTM & GRU</div>

<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 leading-relaxed">

Bổ sung Gating Mechanism và Cell State để duy trì đạo hàm:

$$
c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t
$$

Giảm suy hao tín hiệu nhưng vẫn bị trói buộc vào bước $t-1$.

</div>

</div>

<div class="p-2.5 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50">

<div class="font-semibold text-xs text-emerald-600 dark:text-emerald-400">Seq2Seq + Attention</div>

<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 leading-relaxed">

Bahdanau et al., 2014 tích hợp cơ chế Attention kết nối trực tiếp Encoder và Decoder.

Tuy nhiên, khung xương sống xử lý chuỗi vẫn dựa trên mạng Recurrent tuần tự.

</div>

</div>
</div>

<div class="grid grid-cols-2 gap-3 mt-3 text-[11px]">
<div class="p-2.5 rounded-lg border border-rose-500/30 bg-rose-500/10">

<div class="font-semibold text-rose-700 dark:text-rose-300 text-xs mb-1">Hạn chế 1: Sequential Bottleneck</div>

Trạng thái $h_t$ bắt buộc phải đợi $h_{t-1}$ hoàn tất với độ phức tạp $O(n)$ bước tuần tự, không thể song song hóa theo chiều thời gian trên GPU.

</div>

<div class="p-2.5 rounded-lg border border-amber-500/30 bg-amber-500/10">

<div class="font-semibold text-amber-700 dark:text-amber-300 text-xs mb-1">Hạn chế 2: Long-term Dependency</div>

Đường truyền thông tin giữa vị trí đầu và cuối chuỗi có độ dài $O(n)$, làm suy giảm khả năng học các phụ thuộc ngữ cảnh ở khoảng cách xa.

</div>
</div>

<!--
- Nút cổ chai nén thông tin (RNN, LSTM): Toàn bộ lịch sử 1..t-1 bị nhồi vào một vector trạng thái ẩn có kích thước cố định để nhân ma trận; chuỗi càng dài thông tin đầu câu càng bị pha loãng và suy giảm đạo hàm qua O(n) bước.
- Seq2Seq + Attention: Giữ lại toàn bộ h_1..h_n cho Decoder tra cứu, nhưng bản thân mỗi h_t vẫn phải tính nối tiếp từ h_{t-1}.
- Hai điểm nghẽn: Sequential Bottleneck tốn O(n) bước tuần tự làm tê liệt GPU, và Long-term Dependency có đường truyền dài O(n).
-->
