<template>
  <div class="price-page">
    <div class="page-header">
      <div class="header-inner">
        <span class="eyebrow">PRICE LIST</span>
        <h1>2026 最新报价</h1>
        <p class="desc">佛山好顺门窗铝型材 · 系统门窗型材价格表</p>
      </div>
    </div>

    <div class="price-content">
      <!-- PDF Viewer -->
      <div class="price-card pdf-card">
        <div class="card-head">
          <h3>完整报价单（PDF）</h3>
          <a :href="pdfUrl" target="_blank" class="download-link">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4M7 10l5 5 5-5M12 15V3"/>
            </svg>
            打开 PDF
          </a>
        </div>
        <div class="pdf-frame">
          <iframe :src="pdfUrl" title="2026最新报价" loading="lazy"></iframe>
          <div class="pdf-fallback" v-if="!pdfSupported">
            <p>当前环境不支持内嵌预览，请点击上方「打开 PDF」按钮查看。</p>
          </div>
        </div>
      </div>

      <!-- Price Images -->
      <div class="price-card images-card">
        <div class="card-head">
          <h3>报价明细图</h3>
        </div>
        <div class="price-images">
          <figure v-for="(img, idx) in priceImages" :key="idx" class="price-figure">
            <img :src="img" :alt="`报价图 ${idx + 1}`" loading="lazy" @click="openLightbox(img)" />
          </figure>
        </div>
      </div>
    </div>

    <!-- Lightbox -->
    <transition name="fade">
      <div class="lightbox" v-if="lightboxImg" @click="lightboxImg = null">
        <img :src="lightboxImg" alt="预览" />
        <button class="close-btn" @click.stop="lightboxImg = null">×</button>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const pdfUrl = ref('/price/2026-price-list.pdf')
const priceImages = ref([
  '/price/1.png',
  '/price/2.png',
])
const lightboxImg = ref(null)
const pdfSupported = ref(true)

function openLightbox(img) {
  lightboxImg.value = img
}

onMounted(() => {
  // Detect if iframe PDF preview is feasible (basic check)
  const isMobile = /Android|iPhone|iPad|iPod/i.test(navigator.userAgent)
  pdfSupported.value = !isMobile
})
</script>

<style scoped>
.price-page {
  min-height: calc(100vh - 64px);
  background: linear-gradient(180deg, var(--hs-gray-50) 0%, #fff 100%);
}

@media (max-width: 768px) {
  .price-page {
    min-height: calc(100vh - 56px);
    min-height: calc(100dvh - 56px);
  }
}

.page-header {
  background: linear-gradient(135deg, var(--hs-navy) 0%, var(--hs-navy-light) 100%);
  padding: 60px 24px 50px;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.page-header::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 4px;
  background: linear-gradient(90deg, transparent, var(--hs-gold), transparent);
}

.eyebrow {
  display: inline-block;
  font-size: 12px;
  letter-spacing: 4px;
  color: var(--hs-gold);
  margin-bottom: 12px;
  font-weight: 600;
}

.page-header h1 {
  font-family: var(--font-serif);
  font-size: clamp(28px, 5vw, 48px);
  color: #fff;
  font-weight: 700;
  letter-spacing: clamp(2px, 0.6vw, 4px);
  margin-bottom: 12px;
  line-height: 1.3;
}

.desc {
  color: rgba(255, 255, 255, 0.7);
  font-size: clamp(13px, 2.5vw, 15px);
  letter-spacing: 1px;
  line-height: 1.7;
}

.price-content {
  max-width: var(--page-max-width);
  margin: 0 auto;
  padding: 48px 24px 80px;
  display: grid;
  grid-template-columns: 1fr;
  gap: 32px;
}

.price-card {
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 4px 30px rgba(10, 22, 40, 0.06);
  overflow: hidden;
  border: 1px solid var(--hs-gray-100);
}

.card-head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 24px;
  border-bottom: 1px solid var(--hs-gray-100);
}

.card-head h3 {
  font-family: var(--font-serif);
  font-size: 18px;
  color: var(--hs-navy);
  font-weight: 600;
  letter-spacing: 1px;
}

.download-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: var(--hs-gold-dark);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  padding: 6px 14px;
  border: 1px solid var(--hs-gold);
  border-radius: 8px;
  transition: all var(--transition-fast);
}

.download-link:hover {
  background: var(--hs-gold);
  color: #fff;
}

.download-link svg { width: 16px; height: 16px; }

.pdf-frame {
  position: relative;
  height: 700px;
  background: var(--hs-gray-100);
}

.pdf-frame iframe {
  width: 100%;
  height: 100%;
  border: none;
}

.pdf-fallback {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--hs-gray-500);
  font-size: 14px;
}

.price-images {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px;
  padding: 24px;
}

.price-figure {
  margin: 0;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(10, 22, 40, 0.08);
  cursor: zoom-in;
  transition: transform var(--transition-fast);
}

.price-figure:hover { transform: translateY(-4px); }

.price-figure img {
  width: 100%;
  display: block;
}

/* Lightbox */
.lightbox {
  position: fixed;
  inset: 0;
  z-index: 9999;
  background: rgba(0, 0, 0, 0.92);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px;
  cursor: zoom-out;
}

.lightbox img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  border-radius: 4px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.close-btn {
  position: absolute;
  top: 24px;
  right: 32px;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  font-size: 28px;
  cursor: pointer;
  transition: all var(--transition-fast);
}

.close-btn:hover { background: rgba(255, 255, 255, 0.2); }

.fade-enter-active, .fade-leave-active { transition: opacity 0.3s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }

@media (max-width: 768px) {
  .page-header {
    padding: 36px 18px 30px;
  }

  .eyebrow {
    font-size: 11px;
    letter-spacing: 3px;
    margin-bottom: 10px;
  }

  .page-header h1 {
    letter-spacing: 2px;
    margin-bottom: 10px;
  }

  .desc {
    font-size: 13px;
    letter-spacing: 0.5px;
  }

  .price-content { padding: 28px 14px 60px; }
  .card-head { padding: 16px 18px; gap: 12px; flex-wrap: wrap; }
  .card-head h3 { font-size: 16px; }
  .pdf-frame { height: 420px; }
  .price-images { grid-template-columns: 1fr; padding: 14px; gap: 14px; }
}
</style>
