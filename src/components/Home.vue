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

// 實用小技巧資料
const tips = ref([
  { id: 1, title: '一頁一重點', desc: '觀眾的注意力有限，試著將複雜的資訊拆分成多張投影片，避免版面擁擠。', icon: '🎯' },
  { id: 2, title: '善用對齊', desc: '無論是置左、置中或置右，將畫面中的文字與圖片對齊，能瞬間提升專業度。', icon: '📏' },
  { id: 3, title: '圖表簡化', desc: '去除多餘的格線、圖例與立體效果，直接在圖表上方標示出想傳達的關鍵數據。', icon: '📊' }
])
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
        <div class="tip-card" v-for="tip in tips" :key="tip.id">
          <div class="tip-icon">{{ tip.icon }}</div>
          <div class="tip-content">
            <h4>{{ tip.title }}</h4>
            <p>{{ tip.desc }}</p>
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

.tip-card {
  display: flex;
  gap: 1.2rem;
  background: var(--card-bg);
  border: 1px solid var(--nav-border);
  padding: 1.5rem;
  border-radius: var(--border-radius);
  transition: all 0.3s ease;
}

.tip-card:hover {
  transform: translateY(-2px);
  border-color: var(--text-muted);
  box-shadow: 0 6px 16px rgba(0,0,0,0.04);
}

.tip-icon { font-size: 1.8rem; line-height: 1.2; }
.tip-content h4 { margin: 0 0 0.4rem 0; font-size: 1.1rem; color: var(--text-main); }
.tip-content p { margin: 0; font-size: 0.95rem; color: var(--text-muted); line-height: 1.6; }

</style>