<script setup>
// WordCard.vue - 单词卡片组件
// props：父组件传入的单词数据；emit：向外抛事件
// 翻转状态由组件自己管理，认识/不认识交给父组件处理
import { ref } from 'vue'

const props = defineProps({
  en: { type: String, required: true },
  zh: { type: String, required: true }
})

const emit = defineEmits(['known', 'unknown'])

const flipped = ref(false) // 响应式状态：是否已翻面

function flip() {
  flipped.value = !flipped.value
}
</script>

<template>
  <div class="flashcard-wrap">
    <!-- :class 绑定：flipped 为 true 时加 flipped 类，触发 3D 翻转 -->
    <div class="flashcard" :class="{ flipped: flipped }" @click="flip">
      <div class="card-inner">
        <div class="card-face card-front">
          <p class="word">{{ en }}</p>
        </div>
        <div class="card-face card-back">
          <p class="word">{{ zh }}</p>
        </div>
      </div>
    </div>
    <div class="btns">
      <button class="btn-known" @click="emit('known')">认识</button>
      <button class="btn-unknown" @click="emit('unknown')">不认识</button>
    </div>
  </div>
</template>
