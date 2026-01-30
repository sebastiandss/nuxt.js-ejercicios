<template>
  <div class="pb-container">
    <div class="icon-area">
      <span v-if="isNewRecord" class="crown">👑</span>
      <span v-else class="trophy">🏆</span>
    </div>
    
    <div class="info-area">
      <h3 class="pb-title">
        {{ isNewRecord ? 'New Personal Best!' : 'Personal Best' }}
      </h3>
      <p class="pb-value">{{ bestWpm }} WPM</p>
    </div>
  </div>
</template>

<script setup>
defineProps({
  bestWpm: { type: Number, default: 0 },
  isNewRecord: { type: Boolean, default: false }
})
</script>

<style scoped>
.pb-container {
  display: flex;
  align-items: center;
  gap: 1rem;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid #333;
  padding: 1rem 1.5rem;
  border-radius: 12px;
  margin-bottom: 2rem;
}

.icon-area { font-size: 2rem; }

.pb-title {
  color: #9ca3af;
  font-size: 0.85rem;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.pb-value {
  color: #fff;
  font-size: 1.5rem;
  font-weight: 700;
  margin: 0;
}

/* Efecto dorado si es nuevo récord */
.crown { animation: bounce 1s infinite; }
</style>