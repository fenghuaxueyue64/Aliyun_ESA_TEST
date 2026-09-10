<template>
  <div class="contact-page">
    <div class="contact-bg"></div>
    <div class="contact-inner">
      <div class="contact-header">
        <span class="eyebrow">CONTACT US</span>
        <h1>联系我们</h1>
        <div class="divider"></div>
      </div>

      <div class="contact-grid">
        <!-- Brand card -->
        <div class="contact-card brand-card">
          <img src="/art_res/0_logo.png" alt="HAOSHUN" class="brand-logo" />
          <h2>佛山好顺门窗铝型材</h2>
          <p class="brand-en">FOSHAN HAOSHUN ALUMINUM PROFILES</p>
          <p class="brand-desc">专注系统门窗铝型材研发、生产与销售，匠心品质，服务全国。</p>
        </div>

        <!-- Info card -->
        <div class="contact-card info-card">
          <div class="info-item">
            <div class="info-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/>
              </svg>
            </div>
            <div class="info-text">
              <span class="info-label">微信 / WeChat</span>
              <span class="info-value">19019412199</span>
              <button class="copy-btn" @click="copyText('19019412199')">复制</button>
            </div>
          </div>

          <div class="info-item">
            <div class="info-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.13.96.36 1.9.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.91.34 1.85.57 2.81.7A2 2 0 0 1 22 16.92z"/>
              </svg>
            </div>
            <div class="info-text">
              <span class="info-label">电话 / Tel</span>
              <span class="info-value">19019412199</span>
            </div>
          </div>

          <div class="info-item">
            <div class="info-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M21 10c0 7-9 13-9 13s-9-6-9-13a9 9 0 0 1 18 0z"/><circle cx="12" cy="10" r="3"/>
              </svg>
            </div>
            <div class="info-text">
              <span class="info-label">地址 / Address</span>
              <span class="info-value">广东 · 佛山</span>
            </div>
          </div>
        </div>
      </div>

      <!-- QR / WeChat card -->
      <div class="wechat-banner">
        <div class="wechat-text">
          <h3>扫码或添加微信，获取专属报价</h3>
          <p>微信搜索 <strong>19019412199</strong>，备注「门窗型材」即可对接业务经理。</p>
        </div>
        <div class="wechat-qr">
          <div class="qr-placeholder">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
              <rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/>
              <rect x="3" y="14" width="7" height="7" rx="1"/><path d="M14 14h3v3M21 14v7M14 21h3"/>
            </svg>
            <span>微信二维码</span>
          </div>
        </div>
      </div>

      <footer class="contact-footer">
        <p>© 2026 佛山好顺门窗铝型材 · 版权所有</p>
      </footer>
    </div>

    <transition name="toast">
      <div class="toast" v-if="toast">{{ toast }}</div>
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const toast = ref('')

function copyText(text) {
  navigator.clipboard?.writeText(text).then(() => {
    toast.value = '已复制到剪贴板'
    setTimeout(() => (toast.value = ''), 2000)
  }).catch(() => {
    toast.value = '复制失败，请手动复制'
    setTimeout(() => (toast.value = ''), 2000)
  })
}
</script>

<style scoped>
.contact-page {
  position: relative;
  min-height: calc(100vh - 64px);
  background: linear-gradient(160deg, var(--hs-navy) 0%, #0d1e36 100%);
  overflow: hidden;
}

@media (max-width: 768px) {
  .contact-page {
    min-height: calc(100vh - 56px);
    min-height: calc(100dvh - 56px);
  }
}

.contact-bg {
  position: absolute;
  inset: 0;
  background-image:
    radial-gradient(circle at 20% 20%, rgba(201, 169, 110, 0.08), transparent 40%),
    radial-gradient(circle at 80% 60%, rgba(201, 169, 110, 0.06), transparent 40%);
}

.contact-inner {
  position: relative;
  z-index: 2;
  max-width: 1000px;
  margin: 0 auto;
  padding: 70px 24px 50px;
}

.contact-header {
  text-align: center;
  margin-bottom: 50px;
}

.eyebrow {
  display: inline-block;
  font-size: 12px;
  letter-spacing: 4px;
  color: var(--hs-gold);
  margin-bottom: 12px;
  font-weight: 600;
}

.contact-header h1 {
  font-family: var(--font-serif);
  font-size: clamp(28px, 5vw, 52px);
  color: #fff;
  font-weight: 700;
  letter-spacing: clamp(2px, 0.6vw, 4px);
  line-height: 1.3;
}

.divider {
  width: 80px;
  height: 2px;
  margin: 20px auto 0;
  background: linear-gradient(90deg, transparent, var(--hs-gold), transparent);
}

.contact-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 24px;
  margin-bottom: 32px;
}

.contact-card {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(201, 169, 110, 0.18);
  border-radius: 16px;
  padding: 36px;
  backdrop-filter: blur(10px);
}

