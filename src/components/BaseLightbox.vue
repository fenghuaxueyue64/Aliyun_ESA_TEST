<template>
  <teleport to="body">
    <transition name="lb-fade">
      <div v-if="open" class="lb-overlay" @click="$emit('close')">
        <button class="lb-close" aria-label="关闭" @click.stop="$emit('close')">&times;</button>
        <img class="lb-img" :src="src" :alt="alt" @click.stop />
        <p class="lb-hint">点击空白处关闭</p>
      </div>
    </transition>
  </teleport>
</template>

<script setup>
defineProps({
  open: { type: Boolean, default: false },
  src: { type: String, default: '' },
  alt: { type: String, default: '' },
})
defineEmits(['close'])
</script>

<style scoped>
.lb-overlay {
  position: fixed;
  inset: 0;
  z-index: 1000;
  background: rgba(5, 10, 20, 0.94);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4vh 4vw;
  cursor: zoom-out;
}
.lb-img {
  max-width: 100%;
  max-height: 88%;
  object-fit: contain;
  box-shadow: 0 24px 90px rgba(0, 0, 0, 0.65);
  border-radius: 4px;
  cursor: default;
}
.lb-close {
  position: absolute;
  top: 22px;
  right: 26px;
  width: 46px;
  height: 46px;
  border-radius: 50%;
  border: 1px solid rgba(201, 169, 110, 0.45);
  background: rgba(10, 22, 40, 0.6);
  color: var(--hs-gold);
  font-size: 28px;
  line-height: 1;
  cursor: pointer;
  transition: all var(--transition-fast);
}
.lb-close:hover {
  background: rgba(201, 169, 110, 0.2);
  border-color: var(--hs-gold);
}
.lb-hint {
  margin-top: 16px;
  color: rgba(255, 255, 255, 0.45);
  font-size: 12px;
  letter-spacing: 1px;
}
.lb-fade-enter-active,
.lb-fade-leave-active {
  transition: opacity 0.25s ease;
}
.lb-fade-enter-from,
.lb-fade-leave-to {
  opacity: 0;
}
</style>
