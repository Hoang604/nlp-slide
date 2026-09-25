<script setup lang="ts">
interface Props {
  name: string
  type?: 'encoder' | 'decoder' | 'both'
  inputShape?: string
  outputShape?: string
  invariant?: string
  desc?: string
}

const props = withDefaults(defineProps<Props>(), {
  type: 'both',
  inputShape: 'B × T × d_model',
  outputShape: 'B × T × d_model'
})

const typeMap = {
  encoder: { label: 'Encoder Only', class: 'bg-blue-500/20 text-blue-700 dark:text-blue-300 border-blue-500/40' },
  decoder: { label: 'Decoder Only', class: 'bg-purple-500/20 text-purple-700 dark:text-purple-300 border-purple-500/40' },
  both: { label: 'Encoder & Decoder', class: 'bg-emerald-500/20 text-emerald-700 dark:text-emerald-300 border-emerald-500/40' }
}
</script>

<template>
  <div class="p-3.5 rounded-xl border border-gray-200 dark:border-gray-800 bg-white/60 dark:bg-gray-900/60 shadow-sm flex flex-col justify-between h-full">
    <div>
      <!-- Header -->
      <div class="flex items-center justify-between gap-2 border-b border-gray-100 dark:border-gray-800 pb-2">
        <h4 class="font-bold text-sm text-gray-900 dark:text-white flex items-center gap-2">
          <span class="w-1.5 h-1.5 rounded-sm bg-primary"></span>
          {{ name }}
        </h4>
        <span class="text-[9px] font-semibold px-2 py-0.5 rounded border uppercase tracking-wider"
          :class="typeMap[type].class">
          {{ typeMap[type].label }}
        </span>
      </div>

      <!-- Description -->
      <p v-if="desc" class="text-[11px] text-gray-600 dark:text-gray-300 my-2">
        {{ desc }}
      </p>

      <!-- Tensor Dimension Contract -->
      <div class="my-2.5 p-2 rounded-lg bg-gray-50 dark:bg-gray-950/60 border border-gray-100 dark:border-gray-800/80 flex items-center justify-between text-xs">
        <div>
          <span class="text-[9px] uppercase tracking-wider text-gray-400 font-semibold block">Input Shape</span>
          <span class="font-mono font-bold text-blue-600 dark:text-blue-400">{{ inputShape }}</span>
        </div>
        <div class="text-gray-400 text-xs">➔</div>
        <div class="text-right">
          <span class="text-[9px] uppercase tracking-wider text-gray-400 font-semibold block">Output Shape</span>
          <span class="font-mono font-bold text-emerald-600 dark:text-emerald-400">{{ outputShape }}</span>
        </div>
      </div>
    </div>

    <!-- Core Invariant Section -->
    <div v-if="invariant" class="mt-2 pt-2 border-t border-gray-100 dark:border-gray-800 text-[11px] text-amber-800 dark:text-amber-300 flex items-start gap-1.5">
      <svg class="w-3.5 h-3.5 mt-0.5 text-amber-600 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <rect width="18" height="11" x="3" y="11" rx="2" ry="2" />
        <path d="M7 11V7a5 5 0 0 1 10 0v4" />
      </svg>
      <div>
        <span class="font-semibold mr-1">Invariant:</span>
        <span>{{ invariant }}</span>
      </div>
    </div>
  </div>
</template>
