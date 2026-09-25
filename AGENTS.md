# Hướng Dẫn Dành Cho AI Agents & Contributors (AGENTS.md)

Tài liệu này quy định toàn bộ các nguyên tắc kiến trúc, cú pháp và văn phong bắt buộc phải tuân thủ nghiêm ngặt khi tạo, chỉnh sửa hoặc mở rộng nội dung trong dự án slide thuyết trình (Slidev) của repository này.

---

## 1. Quy tắc Thụt lề & Cú pháp Markdown (BẮT BUỘC - CRITICAL)

### ⚠️ Nguyên tắc Sát lề Trái:
> **"Viết sát lề trái (0 indent) hoặc tối đa 2 spaces."**

Mọi thẻ HTML, Vue component, template slots (`<template #left>`, `<template #right>`), và nội dung Markdown lồng nhau **luôn luôn phải được viết sát lề trái** (0 khoảng trắng) hoặc thụt lề tối đa **2 spaces**.

### 🚫 Điều cấm kỵ:
* **TUYỆT ĐỐI KHÔNG dùng phím Tab (`\t`):** Phím Tab tương đương 4 khoảng trắng.
* **TUYỆT ĐỐI KHÔNG thụt lề $\ge 4$ spaces** ở đầu bất kỳ dòng văn bản, thẻ HTML hoặc khối toán học nào.

### 🔍 Nguyên nhân Kỹ thuật (CommonMark / Markdown-it):
* Theo đặc tả chuẩn **CommonMark**, bất kỳ dòng nào bắt đầu bằng **1 Tab (`\t`)** hoặc **4 khoảng trắng (4 spaces)** sẽ tự động bị trình phân tích cú pháp hiểu là một **Indented Code Block** (`<pre><code>...</code></pre>`).
* Trong Slidev (`markdown-it` kết hợp `unplugin-vue-markdown`), Markdown parser chạy **trước** Vue compiler. Nếu thụt lề Tab hoặc $\ge 4$ spaces, parser sẽ đóng gói nội dung đó thành khối code thô trước khi Vue kịp nhận ra thẻ slot hay component, dẫn đến:
  1. Hỏng hoàn toàn cơ chế Slot (`#left`, `#right`).
  2. Công thức $\KaTeX$ (`$...$`) và thẻ danh sách (`<ul><li>`) bị render thành text thô, không định dạng.
  3. Màn hình trắng (blank screen) hoặc lỗi vỡ bố cục giao diện.

### ✅ Ví dụ chuẩn mực (Đúng):
```markdown
<!-- ĐÚNG: Viết sát lề trái (0 indent) -->
<CompareCols leftTitle="Recurrent Architecture" rightTitle="Transformer Architecture">
<template #left>

* Cơ chế lan truyền tuần tự theo thời gian.
* Không thể tính toán song song trên GPU.

</template>

<template #right>

* Cơ chế Self-Attention tính toán đồng thời.
* Tối ưu hóa trên phần cứng tính toán song song.

</template>
</CompareCols>
```

### 🚫 Ví dụ vi phạm (Sai):
```markdown
<!-- SAI: Thụt lề 4 spaces hoặc dùng Tab khiến slot bị hiểu thành code block -->
<CompareCols leftTitle="Recurrent Architecture" rightTitle="Transformer Architecture">
    <template #left>
        <ul>
            <li>Nội dung này sẽ bị biến thành khối code thô!</li>
        </ul>
    </template>
</CompareCols>
```

---

## 2. Quy tắc Dòng Trống cho Công thức Toán ($\KaTeX$) trong HTML/Vue (CRITICAL)

### ⚠️ Nguyên tắc Dòng Trống (Blank Line Requirement):
> **"Bắt buộc phải có một dòng trống (blank line) giữa thẻ HTML / Vue Component và công thức KaTeX hoặc văn bản Markdown."**

