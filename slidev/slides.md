---
layout: intro
theme: seriph
title: "Kiến trúc Transformer trong Xử lý Ngôn ngữ Tự nhiên"
info: |
  Báo cáo chuyên đề: Xử lý Ngôn ngữ Tự nhiên
  Phân tích kiến trúc Transformer theo Vaswani et al., 2017
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
<span>Báo cáo Chuyên đề</span>
<span>·</span>
<span>Vaswani et al., 2017</span>
</div>

<h1 class="!text-5xl font-sans font-bold text-gray-900 dark:text-white mt-6 tracking-tight">
Kiến trúc Transformer
</h1>

<p class="text-sm text-gray-600 dark:text-gray-300 mt-2.5 max-w-xl leading-relaxed">
Bản chất Toán học, Cơ chế Self-Attention và Mô hình hóa Chuỗi Song song
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

<!--
[Thời lượng: 20s | Mở đầu]
- Mục tiêu vận hành: Định vị rõ câu hỏi kỹ thuật trung tâm mà bài báo Vaswani et al., 2017 giải quyết: Làm thế nào để một mạng nơ-ron nắm bắt sự phụ thuộc giữa mọi từ trong một câu ở khoảng cách bất kỳ mà không cần xử lý tuần tự từng từ một theo thời gian.
- Cầu nối sang Trang 2: Trước khi đi vào cách Transformer giải quyết câu hỏi đó, ta cần xác định rõ dữ liệu đầu vào và đầu ra của bài toán ánh xạ chuỗi được biểu diễn thành các con số như thế nào.
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
transition: slide-up
layout: center
class: text-center
---

# Kết luận

<div class="text-xs text-gray-600 dark:text-gray-300 max-w-2xl mx-auto mt-3 leading-relaxed space-y-2.5 text-left">

<div class="p-3 rounded-lg border border-gray-200 dark:border-gray-800 bg-white/60 dark:bg-gray-900/60">

**Đóng góp Kiến trúc:** Thay thế hoàn toàn cấu trúc đệ quy và tích chập bằng cơ chế Self-Attention, rút ngắn khoảng cách truyền thông tin giữa hai vị trí bất kỳ trong chuỗi về mức hằng số $O(1)$.

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
[Thời lượng: 30s - 1p | Kết luận]
- Mục tiêu vận hành: Tổng hợp chuỗi nhân quả từ đầu bài tới cuối bài thông qua hai chỉ số cốt lõi.
- Cơ chế tổng kết:
  1. Bằng cách thay phép truyền trạng thái ẩn nối tiếp nhau bằng phép nhân ma trận tương quan trực tiếp giữa mọi cặp vị trí, Transformer đưa khoảng cách giao tiếp giữa hai token bất kỳ về đúng 1 bước.
  2. Bằng cách loại bỏ sự phụ thuộc thời gian trong quá trình lan truyền thuận và lan truyền ngược khi huấn luyện, toàn bộ chuỗi văn bản được xử lý đồng thời trên GPU trong 1 bước tuần tự.
- Hướng mở rộng cho phần hỏi đáp:
  1. Từ nhánh Encoder phát triển thành dòng mô hình biểu diễn ngữ cảnh hai chiều như BERT.
  2. Từ nhánh Decoder với Causal Masking phát triển thành dòng mô hình sinh văn bản tự hồi quy quy mô lớn như GPT và LLaMA.
  3. Từ hạn chế bậc hai O(n^2 * d) phát triển thành các kỹ thuật tối ưu như FlashAttention, Sparse Attention và Linear Attention.
-->
