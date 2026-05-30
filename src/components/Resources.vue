<script setup>
import { ref } from 'vue'

// 定義資源分類與清單
const resourceCategories = ref([
  {
    title: '📸 免費高畫質圖庫',
    desc: '簡報需要高質感的背景或情境照時，這些網站是你的好幫手。',
    items: [
      { name: 'Unsplash', url: 'https://unsplash.com/', desc: '極具設計感與質感的免費攝影圖庫，全球創作者首選。' },
      { name: 'Pexels', url: 'https://www.pexels.com/', desc: '提供超高畫質相片與影片的免費素材網，商業用途也安全。' },
      { name: 'Pixabay', url: 'https://pixabay.com/', desc: '涵蓋向量圖、插畫與照片的綜合型素材庫。' }
    ]
  },
  {
    title: '✨ 質感 Icon 圖示',
    desc: '用圖示取代文字，讓簡報版面更清爽易讀。',
    items: [
      { name: 'Iconify', url: 'https://icones.js.org/', desc: '開源圖示大集合，收錄超過十萬個高質量 SVG 圖示。' },
      { name: 'Flaticon', url: 'https://www.flaticon.com/', desc: '全球最大的免費向量圖示庫，風格多樣齊全。' },
      { name: 'Google Fonts Icons', url: 'https://fonts.google.com/icons', desc: 'Google 官方推出的 Material Design 質感圖示。' }
    ]
  },
  {
    title: '🎨 靈感與配色工具',
    desc: '不知道簡報該用什麼顏色？來這裡找靈感吧。',
    items: [
      { name: 'Color Hunt', url: 'https://colorhunt.co/', desc: '設計師精選的復古、現代等多風格調色盤大集合。' },
      { name: 'Coolors', url: 'https://coolors.co/', desc: '超快速的配色生成工具，支援一鍵鎖定與複製色碼。' }
    ]
  }
])
</script>

<template>
  <div class="resources-page">
    <h2 class="page-title">實用設計資源庫</h2>
    <p class="intro-text">為你整理了簡報排版時最常使用到的優質設計資源，點擊即可開新分頁前往網站：</p>

    <div class="category-section" v-for="(category, index) in resourceCategories" :key="index">
      <div class="category-header">
        <h3>{{ category.title }}</h3>
        <p>{{ category.desc }}</p>
      </div>
      
      <div class="resource-grid">
        <a :href="item.url" target="_blank" rel="noopener noreferrer" class="resource-card" v-for="item in category.items" :key="item.name">
          <div class="card-content">
            <h4>{{ item.name }} <span class="arrow">↗</span></h4>
            <p>{{ item.desc }}</p>
          </div>
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.intro-text { margin-bottom: 3rem; color: var(--text-muted); font-size: 1.1rem; }

.category-section { margin-bottom: 3rem; }
.category-header h3 { font-size: 1.4rem; margin: 0 0 0.5rem 0; color: var(--text-main); }
.category-header p { color: var(--text-muted); margin-top: 0; margin-bottom: 1.5rem; }

.resource-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 1.5rem; }

.resource-card { background: var(--card-bg); border: 1px solid var(--nav-border); border-radius: 16px; padding: 1.5rem; text-decoration: none; color: inherit; transition: all 0.3s ease; display: flex; flex-direction: column; box-shadow: 0 4px 12px rgba(0,0,0,0.02); position: relative; overflow: hidden; }
.resource-card:hover { transform: translateY(-4px); box-shadow: 0 12px 24px rgba(0, 113, 227, 0.15); border-color: var(--primary-color); }

.resource-card h4 { margin: 0 0 0.8rem 0; color: var(--text-main); font-size: 1.2rem; display: flex; align-items: center; justify-content: space-between; transition: color 0.3s; }
.resource-card:hover h4 { color: var(--primary-color); }

.arrow { color: #a1a1a6; font-size: 1rem; transition: transform 0.3s, color 0.3s; }
.resource-card:hover .arrow { transform: translate(4px, -4px); color: var(--primary-color); }

.resource-card p { margin: 0; color: var(--text-muted); font-size: 0.95rem; line-height: 1.6; }
</style>