### 🔍 Nguyên nhân Kỹ thuật (HTML Block Parsing):
* Theo đặc tả CommonMark, khi một thẻ HTML (`<div>`, `<p>`) hoặc Vue Component (`<MathCard>`, `<InsightBox>`, `<CompareCols>`) được mở, trình biên dịch Markdown/KaTeX **sẽ bị tắt** nếu nội dung dính liền với thẻ.
* Muốn KaTeX (`$...$` hoặc `$$...$$`) render thành công bên trong thẻ HTML hoặc component slot, **bắt buộc phải có ít nhất 1 dòng trống** trước và sau công thức.

### ✅ Ví dụ chuẩn mực (Đúng):
```markdown
<div class="my-container">

Công thức tính trạng thái ẩn:

$$
h_t = \tanh(W x_t + U h_{t-1})
$$

Văn bản diễn giải tiếp theo.

</div>
```

### 🚫 Ví dụ vi phạm (Sai - Biến KaTeX thành chữ thô):
```markdown
<div class="my-container">
Công thức tính trạng thái ẩn:
$$h_t = \tanh(W x_t + U h_{t-1})$$
</div>
```

---

## 3. Quy tắc Hiển thị Khối Mã Nguồn (Code Blocks)

* **Luôn sử dụng Fenced Code Blocks với 3 dấu backticks (` ``` `):**
  ````markdown
  ```python
  import torch
  import torch.nn as nn
  ```
  ````
* **Tuyệt đối không** dựa vào thụt lề 4 khoảng trắng để hiển thị mã nguồn.

---

## 4. Kiến trúc Đa File (Modular Slide Architecture) & Nguyên Lý Điều Hướng

### 📂 Tổ chức Tệp tin:
* **`slides.md` đóng vai trò là Entrypoint:** Chỉ chứa metadata toàn cục (theme, fonts, highlighter, transitions), trang bìa, mục lục và các lệnh import qua cú pháp `src: ./pages/...`.
* **Phân rã nội dung:** Toàn bộ nội dung từng chương phải được tách thành các tệp độc lập trong thư mục `pages/` (ví dụ: `01-motivation.md`, `02-attention.md`,...).

### 🔄 Nguyên lý Điều hướng & Hiệu ứng Chuyển trang:
* **Nguyên lý Trải phẳng Tuyến tính (Linear Flattening):** Slidev tự động nối tuần tự các file con thành một dòng thời gian duy nhất với số trang đánh số liên tục.
* **Quy ước Trục Thị giác:**
  * **Chuyển giữa các chương lớn:** Sử dụng `transition: slide-up` (trượt dọc) để báo hiệu bước chuyển phân đoạn nội dung.
  * **Di chuyển chi tiết trong cùng một chương:** Sử dụng `transition: slide-left` (trượt ngang) để duy trì luồng diễn giải logic liên tục.

---

## 5. Quy chuẩn Bố cục & Giới hạn Chiều cao Slide (Viewport Bounds)

### ⚠️ Giới hạn Khung hình (16:9):
* Chiều cao mặc định của Slidev là 540px. Không gian hiển thị khả dụng sau khi trừ tiêu đề slide và lề trang là khoảng 360px - 400px.
* **Ràng buộc Chiều cao Thẻ:** Mọi component dạng cột so sánh (`<CompareCols />`) hoặc thẻ nội dung lớn phải được giới hạn chiều cao tối đa (`max-h-[350px]`).
* **Định dạng Nội dung Gọn gàng:** Sử dụng padding vừa phải (`p-3`), cỡ chữ compact (`text-[11px]`), khoảng cách dòng hợp lý (`leading-relaxed`) để toàn bộ nội dung nằm gọn hoàn toàn trong khung nhìn, **tuyệt đối không để xảy ra hiện tượng tràn viền hoặc cắt chân giao diện**.

---

## 6. Quy tắc Ngôn ngữ & Văn phong Kỹ thuật (CRITICAL)

### ⚠️ Ngôn ngữ Nhất quán - Tuyệt đối Không Song ngữ:
* **Thuật ngữ kỹ thuật:** Viết 100% bằng tiếng Anh nguyên bản (ví dụ: `Sequential Bottleneck`, `Self-Attention`, `Query`, `Key`, `Value`, `Multi-Head Attention`, `Feed-Forward Network`, `Residual Connection`, `Layer Normalization`, `Causal Masking`, `Gradient Vanishing`, `Batch Size`).
* **Văn bản diễn giải:** Viết 100% bằng tiếng Việt chuẩn mực, rõ ràng, gãy gọn.
* **TUYỆT ĐỐI KHÔNG viết song ngữ mở ngoặc:** Cấm hoàn toàn các dạng `Thuật ngữ (Tiếng Anh)` hoặc `Tiếng Anh (Tiếng Việt)` (ví dụ: cấm viết `Sequential Bottleneck (Điểm nghẽn tuần tự)`).

### ⚠️ Văn phong Trung tính & Học thuật:
* Dùng văn phong báo cáo khoa học, khách quan, chính xác, trang nghiêm.
* **TUYỆT ĐỐI KHÔNG** dùng các từ ngữ giật gân, phóng đại, cảm tính (như *"cốt tử"*, *"chí mạng"*, *"vũ khí"*, *"kinh điển"*, *"tuyên ngôn"*, *"đỉnh cao"*).
* **Không sử dụng dấu chấm than (`!`)** trong toàn bộ nội dung báo cáo học thuật.

---

## 7. Thư viện Component Tái sử dụng (`slidev/components/`)

Tuyệt đối không tạo lại bảng hoặc khung giao diện rời rạc, hãy sử dụng nhất quán 9 component chuyên dụng đã được tối ưu hóa:

| Component | Mục đích Sử dụng | Tham số Cốt lõi (Props) |
| :--- | :--- | :--- |
| `<TensorBadge />` | Hiển thị kích thước tensor với mã màu và hiệu ứng pulse. | `shape`, `name`, `color` (`blue`, `purple`, `amber`, `emerald`, `rose`, `cyan`) |
| `<EmbeddingSum />` | Trực quan hóa phép cộng $Token\ Embedding + Positional\ Encoding$. | `token`, `tokenDim`, `pos`, `posDim`, `note` |
| `<MathCard />` | Khung công thức KaTeX kèm Bản chất (Intuition) và Kỹ thuật (Caveat). | `title`, `badge`, `color`, `intuition`, `caveat` |
| `<ComplexityTable />` | Bảng so sánh độ phức tạp tính toán giữa các tầng theo Table 1 paper gốc. | `highlight` (`self-attention`, `recurrent`, `convolutional`) |
| `<ModuleCard />` | Thẻ module kiến trúc với Invariant và Input/Output shape. | `name`, `type` (`encoder`, `decoder`, `both`), `inputShape`, `outputShape`, `invariant` |
| `<AttentionGrid />` | Ma trận attention weights tương tác và causal masking ($-\infty$). | `mode` (`self`, `causal`), `tokens`, `highlightFrom`, `highlightTo` |
| `<ArchitectureMinimap />` | Bản đồ định vị kiến trúc thu nhỏ, làm nổi bật khối đang trình bày. | `active` (`input`, `pe`, `enc-mha`, `enc-ffn`, `dec-mask`, `dec-cross`, `dec-ffn`, `all`) |
| `<CompareCols />` | Bố cục so sánh 2 cột đối xứng với slots `#left` và `#right`. | `leftTitle`, `rightTitle`, `leftBadge`, `rightBadge`, `leftColor`, `rightColor` |
| `<InsightBox />` | Hộp phân tích kỹ thuật và đào sâu bản chất. | `type` (`insight`, `question`, `warning`, `trick`), `title` |

---

## 8. Quy chuẩn Cú pháp Toán học ($\KaTeX$)

* **Inline Math:** Sử dụng cặp dấu `$ ... $` kẹp sát ký tự, không chứa khoảng trắng ở hai đầu (ví dụ: `$d_{model} = 512$`, `$O(1)$`).
* **Block Math:** Sử dụng cặp dấu `$$ ... $$` nằm trên dòng riêng biệt, **viết sát lề trái và bắt buộc có dòng trống phân cách** với các phần tử xung quanh.
