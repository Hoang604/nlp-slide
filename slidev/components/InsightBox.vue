<script setup lang="ts">
interface Props {
  type?: 'question' | 'insight' | 'warning' | 'trick'
  title?: string
}

const props = withDefaults(defineProps<Props>(), {
  type: 'insight',
  title: 'Phân tích Kỹ thuật'
})

const config = {
  question: {
    svgPath: 'M12 16v-4M12 8h.01',
    isCircle: true,
    label: 'Khảo sát Kỹ thuật',
    border: 'border-purple-500/40',
    bg: 'bg-purple-500/10 dark:bg-purple-950/30',
    text: 'text-purple-900 dark:text-purple-200'
  },
  insight: {
    svgPath: 'M12 2v4M12 18v4M4.93 4.93l2.83 2.83M16.24 16.24l2.83 2.83M2 12h4M18 12h4M4.93 19.07l2.83-2.83M16.24 7.76l2.83-2.83',
    isCircle: false,
    label: 'Nguyên lý Cốt lõi',
    border: 'border-blue-500/40',
    bg: 'bg-blue-500/10 dark:bg-blue-950/30',
    text: 'text-blue-900 dark:text-blue-200'
  },
  warning: {
    svgPath: 'm21.73 18-8-14a2 2 0 0 0-3.48 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3ZM12 9v4M12 17h.01',
    isCircle: false,
    label: 'Đặc thù Kiến trúc',
    border: 'border-rose-500/40',
    bg: 'bg-rose-500/10 dark:bg-rose-950/30',
    text: 'text-rose-900 dark:text-rose-200'
  },
  trick: {
    svgPath: 'M13 2L3 14h9l-1 8 10-12h-9l1-8z',
    isCircle: false,
    label: 'Phương pháp Thực nghiệm',
    border: 'border-amber-500/40',
    bg: 'bg-amber-500/10 dark:bg-amber-950/30',
    text: 'text-amber-900 dark:text-amber-200'
  }
}

const active = config[props.type] ?? config.insight
</script>

<template>
  <div class="my-2 p-3 rounded-lg border shadow-sm transition-all text-xs"
    :class="[active.border, active.bg, active.text]">
    <div class="flex items-center gap-1.5 mb-1 font-semibold text-xs">
      <svg class="w-3.5 h-3.5 shrink-0" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle v-if="active.isCircle" cx="12" cy="12" r="10" />
        <path :d="active.svgPath" />
      </svg>
      <span>{{ title || active.label }}</span>
    </div>
    <div class="pl-5 text-[11px] leading-relaxed space-y-1 opacity-90">
      <slot />
    </div>
  </div>
</template>
