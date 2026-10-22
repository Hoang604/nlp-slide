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
- Sequential Ops O(1): Thay vòng lặp O(n) theo thời gian của RNN bằng một phép nhân ma trận song song trên GPU.
- Max Path Length O(1): Mọi cặp từ trong câu tương tác trực tiếp ngay tại tầng đầu tiên thay vì đi qua O(n) bước trung gian.
- Complexity O(n^2 * d): Nhanh hơn O(n * d^2) của RNN khi câu ngắn (n < d), nhưng khi chuỗi n rất dài thì ma trận Attention [n x n] tăng bậc hai về bộ nhớ và tính toán.
-->
