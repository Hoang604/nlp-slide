---
transition: slide-up
---

# Giới thiệu Bài toán Sequence Transduction

Bài toán ánh xạ chuỗi đầu vào $X = (x_1, x_2, \dots, x_n)$ sang chuỗi đầu ra $Y = (y_1, y_2, \dots, y_m)$ có độ dài biến thiên trong dịch máy và xử lý ngôn ngữ tự nhiên:

<div class="mt-3">

<CompareCols leftTitle="Biểu diễn Dữ liệu Input" rightTitle="Đặc tả Kết quả Output" leftColor="blue" rightColor="emerald" leftBadge="Source Sequence" rightBadge="Target Sequence">
<template #left>

* Chuỗi văn bản nguồn được phân tách thành $n$ token rời rạc thuộc không gian từ vựng kích thước $V$.
* Mỗi token $x_t$ được ánh xạ qua ma trận Token Embedding thành vector đặc trưng liên tục có số chiều $d_{model} = 512$.
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
<TensorBadge name="Input Tensor" shape="B × n × 512" color="blue" />
<span class="font-mono text-gray-500">➔ Mô hình Sequence-to-Sequence ➔</span>
<TensorBadge name="Output Distribution" shape="B × m × V" color="emerald" />
</div>

<!--
[Thời lượng: 1p - 1p30s | Giới thiệu bài toán: Input & Output]
- Mục tiêu vận hành: Xác định rõ hình dạng toán học của dữ liệu khi đi vào và đi ra khỏi mô hình dịch máy hoặc chuyển đổi chuỗi.
- Cơ chế sinh & Ánh xạ thực thể <-> Ký hiệu:
  1. Phía Input: Giả sử câu tiếng Anh đầu vào gồm 5 token ["The", "animal", "didn't", "cross", "it"]. Máy tính không nhân ma trận trực tiếp trên chữ cái, nên mỗi token được tra cứu thành một vector gồm 512 số thực thể hiện đặc trưng ngữ nghĩa <-> x_t thuộc R^512. Khi gom B câu vào cùng một lô xử lý, toàn bộ đầu vào là một khối số 3 chiều [B, 5, 512] <-> X thuộc R^(B x n x d_model).
  2. Phía Output: Câu tiếng Việt đích có thể dài 6 hoặc 7 token, nghĩa là độ dài m khác n. Do đó mô hình không thể xuất một lần ra ngay câu đích cố định, mà tại mỗi bước t phải nhìn vào toàn bộ câu nguồn X cùng các từ đích đã sinh từ bước 1 đến t-1 để tính bảng điểm xác suất trên toàn bộ từ điển V (ví dụ 37000 token), chọn ra token có xác suất cao nhất rồi lặp lại <-> Công thức tích xác suất có điều kiện P(Y | X).
- Khoảng trống & Cầu nối sang Trang 3: Bài toán đòi hỏi mô hình phải nén và kết nối ngữ cảnh của n vector đầu vào để sinh từng từ đầu ra. Vậy trước năm 2017, các kiến trúc mạng nơ-ron đã kết nối n vector này bằng cơ chế nào và tại sao cơ chế đó bị nghẽn?
-->

---
transition: slide-up
---

# Các Nghiên cứu Liên quan trong Mô hình hóa Chuỗi

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
[Thời lượng: 1p | Các nghiên cứu liên quan]
- Mục tiêu vận hành: Chẩn đoán nguyên nhân cơ học vì sao mọi biến thể dựa trên mạng Recurrent đều không thể mở rộng quy mô tính toán và suy giảm chất lượng trên câu dài.
- Cơ chế sinh & Ánh xạ thực thể <-> Ký hiệu:
  1. Vanilla RNN: Để đọc từ thứ 5 là "it", mô hình lấy vector của từ thứ 5 cộng với trạng thái ẩn h_4 vừa truyền sang rồi ép qua hàm tanh <-> h_5 = tanh(W x_5 + U h_4). Khi tính đạo hàm ngược từ từ thứ 50 về từ thứ 1, đạo hàm phải nhân liên tiếp 50 lần ma trận U và đạo hàm hàm tanh nhỏ hơn 1, làm tín hiệu tiến về 0 <-> Gradient Vanishing.
  2. LSTM & GRU: Thay phép nhân ma trận liên tiếp bằng một đường truyền cộng tuyến tính qua cổng kiểm soát f_t và i_t <-> c_t = f_t * c_{t-1} + i_t * c~_t. Giải quyết được phần nào việc mất đạo hàm, nhưng để tính c_5 thì máy tính vẫn phải đợi c_4 tính xong.
  3. Seq2Seq + Attention của Bahdanau: Cho phép Decoder nhìn lại mọi trạng thái h_1..h_n của Encoder, nhưng bản thân việc tạo ra h_1..h_n vẫn phải chạy nối đuôi nhau trên RNN.
- Hai điểm nghẽn tất yếu sinh ra từ cấu trúc trên:
  1. Sequential Bottleneck: Dù GPU có hàng ngàn nhân tính toán song song, khi gặp câu dài 100 từ thì ở bước t=2 toàn bộ GPU vẫn phải chờ bước t=1 tính xong h_1 <-> O(n) bước tuần tự.
  2. Long-term Dependency: Thông tin từ đại từ "it" ở cuối câu muốn liên kết với danh từ "animal" ở đầu câu phải đi vòng qua toàn bộ các trạng thái trung gian h_2, h_3, h_4 <-> Đường truyền dài O(n).
- Khoảng trống & Cầu nối sang Trang 4: Muốn triệt tiêu cùng lúc cả hai hạn chế này, ta phải cắt bỏ hoàn toàn mũi tên truyền trạng thái từ t-1 sang t. Trang tiếp theo trình bày tổng thể luồng hoạt động của Transformer khi loại bỏ hoàn toàn phép đệ quy.
-->
