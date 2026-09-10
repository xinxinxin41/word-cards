<script setup>
// StatsBar.vue - 学习统计组件
// props：已认识数量、总数；用计算属性算出百分比
import { computed } from 'vue'

const props = defineProps({
  known: { type: Number, default: 0 },
  total: { type: Number, default: 0 }
})

const percent = computed(() => {
  if (props.total === 0) return 0
  return Math.round((props.known / props.total) * 100)
})
</script>

<template>
  <div class="stats">
    <p class="stat-text">已认识 {{ known }} / {{ total }} 个</p>
    <div class="progress">
      <div class="progress-bar" :style="{ width: percent + '%' }"></div>
    </div>
  </div>
</template>
