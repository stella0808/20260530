<script setup>
import { ref } from 'vue'

// 布林值狀態：用來判斷現在是否顯示「修改後」的畫面
const isAfter = ref(false)
</script>

<template>
  <div class="comparison-page">
    <h2 class="page-title">修改前 / 修改後 比較</h2>
    
    <div class="toggle-container">
      <span :class="{ active: !isAfter }" @click="isAfter = false">修改前</span>
      <button class="toggle-btn" @click="isAfter = !isAfter">
        切換視角
      </button>
      <span :class="{ active: isAfter }" @click="isAfter = true">修改後</span>
    </div>

    <!-- 使用 v-if / v-else 進行條件渲染 -->
    <div class="slide-container">
      <transition name="flip" mode="out-in">
        <div v-if="!isAfter" class="slide before">
          <div class="slide-content bad-layout">
            <div class="text-col">
              <h3>大數據分析報告</h3>
              <p>大數據是指無法在一定時間內用常規軟體工具對其內容進行擷取、管理和處理的資料集合。大數據的特點包含：大量、高速、多樣、真實。我們必須要好好分析它...</p>
              <ul><li>資料量大</li><li>速度快</li><li>多樣化</li></ul>
            </div>
            <!-- 塞入一張看起來隨意放置的圖片 -->
            <img src="https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=80" class="bad-image" alt="Data" />
          </div>
          <div class="analysis">❌ 問題：字體過小、行距擁擠、圖片隨意擺放且缺乏重點</div>
        </div>

        <div v-else class="slide after">
          <div class="slide-content good-layout">
            <div class="text-col">
              <h3>大數據分析 4V 特徵</h3>
              <div class="grid-4">
                <div class="v-card">Volume<br><small>大量</small></div>
                <div class="v-card">Velocity<br><small>高速</small></div>
                <div class="v-card">Variety<br><small>多樣</small></div>
                <div class="v-card">Veracity<br><small>真實</small></div>
              </div>
            </div>
            <!-- 相同圖片，但在修改後擁有更好的排版與圓角設計 -->
            <div class="image-col">
              <img src="https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=80" class="good-image" alt="Clean Data" />
            </div>
          </div>
          <div class="analysis success">✅ 改善：圖文分明、圖形化排版、去蕪存菁</div>
        </div>
      </transition>
    </div>
  </div>
</template>

<style scoped>
.toggle-container {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.toggle-container span {
  cursor: pointer;
  user-select: none; /* 防止使用者點擊過快時不小心反白選取文字 */
  transition: color 0.3s;
}

.toggle-btn {
  padding: 0.5rem 1.5rem;
  border-radius: 20px;
  border: 1px solid var(--text-muted);
  background: var(--card-bg);
  color: var(--text-main);
  cursor: pointer;
  transition: 0.3s;
}

.toggle-btn:hover { filter: brightness(0.95); }
.active { font-weight: bold; color: var(--primary-color); }

/* 外層容器設定 3D 視角 */
.slide-container {
  perspective: 1000px;
}

/* 翻轉動畫過渡設定 */
.slide.flip-enter-active,
.slide.flip-leave-active {
  transition: opacity 0.4s ease, transform 0.4s ease;
}
.flip-enter-from {
  opacity: 0;
  transform: rotateY(-90deg);
}
.flip-leave-to {
  opacity: 0;
  transform: rotateY(90deg);
}

.slide {
  aspect-ratio: 16 / 9;
  padding: 3rem 3rem 4rem 3rem; /* 增加底部留白，避免內容與絕對定位的 analysis 標籤重疊 */
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  position: relative;
  background: var(--card-bg);
  transition: background-color 0.3s ease;
}

.before { border: 4px solid #ffcccc; }
.after { border: 4px solid #ccffcc; }

/* 投影片內容的共用佈局 */
.slide-content {
  display: flex;
  gap: 2rem;
  align-items: center;
}

/* 修改前的糟糕排版樣式 */
.bad-layout { align-items: flex-start; }
.bad-image {
  width: 200px;
  border: 1px solid #ccc;
  transform: rotate(3deg); /* 故意讓圖片看起來沒對齊、隨意放置 */
}

/* 修改後的良好排版樣式 */
.good-layout .text-col { flex: 1.2; }
.good-layout .image-col { flex: 0.8; }
.good-image {
  width: 100%;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.grid-4 { 
  display: grid; 
  /* 使用 auto-fit 讓卡片在空間不夠時自動換行，增強彈性 */
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); 
  gap: 1.2rem; 
  margin-top: 2rem; 
}
.v-card { 
  background: var(--card-bg); 
  padding: 1.5rem; 
  text-align: center; 
  border-radius: 12px; 
  font-weight: bold;
  box-shadow: 0 4px 8px rgba(0,0,0,0.05); /* 增加陰影，讓卡片與背景產生層次 */
  border: 1px solid rgba(0,0,0,0.03);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.v-card:hover {
  transform: translateY(-5px); /* 加入微互動：滑鼠懸浮時微微浮起 */
  box-shadow: 0 8px 16px rgba(0,0,0,0.1);
}
.analysis { position: absolute; bottom: 1rem; left: 1rem; background: #ffebee; color: #c62828; padding: 0.5rem 1rem; border-radius: 8px; font-size: 0.9rem;}
.success { background: #e8f5e9; color: #2e7d32; }

/* 響應式設計：小螢幕時自動調整排版 */
@media (max-width: 768px) {
  .slide { aspect-ratio: auto; padding: 2rem 2rem 4rem 2rem; }
  .slide-content { flex-direction: column; }
}
</style>