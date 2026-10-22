---
transition: slide-left
---

# Multi-Head Attention & Causal Masking

<div class="relative w-[860px] h-[368px] mx-auto mt-1 select-none font-mono text-xs">

<!-- Hàng 1: Công thức Toán học đặt trực tiếp ở đỉnh 2 nửa -->
<div class="absolute left-[0px] top-[0px] w-[434px] text-center">
<div class="text-[10px] font-sans font-bold text-purple-700 dark:text-purple-300 tracking-wide">
1. MULTI-HEAD ATTENTION (h = 8, d<sub>k</sub> = d<sub>model</sub> / h = 64)
</div>
<div class="text-[9.5px] text-gray-800 dark:text-gray-200 -mt-3">

$\begin{matrix} \text{MHA}(Q,K,V) = \text{Concat}(\text{head}_1,\dots,\text{head}_h)W^O \\[2pt] \text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V) \end{matrix}$

</div>
</div>

<div class="absolute left-[456px] top-[0px] w-[404px] text-center">
<div class="text-[10px] font-sans font-bold text-rose-700 dark:text-rose-300 tracking-wide">
2. CAUSAL MASKING TẠI DECODER (CHẶN TƯƠNG LAI j &gt; i)
</div>
<div class="text-[10px] text-gray-800 dark:text-gray-200 -mt-3">

$S_{i,j} = \frac{q_i k_j^T}{\sqrt{d_k}} + M_{i,j}, \quad M_{i,j} = \begin{cases} 0 & (j \le i) \\ -\infty & (j > i) \end{cases}$

</div>
</div>

<!-- Lớp SVG: Đường phân cách dọc, mũi tên luồng dữ liệu Multi-Head (Phương án B) và mũi tên Causal Masking -->
<svg width="860" height="368" viewBox="0 0 860 368" class="absolute inset-0 pointer-events-none overflow-visible">
<!-- Đường phân cách dọc giữa 2 nửa -->
<line x1="444" y1="6" x2="444" y2="364" stroke="#d1d5db" stroke-width="1.2" stroke-dasharray="4 4" />

<!-- ==================== CỤM TRÁI (MHA - PHƯƠNG ÁN B) ==================== -->
<!-- 1. Mũi tên từ Ma trận X (215, 92) chẻ 3 nhánh xuống đỉnh Head 1 (x=68), Head 2 (x=200), Head 8 (x=366) tại y=112 -->
<line x1="215" y1="92" x2="215" y2="100" stroke="#6b7280" stroke-width="1.6" />
<line x1="68" y1="100" x2="366" y2="100" stroke="#6b7280" stroke-width="1.6" />
<line x1="68" y1="100" x2="68" y2="107" stroke="#2563eb" stroke-width="1.6" />
<polygon points="68,112 64,105 72,105" fill="#2563eb" />
<line x1="200" y1="100" x2="200" y2="107" stroke="#7c3aed" stroke-width="1.6" />
<polygon points="200,112 196,105 204,105" fill="#7c3aed" />
<line x1="366" y1="100" x2="366" y2="107" stroke="#d97706" stroke-width="1.6" />
<polygon points="366,112 362,105 370,105" fill="#d97706" />

<!-- 2. Luồng nối bên trong HEAD 1 (cx=68: x_Q=30, x_K=68, x_V=106) -->
<!-- Tách 3 nhánh qua W_1^Q, W_1^K, W_1^V xuống Q_1, K_1, V_1 (y=157) -->
<line x1="30" y1="130" x2="106" y2="130" stroke="#3b82f6" stroke-width="1.2" />
<line x1="30" y1="130" x2="30" y2="134" stroke="#3b82f6" stroke-width="1.2" />
<line x1="68" y1="128" x2="68" y2="134" stroke="#3b82f6" stroke-width="1.2" />
<line x1="106" y1="130" x2="106" y2="134" stroke="#3b82f6" stroke-width="1.2" />
<line x1="30" y1="147" x2="30" y2="153" stroke="#2563eb" stroke-width="1.3" />
<polygon points="30,157 27,151 33,151" fill="#2563eb" />
<line x1="68" y1="147" x2="68" y2="153" stroke="#d97706" stroke-width="1.3" />
<polygon points="68,157 65,151 71,151" fill="#d97706" />
<line x1="106" y1="147" x2="106" y2="153" stroke="#0891b2" stroke-width="1.3" />
<polygon points="106,157 103,151 109,151" fill="#0891b2" />
<!-- Q_1 (30,175) và K_1 (68,175) hội tụ (\ /) vào Lưới [3x3] (cx_grid=49, y=190) -->
<line x1="30" y1="175" x2="46" y2="186" stroke="#2563eb" stroke-width="1.3" />
<line x1="68" y1="175" x2="52" y2="186" stroke="#d97706" stroke-width="1.3" />
<polygon points="49,190 45,184 53,184" fill="#2563eb" />
<!-- V_1 (106,175) đi thẳng xuống (|) rồi cùng đáy [3x3] (49,226) hội tụ (\ /) vào head_1 (y=244) -->
<line x1="106" y1="175" x2="106" y2="226" stroke="#0891b2" stroke-width="1.4" />
<line x1="49" y1="226" x2="62" y2="240" stroke="#2563eb" stroke-width="1.4" />
<polygon points="64,244 58,240 64,237" fill="#2563eb" />
<line x1="106" y1="226" x2="76" y2="240" stroke="#0891b2" stroke-width="1.4" />
<polygon points="73,244 74,237 80,240" fill="#0891b2" />

<!-- 3. Luồng nối bên trong HEAD 2 (cx=200: x_Q=162, x_K=200, x_V=238) -->
<line x1="162" y1="130" x2="238" y2="130" stroke="#8b5cf6" stroke-width="1.2" />
<line x1="162" y1="130" x2="162" y2="134" stroke="#8b5cf6" stroke-width="1.2" />
<line x1="200" y1="128" x2="200" y2="134" stroke="#8b5cf6" stroke-width="1.2" />
<line x1="238" y1="130" x2="238" y2="134" stroke="#8b5cf6" stroke-width="1.2" />
<line x1="162" y1="147" x2="162" y2="153" stroke="#2563eb" stroke-width="1.3" />
<polygon points="162,157 159,151 165,151" fill="#2563eb" />
<line x1="200" y1="147" x2="200" y2="153" stroke="#d97706" stroke-width="1.3" />
<polygon points="200,157 197,151 203,151" fill="#d97706" />
<line x1="238" y1="147" x2="238" y2="153" stroke="#0891b2" stroke-width="1.3" />
<polygon points="238,157 235,151 241,151" fill="#0891b2" />
<!-- Q_2 (162,175) và K_2 (200,175) hội tụ (\ /) vào Lưới [3x3] (cx_grid=181, y=190) -->
<line x1="162" y1="175" x2="178" y2="186" stroke="#2563eb" stroke-width="1.3" />
<line x1="200" y1="175" x2="184" y2="186" stroke="#d97706" stroke-width="1.3" />
<polygon points="181,190 177,184 185,184" fill="#7c3aed" />
<!-- V_2 (238,175) đi thẳng xuống (|) rồi cùng đáy [3x3] (181,226) hội tụ (\ /) vào head_2 (y=244) -->
<line x1="238" y1="175" x2="238" y2="226" stroke="#0891b2" stroke-width="1.4" />
<line x1="181" y1="226" x2="194" y2="240" stroke="#7c3aed" stroke-width="1.4" />
<polygon points="196,244 190,240 196,237" fill="#7c3aed" />
<line x1="238" y1="226" x2="208" y2="240" stroke="#0891b2" stroke-width="1.4" />
<polygon points="205,244 206,237 212,240" fill="#0891b2" />