.brand-card {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.brand-logo {
  height: 72px;
  margin-bottom: 20px;
}

.brand-card h2 {
  font-family: var(--font-serif);
  color: var(--hs-gold);
  font-size: 22px;
  font-weight: 600;
  letter-spacing: 2px;
  margin-bottom: 8px;
}

.brand-en {
  color: rgba(255, 255, 255, 0.5);
  font-size: 12px;
  letter-spacing: 1px;
  margin-bottom: 18px;
}

.brand-desc {
  color: rgba(255, 255, 255, 0.75);
  font-size: 14px;
  line-height: 1.8;
}

.info-card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 24px;
}

.info-item {
  display: flex;
  align-items: flex-start;
  gap: 16px;
}

.info-icon {
  width: 44px;
  height: 44px;
  flex-shrink: 0;
  border-radius: 12px;
  background: rgba(201, 169, 110, 0.12);
  color: var(--hs-gold);
  display: flex;
  align-items: center;
  justify-content: center;
}

.info-icon svg { width: 22px; height: 22px; }

.info-text {
  display: flex;
  flex-direction: column;
  gap: 4px;
  position: relative;
}

.info-label {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.5);
  letter-spacing: 1px;
}

.info-value {
  font-size: 18px;
  color: #fff;
  font-weight: 600;
  letter-spacing: 1px;
  font-variant-numeric: tabular-nums;
}

.copy-btn {
  position: absolute;
  right: 0;
  top: 0;
  background: transparent;
  border: 1px solid rgba(201, 169, 110, 0.4);
  color: var(--hs-gold);
  font-size: 12px;
  padding: 3px 10px;
  border-radius: 6px;
  cursor: pointer;
  transition: all var(--transition-fast);
}

.copy-btn:hover { background: var(--hs-gold); color: var(--hs-navy); }

/* WeChat banner */
.wechat-banner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  background: linear-gradient(135deg, rgba(201, 169, 110, 0.12), rgba(201, 169, 110, 0.04));
  border: 1px solid rgba(201, 169, 110, 0.25);
  border-radius: 16px;
  padding: 32px 36px;
  margin-bottom: 40px;
}

.wechat-text h3 {
  font-family: var(--font-serif);
  color: #fff;
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 8px;
}

.wechat-text p {
  color: rgba(255, 255, 255, 0.7);
  font-size: 14px;
}

.wechat-text strong { color: var(--hs-gold); }

.wechat-qr {
  flex-shrink: 0;
}

.qr-placeholder {
  width: 96px;
  height: 96px;
  border-radius: 12px;
  background: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 6px;
  color: var(--hs-navy);
}

.qr-placeholder svg { width: 44px; height: 44px; }
.qr-placeholder span { font-size: 10px; color: var(--hs-gray-500); }

.contact-footer {
  text-align: center;
  color: rgba(255, 255, 255, 0.4);
  font-size: 13px;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
}

/* Toast */
.toast {
  position: fixed;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  background: var(--hs-gold);
  color: var(--hs-navy);
  padding: 12px 28px;
  border-radius: 50px;
  font-size: 14px;
  font-weight: 600;
  z-index: 9999;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
}

.toast-enter-active, .toast-leave-active { transition: all 0.3s; }
.toast-enter-from, .toast-leave-to { opacity: 0; transform: translateX(-50%) translateY(20px); }

@media (max-width: 768px) {
  .contact-inner {
    padding: 40px 16px 36px;
  }

  .contact-header {
    margin-bottom: 32px;
  }

  .eyebrow {
    font-size: 11px;
    letter-spacing: 3px;
    margin-bottom: 10px;
  }

  .contact-header h1 {
    letter-spacing: 2px;
  }

  .divider {
    width: 56px;
    margin-top: 14px;
  }

  .contact-grid {
    grid-template-columns: 1fr;
    gap: 16px;
    margin-bottom: 20px;
  }

  .contact-card {
    padding: 24px 20px;
  }

  .brand-logo {
    height: 56px;
    margin-bottom: 14px;
  }

  .brand-card h2 {
    font-size: 18px;
    letter-spacing: 1px;
  }

  .brand-en {
    font-size: 11px;
    letter-spacing: 0.5px;
    margin-bottom: 14px;
  }

  .brand-desc {
    font-size: 13px;
    line-height: 1.7;
  }

  .info-card {
    gap: 18px;
  }

  .info-item {
    gap: 12px;
  }

  .info-icon {
    width: 40px;
    height: 40px;
  }

  .info-value {
    font-size: 16px;
  }

  .wechat-banner {
    flex-direction: column;
    text-align: center;
    padding: 22px 18px;
    margin-bottom: 28px;
    gap: 18px;
  }

  .wechat-text h3 {
    font-size: 17px;
    letter-spacing: 0.5px;
  }

  .wechat-text p {
    font-size: 13px;
    line-height: 1.7;
  }

  .qr-placeholder {
    width: 88px;
    height: 88px;
  }

  .contact-footer {
    font-size: 12px;
  }
}
</style>
