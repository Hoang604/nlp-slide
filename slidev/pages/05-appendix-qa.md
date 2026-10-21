---
transition: slide-left
layout: section
class: text-center
---
<!--
# PHẦN 05
## Các Vấn đề Kỹ thuật Mở rộng
### So sánh Chuẩn hóa, Biến thể Kiến trúc & Hướng Phát triển-->

<!-----
transition: slide-left
---

# Lựa chọn Cơ chế Chuẩn hóa trong NLP

So sánh giữa Batch Normalization và Layer Normalization trong xử lý ngôn ngữ tự nhiên:

<CompareCols leftTitle="Batch Normalization" rightTitle="Layer Normalization" leftColor="rose" rightColor="emerald" leftBadge="Across Batch" rightBadge="Across Features">
<template #left>

Chuẩn hóa theo trục batch cho từng chiều đặc trưng:

$$
\mu_j = \frac{1}{B} \sum_{i=1}^B x_{i,j}
$$

* Phụ thuộc vào kích thước Batch Size.
* Kém hiệu quả với chuỗi có độ dài thay đổi và chứa nhiều padding tokens.

</template>

<template #right>

Chuẩn hóa độc lập cho từng token trên toàn bộ số chiều đặc trưng ($d_{model} = 512$):

$$
\mu = \frac{1}{d} \sum_{i=1}^d x_i, \quad \sigma^2 = \frac{1}{d} \sum_{i=1}^d (x_i - \mu)^2
$$

* Độc lập hoàn toàn với kích thước Batch Size.
* Phù hợp với quá trình Autoregressive Inference.

</template>
</CompareCols>

<div class="mt-3">
<InsightBox type="question" title="Đặc tính Kỹ thuật">

Layer Normalization chuẩn hóa trên không gian đặc trưng của từng vector từ, loại bỏ sự phụ thuộc vào các mẫu dữ liệu khác trong cùng lô tính toán.

</InsightBox>
</div>

---
transition: slide-left
---

# Vị trí Đặt Chuẩn hóa: Post-LN và Pre-LN

Sự thay đổi vị trí của Layer Normalization trong các mô hình Transformer hiện đại:

<CompareCols leftTitle="Post-LN" rightTitle="Pre-LN" leftColor="amber" rightColor="blue" leftBadge="Vaswani et al." rightBadge="Modern LLMs">
<template #left>

$$
x_{l+1} = \text{LayerNorm}(x_l + \text{Sublayer}(x_l))
$$

* Layer Normalization nằm trên đường truyền chính.
* Gradient tại các tầng đầu nhạy cảm khi độ sâu mô hình tăng cao, yêu cầu thiết lập Learning Rate Warmup chính xác để tránh phân kỳ.

</template>

<template #right>

$$
x_{l+1} = x_l + \text{Sublayer}(\text{LayerNorm}(x_l))
$$

* Layer Normalization nằm trong nhánh Sublayer.
* Thiết lập Identity Path cho gradient, giúp ổn định quá trình tối ưu hóa trên các mô hình có độ sâu lớn.

</template>
</CompareCols>

<div class="mt-3 flex justify-center">
<TensorBadge name="Xu hướng Thiết kế" shape="Pre-LN + RMSNorm + RoPE" color="purple" />
</div>

---
transition: slide-left
---

# Ba Hướng Phát triển Kiến trúc từ Transformer

Kiến trúc Transformer gốc đóng vai trò nền tảng cho ba dòng mô hình chính:

<div class="grid grid-cols-3 gap-3.5 mt-5">
<div class="p-3 rounded-lg border border-blue-500/30 bg-blue-500/10 text-[11px]">
<div class="font-semibold text-xs text-blue-700 dark:text-blue-300">Encoder-Only</div>
<div class="font-mono text-[9px] text-gray-500 mt-0.5">BERT, RoBERTa, DeBERTa</div>
<ul class="list-disc pl-4 mt-2 space-y-1 text-gray-600 dark:text-gray-300">
<li>Cơ chế Bidirectional Attention toàn cục.</li>
<li>Mục tiêu huấn luyện: Masked Language Modeling.</li>
<li>Phù hợp cho các bài toán phân loại văn bản và trích xuất thực thể.</li>
</ul>
</div>

<div class="p-3 rounded-lg border border-purple-500/30 bg-purple-500/10 text-[11px]">
<div class="font-semibold text-xs text-purple-700 dark:text-purple-300">Decoder-Only</div>
<div class="font-mono text-[9px] text-gray-500 mt-0.5">GPT, LLaMA, Claude</div>
<ul class="list-disc pl-4 mt-2 space-y-1 text-gray-600 dark:text-gray-300">
<li>Cơ chế Causal Masked Attention.</li>
<li>Mục tiêu huấn luyện: Next Token Prediction.</li>
<li>Nền tảng cho các Large Language Models.</li>
</ul>
</div>

<div class="p-3 rounded-lg border border-emerald-500/30 bg-emerald-500/10 text-[11px]">
<div class="font-semibold text-xs text-emerald-700 dark:text-emerald-300">Encoder-Decoder</div>
<div class="font-mono text-[9px] text-gray-500 mt-0.5">T5, BART, mT5</div>
<ul class="list-disc pl-4 mt-2 space-y-1 text-gray-600 dark:text-gray-300">
<li>Kế thừa đầy đủ cấu trúc Encoder-Decoder gốc.</li>
<li>Chuẩn hóa các tác vụ dưới dạng bài toán Text-to-Text.</li>
<li>Phù hợp cho bài toán dịch máy và tóm tắt văn bản.</li>
</ul>
</div>
</div>-->

---
layout: center
class: text-center
---

# Kết luận

<div class="text-xs text-gray-600 dark:text-gray-300 max-w-xl mx-auto mt-4 leading-relaxed">

Kiến trúc Transformer đã chứng minh tính hiệu quả vượt trội trong việc mở rộng quy mô tính toán và biểu diễn ngữ cảnh, trở thành kiến trúc nền tảng trong xử lý ngôn ngữ tự nhiên và các lĩnh vực học máy liên ngành.

</div>

<div class="mt-7 flex justify-center gap-3">
<TensorBadge name="Complexity" shape="O(1) Sequential Operations" color="blue" />
<TensorBadge name="Hardware" shape="100% GPU Parallel" color="emerald" />
<TensorBadge name="Architecture" shape="Multi-Head Self-Attention" color="purple" />
</div>

<div class="mt-10 text-base font-semibold text-gray-800 dark:text-gray-200">
Phần Thảo luận & Trả lời Câu hỏi
</div>
