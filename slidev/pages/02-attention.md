---
transition: slide-up
layout: section
class: text-center
---

# PHẦN 02
## Cơ chế Self-Attention & Nền tảng Toán học
### Cấu trúc Query, Key, Value, Scaled Dot-Product & Multi-Head Attention

---
transition: slide-left
---

# Cấu trúc Query, Key, Value

Cơ chế Self-Attention mô hình hóa mối quan hệ giữa các token thông qua ba vector biểu diễn:

<div class="grid grid-cols-3 gap-3.5 mt-4">
<div class="p-3 rounded-lg border border-blue-500/30 bg-blue-500/10">

<div class="flex items-center justify-between">
<span class="font-semibold text-xs text-blue-700 dark:text-blue-300">Query (Q)</span>
<TensorBadge shape="B × T × d_k" color="blue" />
</div>

<p class="text-[11px] text-gray-700 dark:text-gray-300 mt-2 leading-relaxed">
Vector biểu diễn cho vị trí hiện tại đang cần tổng hợp thông tin ngữ cảnh từ các vị trí khác trong chuỗi.
</p>

</div>

<div class="p-3 rounded-lg border border-amber-500/30 bg-amber-500/10">

<div class="flex items-center justify-between">
<span class="font-semibold text-xs text-amber-700 dark:text-amber-300">Key (K)</span>
<TensorBadge shape="B × T × d_k" color="amber" />
</div>

<p class="text-[11px] text-gray-700 dark:text-gray-300 mt-2 leading-relaxed">
Vector đặc trưng của từng vị trí trong chuỗi, dùng để tính toán mức độ liên quan với Query thông qua tích vô hướng.
</p>

</div>

<div class="p-3 rounded-lg border border-emerald-500/30 bg-emerald-500/10">

<div class="flex items-center justify-between">
<span class="font-semibold text-xs text-emerald-700 dark:text-emerald-300">Value (V)</span>
<TensorBadge shape="B × T × d_v" color="emerald" />
</div>

<p class="text-[11px] text-gray-700 dark:text-gray-300 mt-2 leading-relaxed">
Vector chứa nội dung biểu diễn sẽ được tổng hợp dựa trên trọng số phân phối từ hàm Softmax.
</p>

</div>
</div>

<div class="mt-4 p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-gray-50/50 dark:bg-gray-900/50 text-[11px]">

Biến đổi tuyến tính từ ma trận đầu vào $X \in \mathbb{R}^{B \times T \times d_{model}}$:

* $Q = X W_Q$ với $W_Q \in \mathbb{R}^{d_{model} \times d_k}$
* $K = X W_K$ với $W_K \in \mathbb{R}^{d_{model} \times d_k}$
* $V = X W_V$ với $W_V \in \mathbb{R}^{d_{model} \times d_v}$

</div>

---
transition: slide-left
---

# Scaled Dot-Product Attention

Công thức tính toán Attention cơ bản trong kiến trúc Transformer:

<MathCard title="Scaled Dot-Product Attention" badge="Formula" color="blue"
  intuition="Tính tích vô hướng giữa Query và Key để đo lường độ tương đồng, chuẩn hóa phân phối xác suất qua Softmax và tính tổng có trọng số của Value."
  caveat="Ma trận Attention Weight có kích thước [T, T] thể hiện sự tương tác giữa mọi cặp token trong câu.">

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V
$$

</MathCard>

<div class="grid grid-cols-4 gap-2 mt-4 text-center">
<div class="p-2 rounded border border-gray-200 dark:border-gray-800 bg-white/40 dark:bg-gray-900/40">
<div class="text-[10px] text-gray-400 font-semibold">1. Tích vô hướng</div>
<div class="font-mono text-xs font-bold mt-0.5 text-blue-600">S = Q · K^T</div>
<div class="text-[9px] text-gray-500 font-mono">[B, T, T]</div>
</div>

<div class="p-2 rounded border border-gray-200 dark:border-gray-800 bg-white/40 dark:bg-gray-900/40">
<div class="text-[10px] text-gray-400 font-semibold">2. Scaling</div>
<div class="font-mono text-xs font-bold mt-0.5 text-amber-600">S' = S / √d_k</div>
<div class="text-[9px] text-gray-500 font-mono">[B, T, T]</div>
</div>

