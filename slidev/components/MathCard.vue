<script setup lang="ts">
interface Props {
  title: string
  badge?: string
  color?: 'blue' | 'purple' | 'amber' | 'emerald' | 'rose'
  intuition?: string
  caveat?: string
}

withDefaults(defineProps<Props>(), {
  badge: 'Formula',
  color: 'blue'
})

const colorMap: Record<string, { border: string; header: string; badge: string }> = {
  blue: {
    border: 'border-blue-500/30 dark:border-blue-500/40',
    header: 'bg-blue-500/10 text-blue-800 dark:text-blue-200',
    badge: 'bg-blue-500/20 text-blue-700 dark:text-blue-300'
  },
  purple: {
    border: 'border-purple-500/30 dark:border-purple-500/40',
    header: 'bg-purple-500/10 text-purple-800 dark:text-purple-200',
    badge: 'bg-purple-500/20 text-purple-700 dark:text-purple-300'
  },
  amber: {
    border: 'border-amber-500/30 dark:border-amber-500/40',
    header: 'bg-amber-500/10 text-amber-800 dark:text-amber-200',
    badge: 'bg-amber-500/20 text-amber-700 dark:text-amber-300'
  },
  emerald: {
    border: 'border-emerald-500/30 dark:border-emerald-500/40',
    header: 'bg-emerald-500/10 text-emerald-800 dark:text-emerald-200',
    badge: 'bg-emerald-500/20 text-emerald-700 dark:text-emerald-300'
  },
  rose: {
    border: 'border-rose-500/30 dark:border-rose-500/40',
    header: 'bg-rose-500/10 text-rose-800 dark:text-rose-200',
    badge: 'bg-rose-500/20 text-rose-700 dark:text-rose-300'
  }
}
</script>

<template>
  <div class="rounded-xl border overflow-hidden bg-white/70 dark:bg-gray-900/70 shadow-sm transition-all"
    :class="colorMap[color]?.border ?? colorMap.blue.border">
    <!-- Header -->
    <div class="px-4 py-2 flex items-center justify-between border-b"
      :class="[colorMap[color]?.header ?? colorMap.blue.header, colorMap[color]?.border ?? colorMap.blue.border]">
      <span class="font-bold text-sm tracking-wide flex items-center gap-2">
        <svg class="w-3.5 h-3.5 opacity-80" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M4 19L9 5h3l5 14M6 14h10" />
        </svg>
        {{ title }}
      </span>
      <span class="text-[10px] font-mono uppercase px-2 py-0.5 rounded font-semibold tracking-wider"
        :class="colorMap[color]?.badge ?? colorMap.blue.badge">
        {{ badge }}
      </span>
    </div>

    <!-- Main Content: LaTeX Formula Container -->
    <div class="p-4 bg-gray-50/50 dark:bg-gray-950/40 flex flex-col items-center justify-center min-h-[70px]">
      <slot />
    </div>

    <!-- Principle / Intuition Section -->
    <div v-if="intuition || $slots.intuition" class="px-4 py-2.5 border-t border-gray-100 dark:border-gray-800 text-xs bg-white/40 dark:bg-gray-900/40 flex items-start gap-2">
      <svg class="w-3.5 h-3.5 mt-0.5 text-blue-600 dark:text-blue-400 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="12" cy="12" r="10" />
        <path d="M12 16v-4M12 8h.01" />
      </svg>
      <div class="flex-1 text-gray-700 dark:text-gray-300">
        <span class="font-bold mr-1">Nguyên lý:</span>
        <slot name="intuition">{{ intuition }}</slot>
      </div>
    </div>

    <!-- Technical Note Section -->
    <div v-if="caveat || $slots.caveat" class="px-4 py-2 border-t border-gray-100 dark:border-gray-800 text-[11px] bg-amber-500/5 dark:bg-amber-900/10 text-amber-800 dark:text-amber-200 flex items-start gap-2">
      <svg class="w-3.5 h-3.5 mt-0.5 text-amber-600 dark:text-amber-400 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="m21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z" />
        <path d="M12 9v4M12 17h.01" />
      </svg>
      <div class="flex-1">
        <span class="font-semibold mr-1">Lưu ý:</span>
        <slot name="caveat">{{ caveat }}</slot>
      </div>
    </div>
  </div>
</template>
