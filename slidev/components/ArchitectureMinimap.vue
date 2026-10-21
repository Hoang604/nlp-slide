<script setup lang="ts">
interface Props {
  active?: 'input' | 'pe' | 'enc-mha' | 'enc-ffn' | 'dec-mask' | 'dec-cross' | 'dec-ffn' | 'linear' | 'all'
}

const props = withDefaults(defineProps<Props>(), {
  active: 'all'
})

function isActive(target: string): boolean {
  if (props.active === 'all') return false
  return props.active === target
}
</script>

<template>
  <div class="p-2 rounded-xl border border-gray-200 dark:border-gray-800 bg-white/70 dark:bg-gray-900/70 shadow-sm max-w-[245px] mx-auto select-none">
    <div class="text-[9.5px] font-bold text-gray-500 dark:text-gray-400 tracking-wider text-center mb-0.5 uppercase font-mono">
      Transformer Architecture
    </div>

    <svg viewBox="0 0 320 415" class="w-full h-auto text-[10px] font-sans">
      <defs>
        <!-- Arrowhead Marker -->
        <marker id="arrow" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="4.5" markerHeight="4.5" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-gray-400 dark:fill-gray-500" />
        </marker>
        <marker id="arrow-emerald" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="4" markerHeight="4" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-emerald-500" />
        </marker>
        <marker id="arrow-blue" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-blue-500" />
        </marker>
        <marker id="arrow-active" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-purple-600" />
        </marker>
      </defs>

      <!-- ================= ENCODER STACK (Nx) ================= -->
      <rect x="10" y="102" width="132" height="176" rx="8"
        class="fill-blue-500/5 stroke-blue-500/30 stroke-dashed" stroke-width="1.5" />
      <text x="16" y="114" class="fill-blue-600 dark:fill-blue-400 font-bold text-[8.5px] font-mono">ENCODER (Nx)</text>

      <!-- Enc Sub-layer 2: Add & Norm + Feed Forward -->
      <rect x="30" y="120" width="102" height="15" rx="3"
        class="fill-emerald-500/15 dark:fill-emerald-900/30 stroke-emerald-500/50" stroke-width="1.1" />
      <text x="81" y="130.5" text-anchor="middle" class="fill-emerald-800 dark:fill-emerald-300 font-mono text-[8px] font-semibold">Add &amp; Norm</text>

      <line x1="81" y1="143" x2="81" y2="136" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.2" marker-end="url(#arrow)" />

      <rect x="30" y="144" width="102" height="24" rx="4"
        :class="isActive('enc-ffn') ? 'fill-blue-500 stroke-blue-600 text-white shadow-lg' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.4" />
      <text x="81" y="159" text-anchor="middle"
        :class="isActive('enc-ffn') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Feed Forward</text>

      <!-- Residual Bypass Loop: Enc FFN -->
      <path d="M 81 177 L 19 177 L 19 127.5 L 28 127.5" fill="none"
        class="stroke-emerald-500/80" stroke-width="1.1" stroke-dasharray="2 1.5" marker-end="url(#arrow-emerald)" />

      <!-- Arrow: Enc Add&Norm 1 -> Enc FFN -->
      <line x1="81" y1="192" x2="81" y2="170" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Enc Sub-layer 1: Add & Norm + Multi-Head Attention -->
      <rect x="30" y="193" width="102" height="15" rx="3"
        class="fill-emerald-500/15 dark:fill-emerald-900/30 stroke-emerald-500/50" stroke-width="1.1" />
      <text x="81" y="203.5" text-anchor="middle" class="fill-emerald-800 dark:fill-emerald-300 font-mono text-[8px] font-semibold">Add &amp; Norm</text>

      <line x1="81" y1="216" x2="81" y2="209" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.2" marker-end="url(#arrow)" />

      <rect x="30" y="217" width="102" height="24" rx="4"
        :class="isActive('enc-mha') ? 'fill-blue-500 stroke-blue-600 text-white' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.4" />
      <text x="81" y="232" text-anchor="middle"
        :class="isActive('enc-mha') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Multi-Head Attention</text>

      <!-- Residual Bypass Loop: Enc MHA -->
      <path d="M 81 252 L 19 252 L 19 200.5 L 28 200.5" fill="none"
        class="stroke-emerald-500/80" stroke-width="1.1" stroke-dasharray="2 1.5" marker-end="url(#arrow-emerald)" />

      <!-- ================= ENCODER INPUT (Outside Nx) ================= -->
      <!-- Arrow: Input PE -> Enc MHA -->
      <line x1="81" y1="304" x2="81" y2="243" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Enc Positional Encoding Circle & Label -->
      <circle cx="81" cy="316" r="9.5" class="fill-blue-500/20 stroke-blue-500" stroke-width="1.4" />
      <text x="81" y="319" text-anchor="middle" class="fill-blue-700 dark:fill-blue-300 font-bold text-[11px]">+</text>
      <text x="56" y="319" text-anchor="end" class="fill-gray-500 text-[8px] font-mono">Pos Enc</text>

      <!-- Arrow: Input Emb -> Circle -->
      <line x1="81" y1="350" x2="81" y2="328" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Enc Input Embedding Box -->
      <rect x="31" y="352" width="100" height="22" rx="4"
        :class="isActive('input') ? 'fill-blue-500 stroke-blue-600 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="81" y="366" text-anchor="middle" class="fill-gray-600 dark:fill-gray-300 text-[8.5px]">Input Embedding</text>

      <!-- Enc Input Text -->
      <text x="81" y="396" text-anchor="middle" class="fill-gray-500 text-[8.5px] font-semibold uppercase">Inputs</text>
      <line x1="81" y1="384" x2="81" y2="375" class="stroke-gray-400" stroke-width="1" />

      <!-- ================= CROSS-ATTENTION DATA FLOW ================= -->
      <!-- Path from Encoder top (x=105 to avoid ENCODER (Nx) label) -> Decoder Cross-Attention -->
      <path d="M 105 120 L 105 90 L 156 90 L 156 196 L 180 196"
        fill="none"
        :class="isActive('dec-cross') ? 'stroke-blue-500 stroke-2' : 'stroke-blue-500/80 stroke-[1.5]'"
        marker-end="url(#arrow-blue)" />
      <!-- K, V Label on Path -->
      <rect x="142" y="132" width="28" height="15" rx="3" class="fill-blue-500 text-white" />
      <text x="156" y="142.5" text-anchor="middle" class="fill-white font-mono text-[8px] font-bold">K, V</text>

      <!-- ================= DECODER STACK (Nx) ================= -->
      <rect x="172" y="76" width="138" height="202" rx="8"
        class="fill-purple-500/5 stroke-purple-500/30 stroke-dashed" stroke-width="1.5" />
      <text x="178" y="88" class="fill-purple-600 dark:fill-purple-400 font-bold text-[8px] font-mono">DECODER (Nx)</text>

      <!-- Dec Sub-layer 3: Add & Norm + Feed Forward -->
      <rect x="184" y="94" width="102" height="14" rx="3"
        class="fill-emerald-500/15 dark:fill-emerald-900/30 stroke-emerald-500/50" stroke-width="1.1" />
      <text x="235" y="104" text-anchor="middle" class="fill-emerald-800 dark:fill-emerald-300 font-mono text-[8px] font-semibold">Add &amp; Norm</text>

      <line x1="235" y1="115" x2="235" y2="109" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.2" marker-end="url(#arrow)" />

      <rect x="184" y="116" width="102" height="21" rx="4"
        :class="isActive('dec-ffn') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.4" />
      <text x="235" y="129.5" text-anchor="middle"
        :class="isActive('dec-ffn') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Feed Forward</text>

      <!-- Residual Bypass Loop: Dec FFN -->
      <path d="M 235 144 L 298 144 L 298 101 L 288 101" fill="none"
        class="stroke-emerald-500/80" stroke-width="1.1" stroke-dasharray="2 1.5" marker-end="url(#arrow-emerald)" />

      <!-- Arrow: Dec Add&Norm 2 -> Dec FFN -->
      <line x1="235" y1="156" x2="235" y2="139" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Dec Sub-layer 2: Add & Norm + Cross Attention -->
      <rect x="184" y="157" width="102" height="14" rx="3"
        class="fill-emerald-500/15 dark:fill-emerald-900/30 stroke-emerald-500/50" stroke-width="1.1" />
      <text x="235" y="167" text-anchor="middle" class="fill-emerald-800 dark:fill-emerald-300 font-mono text-[8px] font-semibold">Add &amp; Norm</text>

      <line x1="235" y1="178" x2="235" y2="172" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.2" marker-end="url(#arrow)" />

      <rect x="184" y="179" width="102" height="21" rx="4"
        :class="isActive('dec-cross') ? 'fill-purple-600 stroke-purple-700 text-white ring-2 ring-purple-300' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.4" />
      <text x="235" y="192.5" text-anchor="middle"
        :class="isActive('dec-cross') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Cross Attention</text>

      <!-- Residual Bypass Loop: Dec Cross-Attn -->
      <path d="M 235 207 L 298 207 L 298 164 L 288 164" fill="none"
        class="stroke-emerald-500/80" stroke-width="1.1" stroke-dasharray="2 1.5" marker-end="url(#arrow-emerald)" />

      <!-- Arrow: Dec Add&Norm 1 -> Dec Cross-Attn -->
      <line x1="235" y1="219" x2="235" y2="202" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Dec Sub-layer 1: Add & Norm + Masked Attention -->
      <rect x="184" y="220" width="102" height="14" rx="3"
        class="fill-emerald-500/15 dark:fill-emerald-900/30 stroke-emerald-500/50" stroke-width="1.1" />
      <text x="235" y="230" text-anchor="middle" class="fill-emerald-800 dark:fill-emerald-300 font-mono text-[8px] font-semibold">Add &amp; Norm</text>

      <line x1="235" y1="241" x2="235" y2="235" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.2" marker-end="url(#arrow)" />

      <rect x="184" y="242" width="102" height="21" rx="4"
        :class="isActive('dec-mask') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.4" />
      <text x="235" y="255.5" text-anchor="middle"
        :class="isActive('dec-mask') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Masked Attention</text>

      <!-- Residual Bypass Loop: Dec Masked-Attn -->
      <path d="M 235 271 L 298 271 L 298 227 L 288 227" fill="none"
        class="stroke-emerald-500/80" stroke-width="1.1" stroke-dasharray="2 1.5" marker-end="url(#arrow-emerald)" />

      <!-- ================= DECODER OUTPUT (Top - Outside Nx) ================= -->
      <!-- Arrow: Dec Add&Norm 3 -> Linear -->
      <line x1="235" y1="93" x2="235" y2="68" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Linear Box -->
      <rect x="195" y="48" width="80" height="17" rx="3.5"
        :class="isActive('linear') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="235" y="59.5" text-anchor="middle" class="fill-gray-700 dark:fill-gray-300 text-[8px] font-medium">Linear</text>

      <!-- Arrow: Linear -> Softmax -->
      <line x1="235" y1="47" x2="235" y2="38" class="stroke-gray-400" stroke-width="1.2" marker-end="url(#arrow)" />

      <!-- Softmax Box -->
      <rect x="195" y="20" width="80" height="16" rx="3.5"
        :class="isActive('linear') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="235" y="30.5" text-anchor="middle" class="fill-gray-700 dark:fill-gray-300 text-[8px] font-medium">Softmax</text>

      <!-- Output Probabilities Text -->
      <line x1="235" y1="19" x2="235" y2="12" class="stroke-gray-400" stroke-width="1" marker-end="url(#arrow)" />
      <text x="235" y="8" text-anchor="middle" class="fill-purple-700 dark:fill-purple-300 text-[8px] font-bold">Probabilities</text>

      <!-- ================= DECODER INPUT (Bottom - Outside Nx) ================= -->
      <!-- Arrow: Output PE -> Masked MHA -->
      <line x1="235" y1="304" x2="235" y2="265" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Dec Positional Encoding Circle & Label -->
      <circle cx="235" cy="316" r="9.5" class="fill-purple-500/20 stroke-purple-500" stroke-width="1.4" />
      <text x="235" y="319" text-anchor="middle" class="fill-purple-700 dark:fill-purple-300 font-bold text-[11px]">+</text>
      <text x="260" y="319" class="fill-gray-500 text-[8px] font-mono">Pos Enc</text>

      <!-- Arrow: Output Emb -> Circle -->
      <line x1="235" y1="350" x2="235" y2="328" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.4" marker-end="url(#arrow)" />

      <!-- Dec Output Embedding Box -->
      <rect x="181" y="352" width="108" height="22" rx="4"
        :class="isActive('input') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="235" y="366" text-anchor="middle" class="fill-gray-600 dark:fill-gray-300 text-[8.5px]">Output Embedding</text>

      <!-- Dec Output Text -->
      <text x="235" y="396" text-anchor="middle" class="fill-gray-500 text-[8px] font-semibold uppercase">Outputs (shifted right)</text>
      <line x1="235" y1="384" x2="235" y2="375" class="stroke-gray-400" stroke-width="1" />
    </svg>
  </div>
</template>
