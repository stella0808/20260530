<script setup>
import { ref, computed, watch } from 'vue'
import Home from './components/Home.vue'
import Mistakes from './components/Mistakes.vue'
import Comparison from './components/Comparison.vue'
import Colors from './components/Colors.vue'
import Typography from './components/Typography.vue'
import Resources from './components/Resources.vue'
import About from './components/About.vue'

// 定義所有的頁面元件
const routes = {
  Home,
  Mistakes,
  Comparison,
  Typography,
  Colors,
  Resources,
  About
}

// 當前所在的頁面名稱
const currentPage = ref('Home')

// 使用 computed 自動對應要渲染的元件
const currentComponent = computed(() => routes[currentPage.value])

// 監聽頁面切換：每次切換頁面時，平滑滾動回網頁頂部
watch(currentPage, () => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
})

// 實作深色模式 (Dark Mode) 切換
const isDarkMode = ref(false)
watch(isDarkMode, (isDark) => {
  if (isDark) {
    document.body.classList.add('dark-theme')
  } else {
    document.body.classList.remove('dark-theme')
  }
})
</script>

<template>
  <div class="app-container">
    <!-- 全站共用的動態光暈背景層 -->
    <div class="background-blobs">
      <div class="blob blob-1"></div>
      <div class="blob blob-2"></div>
      <div class="blob blob-3"></div>
      <div class="blob blob-4"></div>
      <div class="deco-shape shape-left">
        <svg viewBox="0 0 24 24" width="120" height="120" stroke="currentColor" stroke-width="0.5" fill="none"><circle cx="12" cy="12" r="10"></circle></svg>
      </div>
      <div class="deco-shape shape-right">
        <svg viewBox="0 0 24 24" width="100" height="100" stroke="currentColor" stroke-width="0.5" fill="none"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect></svg>
      </div>
      <div class="deco-shape shape-bottom-left">
        <svg viewBox="0 0 24 24" width="80" height="80" stroke="currentColor" stroke-width="0.5" fill="none"><polygon points="12 2 2 22 22 22"></polygon></svg>
      </div>
    </div>

    <!-- 頂部導覽列 -->
    <nav class="navbar">
      <div class="logo" @click="currentPage = 'Home'" title="回首頁">
        <svg viewBox="0 0 24 24" width="22" height="22" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <circle cx="13.5" cy="6.5" r=".5"></circle>
          <circle cx="17.5" cy="10.5" r=".5"></circle>
          <circle cx="8.5" cy="7.5" r=".5"></circle>
          <circle cx="6.5" cy="12.5" r=".5"></circle>
          <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10c1.02 0 1.5-.42 1.5-1.5 0-.42-.14-.81-.39-1.11-.24-.3-.42-.68-.42-1.08a1.5 1.5 0 0 1 1.5-1.5h2.15c2.84 0 5.16-2.32 5.16-5.16C21.5 6.24 17.26 2 12 2z"></path>
        </svg>
      </div>
      <div class="nav-links">
        <button :class="{ active: currentPage === 'Home' }" @click="currentPage = 'Home'">
          <svg viewBox="0 0 24 24" width="18" height="18" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
            <polyline points="9 22 9 12 15 12 15 22"></polyline>
          </svg>
          首頁
        </button>
        <button :class="{ active: currentPage === 'Mistakes' }" @click="currentPage = 'Mistakes'">常見錯誤</button>
        <button :class="{ active: currentPage === 'Comparison' }" @click="currentPage = 'Comparison'">修改對比</button>
        <button :class="{ active: currentPage === 'Typography' }" @click="currentPage = 'Typography'">字體挑選</button>
        <button :class="{ active: currentPage === 'Colors' }" @click="currentPage = 'Colors'">配色教學</button>
        <button :class="{ active: currentPage === 'Resources' }" @click="currentPage = 'Resources'">實用資源庫</button>
        <button :class="{ active: currentPage === 'About' }" @click="currentPage = 'About'">關於本站</button>
        <button class="theme-toggle" @click="isDarkMode = !isDarkMode">
          {{ isDarkMode ? '☀️ 淺色' : '🌙 深色' }}
        </button>
      </div>
    </nav>

    <!-- 內容切換區 (動態元件) -->
    <main class="main-content">
      <transition name="fade" mode="out-in">
        <component :is="currentComponent" @navigate="currentPage = $event" />
      </transition>
    </main>
  </div>
</template>

<style>
/* 全域 Notion/Apple 風格設定 */
:root {
  --bg-color: #ffffff;
  --text-main: #333333;
  --text-muted: #666666;
  --primary-color: #0071e3; /* Apple 藍 */
  --card-bg: #fbfbfd;
  --border-radius: 16px;
  --nav-bg: rgba(255, 255, 255, 0.8);
  --nav-border: #eaeaea;
  --bg-pattern: radial-gradient(#e5e7eb 2px, transparent 2px);
}

/* 深色模式變數 */
body.dark-theme {
  --bg-color: #121212;
  --text-main: #f5f5f7;
  --text-muted: #a1a1a6;
  --card-bg: #1e1e1e;
  --nav-bg: rgba(18, 18, 18, 0.8);
  --nav-border: #333333;
  --bg-pattern: radial-gradient(#333333 2px, transparent 2px);
}

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  overflow-x: hidden; /* 防止全寬度背景產生水平捲動條 */
  background-color: var(--bg-color);
  background-image: var(--bg-pattern);
  background-size: 32px 32px;
  color: var(--text-main);
  line-height: 1.6;
  transition: background-color 0.3s ease, color 0.3s ease;
}

/* 頁面切換的淡入淡出過渡動畫 */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s cubic-bezier(0.25, 1, 0.5, 1);
}
.fade-enter-from {
  opacity: 0;
  transform: translateY(15px) scale(0.98);
}
.fade-leave-to {
  opacity: 0;
  transform: translateY(-15px) scale(0.98);
}

