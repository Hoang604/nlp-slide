<script setup lang="ts">
interface Props {
  leftTitle: string
  rightTitle: string
  leftBadge?: string
  rightBadge?: string
  leftColor?: 'blue' | 'purple' | 'amber' | 'emerald' | 'rose'
  rightColor?: 'blue' | 'purple' | 'amber' | 'emerald' | 'rose'
}

withDefaults(defineProps<Props>(), {
  leftColor: 'rose',
  rightColor: 'emerald'
})

const colorStyles: Record<string, { border: string; headerBg: string; title: string }> = {
  blue: {
    border: 'border-blue-500/30 dark:border-blue-500/40',
    headerBg: 'bg-blue-500/10 text-blue-800 dark:text-blue-300',
    title: 'text-blue-700 dark:text-blue-300'
  },
  purple: {
    border: 'border-purple-500/30 dark:border-purple-500/40',
    headerBg: 'bg-purple-500/10 text-purple-800 dark:text-purple-300',
    title: 'text-purple-700 dark:text-purple-300'
  },
  amber: {
    border: 'border-amber-500/30 dark:border-amber-500/40',
    headerBg: 'bg-amber-500/10 text-amber-800 dark:text-amber-300',
    title: 'text-amber-700 dark:text-amber-300'
  },
  emerald: {
    border: 'border-emerald-500/30 dark:border-emerald-500/40',
    headerBg: 'bg-emerald-500/10 text-emerald-800 dark:text-emerald-300',
    title: 'text-emerald-700 dark:text-emerald-300'
  },
  rose: {
    border: 'border-rose-500/30 dark:border-rose-500/40',
    headerBg: 'bg-rose-500/10 text-rose-800 dark:text-rose-300',
    title: 'text-rose-700 dark:text-rose-300'
  }
}
</script>

<template>
  <div class="grid grid-cols-2 gap-3.5 max-h-[350px]">
    <!-- Left Column -->
    <div class="rounded-lg border overflow-hidden bg-white/70 dark:bg-gray-900/70 shadow-sm flex flex-col"
      :class="colorStyles[leftColor]?.border ?? colorStyles.rose.border">
      <div class="px-3 py-1.5 border-b flex items-center justify-between"
        :class="[colorStyles[leftColor]?.headerBg ?? colorStyles.rose.headerBg, colorStyles[leftColor]?.border ?? colorStyles.rose.border]">
        <span class="font-semibold text-xs tracking-wide" :class="colorStyles[leftColor]?.title">{{ leftTitle }}</span>
        <span v-if="leftBadge" class="text-[9px] font-mono uppercase px-1.5 py-0.5 rounded font-medium bg-black/5 dark:bg-white/10">
          {{ leftBadge }}
        </span>
      </div>
      <div class="p-3 text-[11px] leading-relaxed space-y-1.5 flex-1">
        <slot name="left" />
      </div>
    </div>

    <!-- Right Column -->
    <div class="rounded-lg border overflow-hidden bg-white/70 dark:bg-gray-900/70 shadow-sm flex flex-col"
      :class="colorStyles[rightColor]?.border ?? colorStyles.emerald.border">
      <div class="px-3 py-1.5 border-b flex items-center justify-between"
        :class="[colorStyles[rightColor]?.headerBg ?? colorStyles.emerald.headerBg, colorStyles[rightColor]?.border ?? colorStyles.emerald.border]">
        <span class="font-semibold text-xs tracking-wide" :class="colorStyles[rightColor]?.title">{{ rightTitle }}</span>
        <span v-if="rightBadge" class="text-[9px] font-mono uppercase px-1.5 py-0.5 rounded font-medium bg-black/5 dark:bg-white/10">
          {{ rightBadge }}
        </span>
      </div>
      <div class="p-3 text-[11px] leading-relaxed space-y-1.5 flex-1">
        <slot name="right" />
      </div>
    </div>
  </div>
</template>
