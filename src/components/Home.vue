<script setup>
import { ref } from 'vue'

// 定義要傳遞給父層的事件，用來切換頁面
const emit = defineEmits(['navigate'])

// 使用 ref 建立卡片資料
const features = ref([
  { id: 1, title: '乾淨排版', desc: '學習留白藝術，讓觀眾視線集中。', icon: '📐' },
  { id: 2, title: '字體選擇', desc: '告別新細明體，找出最適合的專業字型。', icon: 'Aa' },
  { id: 3, title: '精準配色', desc: '控制顏色數量，讓重點自然浮現。', icon: '🎨' }
])

// 實用小技巧資料 (擴展為 6 個，並加入背面教學內容)
const tips = ref([
  { id: 1, title: '一頁一重點', desc: '觀眾的注意力有限，試著將複雜的資訊拆分成多張投影片，避免版面擁擠。', icon: '🎯', backTitle: '💡 實踐方法', backDesc: '限制每頁文字在 50 字以內，或一次只講述一個核心概念。' },
  { id: 2, title: '善用對齊', desc: '無論是置左、置中或置右，將畫面中的文字與圖片對齊，能瞬間提升專業度。', icon: '📏', backTitle: '💡 實踐方法', backDesc: '開啟簡報軟體的「格線與輔助線」功能，確保邊緣完美對齊。' },
  { id: 3, title: '圖表簡化', desc: '去除多餘的格線、圖例與立體效果，直接在圖表上方標示出想傳達的關鍵數據。', icon: '📊', backTitle: '💡 實踐方法', backDesc: '把非重點數據的顏色調淡（如灰色），高亮你想強調的特定長條圖。' },
  { id: 4, title: '高對比度', desc: '確保文字與背景顏色有足夠的對比，讓坐在最後一排的觀眾也能輕鬆看清。', icon: '🌗', backTitle: '💡 實踐方法', backDesc: '深色背景配淺色文字，避免使用相近色（如深藍配黑）。' },
  { id: 5, title: '一致的視覺', desc: '全篇簡報使用相同的字體、顏色組合與圖示風格，建立專業品牌感。', icon: '✨', backTitle: '💡 實踐方法', backDesc: '建立「投影片母片」，預先設定好標題與內文的位置和專屬樣式。' },
  { id: 6, title: '高畫質素材', desc: '模糊、帶有浮水印或比例變形的圖片會大幅降低簡報的質感。', icon: '🖼️', backTitle: '💡 實踐方法', backDesc: '推薦使用 Unsplash、Pexels 等免費的高畫質圖庫網站。' }
])

// 追蹤被點擊翻轉的卡片 ID (為了支援手機版點擊翻轉)
const flippedIds = ref([])
const toggleFlip = (id) => {
  if (flippedIds.value.includes(id)) {
    flippedIds.value = flippedIds.value.filter(tipId => tipId !== id)
  } else {
    flippedIds.value.push(id)
  }
}
</script>

<template>
  <div class="home">
    <section class="hero">
      <h1 class="title">簡報設計入門</h1>
      <p class="subtitle">沒有設計基礎，也能做出乾淨、專業的高分報告。</p>
      <button class="btn-primary" @click="emit('navigate', 'Mistakes')">開始學習</button>
    </section>

    <!-- 使用 v-for 渲染特色卡片 -->
    <section class="features">
      <div class="card" v-for="feature in features" :key="feature.id">
        <div class="card-icon">{{ feature.icon }}</div>
        <h3>{{ feature.title }}</h3>
        <p>{{ feature.desc }}</p>
      </div>
    </section>

    <!-- 實用小技巧區塊 -->
    <section class="tips-section">
      <h2 class="section-title">💡 實用小技巧</h2>
      <div class="tips-grid">
        <div 
          class="tip-card-wrapper" 
          v-for="tip in tips" 
          :key="tip.id"
          @click="toggleFlip(tip.id)"
          @mouseleave="flippedIds = flippedIds.filter(id => id !== tip.id)"
        >
          <div class="tip-card-inner" :class="{ 'is-flipped': flippedIds.includes(tip.id) }">
            <!-- 卡片正面 -->
            <div class="tip-card-front">
              <div class="tip-icon">{{ tip.icon }}</div>
              <div class="tip-content">
                <h4>{{ tip.title }}</h4>
                <p>{{ tip.desc }}</p>
              </div>
            </div>
            <!-- 卡片背面 -->
            <div class="tip-card-back">
              <h4>{{ tip.backTitle }}</h4>
              <p>{{ tip.backDesc }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.home {
  position: relative;
}

.hero {
  text-align: center;
  padding: 4rem 2rem;
  /* 改用半透明背景 + 毛玻璃濾鏡，讓光暈能透出來 */
  background: radial-gradient(circle at 10% 20%, rgba(0, 113, 227, 0.1), transparent 40%), rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid var(--nav-border);
  border-radius: 24px;
  margin-bottom: 2rem;
  animation: fadeIn 0.8s ease-out;
}

body.dark-theme .hero {
  background: radial-gradient(circle at 10% 20%, rgba(0, 113, 227, 0.15), transparent 40%), rgba(30, 30, 30, 0.4);
}

.title {
  font-size: 3rem;
  font-weight: 700;
  letter-spacing: -0.5px;
  margin-bottom: 1rem;
}

.subtitle {
  font-size: 1.25rem;
  color: var(--text-muted);
  margin-bottom: 2.5rem;
}

.btn-primary {
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 0.8rem 2rem;
  border-radius: 30px;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 113, 227, 0.3);
}

