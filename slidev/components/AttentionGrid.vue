<script setup lang="ts">
import { computed } from 'vue'

interface Props {
  tokens?: string[]
  mode?: 'self' | 'causal'
  highlightFrom?: string
  highlightTo?: string
}

const props = withDefaults(defineProps<Props>(), {
  tokens: () => ['The', 'animal', 'didn\'t', 'cross', 'it'],
  mode: 'self',
  highlightFrom: 'it',
  highlightTo: 'animal'
})

// Deterministic mock weight calculation for pedagogical illustration
function getWeight(fromIdx: number, toIdx: number): { val: number; isMasked: boolean } {
  if (props.mode === 'causal' && toIdx > fromIdx) {
    return { val: 0, isMasked: true }
  }
  
  const fromToken = props.tokens[fromIdx]
  const toToken = props.tokens[toIdx]

  // Highlight special coreference pair
  if (fromToken === props.highlightFrom && toToken === props.highlightTo) {
    return { val: 0.88, isMasked: false }
  }
  if (fromIdx === toIdx) {
    return { val: 0.45, isMasked: false }
  }
  // Pseudo weights
  const pseudo = ((fromIdx * 3 + toIdx * 7) % 10) / 25 + 0.05
  return { val: parseFloat(pseudo.toFixed(2)), isMasked: false }
}
</script>

<template>
  <div class="p-2.5 rounded-xl border border-gray-200 dark:border-gray-800 bg-white/70 dark:bg-gray-900/70 shadow-sm text-xs">
    <!-- Header Controls & Mode Info -->
    <div class="flex items-center justify-between mb-1.5">
      <div class="font-semibold flex items-center gap-1.5 text-xs">
        <svg class="w-3.5 h-3.5 text-blue-600" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect width="18" height="18" x="3" y="3" rx="2" />
          <path d="M3 9h18M9 21V9" />
        </svg>
        <span>Attention Weight Matrix</span>
        <span class="text-[9px] font-mono px-1.5 py-0.5 rounded uppercase font-semibold"
          :class="mode === 'causal' ? 'bg-rose-500/20 text-rose-700 dark:text-rose-300' : 'bg-blue-500/20 text-blue-700 dark:text-blue-300'">
          {{ mode === 'causal' ? 'Causal Mask' : 'Bidirectional Attention' }}
        </span>
      </div>
      <div class="text-[10px] text-gray-500 font-mono">
        Shape: [{{ tokens.length }} × {{ tokens.length }}]
      </div>
    </div>

    <!-- Matrix Table -->
    <div class="overflow-x-auto">
      <div class="inline-block min-w-full">
        <!-- Column Headers (Keys) -->
        <div class="flex items-center">
          <div class="w-16 text-[9px] font-bold text-gray-400 text-center font-mono">Q \ K</div>
          <div v-for="(k, kidx) in tokens" :key="'col-' + kidx"
            class="w-14 text-center font-bold font-mono text-[10px] py-0.5 text-gray-700 dark:text-gray-300 truncate px-1">
            {{ k }}
          </div>
        </div>

        <!-- Matrix Rows (Queries) -->
        <div v-for="(q, qidx) in tokens" :key="'row-' + qidx" class="flex items-center mt-1">
          <!-- Row Header -->
          <div class="w-16 font-bold font-mono text-[10px] text-right pr-2 text-gray-700 dark:text-gray-300 truncate">
            {{ q }}
          </div>

          <!-- Cells -->
          <div v-for="(k, kidx) in tokens" :key="'cell-' + qidx + '-' + kidx"
            class="w-14 h-7 mx-0.5 rounded flex items-center justify-center font-mono text-[10px] transition-all relative border border-transparent"
            :class="[
              getWeight(qidx, kidx).isMasked 
                ? 'bg-gray-200/50 dark:bg-gray-800/60 text-gray-400 dark:text-gray-600 border-dashed border-gray-300 dark:border-gray-700' 
                : getWeight(qidx, kidx).val >= 0.8
                  ? 'bg-blue-600 text-white font-bold ring-1 ring-blue-400 shadow-sm'
                  : getWeight(qidx, kidx).val >= 0.4
                    ? 'bg-blue-500/50 dark:bg-blue-600/50 text-gray-900 dark:text-white font-medium'
                    : 'bg-blue-500/15 dark:bg-blue-900/30 text-gray-600 dark:text-gray-300'
            ]">
            <span v-if="getWeight(qidx, kidx).isMasked" class="text-[9px] font-bold">-∞</span>
            <span v-else>{{ getWeight(qidx, kidx).val }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Explanatory Legend -->
    <div class="mt-2 pt-1.5 border-t border-gray-100 dark:border-gray-800 flex items-center justify-between text-[10px] text-gray-500">
      <div v-if="mode === 'causal'" class="flex items-center gap-1.5 text-rose-600 dark:text-rose-400 font-medium">
        <svg class="w-3.5 h-3.5 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10" />
        </svg>
        <span>Tam giác trên mang giá trị -∞ nhằm triệt tiêu xác suất sau hàm Softmax.</span>
      </div>
      <div v-else class="flex items-center gap-1.5 text-blue-600 dark:text-blue-400">
        <svg class="w-3.5 h-3.5 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="11" cy="11" r="8" />
          <path d="m21 21-4.3-4.3" />
        </svg>
        <span>Vị trí token "it" phân bổ 88% trọng số chú ý vào danh từ "animal".</span>
      </div>
      <div class="flex items-center gap-1 font-mono text-[9px]">
        <span>Softmax(QKᵀ / √d_k)</span>
      </div>
    </div>
  </div>
</template>