<!-- 4. Luồng nối bên trong HEAD 8 (cx=366: x_Q=328, x_K=366, x_V=404) -->
<line x1="328" y1="130" x2="404" y2="130" stroke="#f59e0b" stroke-width="1.2" />
<line x1="328" y1="130" x2="328" y2="134" stroke="#f59e0b" stroke-width="1.2" />
<line x1="366" y1="128" x2="366" y2="134" stroke="#f59e0b" stroke-width="1.2" />
<line x1="404" y1="130" x2="404" y2="134" stroke="#f59e0b" stroke-width="1.2" />
<line x1="328" y1="147" x2="328" y2="153" stroke="#2563eb" stroke-width="1.3" />
<polygon points="328,157 325,151 331,151" fill="#2563eb" />
<line x1="366" y1="147" x2="366" y2="153" stroke="#d97706" stroke-width="1.3" />
<polygon points="366,157 363,151 369,151" fill="#d97706" />
<line x1="404" y1="147" x2="404" y2="153" stroke="#0891b2" stroke-width="1.3" />
<polygon points="404,157 401,151 407,151" fill="#0891b2" />
<!-- Q_8 (328,175) và K_8 (366,175) hội tụ (\ /) vào Lưới [3x3] (cx_grid=347, y=190) -->
<line x1="328" y1="175" x2="344" y2="186" stroke="#2563eb" stroke-width="1.3" />
<line x1="366" y1="175" x2="350" y2="186" stroke="#d97706" stroke-width="1.3" />
<polygon points="347,190 343,184 351,184" fill="#d97706" />
<!-- V_8 (404,175) đi thẳng xuống (|) rồi cùng đáy [3x3] (347,226) hội tụ (\ /) vào head_8 (y=244) -->
<line x1="404" y1="175" x2="404" y2="226" stroke="#0891b2" stroke-width="1.4" />
<line x1="347" y1="226" x2="360" y2="240" stroke="#d97706" stroke-width="1.4" />
<polygon points="362,244 356,240 362,237" fill="#d97706" />
<line x1="404" y1="226" x2="374" y2="240" stroke="#0891b2" stroke-width="1.4" />
<polygon points="371,244 372,237 378,240" fill="#0891b2" />

<!-- 5. Mũi tên từ 3 Head (y=270) hội tụ xuống thanh Concat (y=284) -->
<line x1="68" y1="270" x2="68" y2="279" stroke="#2563eb" stroke-width="1.6" />
<polygon points="68,284 64,277 72,277" fill="#2563eb" />
<line x1="200" y1="270" x2="200" y2="279" stroke="#7c3aed" stroke-width="1.6" />
<polygon points="200,284 196,277 204,277" fill="#7c3aed" />
<line x1="366" y1="270" x2="366" y2="279" stroke="#d97706" stroke-width="1.6" />
<polygon points="366,284 362,277 370,277" fill="#d97706" />

<!-- 6. Mũi tên từ thanh Concat (y=310) qua phép nhân W^O xuống Đầu ra MHA (y=338) -->
<line x1="215" y1="310" x2="215" y2="332" stroke="#059669" stroke-width="1.8" />
<polygon points="215,338 211,330 219,330" fill="#059669" />

<!-- ==================== CỤM PHẢI (CAUSAL MASKING - CĂN GIỮA DỌC) ==================== -->
<!-- Mũi tên chẻ 4 nhánh từ Hàng 2 (q_2, y=182) của ma trận 4x4 sang 4 ô cột Row 2 (x=648) -->
<line x1="614" y1="182" x2="630" y2="182" stroke="#2563eb" stroke-width="1.8" />
<circle cx="630" cy="182" r="2.5" fill="#2563eb" />
<!-- Trục dọc nửa trên (j <= 2: hợp lệ, nét liền màu xanh) -->
<line x1="630" y1="130" x2="630" y2="182" stroke="#2563eb" stroke-width="1.8" />
<line x1="630" y1="130" x2="641" y2="130" stroke="#2563eb" stroke-width="1.8" />
<polygon points="647,130 640,126 640,134" fill="#2563eb" />
<line x1="630" y1="180" x2="641" y2="180" stroke="#2563eb" stroke-width="1.8" />
<polygon points="647,180 640,176 640,184" fill="#2563eb" />
<!-- Trục dọc nửa dưới (j > 2: tương lai bị chặn = 0, nét đứt màu đỏ hồng) -->
<line x1="630" y1="182" x2="630" y2="280" stroke="#e11d48" stroke-width="1.6" stroke-dasharray="3 2" />
<line x1="630" y1="230" x2="641" y2="230" stroke="#e11d48" stroke-width="1.6" stroke-dasharray="3 2" />
<polygon points="647,230 640,226 640,234" fill="#e11d48" />
<line x1="630" y1="280" x2="641" y2="280" stroke="#e11d48" stroke-width="1.6" stroke-dasharray="3 2" />
<polygon points="647,280 640,276 640,284" fill="#e11d48" />

<!-- Đường gộp 4 hàng v_1..v_4 sang phải vào Vector tổng hợp z_2 -->
<polyline points="770,130 780,130 780,180 770,180" fill="none" stroke="#4b5563" stroke-width="1.8" />
<polyline points="780,180 780,280 770,280" fill="none" stroke="#9ca3af" stroke-width="1.4" stroke-dasharray="3 2" />
<line x1="770" y1="230" x2="780" y2="230" stroke="#9ca3af" stroke-width="1.4" stroke-dasharray="3 2" />
<line x1="780" y1="205" x2="787" y2="205" stroke="#4b5563" stroke-width="1.8" />
<polygon points="793,205 786,201 786,209" fill="#4b5563" />
</svg>

<!-- ==================== NỬA TRÁI: MULTI-HEAD ATTENTION (PHƯƠNG ÁN B) ==================== -->

<!-- Tầng 1: Khối Ma trận Đầu vào X [T × 512] -->
<div class="absolute left-[105px] top-[62px] w-[220px] border border-gray-600 dark:border-gray-300 divide-y divide-gray-500 bg-gray-500/10 text-[9px]">
<div class="h-[14px] flex items-center justify-between px-2 font-bold text-gray-800 dark:text-gray-200">
<span>x₁ … x<sub>T</sub></span><span class="font-normal text-gray-500">d<sub>model</sub> = 512</span>
</div>
<div class="h-[15px] flex items-center justify-center px-2 font-bold text-gray-700 dark:text-gray-300 bg-gray-500/10">
<span>Đầu vào X [T × 512]</span>
</div>
</div>

<!-- Tầng 2: 3 Khối Head chi tiết theo Phương án B (×W_i^{Q,K,V} -> Q_i, K_i, V_i -> [3×3] × V_i -> head_i) -->

