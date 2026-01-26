<script setup>
defineProps({
  wpm: Number,
  accuracy: Number,
  timer: Number
})
</script>

<template>
  <section class="stats-bar">
    <div class="left-stats">
      <div class="stat-item">WPM: <span class="white">{{ wpm }}</span></div>
      <div class="divider">|</div>
      <div class="stat-item">Accuracy: <span class="red-text">{{ accuracy }}%</span></div>
      <div class="divider">|</div>
      <div class="stat-item">Time: <span class="yellow-text">0:{{ timer.toString().padStart(2, '0') }}</span></div>
    </div>

    <div class="right-controls">
      <div class="control-group">
        <span class="label">Difficulty:</span>
        <div class="static-btns">
          <span class="btn-ui">Easy</span>
          <span class="btn-ui">Medium</span>
          <span class="btn-ui active">Hard</span>
        </div>
      </div>
      <div class="control-group">
        <span class="label">Mode:</span>
        <div class="static-btns">
          <span class="btn-ui active">Timed (60s)</span>
          <span class="btn-ui">Passage</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
    .stats-bar {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        padding-bottom: 1.5rem;
        margin-bottom: 3rem;
    }
    .left-stats, .right-controls {
        display: flex;
        align-items: center;
        gap: 1.5rem; 
    }
    .control-group {
        display: flex; 
        align-items: center; 
        gap: 0.8rem; 
    }
    .static-btns {
        display: flex; 
        gap: 0.4rem;
    }
    .divider {
        color: #334155;
    }
    .white {
        color: white; 
        font-weight: 700; 
    }
    .red-text {
        color: #ef4444; 
        font-weight: 700; 
    }
    .yellow-text {
        color: #eab308; 
        font-weight: 700; 
    }
    .label {
        font-size: 0.85rem; 
        color: #94a3b8; 
    }
    .btn-ui {
        border: 1px solid #334155; 
        color: #94a3b8; 
        padding: 3px 12px; 
        border-radius: 6px; 
        font-size: 0.75rem; 
    }
    .btn-ui.active {
        border-color: #3b82f6; 
        color: #3b82f6; 
    }
</style>