<div class="p-2 rounded border border-gray-200 dark:border-gray-800 bg-white/40 dark:bg-gray-900/40">
<div class="text-[10px] text-gray-400 font-semibold">3. Softmax</div>
<div class="font-mono text-xs font-bold mt-0.5 text-purple-600">A = Softmax(S')</div>
<div class="text-[9px] text-gray-500 font-mono">[B, T, T]</div>
</div>

<div class="p-2 rounded border border-gray-200 dark:border-gray-800 bg-white/40 dark:bg-gray-900/40">
<div class="text-[10px] text-gray-400 font-semibold">4. Chiếu Value</div>
<div class="font-mono text-xs font-bold mt-0.5 text-emerald-600">O = A · V</div>
<div class="text-[9px] text-gray-500 font-mono">[B, T, d_v]</div>
</div>
</div>

---
transition: slide-left
---

# Phân tích Toán học: Phương sai của Tích vô hướng

Giả thiết các thành phần của vector $q$ và $k$ là các biến ngẫu nhiên độc lập có phân phối chuẩn hóa:

$$
\mathbb{E}[q_i] = 0, \quad \text{Var}(q_i) = 1, \quad \mathbb{E}[k_i] = 0, \quad \text{Var}(k_i) = 1
$$

Tích vô hướng là tổng của $d_k$ biến ngẫu nhiên độc lập: $q \cdot k = \sum_{i=1}^{d_k} q_i k_i$.

Phương sai của tích vô hướng tăng tuyến tính theo số chiều $d_k$:

$$
\text{Var}(q \cdot k) = \sum_{i=1}^{d_k} \text{Var}(q_i k_i) = \sum_{i=1}^{d_k} 1 = d_k
$$

<InsightBox type="warning" title="Hệ quả Đối với Quá trình Tối ưu">

Khi $d_k$ lớn (ví dụ $d_k = 64$), độ lệch chuẩn đạt mức $\sqrt{d_k} = 8$. Giá trị tích vô hướng có biên độ lớn, đẩy hàm Softmax vào vùng bão hòa có đạo hàm triệt tiêu về 0.

</InsightBox>

---
transition: slide-left
---

# Cơ chế Ổn định Gradient: Hệ số Chuẩn hóa $\frac{1}{\sqrt{d_k}}$

So sánh hành vi của hàm Softmax trước và sau khi chuẩn hóa tỷ lệ:

<CompareCols leftTitle="Khi Không Chuẩn Hóa" rightTitle="Khi Áp Dụng Hệ Số 1/√d_k" leftColor="rose" rightColor="emerald" leftBadge="Không Scale" rightBadge="Scaled Attention">
<template #left>

* Độ lớn tích vô hướng tăng tỷ lệ thuận với $\sqrt{d_k}$.
* Giá trị đầu vào lớn đẩy hàm Softmax vào vùng bão hòa cực đoan.
* Đạo hàm của Softmax tiến dần về $0$, gây suy giảm gradient khi lan truyền ngược.

</template>

<template #right>

* Chia cho $\sqrt{d_k}$ đưa phương sai của đại lượng đầu vào về mức $1$:

$$
\text{Var}\left(\frac{q \cdot k}{\sqrt{d_k}}\right) = \frac{d_k}{(\sqrt{d_k})^2} = 1
$$

* Phân phối xác suất duy trì độ dốc phù hợp, đảm bảo gradient lan truyền ổn định.

</template>
</CompareCols>

---
transition: slide-left
---

# Multi-Head Attention

Cơ chế Multi-Head Attention chiếu các biểu diễn vào $h$ không gian con độc lập:

<MathCard title="Đặc tả Multi-Head Attention" badge="Vaswani et al. (2017)" color="purple"
  intuition="Cho phép mô hình đồng thời thu nhận thông tin từ nhiều không gian biểu diễn khác nhau tại các vị trí khác nhau."
  caveat="Tổng chi phí tính toán tương đương với Single-Head kích thước đầy đủ do d_k = d_model / h.">

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W^O
$$

$$
\text{với } \text{head}_i = \text{Attention}(Q W_i^Q, K W_i^K, V W_i^V)
$$

</MathCard>

<div class="mt-3.5 flex items-center justify-between text-[11px]">
<TensorBadge name="Input" shape="B × T × 512" color="blue" />
<span>➔ Chiếu tuyến tính h=8 ➔</span>
<TensorBadge name="Từng Head" shape="B × 8 × T × 64" color="purple" />
<span>➔ Concat & W^O ➔</span>
<TensorBadge name="Output" shape="B × T × 512" color="emerald" />
</div>

---
transition: slide-left
---

# Trực quan hóa Ma trận Trọng số Attention

Mô phỏng phân phối trọng số Attention giữa các vị trí token:

<div class="mt-2">
<AttentionGrid mode="self" :tokens="['The', 'animal', 'didn\'t', 'cross', 'it']" highlightFrom="it" highlightTo="animal" />
</div>

<div class="mt-3">
<InsightBox type="insight" title="Hiện tượng Coreference Resolution">

Mô hình tự động học được liên kết phụ thuộc ngữ nghĩa giữa đại từ **it** và danh từ **animal** thông qua quá trình tối ưu hóa trọng số Attention.

</InsightBox>
</div>