<!-- ==================== HEAD 1 (left=6, width=124, cx=68) ==================== -->
<div class="absolute left-[6px] top-[112px] w-[124px] h-[158px] border border-blue-600 dark:border-blue-400 bg-blue-500/5"></div>
<div class="absolute left-[6px] top-[112px] w-[124px] h-[16px] bg-blue-600/20 border-b border-blue-600/50 flex items-center justify-center text-[8.5px] font-bold text-blue-900 dark:text-blue-200">
Head 1 (d<sub>k</sub> = 64)
</div>
<!-- Nhãn phép chiếu ×W_1^Q, ×W_1^K, ×W_1^V -->
<div class="absolute left-[11px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-blue-700 dark:text-blue-300">×W₁<sup>Q</sup></div>
<div class="absolute left-[49px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-amber-700 dark:text-amber-300">×W₁<sup>K</sup></div>
<div class="absolute left-[87px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-cyan-700 dark:text-cyan-300">×W₁<sup>V</sup></div>
<!-- 3 Ô Q_1, K_1, V_1 -->
<div class="absolute left-[16px] top-[157px] w-[28px] h-[18px] border border-blue-600 bg-blue-500/25 flex items-center justify-center text-[8.5px] font-bold text-blue-950 dark:text-blue-100">Q₁</div>
<div class="absolute left-[54px] top-[157px] w-[28px] h-[18px] border border-amber-600 bg-amber-500/25 flex items-center justify-center text-[8.5px] font-bold text-amber-950 dark:text-amber-100">K₁</div>
<div class="absolute left-[92px] top-[157px] w-[28px] h-[18px] border border-cyan-600 bg-cyan-500/25 flex items-center justify-center text-[8.5px] font-bold text-cyan-950 dark:text-cyan-100">V₁</div>
<!-- Lưới 3x3 (cx=49 -> left=31, top=190, 36x36) và dấu × với nhánh V_1 -->
<div class="absolute left-[31px] top-[190px] grid grid-cols-3 border border-blue-600/80 divide-x divide-blue-600/80">
<div class="w-3 h-3 bg-blue-600"></div><div class="w-3 h-3 bg-blue-500/30"></div><div class="w-3 h-3 bg-blue-500/10"></div>
<div class="w-3 h-3 bg-blue-500/20 border-t border-blue-600/80"></div><div class="w-3 h-3 bg-blue-600/85 border-t border-blue-600/80"></div><div class="w-3 h-3 bg-blue-500/25 border-t border-blue-600/80"></div>
<div class="w-3 h-3 bg-blue-500/10 border-t border-blue-600/80"></div><div class="w-3 h-3 bg-blue-500/35 border-t border-blue-600/80"></div><div class="w-3 h-3 bg-blue-600/90 border-t border-blue-600/80"></div>
</div>
<div class="absolute left-[75px] top-[200px] w-[16px] text-center text-[9.5px] font-bold text-gray-600 dark:text-gray-300">×</div>
<!-- Khối kết quả head_1 [T×64] -->
<div class="absolute left-[14px] top-[244px] w-[108px] h-[20px] bg-blue-500/25 border border-blue-600 flex items-center justify-center text-[8.5px] font-bold text-blue-950 dark:text-blue-100">
head₁ [T×64]
</div>

<!-- ==================== HEAD 2 (left=138, width=124, cx=200) ==================== -->
<div class="absolute left-[138px] top-[112px] w-[124px] h-[158px] border border-purple-600 dark:border-purple-400 bg-purple-500/5"></div>
<div class="absolute left-[138px] top-[112px] w-[124px] h-[16px] bg-purple-600/20 border-b border-purple-600/50 flex items-center justify-center text-[8.5px] font-bold text-purple-900 dark:text-purple-200">
Head 2 (d<sub>k</sub> = 64)
</div>
<!-- Nhãn phép chiếu ×W_2^Q, ×W_2^K, ×W_2^V -->
<div class="absolute left-[143px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-blue-700 dark:text-blue-300">×W₂<sup>Q</sup></div>
<div class="absolute left-[181px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-amber-700 dark:text-amber-300">×W₂<sup>K</sup></div>
<div class="absolute left-[219px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-cyan-700 dark:text-cyan-300">×W₂<sup>V</sup></div>
<!-- 3 Ô Q_2, K_2, V_2 -->
<div class="absolute left-[148px] top-[157px] w-[28px] h-[18px] border border-blue-600 bg-blue-500/25 flex items-center justify-center text-[8.5px] font-bold text-blue-950 dark:text-blue-100">Q₂</div>
<div class="absolute left-[186px] top-[157px] w-[28px] h-[18px] border border-amber-600 bg-amber-500/25 flex items-center justify-center text-[8.5px] font-bold text-amber-950 dark:text-amber-100">K₂</div>
<div class="absolute left-[224px] top-[157px] w-[28px] h-[18px] border border-cyan-600 bg-cyan-500/25 flex items-center justify-center text-[8.5px] font-bold text-cyan-950 dark:text-cyan-100">V₂</div>
<!-- Lưới 3x3 (cx=181 -> left=163, top=190, 36x36) và dấu × với nhánh V_2 -->
<div class="absolute left-[163px] top-[190px] grid grid-cols-3 border border-purple-600/80 divide-x divide-purple-600/80">
<div class="w-3 h-3 bg-purple-500/20"></div><div class="w-3 h-3 bg-purple-600/90"></div><div class="w-3 h-3 bg-purple-500/30"></div>
<div class="w-3 h-3 bg-purple-600/80 border-t border-purple-600/80"></div><div class="w-3 h-3 bg-purple-500/15 border-t border-purple-600/80"></div><div class="w-3 h-3 bg-purple-500/45 border-t border-purple-600/80"></div>
<div class="w-3 h-3 bg-purple-500/35 border-t border-purple-600/80"></div><div class="w-3 h-3 bg-purple-600/85 border-t border-purple-600/80"></div><div class="w-3 h-3 bg-purple-500/20 border-t border-purple-600/80"></div>
</div>
<div class="absolute left-[207px] top-[200px] w-[16px] text-center text-[9.5px] font-bold text-gray-600 dark:text-gray-300">×</div>
<!-- Khối kết quả head_2 [T×64] -->
<div class="absolute left-[146px] top-[244px] w-[108px] h-[20px] bg-purple-500/25 border border-purple-600 flex items-center justify-center text-[8.5px] font-bold text-purple-950 dark:text-purple-100">
head₂ [T×64]
</div>

<!-- Dấu ... giữa Head 2 và Head 8 -->
<div class="absolute left-[264px] top-[180px] w-[38px] text-center text-base font-bold text-gray-400">...</div>

