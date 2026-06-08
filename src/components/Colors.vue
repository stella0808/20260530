<script setup>
import { ref, computed } from 'vue'

// 可用的配色主題資料
const themes = ref([
  { id: 1, name: '專業報告 (商務)', colors: ['#1A365D', '#2B6CB0', '#E2E8F0', '#F56565'] },
  { id: 2, name: '文青風格 (教育)', colors: ['#4A3B32', '#85786D', '#F9F6F0', '#D48C70'] },
  { id: 3, name: '科技簡潔 (軟體)', colors: ['#1A202C', '#4A5568', '#EDF2F7', '#3182CE'] }
])

// 當前選中的主題 ID
const activeThemeId = ref(1)

// 使用 computed 計算屬性，當 activeThemeId 改變時，自動更新選中的主題物件
const activeTheme = computed(() => {
  return themes.value.find(t => t.id === activeThemeId.value)
})

// 複製顏色到剪貼簿功能
const copiedColor = ref('')
const copyToClipboard = async (color) => {
  try {
    await navigator.clipboard.writeText(color)
    copiedColor.value = color
    
    // 1.5 秒後恢復原狀顯示色碼
    setTimeout(() => {
      copiedColor.value = ''
    }, 1500)
  } catch (error) {
    console.error('複製失敗:', error)
  }
}

// 下載範本功能 (動態產生當前配色的 JSON 檔案)
const downloadTemplate = () => {
  const templateData = JSON.stringify({
    themeName: activeTheme.value.name,
    colors: activeTheme.value.colors,
    description: '這是一個自動生成的配色範本資料'
  }, null, 2)
  
  const blob = new Blob([templateData], { type: 'application/json' })
  const url = URL.createObjectURL(blob)
  
  const link = document.createElement('a')
  link.href = url
  link.download = `${activeTheme.value.name}_配色範本.json`
  document.body.appendChild(link)
  link.click()
  
  document.body.removeChild(link)
  URL.revokeObjectURL(url)
}
</script>

<template>
  <div class="colors-page">
    <h2 class="page-title">簡報配色推薦</h2>
    <p>點選下方按鈕，查看不同風格的配色建議：</p>
    
    <div class="theme-buttons">
      <button 
        v-for="theme in themes" 
        :key="theme.id"
        :class="{ active: activeThemeId === theme.id }"
        @click="activeThemeId = theme.id"
      >
        {{ theme.name }}
      </button>
    </div>

    <div class="palette-showcase">
      <h3>{{ activeTheme.name }}</h3>
      <div class="colors-grid">
        <div 
          v-for="color in activeTheme.colors" 
          :key="color"
          class="color-block"
          :style="{ backgroundColor: color }"
        @click="copyToClipboard(color)"
        title="點擊複製色碼"
        >
        <span>{{ copiedColor === color ? '✅ 已複製' : color }}</span>
        </div>
      </div>

      <!-- 新增：顏色實際應用範例區塊 -->
      <div class="demo-section">
        <h4>💡 實際應用範例</h4>
        <div class="demo-ui" :style="{ backgroundColor: activeTheme.colors[2] }">
          <header class="demo-header" :style="{ borderBottom: `1px solid ${activeTheme.colors[1]}` }">
            <div class="demo-logo" :style="{ color: activeTheme.colors[0] }">MyBrand</div>
            <button 
              class="demo-btn" 
              :style="{ backgroundColor: activeTheme.colors[3], color: '#ffffff' }"
              @click="downloadTemplate"
            >
              下載範本
            </button>
          </header>
          <main class="demo-main">
            <h1 class="demo-title" :style="{ color: activeTheme.colors[0] }">用色彩說服觀眾</h1>
            <p class="demo-text" :style="{ color: activeTheme.colors[1] }">
              大面積使用淺色做為背景，深色作為主要文字，並將最亮眼的點綴色保留給關鍵數據或行動呼籲按鈕。
            </p>
            <div class="demo-chart">
              <div class="demo-bar" :style="{ backgroundColor: activeTheme.colors[0], width: '85%' }"></div>
              <div class="demo-bar" :style="{ backgroundColor: activeTheme.colors[1], width: '55%' }"></div>
              <div class="demo-bar" :style="{ backgroundColor: activeTheme.colors[3], width: '40%' }"></div>
            </div>
          </main>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.theme-buttons { 
  display: flex; 
  justify-content: center; /* 讓按鈕置中 */
  gap: 1rem; 
  margin-top: 1.5rem; /* 增加與上方文字的距離 */
  margin-bottom: 2rem; 
  flex-wrap: wrap; 
}

.theme-buttons button {
  padding: 0.6rem 1.2rem;
  border: 1px solid var(--nav-border);
  border-radius: 8px;
  background: var(--card-bg);
  color: var(--text-main);
  cursor: pointer;
  transition: 0.2s;
}

.theme-buttons button.active {
  background: var(--primary-color);
  color: white;
  border-color: var(--primary-color);
}

.palette-showcase { background: var(--card-bg); padding: 2rem; border-radius: var(--border-radius); box-shadow: 0 4px 12px rgba(0,0,0,0.05); transition: background-color 0.3s ease; }
.colors-grid { display: flex; gap: 1rem; margin-top: 1rem; height: 100px;}
.color-block { 
  flex: 1; border-radius: 8px; display: flex; align-items: flex-end; justify-content: center; padding-bottom: 0.5rem; color: white; font-size: 0.85rem; font-weight: bold; text-shadow: 0 1px 3px rgba(0,0,0,0.5);
  cursor: pointer;
  transition: transform 0.2s, filter 0.2s;
}
.color-block:hover {
  transform: translateY(-5px);
  filter: brightness(1.1);
}

/* 實際應用範例的樣式 */
.demo-section {
  margin-top: 2.5rem;
  padding-top: 1.5rem;
  border-top: 1px dashed var(--nav-border);
}
.demo-section h4 {
  margin-bottom: 1rem;
  color: var(--text-main);
}
.demo-ui {
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 8px 24px rgba(0,0,0,0.08);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  transition: background-color 0.3s ease;
}
.demo-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
}
.demo-logo {
  font-weight: 800;
  font-size: 1.2rem;
}
.demo-btn {
  border: none;
  padding: 0.5rem 1.2rem;
  border-radius: 20px;
  font-weight: bold;
  cursor: pointer;
  transition: filter 0.2s ease, transform 0.2s ease;
}
.demo-btn:hover {
  filter: brightness(1.1);
  transform: scale(1.05);
}
.demo-main {
  padding: 2.5rem 1.5rem;
}
.demo-title { margin: 0 0 1rem 0; font-size: 1.6rem; }
.demo-text { margin: 0 0 2rem 0; font-size: 1rem; line-height: 1.6; }
.demo-chart {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}
.demo-bar {
  height: 16px;
  border-radius: 8px;
  transition: background-color 0.3s ease, width 0.5s ease-out;
}
</style>