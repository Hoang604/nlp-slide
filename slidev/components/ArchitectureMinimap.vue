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
  <div class="p-2.5 rounded-xl border border-gray-200 dark:border-gray-800 bg-white/70 dark:bg-gray-900/70 shadow-sm max-w-[290px] mx-auto select-none">
    <div class="text-[10px] font-bold text-gray-500 dark:text-gray-400 tracking-wider text-center mb-1 uppercase font-mono">
      Transformer Architecture (Figure 1)
    </div>

    <svg viewBox="0 0 310 395" class="w-full h-auto text-[10px] font-sans">
      <defs>
        <!-- Arrowhead Marker -->
        <marker id="arrow" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-gray-400 dark:fill-gray-500" />
        </marker>
        <marker id="arrow-blue" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-blue-500" />
        </marker>
        <marker id="arrow-active" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
          <path d="M 0 1.5 L 8 5 L 0 8.5 z" class="fill-purple-600" />
        </marker>
      </defs>

      <!-- ================= ENCODER STACK (Nx) ================= -->
      <!-- Nx Outer Box -->
      <rect x="12" y="112" width="126" height="152" rx="8"
        class="fill-blue-500/5 stroke-blue-500/30 stroke-dashed" stroke-width="1.5" />
      <text x="18" y="125" class="fill-blue-600 dark:fill-blue-400 font-bold text-[9px] font-mono">ENCODER (Nx)</text>

      <!-- Enc FFN Box -->
      <rect x="22" y="132" width="106" height="28" rx="5"
        :class="isActive('enc-ffn') ? 'fill-blue-500 stroke-blue-600 text-white shadow-lg' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.5" />
      <text x="75" y="149" text-anchor="middle"
        :class="isActive('enc-ffn') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9.5px]">Feed Forward</text>

      <!-- Arrow: Enc MHA -> Enc FFN -->
      <line x1="75" y1="198" x2="75" y2="164" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Enc MHA Box -->
      <rect x="22" y="200" width="106" height="30" rx="5"
        :class="isActive('enc-mha') ? 'fill-blue-500 stroke-blue-600 text-white' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.5" />
      <text x="75" y="218" text-anchor="middle"
        :class="isActive('enc-mha') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9.5px]">Multi-Head Attention</text>

      <!-- ================= ENCODER INPUT (Outside Nx) ================= -->
      <!-- Arrow: Input PE -> Enc MHA -->
      <line x1="75" y1="288" x2="75" y2="234" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Enc Positional Encoding Circle & Label -->
      <circle cx="75" cy="300" r="10" class="fill-blue-500/20 stroke-blue-500" stroke-width="1.5" />
      <text x="75" y="303" text-anchor="middle" class="fill-blue-700 dark:fill-blue-300 font-bold text-[11px]">+</text>
      <text x="50" y="303" text-anchor="end" class="fill-gray-500 text-[8px] font-mono">Pos Enc</text>

      <!-- Arrow: Input Emb -> Circle -->
      <line x1="75" y1="334" x2="75" y2="313" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Enc Input Embedding Box -->
      <rect x="25" y="336" width="100" height="24" rx="4"
        :class="isActive('input') ? 'fill-blue-500 stroke-blue-600 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="75" y="351" text-anchor="middle" class="fill-gray-600 dark:fill-gray-300 text-[8.5px]">Input Embedding</text>

      <!-- Enc Input Text -->
      <text x="75" y="380" text-anchor="middle" class="fill-gray-500 text-[9px] font-semibold uppercase">Inputs</text>
      <line x1="75" y1="368" x2="75" y2="361" class="stroke-gray-400" stroke-width="1" />

      <!-- ================= CROSS-ATTENTION DATA FLOW ================= -->
      <!-- Path from Encoder top -> Decoder Cross-Attention -->
      <path d="M 75 130 L 75 92 L 152 92 L 152 188 L 178 188"
        fill="none"
        :class="isActive('dec-cross') ? 'stroke-blue-500 stroke-2' : 'stroke-blue-500/80 stroke-[1.5]'"
        marker-end="url(#arrow-blue)" />
      <!-- K, V Label on Path -->
      <rect x="138" y="130" width="28" height="15" rx="3" class="fill-blue-500 text-white" />
      <text x="152" y="141" text-anchor="middle" class="fill-white font-mono text-[8px] font-bold">K, V</text>


      <!-- ================= DECODER STACK (Nx) ================= -->
      <!-- Nx Outer Box -->
      <rect x="168" y="96" width="130" height="168" rx="8"
        class="fill-purple-500/5 stroke-purple-500/30 stroke-dashed" stroke-width="1.5" />
      <text x="290" y="110" text-anchor="end" class="fill-purple-600 dark:fill-purple-400 font-bold text-[9px] font-mono">DECODER (Nx)</text>

      <!-- Dec FFN Box -->
      <rect x="180" y="116" width="108" height="26" rx="5"
        :class="isActive('dec-ffn') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.5" />
      <text x="234" y="132" text-anchor="middle"
        :class="isActive('dec-ffn') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9.5px]">Feed Forward</text>

      <!-- Arrow: Cross MHA -> Dec FFN -->
      <line x1="234" y1="172" x2="234" y2="146" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Dec Cross-Attention Box -->
      <rect x="180" y="174" width="108" height="28" rx="5"
        :class="isActive('dec-cross') ? 'fill-purple-600 stroke-purple-700 text-white ring-2 ring-purple-300' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.5" />
      <text x="234" y="191" text-anchor="middle"
        :class="isActive('dec-cross') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Cross Attention</text>

      <!-- Arrow: Masked MHA -> Cross MHA -->
      <line x1="234" y1="228" x2="234" y2="206" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Dec Masked MHA Box -->
      <rect x="180" y="230" width="108" height="28" rx="5"
        :class="isActive('dec-mask') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-white dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.5" />
      <text x="234" y="247" text-anchor="middle"
        :class="isActive('dec-mask') ? 'fill-white font-bold' : 'fill-gray-700 dark:fill-gray-200 font-medium'"
        class="text-[9px]">Masked Attention</text>

      <!-- ================= DECODER OUTPUT (Top - Outside Nx) ================= -->
      <!-- Arrow: Dec FFN -> Linear -->
      <line x1="234" y1="114" x2="234" y2="88" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Linear Box -->
      <rect x="194" y="66" width="80" height="20" rx="4"
        :class="isActive('linear') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="234" y="79" text-anchor="middle" class="fill-gray-700 dark:fill-gray-300 text-[8.5px] font-medium">Linear</text>

      <!-- Arrow: Linear -> Softmax -->
      <line x1="234" y1="64" x2="234" y2="52" class="stroke-gray-400" stroke-width="1.2" marker-end="url(#arrow)" />

      <!-- Softmax Box -->
      <rect x="194" y="32" width="80" height="18" rx="4"
        :class="isActive('linear') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="234" y="44" text-anchor="middle" class="fill-gray-700 dark:fill-gray-300 text-[8.5px] font-medium">Softmax</text>

      <!-- Output Probabilities Text -->
      <line x1="234" y1="30" x2="234" y2="20" class="stroke-gray-400" stroke-width="1" marker-end="url(#arrow)" />
      <text x="234" y="14" text-anchor="middle" class="fill-purple-700 dark:fill-purple-300 text-[8.5px] font-bold">Probabilities</text>

      <!-- ================= DECODER INPUT (Bottom - Outside Nx) ================= -->
      <!-- Arrow: Output PE -> Masked MHA -->
      <line x1="234" y1="288" x2="234" y2="262" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Dec Positional Encoding Circle & Label -->
      <circle cx="234" cy="300" r="10" class="fill-purple-500/20 stroke-purple-500" stroke-width="1.5" />
      <text x="234" y="303" text-anchor="middle" class="fill-purple-700 dark:fill-purple-300 font-bold text-[11px]">+</text>
      <text x="259" y="303" class="fill-gray-500 text-[8px] font-mono">Pos Enc</text>

      <!-- Arrow: Output Emb -> Circle -->
      <line x1="234" y1="334" x2="234" y2="313" class="stroke-gray-400 dark:stroke-gray-500" stroke-width="1.5" marker-end="url(#arrow)" />

      <!-- Dec Output Embedding Box -->
      <rect x="180" y="336" width="108" height="24" rx="4"
        :class="isActive('input') ? 'fill-purple-600 stroke-purple-700 text-white' : 'fill-gray-100 dark:fill-gray-800 stroke-gray-300 dark:stroke-gray-700'"
        stroke-width="1.2" />
      <text x="234" y="351" text-anchor="middle" class="fill-gray-600 dark:fill-gray-300 text-[8.5px]">Output Embedding</text>

      <!-- Dec Output Text -->
      <text x="234" y="380" text-anchor="middle" class="fill-gray-500 text-[8.5px] font-semibold uppercase">Outputs (shifted right)</text>
      <line x1="234" y1="368" x2="234" y2="361" class="stroke-gray-400" stroke-width="1" />
    </svg>
  </div>
</template>
