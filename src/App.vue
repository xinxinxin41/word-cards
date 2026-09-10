<script setup>
// App.vue - 单词记忆卡片（Vue 3 组件化版）
import { ref, computed, watch } from 'vue'
import WordCard from './components/WordCard.vue'
import StatsBar from './components/StatsBar.vue'

// ===== 数据层 =====
const defaultWords = [
  { en: 'apple', zh: '苹果' },
  { en: 'book', zh: '书本' },
  { en: 'school', zh: '学校' },
  { en: 'student', zh: '学生' },
  { en: 'teacher', zh: '老师' },
  { en: 'computer', zh: '电脑' },
  { en: 'library', zh: '图书馆' },
  { en: 'homework', zh: '作业' },
  { en: 'weather', zh: '天气' },
  { en: 'friend', zh: '朋友' }
]

// 从 localStorage 读取（初始化时执行一次）
function load(key, fallback) {
  try {
    const v = JSON.parse(localStorage.getItem(key))
    return v === null ? fallback : v
  } catch {
    return fallback
  }
}

const customWords = ref(load('vue_card_custom', []))
const knownWords = ref(load('vue_card_known', []))

// 全部单词 = 默认 + 自定义
const words = computed(() => [...defaultWords, ...customWords.value])

// 待学队列：过滤掉已认识的
const queue = computed(() => words.value.filter(w => !knownWords.value.includes(w.en)))

// ===== 学习状态 =====
const index = ref(0) // 当前在队列中的位置（响应式）
const current = computed(() => queue.value[index.value]) // 当前单词

// 自动保存：knownWords / customWords 变化时写入 localStorage
watch(
  [knownWords, customWords],
  () => {
    localStorage.setItem('vue_card_known', JSON.stringify(knownWords.value))
    localStorage.setItem('vue_card_custom', JSON.stringify(customWords.value))
  },
  { deep: true }
)

// ===== 交互逻辑 =====
function handleKnown() {
  if (!current.value) return
  if (!knownWords.value.includes(current.value.en)) {
    knownWords.value.push(current.value.en)
  }
  // 队列变短后修正位置：如果 index 超出长度就归零
  if (index.value >= queue.value.length) index.value = 0
}

function handleUnknown() {
  if (!current.value) return
  index.value = (index.value + 1) % queue.value.length
}

// ===== 添加单词（表单校验） =====
const newEn = ref('')
const newZh = ref('')
const tip = ref('')

function addWord() {
  const en = newEn.value.trim()
  const zh = newZh.value.trim()
  if (!en || !zh) {
    tip.value = '请同时填写英文单词和中文释义！'
    return
  }
  if (words.value.some(w => w.en === en)) {
    tip.value = '该单词已存在！'
    return
  }
  customWords.value.push({ en, zh })
  newEn.value = ''
  newZh.value = ''
  tip.value = '已添加：' + en
}

function reset() {
  knownWords.value = []
  index.value = 0
}
</script>

<template>
  <main class="card">
    <h1>单词记忆卡片</h1>

    <!-- 统计组件：传入已认识/总数 -->
    <StatsBar :known="knownWords.length" :total="words.length" />

    <!-- 卡片组件：key 变化时强制重建，翻转状态自动复位 -->
    <WordCard
      v-if="current"
      :key="current.en + index"
      :en="current.en"
      :zh="current.zh"
      @known="handleKnown"
      @unknown="handleUnknown"
    />

    <!-- 全部学完 -->
    <div v-else class="done">
      <p class="done-emoji">🎉</p>
      <p class="done-text">全部学完啦！共 {{ words.length }} 个单词</p>
    </div>

    <p class="hint" v-if="current">第 {{ index + 1 }} / {{ queue.length }} 个待学单词</p>
    <p class="tip">{{ tip }}</p>

    <!-- 添加单词（v-model 双向绑定） -->
    <div class="add-form">
      <input v-model="newEn" type="text" placeholder="英文单词" maxlength="20">
      <input v-model="newZh" type="text" placeholder="中文释义" maxlength="20">
      <button @click="addWord">添加</button>
    </div>

    <button class="btn-reset" @click="reset">重新开始</button>
  </main>
</template>