<!-- ==================== HEAD 8 (left=304, width=124, cx=366) ==================== -->
<div class="absolute left-[304px] top-[112px] w-[124px] h-[158px] border border-amber-600 dark:border-amber-400 bg-amber-500/5"></div>
<div class="absolute left-[304px] top-[112px] w-[124px] h-[16px] bg-amber-600/20 border-b border-amber-600/50 flex items-center justify-center text-[8.5px] font-bold text-amber-900 dark:text-amber-200">
Head 8 (d<sub>k</sub> = 64)
</div>
<!-- Nhãn phép chiếu ×W_8^Q, ×W_8^K, ×W_8^V -->
<div class="absolute left-[309px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-blue-700 dark:text-blue-300">×W₈<sup>Q</sup></div>
<div class="absolute left-[347px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-amber-700 dark:text-amber-300">×W₈<sup>K</sup></div>
<div class="absolute left-[385px] top-[133px] w-[38px] text-center text-[7.5px] font-bold text-cyan-700 dark:text-cyan-300">×W₈<sup>V</sup></div>
<!-- 3 Ô Q_8, K_8, V_8 -->
<div class="absolute left-[314px] top-[157px] w-[28px] h-[18px] border border-blue-600 bg-blue-500/25 flex items-center justify-center text-[8.5px] font-bold text-blue-950 dark:text-blue-100">Q₈</div>
<div class="absolute left-[352px] top-[157px] w-[28px] h-[18px] border border-amber-600 bg-amber-500/25 flex items-center justify-center text-[8.5px] font-bold text-amber-950 dark:text-amber-100">K₈</div>
<div class="absolute left-[390px] top-[157px] w-[28px] h-[18px] border border-cyan-600 bg-cyan-500/25 flex items-center justify-center text-[8.5px] font-bold text-cyan-950 dark:text-cyan-100">V₈</div>
<!-- Lưới 3x3 (cx=347 -> left=329, top=190, 36x36) và dấu × với nhánh V_8 -->
<div class="absolute left-[329px] top-[190px] grid grid-cols-3 border border-amber-600/80 divide-x divide-amber-600/80">
<div class="w-3 h-3 bg-amber-500/15"></div><div class="w-3 h-3 bg-amber-500/30"></div><div class="w-3 h-3 bg-amber-600/90"></div>
<div class="w-3 h-3 bg-amber-500/40 border-t border-amber-600/80"></div><div class="w-3 h-3 bg-amber-500/20 border-t border-amber-600/80"></div><div class="w-3 h-3 bg-amber-600/80 border-t border-amber-600/80"></div>
<div class="w-3 h-3 bg-amber-600/85 border-t border-amber-600/80"></div><div class="w-3 h-3 bg-amber-500/25 border-t border-amber-600/80"></div><div class="w-3 h-3 bg-amber-500/15 border-t border-amber-600/80"></div>
</div>
<div class="absolute left-[373px] top-[200px] w-[16px] text-center text-[9.5px] font-bold text-gray-600 dark:text-gray-300">×</div>
<!-- Khối kết quả head_8 [T×64] -->
<div class="absolute left-[312px] top-[244px] w-[108px] h-[20px] bg-amber-500/25 border border-amber-600 flex items-center justify-center text-[8.5px] font-bold text-amber-950 dark:text-amber-100">
head₈ [T×64]
</div>

<!-- Tầng 3: Thanh Concat ghép ngang 8 Head (8 × 64 = 512) -->
<div class="absolute left-[6px] top-[284px] w-[422px] h-[26px] border border-gray-600 dark:border-gray-300 flex overflow-hidden text-[8.5px] font-bold">
<div class="w-[29%] bg-blue-500/30 border-r border-gray-500 flex items-center justify-center text-blue-900 dark:text-blue-100">
head₁ (64)
</div>
<div class="w-[29%] bg-purple-500/30 border-r border-gray-500 flex items-center justify-center text-purple-900 dark:text-purple-100">
head₂ (64)
</div>
<div class="w-[13%] bg-gray-500/15 border-r border-gray-500 flex items-center justify-center text-gray-500">
...
</div>
<div class="w-[29%] bg-amber-500/30 flex items-center justify-center text-amber-900 dark:text-amber-100">
head₈ (64)
</div>
</div>

<!-- Khối phép nhân W^O [512 × 512] -->
<div class="absolute left-[76px] top-[314px] text-[8.5px] font-bold text-gray-500">
Concat [T×512]
</div>
<div class="absolute left-[222px] top-[312px] px-1.5 py-0.5 border border-emerald-600 bg-emerald-500/15 text-[8.5px] font-bold text-emerald-800 dark:text-emerald-200">
× W<sup>O</sup> [512×512]
</div>

<!-- Tầng 4: Khối Ma trận Đầu ra MHA(Q, K, V) [T × 512] -->
<div class="absolute left-[105px] top-[338px] w-[220px] border-2 border-emerald-600 dark:border-emerald-400 divide-y divide-emerald-600/60 text-[9px]">
<div class="h-[14px] flex items-center justify-between px-2 font-bold text-emerald-950 dark:text-emerald-100 bg-gradient-to-r from-blue-500/30 via-purple-500/30 to-amber-500/30">
<span>z₁ … z<sub>T</sub></span><span class="font-normal">d<sub>model</sub> = 512</span>
</div>
<div class="h-[15px] flex items-center justify-center px-2 font-bold text-emerald-950 dark:text-emerald-100 bg-gradient-to-r from-blue-500/20 via-purple-500/20 to-amber-500/20">
<span>MHA(Q, K, V) [T × 512]</span>
</div>
</div>

<!-- ==================== NỬA PHẢI: CAUSAL MASKING (CẤU TRÚC KHỐI 3 - 4 Ô) ==================== -->

<!-- Cụm 1: Khối Q (trái) và K^T (trên) bao quanh lưới 4x4 Softmax(QK^T / √d_k + M) -->
<!-- 4 ô cột K^T phía trên -->
<div class="absolute left-[486px] top-[94px] w-[128px] grid grid-cols-4 gap-0.5 text-center text-[8.5px] font-bold">
<div class="h-6 border border-amber-600 bg-amber-500/20 flex items-center justify-center text-amber-800 dark:text-amber-200">k₁</div>
<div class="h-6 border border-amber-600 bg-amber-500/20 flex items-center justify-center text-amber-800 dark:text-amber-200">k₂</div>
<div class="h-6 border border-amber-600 bg-amber-500/20 flex items-center justify-center text-amber-800 dark:text-amber-200">k₃</div>
<div class="h-6 border border-amber-600 bg-amber-500/20 flex items-center justify-center text-amber-800 dark:text-amber-200">k₄</div>
</div>
<div class="absolute left-[486px] top-[120px] w-[128px] grid grid-cols-4 text-center text-[8px] text-gray-400">
<span>↓</span><span>↓</span><span>↓</span><span>↓</span>
</div>

<!-- 4 ô hàng Q bên trái -->
<div class="absolute left-[450px] top-[134px] w-[24px] border border-blue-600 dark:border-blue-400 divide-y divide-blue-600 text-[8.5px] font-bold">
<div class="h-8 bg-blue-500/15 flex items-center justify-center text-blue-800 dark:text-blue-200">q₁</div>
<div class="h-8 bg-blue-500/35 flex items-center justify-center text-blue-950 dark:text-white">q₂</div>
<div class="h-8 bg-blue-500/15 flex items-center justify-center text-blue-800 dark:text-blue-200">q₃</div>
<div class="h-8 bg-blue-500/15 flex items-center justify-center text-blue-800 dark:text-blue-200">q₄</div>
</div>
<div class="absolute left-[475px] top-[134px] w-[10px] divide-y divide-transparent text-[8px] text-gray-400 text-center">
<div class="h-8 flex items-center justify-center">→</div>
<div class="h-8 flex items-center justify-center">→</div>
<div class="h-8 flex items-center justify-center">→</div>
<div class="h-8 flex items-center justify-center">→</div>
</div>

