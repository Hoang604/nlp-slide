---
transition: slide-up
layout: section
class: text-center
---

# PHẦN 01
## Bối cảnh & Giới hạn của Recurrent Neural Network
### Cơ sở hình thành kiến trúc xử lý chuỗi song song

---
transition: slide-left
---

# Kiến trúc Xử lý Chuỗi trước năm 2017

Trong các bài toán Sequence-to-Sequence, các kiến trúc Recurrent Neural Network là mô hình nền tảng:

<div class="grid grid-cols-3 gap-3.5 mt-5">
<div class="p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50">

<div class="font-semibold text-xs text-blue-600 dark:text-blue-400">Vanilla RNN</div>

<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1.5 leading-relaxed">

Trạng thái ẩn cập nhật theo từng bước thời gian:

$$
h_t = \tanh(W x_t + U h_{t-1})
$$

Xảy ra hiện tượng Gradient Vanishing khi chiều dài chuỗi tăng cao.

</div>

</div>

<div class="p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50">

<div class="font-semibold text-xs text-purple-600 dark:text-purple-400">LSTM & GRU</div>

<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1.5 leading-relaxed">

Bổ sung Gating mechanism và Cell State:

$$
c_t = f_t \odot c_{t-1} + i_t \odot \tilde{c}_t
$$

Giảm thiểu sự suy hao gradient nhưng vẫn duy trì cấu trúc tính toán tuần tự.

</div>

</div>

<div class="p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50">

<div class="font-semibold text-xs text-emerald-600 dark:text-emerald-400">Seq2Seq + Attention</div>

<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1.5 leading-relaxed">

Bahdanau et al. (2014) sử dụng Attention làm thành phần phụ trợ kết nối Encoder và Decoder.

Cấu trúc xử lý chính của mô hình vẫn phụ thuộc vào RNN.

</div>

</div>
</div>

<div class="mt-5 flex justify-center">
<TensorBadge name="Đặc tính" shape="Trạng thái h_t phụ thuộc tuyến tính vào h_{t-1}" color="rose" />
</div>

---
transition: slide-left
---

# Hai Giới hạn Cốt lõi của Mạng Recurrent

<CompareCols leftTitle="Sequential Bottleneck" rightTitle="Long-term Dependency" leftColor="rose" rightColor="amber">
<template #left>

Tính toán tại bước $t$ yêu cầu trạng thái ẩn tại bước $t-1$:

$$
h_{t-1} \longrightarrow h_t
$$

Độ dài chuỗi $T$ đòi hỏi $T$ bước tính toán tuần tự liên tiếp.

Không thể song song hóa quá trình huấn luyện trên GPU do ràng buộc phụ thuộc dữ liệu theo thời gian.

</template>

<template #right>

Khoảng cách truyền thông tin giữa vị trí $1$ và vị trí $T$ đòi hỏi $T$ phép nhân ma trận trọng số.

Tín hiệu ngữ cảnh ở đầu chuỗi suy giảm dần khi truyền qua các bước thời gian trung gian.

Khoảng cách truyền thông tin tối đa:

$$
\text{Maximum Path Length} = O(T)
$$

</template>
</CompareCols>

---
transition: slide-left
---

# So sánh Kiến trúc: RNN và Transformer

<CompareCols leftTitle="Recurrent Architecture" rightTitle="Transformer Architecture" leftColor="rose" rightColor="emerald" leftBadge="Sequential" rightBadge="Parallel">
<template #left>

* Lan truyền trạng thái tuần tự theo trục thời gian $t_1 \to t_2 \to \dots \to t_T$.
* Chi phí tính toán tuần tự: $O(T)$ bước.
* Chi phí bộ nhớ trong Backpropagation Through Time tăng tuyến tính theo chiều dài chuỗi.
* Khoảng cách tương tác cực đại giữa hai vị trí: $O(T)$.

</template>

<template #right>

* Loại bỏ cấu trúc hồi quy, tính toán song song toàn bộ chuỗi thông qua Self-Attention.
* Chi phí tính toán tuần tự: $O(1)$ bước.
* Quá trình huấn luyện dựa trên các phép nhân ma trận song song trên GPU.
* Khoảng cách tương tác cực đại giữa hai vị trí bất kỳ: $O(1)$.

</template>
</CompareCols>

<div class="mt-3">
<InsightBox type="insight" title="Động lực Kiến trúc">

Kiến trúc Transformer giải quyết triệt để hạn chế tuần tự của mạng hồi quy bằng cách sử dụng cơ chế Attention làm xương sống cho toàn bộ mô hình.

</InsightBox>
</div>
