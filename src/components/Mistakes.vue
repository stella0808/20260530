<script setup>
import { ref } from 'vue'

const mistakes = ref([
  {
    id: 1,
    title: '字太多 (文字牆)',
    reason: '觀眾會直接開始讀字，而不再聽台上講話。',
    suggestion: '提煉關鍵字，善用條列式或圖表輔助說明。'
  },
  {
    id: 2,
    title: '顏色太亂',
    reason: '過多高飽和度色彩會讓視覺疲勞，找不到重點。',
    suggestion: '遵循「60-30-10」法則，主色不超過兩種。'
  },
  {
    id: 3,
    title: '圖片變形或畫質差',
    reason: '隨意拉伸圖片比例會顯得非常不專業。',
    suggestion: '按住 Shift 鍵等比例縮放，並使用高畫質免費圖庫 (如 Unsplash)。'
  }
])

// 狀態：記錄目前展開的卡片 ID (null 代表全關閉)
const activeId = ref(null)
const toggleCard = (id) => {
  activeId.value = activeId.value === id ? null : id
}
</script>

<template>
  <div class="mistakes-page">
    <h2 class="page-title">簡報常見錯誤</h2>
    <div class="mistake-list">
      <div class="mistake-card" v-for="item in mistakes" :key="item.id" @click="toggleCard(item.id)">
        <div class="card-header">
          <h3>❌ {{ item.title }}</h3>
          <span class="toggle-icon" :class="{ 'is-open': activeId === item.id }">▼</span>
        </div>
        <transition name="fade">
          <div class="content" v-show="activeId === item.id">
            <div class="divider"></div>
            <p><strong>原因：</strong> {{ item.reason }}</p>
            <p class="suggestion"><strong>💡 建議：</strong> {{ item.suggestion }}</p>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page-title {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.mistake-card {
  background: var(--card-bg);
  border: 1px solid var(--nav-border);
  border-radius: var(--border-radius);
  padding: 1.5rem;
  margin-bottom: 1.5rem;
  box-shadow: 0 2px 8px rgba(0,0,0,0.02);
  transition: background-color 0.3s ease, border-color 0.3s ease, transform 0.2s, box-shadow 0.2s;
  cursor: pointer;
}

.mistake-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.06);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.toggle-icon {
  color: var(--text-muted);
  font-size: 0.9rem;
  font-weight: bold;
  transition: transform 0.3s ease;
}

.toggle-icon.is-open {
  transform: rotate(180deg);
}

.mistake-card h3 {
  color: #d93025; /* 警告紅 */
  margin: 0;
}

.divider {
  height: 1px;
  background: var(--nav-border);
  margin: 1.2rem 0;
}

.suggestion {
  color: #0071e3;
}
</style>