<!-- Lưới 4x4: Tam giác dưới màu xanh (trọng số hợp lệ), Tam giác trên gán -∞ ➔ 0 -->
<div class="absolute left-[486px] top-[134px] border border-gray-500 dark:border-gray-400 divide-y divide-gray-500 dark:divide-gray-400 text-[8px]">
<!-- Hàng q_1 -->
<div class="grid grid-cols-4 divide-x divide-gray-500 dark:divide-gray-400 opacity-55">
<div class="w-[32px] h-8 bg-blue-600"></div>
<div class="w-[32px] h-8 bg-rose-500/15 flex items-center justify-center text-rose-700 dark:text-rose-300 font-bold">-∞→0</div>
<div class="w-[32px] h-8 bg-rose-500/15 flex items-center justify-center text-rose-700 dark:text-rose-300 font-bold">-∞→0</div>
<div class="w-[32px] h-8 bg-rose-500/15 flex items-center justify-center text-rose-700 dark:text-rose-300 font-bold">-∞→0</div>
</div>
<!-- Hàng q_2 (Đóng khung nổi bật, chẻ 4 nhánh sang phải) -->
<div class="grid grid-cols-4 divide-x-2 divide-blue-600 dark:divide-blue-400 ring-2 ring-blue-600 dark:ring-blue-400 relative z-10">
<div class="w-[32px] h-8 bg-blue-500/45"></div>
<div class="w-[32px] h-8 bg-blue-600/85"></div>
<div class="w-[32px] h-8 bg-rose-500/20 flex items-center justify-center text-rose-700 dark:text-rose-300 font-bold">-∞→0</div>
<div class="w-[32px] h-8 bg-rose-500/20 flex items-center justify-center text-rose-700 dark:text-rose-300 font-bold">-∞→0</div>
</div>
<!-- Hàng q_3 -->
<div class="grid grid-cols-4 divide-x divide-gray-500 dark:divide-gray-400 opacity-55">
<div class="w-[32px] h-8 bg-blue-500/25"></div>
<div class="w-[32px] h-8 bg-blue-500/45"></div>
<div class="w-[32px] h-8 bg-blue-600/75"></div>
<div class="w-[32px] h-8 bg-rose-500/15 flex items-center justify-center text-rose-700 dark:text-rose-300 font-bold">-∞→0</div>
</div>
<!-- Hàng q_4 -->
<div class="grid grid-cols-4 divide-x divide-gray-500 dark:divide-gray-400 opacity-55">
<div class="w-[32px] h-8 bg-blue-500/15"></div>
<div class="w-[32px] h-8 bg-blue-500/30"></div>
<div class="w-[32px] h-8 bg-blue-500/45"></div>
<div class="w-[32px] h-8 bg-blue-600/80"></div>
</div>
</div>
<div class="absolute left-[476px] top-[270px] w-[148px] text-center text-[9px] font-sans text-gray-500">
Softmax(QKᵀ/√d<sub>k</sub> + M)
</div>

<!-- Cụm 2: Cột Row 2 (4 ô) × Ma trận V (4 thanh v_1, v_2, v_3, v_4) -->
<div class="absolute left-[641px] top-[94px] w-[44px] text-center text-[9.5px] font-sans text-gray-500">
Row 2
</div>
<div class="absolute left-[692px] top-[94px] w-[78px] text-center text-[9.5px] font-sans text-gray-500">
Ma trận V
</div>

<!-- Hàng 1: Ô 1 (Vừa) × v_1 (Quá khứ - Hợp lệ) -->
<div class="absolute left-[648px] top-[114px] w-[30px] h-8 border-2 border-blue-600 dark:border-blue-400 bg-blue-500/45"></div>
<div class="absolute left-[678px] top-[114px] w-3.5 h-8 flex items-center justify-center font-bold text-gray-500">×</div>
<div class="absolute left-[692px] top-[114px] w-[78px] h-8 border border-gray-600 dark:border-gray-300 bg-cyan-500 flex items-center justify-center font-bold text-white">
v₁
</div>

<div class="absolute left-[692px] top-[146px] w-[78px] h-[18px] flex items-center justify-center font-bold text-gray-500">+</div>

<!-- Hàng 2: Ô 2 (Đậm) × v_2 (Hiện tại - Hợp lệ) -->
<div class="absolute left-[648px] top-[164px] w-[30px] h-8 border-2 border-blue-600 dark:border-blue-400 bg-blue-600/85"></div>
<div class="absolute left-[678px] top-[164px] w-3.5 h-8 flex items-center justify-center font-bold text-gray-500">×</div>
<div class="absolute left-[692px] top-[164px] w-[78px] h-8 border border-gray-600 dark:border-gray-300 bg-amber-500/80 flex items-center justify-center font-bold text-white">
v₂
</div>

<div class="absolute left-[692px] top-[196px] w-[78px] h-[18px] flex items-center justify-center font-bold text-gray-400">+</div>

<!-- Hàng 3: Ô 3 (= 0) × v_3 (Tương lai - Bị triệt tiêu) -->
<div class="absolute left-[648px] top-[214px] w-[30px] h-8 border-2 border-rose-500 dark:border-rose-400 bg-rose-500/10 flex items-center justify-center font-bold text-rose-600 dark:text-rose-400">
0
</div>
<div class="absolute left-[678px] top-[214px] w-3.5 h-8 flex items-center justify-center font-bold text-gray-400">×</div>
<div class="absolute left-[692px] top-[214px] w-[78px] h-8 border border-dashed border-gray-400 dark:border-gray-600 bg-gray-200/40 dark:bg-gray-800/40 flex items-center justify-center font-bold text-gray-400 line-through">
v₃
</div>

<div class="absolute left-[692px] top-[246px] w-[78px] h-[18px] flex items-center justify-center font-bold text-gray-400">+</div>

<!-- Hàng 4: Ô 4 (= 0) × v_4 (Tương lai - Bị triệt tiêu) -->
<div class="absolute left-[648px] top-[264px] w-[30px] h-8 border-2 border-rose-500 dark:border-rose-400 bg-rose-500/10 flex items-center justify-center font-bold text-rose-600 dark:text-rose-400">
0
</div>
<div class="absolute left-[678px] top-[264px] w-3.5 h-8 flex items-center justify-center font-bold text-gray-400">×</div>
<div class="absolute left-[692px] top-[264px] w-[78px] h-8 border border-dashed border-gray-400 dark:border-gray-600 bg-gray-200/40 dark:bg-gray-800/40 flex items-center justify-center font-bold text-gray-400 line-through">
v₄
</div>

<!-- Cụm 3: Vector tổng hợp z_2 đặt ở bên phải (Chỉ chứa màu của v_1 và v_2) -->
<div class="absolute left-[786px] top-[156px] w-[74px] text-center text-[9px] font-sans font-semibold text-gray-600 dark:text-gray-300 leading-tight">
Vector<br/>tổng hợp
</div>
<div class="absolute left-[794px] top-[189px] w-[66px] h-8 border-2 border-gray-800 dark:border-white overflow-hidden flex items-center justify-center whitespace-nowrap">
<div class="absolute inset-0 flex flex-col">
<div class="bg-cyan-500" style="height: 35%"></div>
<div class="bg-amber-500" style="height: 65%"></div>
</div>
<span class="relative z-10 font-bold text-white drop-shadow text-[9.5px] tracking-tighter">z₂ (v₁,v₂)</span>
</div>

</div>

