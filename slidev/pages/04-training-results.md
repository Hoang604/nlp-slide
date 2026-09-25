---
transition: slide-up
layout: section
class: text-center
---

# PHẦN 04
## Chi phí Tính toán, Huấn luyện & Đánh giá Thực nghiệm
### Phân tích Độ phức tạp & Kết quả Thực nghiệm

---
transition: slide-left
---

# Phân tích Độ phức tạp Tính toán (Bảng 1, Vaswani et al., 2017)

Đánh giá định lượng chi phí tính toán giữa Self-Attention, Recurrent và Convolutional:

<div class="mt-2.5">
<ComplexityTable highlight="self-attention" />
</div>

<div class="grid grid-cols-2 gap-3 mt-2 text-[10.5px]">
<div class="p-2.5 rounded-lg border border-blue-500/20 bg-blue-500/5">

<div class="font-semibold text-blue-700 dark:text-blue-300 text-xs mb-1">

Trường hợp $n < d$ trong các tác vụ NLP:

</div>

* Độ dài câu $n$ thông thường nhỏ hơn kích thước vector biểu diễn $d_{model} = 512$.
* Chi phí tính toán $O(n^2 \cdot d)$ của Self-Attention tối ưu hơn so với $O(n \cdot d^2)$ của các lớp Recurrent.

</div>

<div class="p-2.5 rounded-lg border border-purple-500/20 bg-purple-500/5">

<div class="font-semibold text-purple-700 dark:text-purple-300 text-xs mb-1">

Số lượng Phép toán Tuần tự:

</div>

* Recurrent đòi hỏi $O(n)$ bước tính toán tuần tự không thể chia nhỏ.
* Self-Attention thực hiện toàn bộ quá trình ánh xạ ngữ cảnh trong $O(1)$ bước tính toán song song.

</div>
</div>



---
transition: slide-left
---

# Kết quả Thực nghiệm trên Tác vụ Machine Translation (WMT 2014)

Hiệu năng của mô hình Transformer trên bộ dữ liệu chuẩn WMT 2014:

<div class="grid grid-cols-2 gap-5 mt-4">
<div class="space-y-3">
<div class="p-3 rounded-lg border border-emerald-500/30 bg-emerald-500/10">
<div class="text-[10px] text-emerald-600 dark:text-emerald-400 font-semibold uppercase tracking-wider">English-to-German</div>
<div class="text-2xl font-bold text-gray-900 dark:text-white mt-0.5">28.4 BLEU</div>
<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1">Vượt mô hình tốt nhất trước đó 2.0 điểm BLEU.</div>
</div>

<div class="p-3 rounded-lg border border-blue-500/30 bg-blue-500/10">
<div class="text-[10px] text-blue-600 dark:text-blue-400 font-semibold uppercase tracking-wider">English-to-French</div>
<div class="text-2xl font-bold text-gray-900 dark:text-white mt-0.5">41.8 BLEU</div>
<div class="text-[11px] text-gray-600 dark:text-gray-300 mt-1">Đạt kết quả State-of-the-Art với chi phí tính toán thấp hơn các baseline.</div>
</div>
</div>

<div class="p-3.5 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50 text-[11px] flex flex-col justify-between">
<div>
<div class="font-semibold text-xs text-gray-800 dark:text-gray-200 mb-2">Chi phí Huấn luyện:</div>
<ul class="space-y-1.5 list-disc pl-4 text-gray-600 dark:text-gray-300">
<li><strong>Transformer Base (65M tham số):</strong> Thời gian huấn luyện 12 giờ trên 8 GPU NVIDIA P100.</li>
<li><strong>Transformer Big (213M tham số):</strong> Thời gian huấn luyện 3.5 ngày.</li>
<li>Hiệu suất tính toán cao nhờ khả năng song song hóa hoàn toàn trên phần cứng.</li>
</ul>
</div>

<div class="mt-3 pt-2.5 border-t border-gray-100 dark:border-gray-800 text-[10px] text-gray-400">
Số liệu trích xuất từ Bảng 2, Vaswani et al. (2017).
</div>
</div>
</div>
