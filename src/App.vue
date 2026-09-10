<template>
  <div class="app-container" :class="{ 'sidebar-open': sidebarOpen }">
    <!-- Header -->
    <header class="app-header" :class="{ scrolled: headerScrolled }">
      <div class="header-inner">
        <button class="menu-toggle" @click="sidebarOpen = !sidebarOpen" aria-label="菜单">
          <span></span><span></span><span></span>
        </button>
        <div class="header-logo" @click="goToSection('cover')">
          <img src="/art_res/0_logo.png" alt="HAOSHUN" class="logo-img" />
          <span class="logo-text">好顺门窗铝材</span>
        </div>
        <nav class="header-nav">
          <a v-for="item in navItems" :key="item.id"
             :class="{ active: currentSection === item.id }"
             @click.prevent="goToSection(item.id)"
             href="#">{{ item.label }}</a>
        </nav>
      </div>
    </header>

    <!-- Sidebar -->
    <aside class="sidebar" :class="{ open: sidebarOpen }">
      <div class="sidebar-overlay" @click="sidebarOpen = false"></div>
      <div class="sidebar-panel">
        <div class="sidebar-header">
          <img src="/art_res/0_logo.png" alt="HAOSHUN" class="sidebar-logo" />
          <h3>产品目录</h3>
        </div>
        <nav class="sidebar-nav">
          <a v-for="(item, idx) in catalogItems" :key="idx"
             :class="{ active: currentPage === idx }"
             @click="goToPage(idx); sidebarOpen = false"
             href="#">
            <span class="page-num">{{ String(idx + 1).padStart(2, '0') }}</span>
            <span class="page-title">{{ item.title }}</span>
          </a>
        </nav>
        <div class="sidebar-footer">
          <div class="contact-mini">
            <p>佛山好顺门窗铝型材</p>
            <p class="wechat">微信: 19019412199</p>
          </div>
        </div>
      </div>
    </aside>

    <!-- Main Content -->
    <main class="main-content">
      <!-- Cover Section -->
      <section id="cover" class="section cover-section" v-show="currentSection === 'cover'">
        <CoverPage @enter-catalog="goToSection('catalog')" />
      </section>

      <!-- Catalog Section -->
      <section id="catalog" class="section catalog-section" v-show="currentSection === 'catalog'">
        <CatalogViewer
          :pages="catalogPages"
          :initial-page="currentPage"
          @page-change="onPageChange"
        />
      </section>

      <!-- Price Section -->
      <section id="price" class="section price-section" v-show="currentSection === 'price'">
        <PricePage />
      </section>

      <!-- Contact Section -->
      <section id="contact" class="section contact-section" v-show="currentSection === 'contact'">
        <ContactPage @go-catalog="goToSection('catalog')" />
      </section>
    </main>

    <!-- Page Navigation (for catalog) -->
    <div class="page-nav-dock" v-if="currentSection === 'catalog'">
      <button @click="prevPage" :disabled="currentPage === 0" class="nav-btn">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M15 18l-6-6 6-6"/></svg>
      </button>
      <span class="page-indicator">{{ currentPage + 1 }} / {{ catalogPages.length }}</span>
      <button @click="nextPage" :disabled="currentPage >= catalogPages.length - 1" class="nav-btn">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 18l6-6-6-6"/></svg>
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import CoverPage from './components/CoverPage.vue'
import CatalogViewer from './components/CatalogViewer.vue'
import PricePage from './components/PricePage.vue'
import ContactPage from './components/ContactPage.vue'

// Navigation state
const currentSection = ref('cover')
const currentPage = ref(0)
const sidebarOpen = ref(false)
const headerScrolled = ref(false)

const navItems = [
  { id: 'cover', label: '首页' },
  { id: 'catalog', label: '产品目录' },
  { id: 'price', label: '报价信息' },
  { id: 'contact', label: '联系我们' },
]

// Catalog pages data - all product images
const catalogPages = computed(() => {
  const pages = []
  // Webp cover/spread pages (1-5)
  for (let i = 1; i <= 5; i++) {
    pages.push({
      src: `/art_res/${i}.webp`,
      type: 'spread',
      title: getSpreadTitle(i),
    })
  }
  // Product detail pages (6-36)
  for (let i = 6; i <= 36; i++) {
    pages.push({
      src: `/art_res/${i}.jpg`,
      type: 'product',
      title: `产品系列 ${i - 5}`,
    })
  }
  return pages
})

// Catalog items for sidebar (product pages only, with extracted titles)
const catalogItems = computed(() => {
  return catalogPages.value.map((p, i) => ({
    title: p.title,
    type: p.type,
  }))
})

function getSpreadTitle(num) {
  const titles = {
    1: '封面 · 品牌形象',
    2: '企业简介',
    3: '产品概览',
    4: '系列产品',
    5: '技术参数',
  }
  return titles[num] || `第 ${num} 页`
}