<!--
- Multi-Head Attention (Nửa trái):
  + Cơ chế: Thay vì tính Attention 1 lần trên không gian 512 chiều, mô hình dùng h = 8 bộ ma trận W_i^Q, W_i^K, W_i^V [512 × 64] chiếu đầu vào X sang 8 không gian con độc lập (d_k = 64) để bắt đồng thời nhiều mối quan hệ ngữ pháp/ngữ nghĩa. Khi lập trình, nhân X với ma trận ghép [512 × 512] một lần rồi cắt thành 8 phần để tính song song; cuối cùng Concat ghép ngang (8 × 64 = 512) và nhân W^O [512 × 512] để trộn thông tin giữa 8 nhánh.
  + Chi phí FLOPs vs. Bộ nhớ (RAM/VRAM): Số phép nhân ma trận (FLOPs) của Q_i × K_i^T và A_i × V_i trên 8 Head là 8 × (T^2 × 64) = T^2 × 512, tương đương Single-Head 512 chiều (chỉ thêm phép nhân W^O). Tuy nhiên, vì chiều d_k = 64 bị triệt tiêu khi nhân Q_i × K_i^T, mô hình phải tạo và lưu 8 ma trận Attention Weights [T × T] riêng biệt thay vì 1 ma trận, làm tốn gấp 8 lần RAM/VRAM (B × 8 × T^2 phần tử, đặc biệt nặng khi lưu Activation cho pha Backward) và gấp 8 lần chi phí chạy hàm Softmax.
- Causal Masking tại Decoder (Nửa phải - chỉ đặt tại Masked Self-Attention đáy Decoder, còn Cross-Attention mở toàn bộ [m × n]):
  + Cơ chế trực quan: Tại vị trí i (ví dụ hàng q_2), chỉ các vị trí quá khứ và hiện tại (j ≤ i) được giữ trọng số; vùng tam giác trên (j > i) bị cộng mặt nạ M_{i,j} = -∞ trước Softmax để triệt tiêu thành e^{-∞} = 0 (làm v_3, v_4 nhân với 0 và bị gạch bỏ).
  + Pha Training (Teacher Forcing): Đưa cả câu đích vào cùng lúc, dùng mặt nạ tam giác trên chặn nhìn trước tương lai, gán nhãn đáp án ở đỉnh lệch 1 nấc (hàng i dự đoán từ i + 1) để huấn luyện song song toàn chuỗi.
  + Pha Inference (Autoregressive): Khởi đầu bằng [<SOS>], chạy vòng lặp lấy hàng cuối cùng ở đỉnh qua Linear + Softmax để sinh từ tiếp theo và nối vào đáy cho tới khi gặp <EOS>.
-->

---
transition: slide-left
---

# Cấu trúc Bên trong Tầng Encoder & Decoder

<div class="relative w-[860px] h-[365px] mx-auto mt-2 select-none font-mono text-xs">

<!-- Hàng 1: Công thức Toán học đặt trực tiếp trên 2 Sub-layer (Không bọc border thừa) -->
<div class="absolute left-[0px] top-[0px] w-[310px] text-center">
<div class="text-[10px] font-sans font-bold text-purple-700 dark:text-purple-300 uppercase tracking-wider mb-0.5">
Sub-layer 1: Multi-Head Attention
</div>
<div class="text-xs text-gray-800 dark:text-gray-200">

$h_l = \text{LayerNorm}\big(z_{l-1} + \text{MHA}(z_{l-1})\big)$

</div>
</div>

<div class="absolute left-[330px] top-[0px] w-[530px] text-center">
<div class="text-[10px] font-sans font-bold text-cyan-700 dark:text-cyan-300 uppercase tracking-wider mb-0.5">
Sub-layer 2: Position-wise Feed-Forward Network
</div>
<div class="text-xs text-gray-800 dark:text-gray-200">

$z_l = \text{LayerNorm}\big(h_l + \text{FFN}(h_l)\big), \quad \text{FFN}(h_i) = \max(0, h_i W_1 + b_1)W_2 + b_2$

</div>
</div>

<!-- Lớp SVG vẽ toàn bộ các mũi tên được ngắt đoạn chính xác tại mép từng khối -->
<svg width="860" height="365" viewBox="0 0 860 365" class="absolute inset-0 pointer-events-none overflow-visible">
<!-- Cầu nối tắt Residual 1: từ z_{l-1} (x=22, y=126) vòng qua MHA tới bộ cộng (+) số 1 (x=208, y=131) -->
<polyline points="22,126 22,90 208,90 208,124" fill="none" stroke="#10b981" stroke-width="1.8" stroke-dasharray="4 2" />
<polygon points="208,130 204,122 212,122" fill="#10b981" />

<!-- Cầu nối tắt Residual 2: từ h_l (x=342, y=126) vòng qua FFN tới bộ cộng (+) số 2 (x=728, y=131) -->
<polyline points="342,126 342,90 728,90 728,124" fill="none" stroke="#10b981" stroke-width="1.8" stroke-dasharray="4 2" />
<polygon points="728,130 724,122 732,122" fill="#10b981" />

<!-- Mũi tên ngang ở 3 làn: Làn 1 (y=146), Làn 2 (y=222), Làn 3 (y=298) -->
<!-- Từ Input z_{l-1} (x=44) vào MHA (x=66) -->
<line x1="44" y1="146" x2="66" y2="146" stroke="#6b7280" stroke-width="1.6" />
<line x1="44" y1="222" x2="66" y2="222" stroke="#6b7280" stroke-width="1.6" />
<line x1="44" y1="298" x2="66" y2="298" stroke="#6b7280" stroke-width="1.6" />

<!-- Bên trong MHA (x=76..162): các đường đan chéo biểu diễn trộn thông tin giữa 3 vector -->
<line x1="76" y1="146" x2="162" y2="146" stroke="#9333ea" stroke-width="1.8" />
<line x1="76" y1="146" x2="162" y2="222" stroke="#9333ea" stroke-width="1.4" opacity="0.65" />
<line x1="76" y1="146" x2="162" y2="298" stroke="#9333ea" stroke-width="1.4" opacity="0.45" />
<line x1="76" y1="222" x2="162" y2="146" stroke="#9333ea" stroke-width="1.4" opacity="0.65" />
<line x1="76" y1="222" x2="162" y2="222" stroke="#9333ea" stroke-width="1.8" />
<line x1="76" y1="222" x2="162" y2="298" stroke="#9333ea" stroke-width="1.4" opacity="0.65" />
<line x1="76" y1="298" x2="162" y2="146" stroke="#9333ea" stroke-width="1.4" opacity="0.45" />
<line x1="76" y1="298" x2="162" y2="222" stroke="#9333ea" stroke-width="1.4" opacity="0.65" />
<line x1="76" y1="298" x2="162" y2="298" stroke="#9333ea" stroke-width="1.8" />

<!-- Từ MHA (x=172) -> (+) (x=194..222) -> Norm 1 (x=236..292) -> h_l (x=318) -->
<line x1="172" y1="146" x2="194" y2="146" stroke="#6b7280" stroke-width="1.6" />
<line x1="222" y1="146" x2="236" y2="146" stroke="#6b7280" stroke-width="1.6" />
<line x1="292" y1="146" x2="311" y2="146" stroke="#6b7280" stroke-width="1.6" />
<polygon points="317,146 310,142 310,150" fill="#6b7280" />

<line x1="172" y1="222" x2="194" y2="222" stroke="#6b7280" stroke-width="1.6" />
<line x1="222" y1="222" x2="236" y2="222" stroke="#6b7280" stroke-width="1.6" />
<line x1="292" y1="222" x2="311" y2="222" stroke="#6b7280" stroke-width="1.6" />
<polygon points="317,222 310,218 310,226" fill="#6b7280" />

