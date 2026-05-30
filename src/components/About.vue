<template>
  <div class="about-page">
    <h2 class="page-title">關於本站</h2>
    
    <div class="about-content">
      <h3>為什麼想做這個網站？</h3>
      <p>在大學期間，我發現很多同學的報告內容很精彩，但因為簡報排版不佳，導致簡報的吸引力下降。因此我希望能透過這個網站，幫助大家快速掌握基本的設計思維。</p>
      
      <h3>製作過程與技術</h3>
      <p>本站使用 <strong>Vue 3 (Composition API)</strong> 開發，以組件化的方式管理不同頁面。在設計上，刻意模仿了類似 Notion 的極簡留白風格，讓閱讀體驗更舒服。</p>
      <ul>
        <li><strong>狀態管理：</strong> 使用了 <code>ref</code> 與 <code>computed</code> 控制頁面與配色切換。</li>
        <li><strong>條件與列表：</strong> 運用 <code>v-if</code> 製作對比功能，<code>v-for</code> 渲染卡片資料。</li>
        <li><strong>RWD 響應式：</strong> 使用 CSS Flexbox / Grid 確保手機版也能正常閱讀。</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.about-content {
  background: var(--card-bg);
  padding: 2.5rem;
  border-radius: var(--border-radius);
  line-height: 1.8;
}

.about-content h3 {
  margin-top: 2rem;
  color: var(--primary-color);
}
.about-content h3:first-child { margin-top: 0; }
</style>