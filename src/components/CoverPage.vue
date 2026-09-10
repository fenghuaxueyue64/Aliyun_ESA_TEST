<template>
  <div class="cover">
    <div class="cover-bg" :style="{ backgroundImage: `url(${coverImg})` }">
      <div class="cover-overlay"></div>
    </div>

    <div class="cover-content">
      <div class="cover-logo animate-fade-in-up">
        <img src="/art_res/0_logo.png" alt="HAOSHUN" />
      </div>
      <h1 class="cover-title animate-fade-in-up" style="animation-delay: 0.2s">
        好顺门窗铝型材
      </h1>
      <p class="cover-subtitle animate-fade-in-up" style="animation-delay: 0.4s">
        HAOSHUN ALUMINUM PROFILES
      </p>
      <div class="cover-divider animate-fade-in-up" style="animation-delay: 0.6s"></div>
      <p class="cover-tagline animate-fade-in-up" style="animation-delay: 0.8s">
        匠心品质 · 系统门窗型材专家
      </p>

      <button class="enter-btn animate-fade-in-up" style="animation-delay: 1s"
              @click="$emit('enter-catalog')">
        进入产品画册
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M5 12h14M13 6l6 6-6 6"/>
        </svg>
      </button>
    </div>

    <div class="cover-footer">
      <span>佛山好顺门窗铝型材</span>
      <span class="dot">·</span>
      <span>微信 19019412199</span>
    </div>

    <div class="scroll-hint">
      <div class="mouse"><div class="wheel"></div></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['enter-catalog'])
const coverImg = ref('/art_res/1.webp')
</script>

<style scoped>
.cover {
  position: relative;
  width: 100%;
  height: calc(100vh - 64px);
  min-height: 560px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.cover-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transform: scale(1.05);
  animation: slowZoom 20s ease-in-out infinite alternate;
}

@keyframes slowZoom {
  from { transform: scale(1.05); }
  to { transform: scale(1.12); }
}

.cover-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    160deg,
    rgba(10, 22, 40, 0.82) 0%,
    rgba(10, 22, 40, 0.65) 45%,
    rgba(10, 22, 40, 0.9) 100%
  );
}

.cover-content {
  position: relative;
  z-index: 2;
  text-align: center;
  padding: 0 20px;
  max-width: 800px;
  width: 100%;
}

.cover-logo {
  margin-bottom: 28px;
  opacity: 0;
}

.cover-logo img {
  height: clamp(56px, 14vw, 90px);
  width: auto;
  filter: drop-shadow(0 4px 20px rgba(201, 169, 110, 0.3));
}

.cover-title {
  font-family: var(--font-serif);
  font-size: clamp(30px, 6.5vw, 72px);
  font-weight: 700;
  color: #fff;
  letter-spacing: clamp(2px, 0.8vw, 8px);
  margin-bottom: 14px;
  text-shadow: 0 2px 30px rgba(0, 0, 0, 0.5);
  opacity: 0;
  line-height: 1.25;
  word-break: keep-all;
}

.cover-subtitle {
  font-size: clamp(11px, 1.8vw, 18px);
  letter-spacing: clamp(1.5px, 0.4vw, 6px);
  color: var(--hs-gold);
  font-weight: 500;
  margin-bottom: 24px;
  opacity: 0;
  word-break: keep-all;
  white-space: nowrap;
}

.cover-divider {
  width: 80px;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--hs-gold), transparent);
  margin: 0 auto 28px;
  opacity: 0;
}

.cover-tagline {
  font-size: clamp(14px, 2.2vw, 20px);
  color: rgba(255, 255, 255, 0.85);
  letter-spacing: clamp(1.5px, 0.4vw, 4px);
  margin-bottom: 40px;
  opacity: 0;
  line-height: 1.6;
}

.enter-btn {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 15px 40px;
  background: var(--hs-gold);
  color: var(--hs-navy);
  border: none;
  border-radius: 50px;
  font-size: 16px;
  font-weight: 600;
  letter-spacing: 2px;
  cursor: pointer;
  transition: all var(--transition-normal);
  box-shadow: 0 8px 30px rgba(201, 169, 110, 0.35);
  opacity: 0;
}

.enter-btn:hover {
  background: var(--hs-gold-light);
  transform: translateY(-2px);
  box-shadow: 0 12px 40px rgba(201, 169, 110, 0.5);
}

.enter-btn svg {
  width: 20px;
  height: 20px;
  transition: transform var(--transition-fast);
}

.enter-btn:hover svg {
  transform: translateX(4px);
}

.cover-footer {
  position: absolute;
  bottom: 40px;
  left: 0;
  right: 0;
  z-index: 2;
  text-align: center;
  color: rgba(255, 255, 255, 0.55);
  font-size: 13px;
  letter-spacing: 2px;
}

.cover-footer .dot {
  margin: 0 10px;
  color: var(--hs-gold);
}

.scroll-hint {
  position: absolute;
  bottom: 90px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 2;
}

.mouse {
  width: 24px;
  height: 40px;
  border: 2px solid rgba(255, 255, 255, 0.4);
  border-radius: 12px;
  display: flex;
  justify-content: center;
  padding-top: 8px;
}

.wheel {
  width: 4px;
  height: 8px;
  background: var(--hs-gold);
  border-radius: 2px;
  animation: wheelScroll 1.5s ease-in-out infinite;
}

@keyframes wheelScroll {
  0% { opacity: 0; transform: translateY(-4px); }
  50% { opacity: 1; transform: translateY(0); }
  100% { opacity: 0; transform: translateY(8px); }
}

@media (max-width: 768px) {
  .cover {
    height: calc(100vh - 56px);
    height: calc(100dvh - 56px);
    min-height: 480px;
  }

  .cover-content {
    padding: 0 18px;
  }

  .cover-logo {
    margin-bottom: 20px;
  }

  .cover-logo img {
    height: 56px;
  }

  .cover-title {
    letter-spacing: 3px;
    margin-bottom: 10px;
    line-height: 1.3;
  }

  .cover-subtitle {
    font-size: 11px;
    letter-spacing: 2px;
    margin-bottom: 18px;
    white-space: normal;
    line-height: 1.5;
  }

  .cover-divider {
    width: 56px;
    margin: 0 auto 18px;
  }

  .cover-tagline {
    font-size: 14px;
    letter-spacing: 2px;
    margin-bottom: 28px;
    line-height: 1.7;
  }

  .enter-btn {
    padding: 13px 28px;
    font-size: 14px;
    letter-spacing: 1.5px;
    width: min(100%, 260px);
    justify-content: center;
  }

  .cover-footer {
    bottom: max(16px, env(safe-area-inset-bottom));
    font-size: 11px;
    letter-spacing: 1px;
    line-height: 1.8;
    padding: 0 12px;
  }

  .cover-footer .dot {
    margin: 0 6px;
  }

  .scroll-hint {
    display: none;
  }
}

@media (max-width: 380px) {
  .cover-title {
    font-size: 26px;
    letter-spacing: 2px;
  }

  .cover-subtitle {
    font-size: 10px;
    letter-spacing: 1.5px;
  }

  .cover-tagline {
    font-size: 13px;
    letter-spacing: 1px;
  }

  .cover-footer {
    font-size: 10px;
  }
}
</style>
