<template>
  <div class="catalog">
    <!-- Reader -->
    <div class="reader" :class="{ 'is-fullscreen': isFullscreen }">
      <div class="reader-toolbar">
        <div class="toolbar-left">
          <span class="toolbar-title">{{ currentPageData.title }}</span>
        </div>
        <div class="toolbar-right">
          <button class="tool-btn" @click="toggleZoom" :title="zoomed ? '还原' : '放大'">
            <svg v-if="!zoomed" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="11" cy="11" r="7"/><path d="M21 21l-4-4M11 8v6M8 11h6"/>
            </svg>
            <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="11" cy="11" r="7"/><path d="M21 21l-4-4M8 11h6"/>
            </svg>
          </button>
          <button class="tool-btn" @click="prevPage" :disabled="currentIndex === 0" title="上一页">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M15 18l-6-6 6-6"/></svg>
          </button>
          <span class="page-count">{{ currentIndex + 1 }} / {{ pages.length }}</span>
          <button class="tool-btn" @click="nextPage" :disabled="currentIndex >= pages.length - 1" title="下一页">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 18l6-6-6-6"/></svg>
          </button>
          <button class="tool-btn" @click="toggleFullscreen" :title="isFullscreen ? '退出全屏' : '全屏'">
            <svg v-if="!isFullscreen" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M8 3H5a2 2 0 0 0-2 2v3M16 3h3a2 2 0 0 1 2 2v3M8 21H5a2 2 0 0 1-2-2v-3M16 21h3a2 2 0 0 0 2-2v-3"/>
            </svg>
            <svg v-else viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M8 3v3a2 2 0 0 1-2 2H3M21 8h-3a2 2 0 0 1-2-2V3M3 16h3a2 2 0 0 1 2 2v3M16 21v-3a2 2 0 0 1 2-2h3"/>
            </svg>
          </button>
        </div>
      </div>

      <div class="reader-stage"
           :class="{ 'zoomed': zoomed }"
           @click.self="zoomed && toggleZoom()">
        <transition :name="transitionName" mode="out-in">
          <div class="page-frame" :key="currentIndex">
            <img :src="currentPageData.src"
                 :alt="currentPageData.title"
                 class="page-image"
                 :class="{ 'zoomed': zoomed }"
                 @load="onImgLoad"
                 draggable="false" />
            <div class="page-loading" v-if="imgLoading">
              <div class="spinner"></div>
            </div>
          </div>
        </transition>
      </div>

      <!-- Click zones -->
      <div class="click-zone left" @click="prevPage" v-if="currentIndex > 0"></div>
      <div class="click-zone right" @click="nextPage" v-if="currentIndex < pages.length - 1"></div>
    </div>

    <!-- Thumbnail strip -->
    <div class="thumb-strip" v-if="!isFullscreen">
      <div class="thumb-track" ref="thumbTrack">
        <button v-for="(page, idx) in pages"
                :key="idx"
                class="thumb-item"
                :class="{ active: idx === currentIndex }"
                @click="goToPage(idx)">
          <img :src="page.src" :alt="page.title" loading="lazy" />
          <span class="thumb-num">{{ idx + 1 }}</span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, nextTick, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  pages: { type: Array, required: true },
  initialPage: { type: Number, default: 0 },
})

const emit = defineEmits(['page-change'])

const currentIndex = ref(props.initialPage)
const zoomed = ref(false)
const isFullscreen = ref(false)
const imgLoading = ref(true)
const transitionName = ref('slide-left')
const thumbTrack = ref(null)

const currentPageData = computed(() => props.pages[currentIndex.value])

watch(() => props.initialPage, (val) => {
  if (val !== currentIndex.value) goToPage(val)
})

function goToPage(idx) {
  if (idx < 0 || idx >= props.pages.length || idx === currentIndex.value) return
  transitionName.value = idx > currentIndex.value ? 'slide-left' : 'slide-right'
  currentIndex.value = idx
  zoomed.value = false
  emit('page-change', idx)
  scrollThumbIntoView(idx)
}

function prevPage() { goToPage(currentIndex.value - 1) }
function nextPage() { goToPage(currentIndex.value + 1) }

function toggleZoom() { zoomed.value = !zoomed.value }

function onImgLoad() {
  imgLoading.value = false
  nextTick(() => scrollThumbIntoView(currentIndex.value))
}

async function scrollThumbIntoView(idx) {
  await nextTick()
  if (!thumbTrack.value) return
  const el = thumbTrack.value.children[idx]
  if (el) el.scrollIntoView({ behavior: 'smooth', inline: 'center', block: 'nearest' })
}

function toggleFullscreen() {
  isFullscreen.value = !isFullscreen.value
}

// Keyboard
function handleKeydown(e) {
  if (e.key === 'ArrowLeft') prevPage()
  else if (e.key === 'ArrowRight') nextPage()
  else if (e.key === 'Escape' && zoomed.value) zoomed.value = false
}
window.addEventListener('keydown', handleKeydown)

watch(imgLoading, () => {})

onMounted(() => {
  imgLoading.value = true
})
onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown)
})
</script>