<line x1="172" y1="298" x2="194" y2="298" stroke="#6b7280" stroke-width="1.6" />
<line x1="222" y1="298" x2="236" y2="298" stroke="#6b7280" stroke-width="1.6" />
<line x1="292" y1="298" x2="311" y2="298" stroke="#6b7280" stroke-width="1.6" />
<polygon points="317,298 310,294 310,302" fill="#6b7280" />

<!-- Làn 1 (Cyan): h_1 (x=366) -> FFN (x=388..696) -> (+) (x=714..742) -> Norm 2 (x=752..802) -> z_1 (x=818) -->
<line x1="366" y1="146" x2="388" y2="146" stroke="#0891b2" stroke-width="1.8" />
<line x1="696" y1="146" x2="714" y2="146" stroke="#0891b2" stroke-width="1.8" />
<line x1="742" y1="146" x2="752" y2="146" stroke="#0891b2" stroke-width="1.8" />
<line x1="802" y1="146" x2="811" y2="146" stroke="#0891b2" stroke-width="1.8" />
<polygon points="817,146 810,142 810,150" fill="#0891b2" />

<!-- Làn 2 (Amber): h_2 -> FFN -> (+) -> Norm 2 -> z_2 -->
<line x1="366" y1="222" x2="388" y2="222" stroke="#d97706" stroke-width="1.8" />
<line x1="696" y1="222" x2="714" y2="222" stroke="#d97706" stroke-width="1.8" />
<line x1="742" y1="222" x2="752" y2="222" stroke="#d97706" stroke-width="1.8" />
<line x1="802" y1="222" x2="811" y2="222" stroke="#d97706" stroke-width="1.8" />
<polygon points="817,222 810,218 810,226" fill="#d97706" />

<!-- Làn 3 (Rose): h_3 -> FFN -> (+) -> Norm 2 -> z_3 -->
<line x1="366" y1="298" x2="388" y2="298" stroke="#e11d48" stroke-width="1.8" />
<line x1="696" y1="298" x2="714" y2="298" stroke="#e11d48" stroke-width="1.8" />
<line x1="742" y1="298" x2="752" y2="298" stroke="#e11d48" stroke-width="1.8" />
<line x1="802" y1="298" x2="811" y2="298" stroke="#e11d48" stroke-width="1.8" />
<polygon points="817,298 810,294 810,302" fill="#e11d48" />

<!-- Vách ngăn nét đứt giữa 3 làn bên trong khu vực FFN (y=184 và y=260) -->
<line x1="388" y1="184" x2="696" y2="184" stroke="#9ca3af" stroke-width="1.2" stroke-dasharray="5 4" />
<line x1="388" y1="260" x2="696" y2="260" stroke="#9ca3af" stroke-width="1.2" stroke-dasharray="5 4" />
</svg>

<!-- Nhãn trên 2 đường nối tắt Residual -->
<div class="absolute left-[62px] top-[73px] px-1.5 bg-white dark:bg-[#121212] text-[10px] font-sans font-semibold text-emerald-600 dark:text-emerald-400">
Residual 1 (+ z<sub>l−1</sub>)
</div>
<div class="absolute left-[490px] top-[73px] px-1.5 bg-white dark:bg-[#121212] text-[10px] font-sans font-semibold text-emerald-600 dark:text-emerald-400">
Residual 2 (+ h<sub>l</sub>)
</div>

<!-- Nhãn các bước bên trên khối FFN -->
<div class="absolute left-[388px] top-[104px] w-[308px] flex justify-between px-2 text-[9px] font-sans text-gray-500">
<span>Linear 1 & ReLU</span>
<span>Không gian ẩn d<sub>ff</sub></span>
<span>Linear 2</span>
</div>

<!-- Trạm 1: Đầu vào z_{l-1} (3 hàng x_1, x_2, x_3) -->
<div class="absolute left-[0px] top-[126px] w-[44px] h-[40px] border border-gray-500 bg-gray-500/15 flex items-center justify-center font-bold">
x₁
</div>
<div class="absolute left-[0px] top-[202px] w-[44px] h-[40px] border border-gray-500 bg-gray-500/15 flex items-center justify-center font-bold">
x₂
</div>
<div class="absolute left-[0px] top-[278px] w-[44px] h-[40px] border border-gray-500 bg-gray-500/15 flex items-center justify-center font-bold">
x₃
</div>
<div class="absolute left-[-8px] top-[326px] w-[60px] text-center text-[10px] font-sans text-gray-500">
z<sub>l−1</sub> (d<sub>model</sub>)
</div>

<!-- Trạm 2: Khối MHA (Gộp chung cả 3 hàng để trộn ngữ cảnh) -->
<div class="absolute left-[66px] top-[126px] w-[106px] h-[192px] border-2 border-purple-600 dark:border-purple-400 bg-purple-500/10 flex flex-col items-center justify-between py-2">
<span class="text-[10px] font-sans font-bold text-purple-800 dark:text-purple-200 bg-white/90 dark:bg-gray-900/90 px-1.5 rounded">MHA</span>
<span class="text-[9px] font-sans text-purple-700 dark:text-purple-300 bg-white/90 dark:bg-gray-900/90 px-1 rounded">Trộn giữa các từ</span>
</div>

<!-- Trạm 3: Bộ cộng (+) và LayerNorm 1 -->
<div class="absolute left-[194px] top-[132px] w-7 h-7 rounded-full border-2 border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center font-bold text-emerald-800 dark:text-emerald-200">+</div>
<div class="absolute left-[194px] top-[208px] w-7 h-7 rounded-full border-2 border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center font-bold text-emerald-800 dark:text-emerald-200">+</div>
<div class="absolute left-[194px] top-[284px] w-7 h-7 rounded-full border-2 border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center font-bold text-emerald-800 dark:text-emerald-200">+</div>

<div class="absolute left-[236px] top-[126px] w-[56px] h-[40px] border border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center text-[10px] font-sans font-semibold text-emerald-800 dark:text-emerald-200">Norm</div>
<div class="absolute left-[236px] top-[202px] w-[56px] h-[40px] border border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center text-[10px] font-sans font-semibold text-emerald-800 dark:text-emerald-200">Norm</div>
<div class="absolute left-[236px] top-[278px] w-[56px] h-[40px] border border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center text-[10px] font-sans font-semibold text-emerald-800 dark:text-emerald-200">Norm</div>

<!-- Trạm 4: Các vector trung gian h_1, h_2, h_3 -->
<div class="absolute left-[318px] top-[126px] w-[48px] h-[40px] border-2 border-cyan-600 bg-cyan-500 text-white font-bold flex items-center justify-center">
h₁
</div>
<div class="absolute left-[318px] top-[202px] w-[48px] h-[40px] border-2 border-amber-600 bg-amber-500 text-white font-bold flex items-center justify-center">
h₂
</div>
<div class="absolute left-[318px] top-[278px] w-[48px] h-[40px] border-2 border-rose-600 bg-rose-500 text-white font-bold flex items-center justify-center">
h₃
</div>
<div class="absolute left-[308px] top-[326px] w-[68px] text-center text-[10px] font-sans text-gray-500">
h<sub>l</sub> (d<sub>model</sub>)
</div>

