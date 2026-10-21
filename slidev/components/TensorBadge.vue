<script setup lang="ts">
interface Props {
  shape: string
  name?: string
  desc?: string
  color?: 'blue' | 'purple' | 'amber' | 'emerald' | 'rose' | 'cyan'
}

const props = withDefaults(defineProps<Props>(), {
  color: 'blue'
})

const colorStyles: Record<string, { bg: string; border: string; text: string; dot: string }> = {
  blue: {
    bg: 'bg-blue-500/10 dark:bg-blue-900/25',
    border: 'border-blue-500/30 dark:border-blue-500/40',
    text: 'text-blue-700 dark:text-blue-300',
    dot: 'bg-blue-500'
  },
  purple: {
    bg: 'bg-purple-500/10 dark:bg-purple-900/25',
    border: 'border-purple-500/30 dark:border-purple-500/40',
    text: 'text-purple-700 dark:text-purple-300',
    dot: 'bg-purple-500'
  },
  amber: {
    bg: 'bg-amber-500/10 dark:bg-amber-900/25',
    border: 'border-amber-500/30 dark:border-amber-500/40',
    text: 'text-amber-700 dark:text-amber-300',
    dot: 'bg-amber-500'
  },
  emerald: {
    bg: 'bg-emerald-500/10 dark:bg-emerald-900/25',
    border: 'border-emerald-500/30 dark:border-emerald-500/40',
    text: 'text-emerald-700 dark:text-emerald-300',
    dot: 'bg-emerald-500'
  },
  rose: {
    bg: 'bg-rose-500/10 dark:bg-rose-900/25',
    border: 'border-rose-500/30 dark:border-rose-500/40',
    text: 'text-rose-700 dark:text-rose-300',
    dot: 'bg-rose-500'
  },
  cyan: {
    bg: 'bg-cyan-500/10 dark:bg-cyan-900/25',
    border: 'border-cyan-500/30 dark:border-cyan-500/40',
    text: 'text-cyan-700 dark:text-cyan-300',
    dot: 'bg-cyan-500'
  }
}

const current = colorStyles[props.color] ?? colorStyles.blue
</script>

<template>
  <div class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-md border text-xs font-mono transition-all shadow-sm whitespace-nowrap shrink-0"
    :class="[current.bg, current.border, current.text]">
    <span class="w-1.5 h-1.5 rounded-full animate-pulse" :class="current.dot"></span>
    <span v-if="name" class="font-semibold font-sans tracking-wide mr-1">{{ name }}:</span>
    <span class="font-bold font-mono tracking-tight">{{ shape }}</span>
    <span v-if="desc" class="ml-1 text-[10px] opacity-75 font-sans">({{ desc }})</span>
  </div>
</template>