<style scoped>
.catalog {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 64px);
}

/* Reader */
.reader {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: #0a0e14;
  position: relative;
  overflow: hidden;
  transition: all var(--transition-normal);
}

.reader.is-fullscreen {
  position: fixed;
  inset: 0;
  z-index: 9999;
  height: 100vh;
}

.reader-toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 24px;
  background: rgba(10, 22, 40, 0.9);
  border-bottom: 1px solid rgba(201, 169, 110, 0.12);
  z-index: 10;
}

.toolbar-title {
  font-family: var(--font-serif);
  color: var(--hs-gold);
  font-size: 15px;
  letter-spacing: 2px;
  font-weight: 600;
}

.toolbar-right {
  display: flex;
  align-items: center;
  gap: 8px;
}

.tool-btn {
  width: 38px;
  height: 38px;
  border-radius: 8px;
  border: 1px solid rgba(201, 169, 110, 0.25);
  background: transparent;
  color: var(--hs-gold);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all var(--transition-fast);
}

.tool-btn:hover:not(:disabled) {
  background: rgba(201, 169, 110, 0.15);
  border-color: var(--hs-gold);
}

.tool-btn:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.tool-btn svg {
  width: 18px;
  height: 18px;
}

.page-count {
  color: rgba(255, 255, 255, 0.7);
  font-size: 13px;
  min-width: 54px;
  text-align: center;
  font-variant-numeric: tabular-nums;
  letter-spacing: 1px;
}

/* Stage */
.reader-stage {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background:
    radial-gradient(circle at 50% 40%, rgba(30, 45, 70, 0.4), transparent 70%),
    #0a0e14;
  padding: 24px;
}

.reader-stage.zoomed {
  cursor: zoom-out;
  overflow: auto;
}

.page-frame {
  position: relative;
  max-width: 100%;
  max-height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.6);
}

.page-image {
  max-width: 100%;
  max-height: calc(100vh - 200px);
  object-fit: contain;
  border-radius: 4px;
  user-select: none;
  transition: transform var(--transition-normal);
}

.reader-stage.zoomed .page-image {
  max-width: none;
  max-height: none;
  cursor: zoom-out;
  transform: scale(1.8);
  transform-origin: center;
}

.page-loading {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 3px solid rgba(201, 169, 110, 0.2);
  border-top-color: var(--hs-gold);
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Click zones */
.click-zone {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 25%;
  z-index: 5;
  cursor: pointer;
}
.click-zone.left { left: 0; }
.click-zone.right { right: 0; }

/* Thumbnail strip */
.thumb-strip {
  background: var(--hs-navy);
  border-top: 1px solid rgba(201, 169, 110, 0.12);
  padding: 12px 0;
  flex-shrink: 0;
}

.thumb-track {
  display: flex;
  gap: 10px;
  overflow-x: auto;
  padding: 0 24px;
  scroll-behavior: smooth;
}

.thumb-item {
  position: relative;
  flex-shrink: 0;
  width: 72px;
  height: 96px;
  border-radius: 6px;
  overflow: hidden;
  border: 2px solid transparent;
  background: #000;
  cursor: pointer;
  padding: 0;
  transition: all var(--transition-fast);
}

.thumb-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.6;
  transition: opacity var(--transition-fast);
}

.thumb-item:hover img { opacity: 0.85; }

.thumb-item.active {
  border-color: var(--hs-gold);
  box-shadow: 0 0 0 2px rgba(201, 169, 110, 0.3);
}

.thumb-item.active img { opacity: 1; }

.thumb-num {
  position: absolute;
  bottom: 2px;
  right: 4px;
  font-size: 10px;
  color: #fff;
  background: rgba(0, 0, 0, 0.6);
  padding: 1px 5px;
  border-radius: 3px;
  font-variant-numeric: tabular-nums;
}

/* Slide transitions */
.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}
.slide-left-enter-from { opacity: 0; transform: translateX(40px); }
.slide-left-leave-to { opacity: 0; transform: translateX(-40px); }
.slide-right-enter-from { opacity: 0; transform: translateX(-40px); }
.slide-right-leave-to { opacity: 0; transform: translateX(40px); }

@media (max-width: 768px) {
  .catalog {
    height: calc(100vh - 56px);
  }

  .reader-toolbar {
    padding: 8px 10px;
    gap: 8px;
  }

  .toolbar-title {
    font-size: 12px;
    letter-spacing: 1px;
    max-width: 40vw;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .toolbar-right {
    gap: 6px;
  }

  .tool-btn {
    width: 32px;
    height: 32px;
  }

  .page-count {
    font-size: 12px;
    min-width: 44px;
  }

  .reader-stage {
    padding: 8px;
  }

  .page-image {
    max-height: calc(100vh - 190px);
  }

  .reader.is-fullscreen {
    height: 100dvh;
  }

  .thumb-strip {
    padding: 8px 0;
  }

  .thumb-item {
    width: 52px;
    height: 70px;
  }

  .thumb-track {
    padding: 0 12px;
    gap: 8px;
  }
}
</style>
