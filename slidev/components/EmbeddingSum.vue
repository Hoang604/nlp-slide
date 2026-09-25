<script setup lang="ts">
interface Props {
  token?: string
  tokenDim?: string
  pos?: string
  posDim?: string
  note?: string
}

withDefaults(defineProps<Props>(), {
  token: 'Word Embedding',
  tokenDim: 'd = 512',
  pos: 'Positional Encoding',
  posDim: 'd = 512',
  note: 'Phép cộng từng phần tử bảo toàn số chiều d_model thay vì tăng gấp đôi kích thước như phép Concat.'
})
</script>

<template>
  <div class="p-3.5 rounded-xl border border-gray-200 dark:border-gray-800 bg-white/50 dark:bg-gray-900/50 shadow-sm">
    <div class="flex items-center justify-center gap-3 text-center">
      <!-- Token Embedding Block -->
      <div class="flex-1 p-2.5 rounded-lg border border-blue-500/30 bg-blue-500/10">
        <div class="text-[10px] text-blue-600 dark:text-blue-400 font-medium">Biểu diễn từ vựng</div>
        <div class="text-xs font-bold text-gray-800 dark:text-gray-100 mt-0.5">{{ token }}</div>
        <div class="text-[10px] font-mono text-blue-600/80 dark:text-blue-400/80 mt-0.5">[{{ tokenDim }}]</div>
      </div>

      <!-- Plus Operator -->
      <div class="text-lg font-bold text-gray-400 dark:text-gray-500">+</div>

      <!-- Positional Encoding Block -->
      <div class="flex-1 p-2.5 rounded-lg border border-amber-500/30 bg-amber-500/10">
        <div class="text-[10px] text-amber-600 dark:text-amber-400 font-medium">Biểu diễn vị trí</div>
        <div class="text-xs font-bold text-gray-800 dark:text-gray-100 mt-0.5">{{ pos }}</div>
        <div class="text-[10px] font-mono text-amber-600/80 dark:text-amber-400/80 mt-0.5">[{{ posDim }}]</div>
      </div>

      <!-- Equals Operator -->
      <div class="text-lg font-bold text-gray-400 dark:text-gray-500">=</div>

      <!-- Combined Encoder Input Block -->
      <div class="flex-1 p-2.5 rounded-lg border border-emerald-500/40 bg-emerald-500/15 shadow-sm">
        <div class="text-[10px] text-emerald-600 dark:text-emerald-400 font-bold">Vector Input Encoder</div>
        <div class="text-xs font-bold text-gray-900 dark:text-white mt-0.5">Biểu diễn Toàn diện</div>
        <div class="text-[10px] font-mono text-emerald-600 dark:text-emerald-400 font-bold mt-0.5">[d = 512]</div>
      </div>
    </div>

    <!-- Academic Footnote -->
    <div v-if="note" class="mt-2.5 pt-2 border-t border-gray-100 dark:border-gray-800 text-[11px] text-gray-500 dark:text-gray-400 flex items-start gap-1.5">
      <svg class="w-3.5 h-3.5 mt-0.5 text-blue-600 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="12" cy="12" r="10" />
        <path d="M12 16v-4M12 8h.01" />
      </svg>
      <span>{{ note }}</span>
    </div>
  </div>
</template>
