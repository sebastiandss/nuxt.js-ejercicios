<template>
  <div class="results-page">
    
    <TypingHeader :best="0" />

    <div class="layout-content">
      
      <div class="image-container animate-pop">
        
        <img 
          v-if="isRecord" 
          src="/images/icon-new-pb.svg" 
          alt="New Personal Best" 
          class="result-icon pb-icon"
        />
        
        <img 
          v-else 
          src="/images/icon-completed.svg" 
          alt="Test Completed" 
          class="result-icon"
        />
        
        <h2 class="status-text">
          {{ isRecord ? 'New Personal Best!' : 'Test Completed' }}
        </h2>
      </div>

      <div class="action-area">
        <button @click="goHome" class="restart-btn">
          Restart Test
        </button>
      </div>

    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import TypingHeader from '~/components/TypingHeader.vue'

const router = useRouter()
const route = useRoute()

// Leemos el parámetro de la URL. 
// Nota: Los query params siempre llegan como strings ("true"/"false")
const isRecord = computed(() => {
  return route.query.isRecord === 'true'
})

const goHome = () => {
  router.push('/')
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&display=swap');

.results-page {
  min-height: 100vh;
  background-color: rgb(15, 15, 15);
  color: #fff;
  font-family: 'Sora', sans-serif;
  padding: 3rem 10%;
  display: flex;
  flex-direction: column;
}

.layout-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 2.5rem;
}

/* --- ESTILOS DE LA IMAGEN --- */
.image-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}

.result-icon {
  width: 120px; /* Tamaño base */
  height: auto;
  object-fit: contain;
  filter: drop-shadow(0 0 20px rgba(59, 130, 246, 0.2)); /* Brillo azul sutil */
}

/* La copa puede ser un poco más grande o tener brillo dorado */
.pb-icon {
  width: 140px;
  filter: drop-shadow(0 0 25px rgba(234, 179, 8, 0.3)); /* Brillo dorado */
}

.status-text {
  font-size: 2rem;
  font-weight: 700;
  color: #e5e7eb;
}

/* Animación de entrada "Pop" */
.animate-pop {
  animation: popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
}

@keyframes popIn {
  0% { transform: scale(0.5); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}

/* --- BOTÓN --- */
.restart-btn {
  background: #3b82f6;
  color: white;
  border: none;
  padding: 14px 45px;
  border-radius: 12px;
  font-family: 'Sora', sans-serif;
  font-weight: 600;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
}

.restart-btn:hover {
  background-color: #2563eb;
  transform: translateY(-2px);
  box-shadow: 0 8px 12px rgba(37, 99, 235, 0.25);
}

.restart-btn:active {
  transform: scale(0.98);
}
</style>