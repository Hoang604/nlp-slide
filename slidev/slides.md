---
layout: intro
theme: seriph
title: "Kiến trúc Transformer trong Xử lý Ngôn ngữ Tự nhiên"
info: |
  Báo cáo chuyên đề: Xử lý Ngôn ngữ Tự nhiên
  Phân tích kiến trúc Transformer theo Vaswani et al. (2017)
class: text-center relative overflow-hidden bg-white dark:bg-[#121212]
highlighter: shiki
drawings:
  persist: false
transition: slide-left
mdc: true
duration: 25min
---

<CoverBackdrop />

<div class="relative z-10 flex flex-col items-center justify-center h-full px-8 py-6">

<div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-blue-500/25 bg-blue-500/5 text-blue-600 dark:text-blue-400 text-[11px] font-mono tracking-wider uppercase">
<span>Báo cáo Chuyên đề</span>
<span>·</span>
<span>Vaswani et al. (2017)</span>
</div>

<h1 class="!text-5xl font-sans font-bold text-gray-900 dark:text-white mt-6 tracking-tight">
Kiến trúc Transformer
</h1>

<p class="text-sm text-gray-600 dark:text-gray-300 mt-2.5 max-w-xl leading-relaxed">
Bản chất Toán học, Cơ chế Self-Attention và Đột phá trong Mô hình hóa Chuỗi
</p>

<div class="mt-6 flex justify-center items-center gap-3">
<TensorBadge name="Công bố" shape="NeurIPS 2017" color="blue" />
<TensorBadge name="Cốt lõi" shape="Self-Attention" color="purple" />
<TensorBadge name="Mô hình" shape="Encoder - Decoder" color="emerald" />
</div>

<div class="mt-8 text-xs text-gray-400 font-medium">
Chuyên đề: Xử lý Ngôn ngữ Tự nhiên
</div>

</div>

---
layout: default
transition: slide-up
---

# Cấu trúc Báo cáo

<div class="grid grid-cols-2 gap-5 mt-5">
<div class="space-y-3">
<div class="p-3 rounded-lg border border-blue-500/20 bg-blue-500/5">
<div class="font-semibold text-xs text-blue-700 dark:text-blue-300 flex items-center gap-2">
<span class="w-5 h-5 rounded-full bg-blue-500 text-white flex items-center justify-center text-[10px]">01</span>
Bối cảnh & Giới hạn của Recurrent Neural Network
</div>
<p class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 pl-7">Phân tích Sequential Bottleneck và Long-term Dependency trên RNN và LSTM.</p>
</div>

<div class="p-3 rounded-lg border border-purple-500/20 bg-purple-500/5">
<div class="font-semibold text-xs text-purple-700 dark:text-purple-300 flex items-center gap-2">
<span class="w-5 h-5 rounded-full bg-purple-500 text-white flex items-center justify-center text-[10px]">02</span>
Cơ chế Self-Attention & Nền tảng Toán học
</div>
<p class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 pl-7">Đặc tả Query, Key, Value, Scaled Dot-Product và Multi-Head Attention.</p>
</div>

<div class="p-3 rounded-lg border border-emerald-500/20 bg-emerald-500/5">
<div class="font-semibold text-xs text-emerald-700 dark:text-emerald-300 flex items-center gap-2">
<span class="w-5 h-5 rounded-full bg-emerald-500 text-white flex items-center justify-center text-[10px]">03</span>
Kiến trúc Hoàn chỉnh: Encoder & Decoder
</div>
<p class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 pl-7">Positional Encoding, Add & Norm, Feed-Forward Network và Causal Masking.</p>
</div>
</div>

<div class="space-y-3">
<div class="p-3 rounded-lg border border-amber-500/20 bg-amber-500/5">
<div class="font-semibold text-xs text-amber-700 dark:text-amber-300 flex items-center gap-2">
<span class="w-5 h-5 rounded-full bg-amber-500 text-white flex items-center justify-center text-[10px]">04</span>
Chi phí Tính toán, Huấn luyện & Thực nghiệm
</div>
<p class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 pl-7">Độ phức tạp tính toán theo Bảng 1 và kết quả thực nghiệm WMT 2014.</p>
</div>

<div class="p-3 rounded-lg border border-rose-500/20 bg-rose-500/5">
<div class="font-semibold text-xs text-rose-700 dark:text-rose-300 flex items-center gap-2">
<span class="w-5 h-5 rounded-full bg-rose-500 text-white flex items-center justify-center text-[10px]">05</span>
Các Vấn đề Kỹ thuật Mở rộng
</div>
<p class="text-[11px] text-gray-600 dark:text-gray-300 mt-1 pl-7">So sánh Layer Normalization với Batch Normalization, Pre-LN với Post-LN.</p>
</div>
</div>
</div>

---
src: ./pages/01-motivation.md
---

---
src: ./pages/02-attention.md
---

---
src: ./pages/03-architecture.md
---

---
src: ./pages/04-training-results.md
---

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
<TensorBadge name="Architecture" shape="Attention Is All You Need" color="purple" />
</div>

<div class="mt-10 text-base font-semibold text-gray-800 dark:text-gray-200">
Phần Thảo luận & Trả lời Câu hỏi
</div>
