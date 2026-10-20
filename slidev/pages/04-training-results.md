---
transition: slide-left
---

# Ưu nhược điểm & So sánh với Phương pháp khác

<div class="mt-1">
<ComplexityTable highlight="self-attention" />
</div>

<div class="grid grid-cols-2 gap-3 mt-2 text-[10.5px]">
<div class="p-2 rounded-lg border border-emerald-500/30 bg-emerald-500/10 leading-snug">

<div class="font-semibold text-emerald-700 dark:text-emerald-300 text-xs mb-0.5">
Ưu điểm của Kiến trúc Transformer
</div>

* **Song song hóa và tối ưu khi $n < d$:** Giảm số bước tuần tự xuống $O(1)$ trên GPU; chi phí $O(n^2 \cdot d)$ thấp hơn $O(n \cdot d^2)$ của mạng Recurrent.
* **Bảo toàn ngữ cảnh xa:** Đường truyền thông tin tối đa giữa hai vị trí bất kỳ là $O(1)$, vượt trội so với $O(n)$ và $O(\log_k n)$.

</div>

<div class="p-2 rounded-lg border border-rose-500/30 bg-rose-500/10 leading-snug">

<div class="font-semibold text-rose-700 dark:text-rose-300 text-xs mb-0.5">
Nhược điểm và Giới hạn của Phương pháp
</div>

* **Chi phí bậc hai theo độ dài chuỗi:** Khi $n$ rất lớn, ma trận Attention kích thước $n \times n$ gây tốn bộ nhớ và chi phí tính toán $O(n^2 \cdot d)$.
* **Thiếu tính quy nạp về thứ tự:** Toán tử Self-Attention bất biến với phép hoán vị nên bắt buộc phụ thuộc vào Positional Encoding.

</div>
</div>

<!--
[Thời lượng: 1p | Phương pháp 5/5: Ưu nhược điểm & So sánh đánh giá]
- Mục tiêu vận hành: Đọc và đối sánh 3 cột định lượng trong Table 1 của bài báo để chứng minh khi nào Self-Attention thắng tuyệt đối và khi nào nó bộc lộ điểm yếu.
- Cơ chế sinh & Ánh xạ thực thể <-> Bảng độ phức tạp:
  1. Cột Sequential Ops (Số phép toán tuần tự): Mạng Recurrent phải chạy t = 1..n nên tốn O(n) bước tuần tự; trong khi Self-Attention gom toàn bộ câu vào một phép nhân ma trận QK^T nên chỉ tốn O(1) bước tuần tự trên GPU.
  2. Cột Max Path Length (Đường truyền tối đa giữa 2 từ xa nhất): Trên Recurrent, từ thứ 1 muốn tới từ thứ n phải đi qua n bước O(n); trên Convolutional dùng cửa sổ trượt kích thước k phải xếp chồng O(log_k(n)) tầng; còn trên Self-Attention, mọi cặp từ nhân tích vô hướng trực tiếp với nhau ở ngay tầng đầu tiên nên đường truyền là O(1).
  3. Cột Complexity per Layer (Độ phức tạp mỗi tầng — sinh ra cả ưu điểm lẫn nhược điểm):
     - Phép nhân ma trận Q [n, d] với K^T [d, n] tốn O(n^2 * d) phép tính, còn mạng Recurrent nhân ma trận trạng thái U [d, d] qua n bước tốn O(n * d^2).
     - Ưu điểm: Trong các câu văn thông thường, độ dài câu n (khoảng 30 đến 100 token) nhỏ hơn nhiều so với số chiều d = 512, nên O(n^2 * d) thực tế nhẹ hơn và nhanh hơn O(n * d^2) của RNN.
     - Nhược điểm: Khi độ dài chuỗi n tăng lên hàng chục ngàn token (ví dụ xử lý cả cuốn sách), đại lượng n^2 bùng nổ theo hàm bậc hai — tăng gấp 10 lần độ dài chuỗi sẽ làm bộ nhớ lưu ma trận [n, n] tăng gấp 100 lần. Ngoài ra, nếu quá trình suy luận gặp chuỗi dài vượt xa phạm vi Positional Encoding đã học thì chất lượng biểu diễn vị trí cũng suy giảm.
- Cầu nối sang Trang 9: Từ những ưu thế và giới hạn định lượng này, ta đi đến kết luận tổng quát về giá trị của kiến trúc Transformer.
-->
