---
transition: slide-up
layout: section
class: text-center
---

# PHẦN 03
## Kiến trúc Hoàn chỉnh: Encoder & Decoder
### Positional Encoding, Add & Norm, Feed-Forward Network & Causal Masking

---
transition: slide-left
---

# Tổng thể Kiến trúc Transformer

Transformer sử dụng cấu trúc Encoder-Decoder tự hồi quy gồm $N=6$ tầng giống nhau ở mỗi khối:

<div class="grid grid-cols-3 gap-5 mt-4 items-center">
<div class="col-span-1">
<ArchitectureMinimap active="all" />
</div>

<div class="col-span-2 space-y-3 text-[11px]">
<div class="p-3 rounded-lg border border-blue-500/20 bg-blue-500/5">

**Khối Encoder ($N=6$ tầng)**

Xử lý chuỗi đầu vào theo cơ chế hai chiều:
* 1. Multi-Head Self-Attention.
* 2. Position-wise Feed-Forward Network.

</div>

<div class="p-3 rounded-lg border border-purple-500/20 bg-purple-500/5">

**Khối Decoder ($N=6$ tầng)**

Sinh chuỗi đầu ra theo cơ chế tự hồi quy:
* 1. Masked Multi-Head Attention: giới hạn phạm vi quan sát trong các token quá khứ.
* 2. Cross-Attention: nhận Query từ Decoder, Key và Value từ Encoder Output.
* 3. Position-wise Feed-Forward Network.

</div>

<div class="p-2 rounded-lg bg-emerald-500/10 border border-emerald-500/30 text-emerald-800 dark:text-emerald-300 font-mono text-[10px] text-center">
Kích thước đầu ra của mọi Sub-layer được chuẩn hóa cố định: [B, T, d_model = 512]
</div>
</div>
</div>

---
transition: slide-left
---

# Positional Encoding

<div class="grid grid-cols-2 gap-4">
<div>

<InsightBox type="warning" title="Đặc tính Permutation Invariance">

Toán tử Self-Attention xử lý các phần tử như một tập hợp không có thứ tự:

$$
\text{Attention}(P \cdot X) = P \cdot \text{Attention}(X)
$$

với mọi ma trận hoán vị $P$. Do đó, mô hình cần được bổ sung thông tin vị trí vào chuỗi đầu vào.

</InsightBox>

<div class="mt-3 text-[11px] text-gray-700 dark:text-gray-300 space-y-1">

Hàm vị trí dạng sóng sin và cosin theo công thức:

$$
PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$

$$
PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)
$$

</div>

</div>

<div>
<EmbeddingSum token="Word Embedding" tokenDim="d=512" pos="Positional Encoding" posDim="d=512"
note="Phép cộng từng phần tử bảo toàn kích thước không gian d_model, cho phép mô hình học các mẫu khoảng cách tương đối thông qua phép biến đổi tuyến tính." />
</div>
</div>

---
transition: slide-left
---

# Các Sub-layer trong Khối Encoder

Mỗi tầng Encoder chuẩn hóa quá trình truyền dữ liệu thông qua Residual Connection và Layer Normalization:

<div class="grid grid-cols-2 gap-3.5 mt-2">
<ModuleCard name="Add & Norm" type="both"
inputShape="B × T × 512" outputShape="B × T × 512"
invariant="Tạo đường dẫn gradient trực tiếp qua các tầng sâu, hạn chế hiện tượng suy giảm tín hiệu gradient."
desc="Công thức: Output = LayerNorm(x + Sublayer(x))." />

<ModuleCard name="Feed-Forward Network" type="both"
inputShape="B × T × 512" outputShape="B × T × 512"
invariant="Áp dụng độc lập cho từng vị trí token với kích thước ẩn d_ff = 2048."
desc="Công thức: FFN(x) = max(0, x·W_1 + b_1)·W_2 + b_2." />
</div>

<div class="mt-4 flex justify-between items-center text-[11px]">
<TensorBadge name="Input" shape="B × T × 512" color="blue" />
<span>➔ [Multi-Head Attention] ➔ [Add & Norm] ➔</span>
<TensorBadge name="Intermediate" shape="B × T × 512" color="purple" />
<span>➔ [FFN d_ff=2048] ➔ [Add & Norm] ➔</span>
<TensorBadge name="Encoder Output" shape="B × T × 512" color="emerald" />
</div>

---
transition: slide-left
---

# Decoder: Cơ chế Causal Masking

Trong quá trình sinh văn bản tự hồi quy, tại bước $t$, Decoder chỉ được phép quan sát các token từ $1$ đến $t-1$:

<div class="mt-2">
<AttentionGrid mode="causal" :tokens="['Ngôn', 'ngữ', 'tự', 'nhiên']" />
</div>

<div class="mt-2">
<InsightBox type="trick" title="Cơ chế Triển khai Causal Masking">

Trước khi áp dụng hàm Softmax, ma trận tương quan được cộng thêm giá trị $-\infty$ tại các vị trí $j > i$:

$$
S_{i,j} = \begin{cases} \frac{q_i \cdot k_j}{\sqrt{d_k}} & \text{khi } j \le i \\ -\infty & \text{khi } j > i \end{cases} \implies \text{softmax}(S)_{i,j} = e^{-\infty} = 0
$$

Cơ chế này bảo đảm tính nhân quả của mô hình và cho phép huấn luyện song song toàn bộ chuỗi trên GPU thông qua Teacher Forcing.

</InsightBox>
</div>
