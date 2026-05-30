<script setup>
import { ref } from 'vue'

// 定義字體選項
const fontOptions = ref([
  { 
    id: 1, 
    name: '無襯線體 (黑體)', 
    family: '-apple-system, BlinkMacSystemFont, "Segoe UI", "Microsoft JhengHei", "Noto Sans TC", sans-serif', 
    tag: '✅ 最推薦', 
    desc: '筆畫粗細一致，沒有多餘的裝飾。在螢幕上閱讀最為清晰，給人現代、乾淨、專業的感覺，是絕大多數商業簡報的首選。' 
  },
  { 
    id: 2, 
    name: '襯線體 (明體)', 
    family: '"Times New Roman", "PMingLiU", "Noto Serif TC", serif', 
    tag: '⚠️ 謹慎使用', 
    desc: '筆畫有粗細變化，且末端帶有裝飾線條（襯線）。適合用在列印文件、文學作品，或想營造古典、高級感的「大標題」，但不建議用在大量內文。' 
  },
  { 
    id: 3, 
    name: '系統預設 (新細明體)', 
    family: '"MingLiU", serif', 
    tag: '❌ 避免使用', 
    desc: '這是許多軟體的預設字體，但因為筆畫過細，投影在螢幕上時常會糊在一起看不清楚，容易給人「沒有用心排版」的廉價感。' 
  }
])

// 預設選中第一個選項
const activeFont = ref(fontOptions.value[0])
</script>

<template>
  <div class="typography-page">
    <h2 class="page-title">字體挑選指南</h2>
    <p class="intro-text">俗話說「字體是簡報的靈魂」。試著點擊下方按鈕，觀察同一份內容使用不同字體時，帶來的質感差異：</p>

    <!-- 控制面板：選擇字體 -->
    <div class="font-controls">
      <button 
        v-for="font in fontOptions" 
        :key="font.id"
        class="font-btn"
        :class="{ active: activeFont.id === font.id }"
        @click="activeFont = font"
      >
        {{ font.name }}
      </button>
    </div>

    <!-- 字體特色解說 -->
    <div class="font-info">
      <h3>{{ activeFont.tag }}</h3>
      <p>{{ activeFont.desc }}</p>
    </div>

    <!-- 即時預覽區（模擬簡報畫面） -->
    <div class="preview-board">
      <div class="slide-mockup" :style="{ fontFamily: activeFont.family }">
        <h1 class="slide-title">2024 產品行銷策略</h1>
        <p class="slide-subtitle">打造更直覺的使用者體驗</p>
        <div class="slide-content">
          <p>
            本季度的核心目標在於<strong>提升品牌信任度</strong>與<strong>降低學習成本</strong>。
            透過重新設計的介面與優化後的視覺動線，我們預期能為使用者帶來以下改變：
          </p>
          <ul>
            <li>任務完成速度提升 25%</li>
            <li>新用戶留存率突破 60%</li>
            <li>客服詢問率降低 15%</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.intro-text { margin-bottom: 2rem; color: var(--text-muted); }

.font-controls {
  display: flex; gap: 1rem; margin-bottom: 1.5rem; flex-wrap: wrap;
}

.font-btn {
  padding: 0.8rem 1.5rem; border: 1px solid var(--nav-border); border-radius: 30px;
  background: var(--card-bg); color: var(--text-main); cursor: pointer; transition: all 0.3s ease;
  font-weight: 500;
}
.font-btn:hover { background: rgba(0, 113, 227, 0.05); border-color: var(--primary-color); }
.font-btn.active { background: var(--primary-color); color: #fff; border-color: var(--primary-color); box-shadow: 0 4px 12px rgba(0, 113, 227, 0.3); }

.font-info {
  background: rgba(0, 0, 0, 0.03); padding: 1.5rem; border-radius: 12px; margin-bottom: 2rem;
}
.dark-theme .font-info { background: rgba(255, 255, 255, 0.05); }
.font-info h3 { margin: 0 0 0.5rem 0; font-size: 1.1rem; }
.font-info p { margin: 0; color: var(--text-muted); line-height: 1.6; }

/* 模擬簡報畫面的設計 */
.preview-board {
  background: #e5e5e5; padding: 2rem; border-radius: 16px; display: flex; justify-content: center;
}
.dark-theme .preview-board { background: #2a2a2a; }

.slide-mockup {
  background: #ffffff; color: #333333; width: 100%; max-width: 600px; aspect-ratio: 16/9;
  padding: 3rem; border-radius: 8px; box-shadow: 0 10px 30px rgba(0,0,0,0.1);
  transition: all 0.3s ease; display: flex; flex-direction: column; justify-content: center;
}
.slide-title { font-size: 2.2rem; margin: 0 0 0.5rem 0; color: #1a1a1a; }
.slide-subtitle { font-size: 1.2rem; color: #666; margin: 0 0 2rem 0; border-bottom: 2px solid #0071e3; padding-bottom: 1rem; display: inline-block;}
.slide-content p { font-size: 1.1rem; line-height: 1.6; margin-bottom: 1rem; }
.slide-content ul { font-size: 1.1rem; line-height: 1.8; padding-left: 1.5rem; }
</style>