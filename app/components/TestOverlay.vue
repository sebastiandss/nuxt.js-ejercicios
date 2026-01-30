<template>
  <div class="overlay-wrapper">
    
    <div class="content-layer" :class="{ 'is-blurred': showOverlay && shouldShowButton }">
      <slot />
    </div>

    <div v-if="showOverlay && shouldShowButton" class="overlay-screen">
      <button @click="handleAction" class="primary-btn">
        Start Typing Test
      </button>
      
      <p class="instruction-text">
        Or click the text and start typing
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  showOverlay: Boolean,
  isFinished: Boolean
})

const emit = defineEmits(['action'])

const hasClickedStart = ref(false)

// Lógica simplificada:
// El botón solo aparece si NO hemos dado click a Start Y el test NO ha terminado.
const shouldShowButton = computed(() => {
  if (props.isFinished) return false // Si terminó, no mostrar nada (se irá a otra pág)
  return !hasClickedStart.value
})

const handleAction = () => {
  hasClickedStart.value = true
  emit('action')
}

// Si el test se reinicia externamente (vuelve a estado inicial), reseteamos el botón
watch(() => props.showOverlay, (newVal) => {
  if (newVal === true && !props.isFinished) {
    hasClickedStart.value = false
  }
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&display=swap');

.overlay-wrapper {
  position: relative;
  width: 100%;
}

.content-layer {
  transition: filter 0.3s ease, opacity 0.3s ease;
}

.is-blurred {
  filter: blur(4px);
  opacity: 0.5;
  pointer-events: none;
  user-select: none;
}

.overlay-screen {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 50;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: rgba(10, 10, 10, 0.3);
}

.primary-btn {
  font-family: 'Sora', sans-serif;
  background-color: #3b82f6;
  color: white;
  border: none;
  padding: 0.6rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.primary-btn:hover {
  background-color: #2563eb;
  transform: translateY(-1px);
  box-shadow: 0 10px 15px -3px rgba(59, 130, 246, 0.4);
}

.primary-btn:active {
  transform: scale(0.98);
}

.instruction-text {
  font-family: 'Sora', sans-serif;
  margin-top: 1rem;
  color: #9ca3af;
  font-size: 0.875rem;
  font-weight: 400;
}
</style>