<!-- Trạm 5: 3 Đường ống FFN Độc lập (×W₁ → ReLU → Thanh d_ff đã tắt ô âm → ×W₂) -->
<!-- Làn 1: Của riêng h_1 (100% Cyan) -->
<div class="absolute left-[388px] top-[124px] w-[308px] h-[44px] border-2 border-cyan-600 dark:border-cyan-400 bg-white dark:bg-gray-900 flex items-center justify-between px-2.5">
<span class="text-[10px] font-bold text-cyan-700 dark:text-cyan-300">×W₁</span>
<span class="text-[10px] text-gray-400">→</span>
<span class="text-[9px] font-sans font-bold text-cyan-700 dark:text-cyan-300">ReLU</span>
<span class="text-[10px] text-gray-400">→</span>
<div class="w-[136px] h-6 border border-cyan-600 grid grid-cols-4 divide-x divide-cyan-600">
<div class="bg-cyan-500"></div>
<div class="bg-gray-100 dark:bg-gray-800"></div>
<div class="bg-cyan-600"></div>
<div class="bg-cyan-400"></div>
</div>
<span class="text-[10px] text-gray-400">→</span>
<span class="text-[10px] font-bold text-cyan-700 dark:text-cyan-300">×W₂</span>
</div>

<!-- Nhãn trên vách ngăn 1 -->
<div class="absolute left-[462px] top-[176px] px-2 bg-white dark:bg-[#121212] text-[9px] font-sans text-gray-400">
⨯ Không kết nối chéo giữa các vector ⨯
</div>

<!-- Làn 2: Của riêng h_2 (100% Amber) -->
<div class="absolute left-[388px] top-[200px] w-[308px] h-[44px] border-2 border-amber-500 dark:border-amber-400 bg-white dark:bg-gray-900 flex items-center justify-between px-2.5">
<span class="text-[10px] font-bold text-amber-700 dark:text-amber-300">×W₁</span>
<span class="text-[10px] text-gray-400">→</span>
<span class="text-[9px] font-sans font-bold text-amber-700 dark:text-amber-300">ReLU</span>
<span class="text-[10px] text-gray-400">→</span>
<div class="w-[136px] h-6 border border-amber-600 grid grid-cols-4 divide-x divide-amber-600">
<div class="bg-amber-500"></div>
<div class="bg-amber-400"></div>
<div class="bg-gray-100 dark:bg-gray-800"></div>
<div class="bg-amber-600"></div>
</div>
<span class="text-[10px] text-gray-400">→</span>
<span class="text-[10px] font-bold text-amber-700 dark:text-amber-300">×W₂</span>
</div>

<!-- Nhãn trên vách ngăn 2 -->
<div class="absolute left-[462px] top-[252px] px-2 bg-white dark:bg-[#121212] text-[9px] font-sans text-gray-400">
⨯ Không kết nối chéo giữa các vector ⨯
</div>

<!-- Làn 3: Của riêng h_3 (100% Rose) -->
<div class="absolute left-[388px] top-[276px] w-[308px] h-[44px] border-2 border-rose-500 dark:border-rose-400 bg-white dark:bg-gray-900 flex items-center justify-between px-2.5">
<span class="text-[10px] font-bold text-rose-700 dark:text-rose-300">×W₁</span>
<span class="text-[10px] text-gray-400">→</span>
<span class="text-[9px] font-sans font-bold text-rose-700 dark:text-rose-300">ReLU</span>
<span class="text-[10px] text-gray-400">→</span>
<div class="w-[136px] h-6 border border-rose-600 grid grid-cols-4 divide-x divide-rose-600">
<div class="bg-gray-100 dark:bg-gray-800"></div>
<div class="bg-rose-500"></div>
<div class="bg-rose-600"></div>
<div class="bg-rose-400"></div>
</div>
<span class="text-[10px] text-gray-400">→</span>
<span class="text-[10px] font-bold text-rose-700 dark:text-rose-300">×W₂</span>
</div>

<div class="absolute left-[388px] top-[326px] w-[308px] text-center text-[10px] font-sans text-gray-500">
Độc lập trên từng vị trí token (Dùng chung W₁, W₂)
</div>

<!-- Trạm 6: Bộ cộng (+) và LayerNorm 2 -->
<div class="absolute left-[714px] top-[132px] w-7 h-7 rounded-full border-2 border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center font-bold text-emerald-800 dark:text-emerald-200">+</div>
<div class="absolute left-[714px] top-[208px] w-7 h-7 rounded-full border-2 border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center font-bold text-emerald-800 dark:text-emerald-200">+</div>
<div class="absolute left-[714px] top-[284px] w-7 h-7 rounded-full border-2 border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center font-bold text-emerald-800 dark:text-emerald-200">+</div>

<div class="absolute left-[752px] top-[126px] w-[50px] h-[40px] border border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center text-[10px] font-sans font-semibold text-emerald-800 dark:text-emerald-200">Norm</div>
<div class="absolute left-[752px] top-[202px] w-[50px] h-[40px] border border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center text-[10px] font-sans font-semibold text-emerald-800 dark:text-emerald-200">Norm</div>
<div class="absolute left-[752px] top-[278px] w-[50px] h-[40px] border border-emerald-600 bg-emerald-50 dark:bg-emerald-950 flex items-center justify-center text-[10px] font-sans font-semibold text-emerald-800 dark:text-emerald-200">Norm</div>

<!-- Trạm 7: Đầu ra z_l (z_1, z_2, z_3 giữ nguyên màu độc lập của từng làn) -->
<div class="absolute left-[818px] top-[126px] w-[42px] h-[40px] border-2 border-cyan-700 bg-cyan-600 text-white font-bold flex items-center justify-center">
z₁
</div>
<div class="absolute left-[818px] top-[202px] w-[42px] h-[40px] border-2 border-amber-700 bg-amber-500 text-white font-bold flex items-center justify-center">
z₂
</div>
<div class="absolute left-[818px] top-[278px] w-[42px] h-[40px] border-2 border-rose-700 bg-rose-500 text-white font-bold flex items-center justify-center">
z₃
</div>
<div class="absolute left-[804px] top-[326px] w-[62px] text-center text-[10px] font-sans text-gray-500">
z<sub>l</sub> (d<sub>model</sub>)
</div>

</div>

<!--
- Luồng 2 Sub-layer nối tiếp và Add & Norm: Đường nối tắt chỉ bắc cầu qua từng Sub-layer một. Đầu vào z_{l-1} đi qua khối MHA (nơi các vector từ trộn thông tin ngữ cảnh chéo với nhau), cộng nối tắt Residual 1 với chính z_{l-1} rồi qua LayerNorm tạo ra các vector trung gian h_1, h_2, h_3. Sau đó, đường nối tắt Residual 2 xuất phát từ chính h_l (không phải nối từ đầu) để bắc cầu qua khối FFN.
- Tính chất Position-wise của FFN: Khác với MHA trộn chéo giữa các vị trí, sang khối FFN mỗi vector h_1 (màu lam), h_2 (màu vàng), h_3 (màu hồng) chạy trên một đường ống ngang hoàn toàn độc lập, dùng chung bộ tham số W_1, W_2 nhưng không có bất kỳ tương tác nào với các vector ở hàng khác.
- Cơ chế Phóng rộng - Lọc ReLU - Nén lại trong từng làn FFN: Mỗi vector 512 chiều được nhân W_1 phóng rộng gấp 4 lần lên không gian 2048 chiều, đi qua hàm phi tuyến ReLU để triệt tiêu các thành phần âm về 0, rồi nhân W_2 nén trở lại 512 chiều trước khi cộng Residual 2 và chuẩn hóa thành đầu ra z_1, z_2, z_3.
-->
