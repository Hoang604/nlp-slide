---
layout: intro
theme: seriph
title: "Kiến trúc Transformer trong Xử lý Ngôn ngữ Tự nhiên"
info: |
  Báo cáo giữa kỳ — Môn: Xử lý Ngôn ngữ Tự nhiên
  Bản chất toán học, cơ chế Self-Attention và mô hình hóa chuỗi song song
class: text-center relative overflow-hidden bg-white dark:bg-[#121212]
highlighter: shiki
drawings:
  persist: false
transition: slide-left
mdc: true
duration: 10min
---

<CoverBackdrop />

<div class="relative z-10 flex flex-col items-center justify-center h-full px-8 py-6">

<div class="inline-flex items-center gap-2 px-3 py-1 rounded-full border border-blue-500/25 bg-blue-500/5 text-blue-600 dark:text-blue-400 text-[11px] font-mono tracking-wider uppercase">
<span>Báo cáo Giữa kỳ</span>
<span>·</span>
<span>Xử lý Ngôn ngữ Tự nhiên</span>
</div>

<h1 class="!text-5xl font-sans font-bold text-gray-900 dark:text-white mt-6 tracking-tight">
Kiến trúc Transformer
</h1>

<p class="text-sm text-gray-600 dark:text-gray-300 mt-2.5 max-w-xl leading-relaxed">
Bản chất Toán học, Cơ chế Self-Attention và Mô hình hóa Chuỗi Song song
</p>

<div class="mt-6 flex justify-center items-center gap-3">
<TensorBadge name="Tính toán" shape="Song song O(1)" color="blue" />
<TensorBadge name="Cốt lõi" shape="Self-Attention" color="purple" />
<TensorBadge name="Mô hình" shape="Encoder - Decoder" color="emerald" />
</div>

<div class="mt-8 text-xs text-gray-400 font-medium">
Môn học: Xử lý Ngôn ngữ Tự nhiên
</div>

</div>

<!--
- Bài toán cốt lõi: Nắm bắt phụ thuộc giữa mọi token ở khoảng cách bất kỳ mà không cần xử lý tuần tự theo thời gian.
- Giải pháp của Transformer: Thay thế hoàn toàn cấu trúc đệ quy bằng cơ chế Self-Attention tính toán song song trên GPU.
-->

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
transition: slide-left
layout: center
class: text-center
---

# Kết luận

<div class="text-xs text-gray-600 dark:text-gray-300 max-w-2xl mx-auto mt-3 leading-relaxed space-y-2.5 text-left">

<div class="p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/60 dark:bg-gray-900/60">

**Bản chất Kiến trúc:** Thay thế hoàn toàn cấu trúc đệ quy và tích chập bằng cơ chế Self-Attention, rút ngắn khoảng cách truyền thông tin giữa hai vị trí bất kỳ trong chuỗi về mức hằng số $O(1)$.

</div>

<div class="p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/60 dark:bg-gray-900/60">

**Hiệu quả Tính toán và Tác động:** Giải phóng quá trình huấn luyện khỏi ràng buộc tuần tự theo thời gian, khai thác năng lực tính toán ma trận song song trên phần cứng GPU và đặt nền móng cho các mô hình ngôn ngữ lớn hiện đại.

</div>

</div>

<div class="mt-5 flex justify-center gap-3">
<TensorBadge name="Sequential Ops" shape="O(1)" color="blue" />
<TensorBadge name="Max Path Length" shape="O(1)" color="emerald" />
<TensorBadge name="Core Mechanism" shape="Multi-Head Self-Attention" color="purple" />
</div>

<div class="mt-6 text-sm font-semibold text-gray-800 dark:text-gray-200">
Phần Thảo luận & Trả lời Câu hỏi
</div>

<!--
- Tóm tắt cốt lõi: Đưa cả số bước tính toán tuần tự lẫn khoảng cách truyền thông tin giữa hai token bất kỳ về O(1).
- Ba nhánh phát triển từ Transformer: Encoder-Only (BERT) biểu diễn ngữ cảnh hai chiều; Decoder-Only (GPT, LLaMA) sinh văn bản tự hồi quy; Encoder-Decoder (T5, BART) chuyển đổi chuỗi sang chuỗi.
-->
