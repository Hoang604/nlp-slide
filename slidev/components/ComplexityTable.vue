<script setup lang="ts">
interface Props {
  highlight?: 'self-attention' | 'recurrent' | 'convolutional' | 'all'
}

withDefaults(defineProps<Props>(), {
  highlight: 'self-attention'
})

const rows = [
  {
    id: 'self-attention',
    type: 'Self-Attention',
    complexity: 'O(n² · d)',
    sequential: 'O(1)',
    maxPath: 'O(1)',
    pros: 'Song song hóa tuyệt đối, đường dẫn thông tin trực tiếp',
    badge: 'Paper Core'
  },
  {
    id: 'recurrent',
    type: 'Recurrent (RNN/LSTM)',
    complexity: 'O(n · d²)',
    sequential: 'O(n)',
    maxPath: 'O(n)',
    pros: 'Nghẽn tuần tự t1 → tn, suy hao gradient chuỗi dài',
    badge: 'Baseline'
  },
  {
    id: 'convolutional',
    type: 'Convolutional',
    complexity: 'O(k · n · d)',
    sequential: 'O(1)',
    maxPath: 'O(log_k(n))',
    pros: 'Cần xếp chồng nhiều lớp conv để mở rộng receptive field',
    badge: 'CNN'
  },
  {
    id: 'restricted',
    type: 'Self-Attention (restricted r)',
    complexity: 'O(r · n · d)',
    sequential: 'O(1)',
    maxPath: 'O(n / r)',
    pros: 'Chỉ chú ý trong lân cận r (tiền thân Sparse/Local Attention)',
    badge: 'Variant'
  }
]
</script>

<template>
  <div class="overflow-x-auto rounded-xl border border-gray-200 dark:border-gray-800 shadow-sm text-xs">
    <table class="w-full border-collapse text-left">
      <thead>
        <tr class="bg-gray-100 dark:bg-gray-800/80 text-gray-700 dark:text-gray-200 font-bold border-b border-gray-200 dark:border-gray-700">
          <th class="px-2.5 py-1.5 text-[11px]">Layer Type</th>
          <th class="px-2.5 py-1.5 text-[11px]">Complexity per Layer</th>
          <th class="px-2.5 py-1.5 text-[11px]">Sequential Ops</th>
          <th class="px-2.5 py-1.5 text-[11px]">Maximum Path Length</th>
          <th class="px-2.5 py-1.5 text-[11px]">Đánh giá bản chất</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="r in rows" :key="r.id"
          class="border-b border-gray-100 dark:border-gray-800/60 transition-colors"
          :class="[
            r.id === highlight 
              ? 'bg-blue-500/10 dark:bg-blue-900/30 font-semibold' 
              : 'hover:bg-gray-50/50 dark:hover:bg-gray-800/30 opacity-80'
          ]">
          <td class="px-2.5 py-1 flex items-center gap-1.5">
            <span v-if="r.id === highlight" class="w-1.5 h-1.5 rounded-full bg-blue-500 animate-ping"></span>
            <span>{{ r.type }}</span>
            <span v-if="r.badge" class="ml-1 text-[9px] px-1.5 py-0.2 rounded font-mono uppercase bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300">
              {{ r.badge }}
            </span>
          </td>
          <td class="px-2.5 py-1 font-mono" :class="r.id === highlight ? 'text-blue-600 dark:text-blue-400 font-bold' : ''">
            {{ r.complexity }}
          </td>
          <td class="px-2.5 py-1 font-mono" :class="r.id === highlight ? 'text-emerald-600 dark:text-emerald-400 font-bold' : ''">
            {{ r.sequential }}
          </td>
          <td class="px-2.5 py-1 font-mono" :class="r.id === highlight ? 'text-purple-600 dark:text-purple-400 font-bold' : ''">
            {{ r.maxPath }}
          </td>
          <td class="px-2.5 py-1 text-gray-600 dark:text-gray-400 text-[10.5px]">
            {{ r.pros }}
          </td>
        </tr>
      </tbody>
    </table>
    <div class="px-3 py-1 bg-gray-50 dark:bg-gray-900/60 text-[9.5px] text-gray-500 flex justify-between border-t border-gray-100 dark:border-gray-800">
      <span>* Trích xuất theo Table 1, Vaswani et al. (2017) "Attention Is All You Need".</span>
      <span class="font-mono">n: sequence length, d: representation dimension, k: kernel size</span>
    </div>
  </div>
</template>
