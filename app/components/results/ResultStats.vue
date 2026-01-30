<template>
  <div class="stats-grid">
    <div class="stat-card main-stat">
      <div class="label">WPM</div>
      <div class="value big-value">{{ wpm }}</div>
    </div>

    <div class="stat-card">
      <div class="label">Accuracy</div>
      <div class="value">{{ accuracy }}%</div>
    </div>

    <div class="stat-card">
      <div class="label">Characters</div>
      <div class="value">
        <span class="correct">{{ correctChars }}</span>
        <span class="divider">/</span>
        <span class="wrong">{{ wrongChars }}</span>
      </div>
    </div>

    <div class="stat-card">
      <div class="label">Time</div>
      <div class="value">{{ time }}s</div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  wpm: { type: [Number, String], default: 0 },
  accuracy: { type: [Number, String], default: 0 },
  correctChars: { type: [Number, String], default: 0 },
  wrongChars: { type: [Number, String], default: 0 },
  time: { type: [Number, String], default: 60 }
})
</script>

<style scoped>
.stats-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr); /* 2 columnas en móviles */
  gap: 1.5rem;
  margin-bottom: 2rem;
}

@media (min-width: 768px) {
  .stats-grid {
    grid-template-columns: 2fr 1fr 1fr 1fr; /* WPM ocupa más espacio en escritorio */
  }
}

.stat-card {
  background: #111; /* Un poco más claro que el fondo */
  padding: 1.5rem;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.label {
  color: #6b7280; /* Gris medio */
  font-size: 0.9rem;
  font-weight: 600;
}

.value {
  color: #fff;
  font-size: 2rem;
  font-weight: 700;
  line-height: 1;
}

.big-value {
  color: #3b82f6; /* Azul principal */
  font-size: 3.5rem;
}

.correct { color: #22c55e; }
.wrong { color: #ef4444; }
.divider { color: #374151; margin: 0 5px; font-size: 1.5rem; }
</style>