function goToSection(sectionId) {
  currentSection.value = sectionId
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

function goToPage(idx) {
  currentPage.value = idx
  currentSection.value = 'catalog'
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

function prevPage() {
  if (currentPage.value > 0) {
    currentPage.value--
  }
}

function nextPage() {
  if (currentPage.value < catalogPages.value.length - 1) {
    currentPage.value++
  }
}

function onPageChange(idx) {
  currentPage.value = idx
}

// Scroll handler for header
function handleScroll() {
  headerScrolled.value = window.scrollY > 50
}

// Keyboard navigation
function handleKeydown(e) {
  if (currentSection.value !== 'catalog') return
  if (e.key === 'ArrowLeft') prevPage()
  if (e.key === 'ArrowRight') nextPage()
  if (e.key === 'Escape') sidebarOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  window.addEventListener('keydown', handleKeydown)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  position: relative;
}

/* Header */
.app-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  background: rgba(10, 22, 40, 0.85);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-bottom: 1px solid rgba(201, 169, 110, 0.15);
  transition: all var(--transition-normal);
}

.app-header.scrolled {
  background: rgba(10, 22, 40, 0.95);
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
}

.header-inner {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 24px;
  height: 64px;
  display: flex;
  align-items: center;
  gap: 24px;
}

.menu-toggle {
  display: none;
  flex-direction: column;
  justify-content: center;
  gap: 5px;
  width: 32px;
  height: 32px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}

.menu-toggle span {
  display: block;
  width: 100%;
  height: 2px;
  background: var(--hs-gold);
  border-radius: 2px;
  transition: all var(--transition-fast);
}

.header-logo {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  flex-shrink: 0;
}

.logo-img {
  height: 36px;
  width: auto;
  object-fit: contain;
}

.logo-text {
  font-family: var(--font-serif);
  font-size: 18px;
  font-weight: 600;
  color: var(--hs-gold);
  letter-spacing: 2px;
}

.header-nav {
  display: flex;
  gap: 8px;
  margin-left: auto;
}

.header-nav a {
  padding: 8px 20px;
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  border-radius: 8px;
  transition: all var(--transition-fast);
  letter-spacing: 1px;
}

.header-nav a:hover {
  color: var(--hs-gold);
  background: rgba(201, 169, 110, 0.1);
}

.header-nav a.active {
  color: var(--hs-gold);
  background: rgba(201, 169, 110, 0.15);
}

/* Sidebar */
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  z-index: 200;
  pointer-events: none;
}

.sidebar.open {
  pointer-events: all;
}

.sidebar-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  opacity: 0;
  transition: opacity var(--transition-normal);
}

.sidebar.open .sidebar-overlay {
  opacity: 1;
}

.sidebar-panel {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 300px;
  background: var(--hs-navy);
  transform: translateX(-100%);
  transition: transform var(--transition-normal);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.sidebar.open .sidebar-panel {
  transform: translateX(0);
}

.sidebar-header {
  padding: 24px 20px;
  border-bottom: 1px solid rgba(201, 169, 110, 0.15);
  text-align: center;
}

.sidebar-logo {
  height: 48px;
  margin-bottom: 12px;
}

.sidebar-header h3 {
  font-family: var(--font-serif);
  color: var(--hs-gold);
  font-size: 16px;
  font-weight: 600;
  letter-spacing: 3px;
}

.sidebar-nav {
  flex: 1;
  overflow-y: auto;
  padding: 12px 0;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 11px 24px;
  color: rgba(255, 255, 255, 0.65);
  text-decoration: none;
  font-size: 13.5px;
  transition: all var(--transition-fast);
  border-left: 3px solid transparent;
}

.sidebar-nav a:hover {
  color: #fff;
  background: rgba(255, 255, 255, 0.05);
}

.sidebar-nav a.active {
  color: var(--hs-gold);
  background: rgba(201, 169, 110, 0.08);
  border-left-color: var(--hs-gold);
}

.page-num {
  font-size: 11px;
  color: var(--hs-gold);
  opacity: 0.6;
  font-weight: 600;
  min-width: 28px;
  font-family: var(--font-sans);
}

.page-title {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-footer {
  padding: 20px;
  border-top: 1px solid rgba(201, 169, 110, 0.12);
  background: rgba(0, 0, 0, 0.2);
}

.contact-mini p {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.5);
  line-height: 1.8;
}

.contact-mini .wechat {
  color: var(--hs-gold);
  font-weight: 500;
}

/* Main Content */
.main-content {
  padding-top: 64px;
  min-height: 100vh;
}

.section {
  min-height: calc(100vh - 64px);
}

/* Page Nav Dock */
.page-nav-dock {
  position: fixed;
  bottom: 32px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 90;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 10px 20px;
  background: rgba(10, 22, 40, 0.9);
  backdrop-filter: blur(16px);
  border-radius: 50px;
  border: 1px solid rgba(201, 169, 110, 0.25);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
}

.nav-btn {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border: 1px solid rgba(201, 169, 110, 0.3);
  background: transparent;
  color: var(--hs-gold);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.nav-btn:hover:not(:disabled) {
  background: rgba(201, 169, 110, 0.15);
  border-color: var(--hs-gold);
}

.nav-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.nav-btn svg {
  width: 18px;
  height: 18px;
}

.page-indicator {
  color: var(--hs-gold);
  font-size: 13px;
  font-weight: 500;
  min-width: 60px;
  text-align: center;
  font-variant-numeric: tabular-nums;
  letter-spacing: 1px;
}

/* Responsive */
@media (max-width: 768px) {
  .header-inner {
    padding: 0 14px;
    gap: 12px;
    height: 56px;
  }

  .menu-toggle {
    display: flex;
    width: 36px;
    height: 36px;
    padding: 6px;
  }

  .header-nav {
    display: none;
  }

  .logo-img {
    height: 30px;
  }

  .logo-text {
    display: none;
  }

  .sidebar-panel {
    width: min(280px, 86vw);
  }

  .main-content {
    padding-top: 56px;
  }

  .section {
    min-height: calc(100vh - 56px);
  }

  .page-nav-dock {
    bottom: max(12px, env(safe-area-inset-bottom));
    padding: 8px 14px;
    gap: 10px;
  }

  .nav-btn {
    width: 34px;
    height: 34px;
  }

  .page-indicator {
    font-size: 12px;
    min-width: 48px;
  }
}

@media (max-width: 380px) {
  .header-inner {
    padding: 0 10px;
  }

  .logo-img {
    height: 28px;
  }
}
</style>