button {
  font-family: inherit;
  color: inherit;
}

/* 確保所有大標題都強制跟隨深色模式，並加入顏色漸變動畫 */
h1, h2, h3, h4, h5, h6, .page-title {
  color: var(--text-main);
  transition: color 0.3s ease;
}

/* --- 全站動態光暈背景層設定 --- */
.background-blobs {
  position: fixed; /* 固定在視窗上，不隨捲動軸滾動 */
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  z-index: -1; /* 確保背景在最底層 */
  pointer-events: none;
}

.blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(140px);
  opacity: 0.15;
}

body.dark-theme .blob {
  opacity: 0.08;
}

.blob-1 { width: 400px; height: 400px; background: var(--primary-color); top: -10%; left: -10%; animation: float-blob 20s infinite ease-in-out alternate; }
.blob-2 { width: 300px; height: 300px; background: #8A2BE2; bottom: -5%; right: -5%; animation: float-blob 25s infinite ease-in-out alternate-reverse; }
.blob-3 { width: 350px; height: 350px; background: #00d2ff; top: 30%; right: -15%; animation: float-blob 22s infinite ease-in-out alternate; }
.blob-4 { width: 450px; height: 450px; background: #ff0080; bottom: 15%; left: -20%; animation: float-blob 28s infinite ease-in-out alternate-reverse; }

.deco-shape {
  position: absolute;
  color: var(--text-muted);
  opacity: 0.15;
  z-index: 0;
}

.shape-left { top: 15%; left: 8%; animation: float-shape 15s infinite ease-in-out; }
.shape-right { top: 35%; right: 10%; animation: float-shape 18s infinite ease-in-out reverse; }
.shape-bottom-left { bottom: 25%; left: 12%; animation: float-shape 22s infinite ease-in-out; }

@keyframes float-blob {
  0% { transform: translate(0, 0) scale(1); }
  33% { transform: translate(40px, -50px) scale(1.1); }
  66% { transform: translate(-30px, 30px) scale(0.9); }
  100% { transform: translate(50px, 60px) scale(1.05); }
}

@keyframes float-shape {
  0% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-30px) rotate(15deg); }
  100% { transform: translateY(0) rotate(360deg); }
}
/* ----------------------------- */

.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* 導覽列設計 */
.navbar {
  position: sticky;
  top: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1.5rem;
  background: var(--nav-bg);
  backdrop-filter: blur(12px); /* 毛玻璃效果 */
  border: 1px solid var(--nav-border);
  border-radius: 100px;
  width: 90%;
  max-width: 1000px;
  margin: 0 auto;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
  z-index: 100;
}

.logo {
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-main);
  cursor: pointer;
  padding: 0.4rem;
  border-radius: 50%;
  transition: all 0.3s cubic-bezier(0.25, 1, 0.5, 1);
}

.logo:hover {
  color: var(--primary-color);
  background: rgba(0, 113, 227, 0.1);
  transform: rotate(-10deg) scale(1.1);
}

/* 導覽列按鈕容器：在手機版時可以左右滑動 */
.nav-links {
  display: flex;
  align-items: center;
  overflow-x: auto;
  scrollbar-width: none; /* Firefox 隱藏捲動條 */
  -ms-overflow-style: none; /* IE/Edge 隱藏捲動條 */
}
.nav-links::-webkit-scrollbar { display: none; /* Chrome/Safari 隱藏捲動條 */ }

.nav-links button {
  background: transparent;
  border: none;
  font-size: 0.95rem;
  margin-left: 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  color: var(--text-muted);
  cursor: pointer;
  transition: all 0.2s ease;
  display: inline-flex;
  align-items: center;
  white-space: nowrap; /* 防止按鈕文字在小螢幕時被折行 */
  gap: 0.4rem;
}

.nav-links button:hover {
  color: var(--text-main);
  background: rgba(0, 0, 0, 0.05);
}

body.dark-theme .nav-links button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.nav-links button.active {
  color: var(--primary-color);
  font-weight: 600;
  background: rgba(0, 113, 227, 0.1);
}

.theme-toggle {
  flex-shrink: 0; /* 防止切換按鈕被擠壓 */
  margin-left: 1.5rem !important;
  padding: 0.5rem 1rem;
  border-radius: 20px !important;
  border: 1px solid var(--nav-border) !important;
  background: var(--card-bg) !important;
  color: var(--text-main) !important;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease !important;
}

.theme-toggle:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.main-content {
  flex: 1;
  max-width: 1000px;
  margin: 0 auto;
  padding: 3rem 1.5rem;
  width: 100%;
}
</style>