.btn-primary:hover {
  transform: translateY(-2px) scale(1.02);
  background-color: #005bb5;
  box-shadow: 0 6px 20px rgba(0, 113, 227, 0.4);
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  margin-top: 2rem;
}

.hero, .features, .tips-section {
  position: relative;
  z-index: 1; /* Ensure content is above the blobs */
}

.card {
  background: var(--card-bg);
  padding: 2rem;
  border-radius: var(--border-radius);
  text-align: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease, background-color 0.3s ease;
  box-shadow: 0 4px 12px rgba(0,0,0,0.03);
  
  /* 卡片依序進場動畫 */
  animation: slideUp 0.6s ease-out both;
}

.card:nth-child(1) { animation-delay: 0.1s; }
.card:nth-child(2) { animation-delay: 0.2s; }
.card:nth-child(3) { animation-delay: 0.3s; }

@keyframes slideUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 24px rgba(0,0,0,0.08);
}

/* 複雜的隨機飄浮動畫 */
@keyframes float-blob {
  0% { transform: translate(0, 0) scale(1); }
  33% { transform: translate(40px, -50px) scale(1.1); }
  66% { transform: translate(-30px, 30px) scale(0.9); }
  100% { transform: translate(50px, 60px) scale(1.05); }
}

/* 實用小技巧區塊樣式 */
.tips-section {
  margin-top: 4rem;
  animation: fadeIn 1s ease-out 0.4s both; /* 加入一點延遲進場的動畫 */
}

.section-title {
  font-size: 1.6rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  color: var(--text-main);
}

.tips-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.2rem;
}

/* 翻轉卡片外層容器 */
.tip-card-wrapper {
  perspective: 1000px;
  cursor: pointer;
}

/* 翻轉卡片內層控制 */
.tip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.6s cubic-bezier(0.4, 0.2, 0.2, 1);
  transform-style: preserve-3d;
}

/* 滑鼠懸浮 或 手機點擊時觸發翻轉 */
.tip-card-wrapper:hover .tip-card-inner,
.tip-card-inner.is-flipped {
  transform: rotateY(180deg);
}

/* 共通的正反面樣式 */
.tip-card-front, .tip-card-back {
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  border-radius: var(--border-radius);
  padding: 1.5rem;
  border: 1px solid var(--nav-border);
  box-sizing: border-box;
}

/* 卡片正面 */
.tip-card-front {
  display: flex;
  gap: 1.2rem;
  background: var(--card-bg);
  transition: box-shadow 0.3s ease, border-color 0.3s ease;
}

/* 卡片背面 */
.tip-card-back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform: rotateY(180deg);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: var(--primary-color);
  color: white;
  text-align: center;
  border-color: var(--primary-color);
  box-shadow: 0 6px 16px rgba(0,0,0,0.1);
}

.tip-card-wrapper:hover .tip-card-front {
  border-color: var(--text-muted);
  box-shadow: 0 6px 16px rgba(0,0,0,0.04);
}

.tip-icon { font-size: 1.8rem; line-height: 1.2; }
.tip-content h4 { margin: 0 0 0.4rem 0; font-size: 1.1rem; color: var(--text-main); }
.tip-content p { margin: 0; font-size: 0.95rem; color: var(--text-muted); line-height: 1.6; }

/* 背面文字排版 */
.tip-card-back h4 {
  margin: 0 0 0.6rem 0;
  color: #ffffff;
  font-size: 1.2rem;
  font-weight: bold;
}

.tip-card-back p {
  margin: 0;
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.9);
  line-height: 1.6;
}
</style>