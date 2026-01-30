<script setup>
    defineProps({
        best: Number
    })
</script>

<template>
  <header class="header">
    <div class="logo-section">
        <img src="/images/logo-large.svg" alt="">
    </div>
    <div class="score-section">
        <img src="/images/icon-personal-best.svg" alt="">
      <p class="best-score">Personal best: <span>{{ best }} WPM</span></p>
    </div>
  </header>

  
</template>

<style scoped>
    .header {
        display: flex; 
        justify-content: space-between; 
        align-items: center; 
        margin-bottom: 2.5rem; 
    }
    .logo-section, .score-section {
        display: flex; 
        align-items: center; 
        gap: 1rem; 
    }
    .best-score span {
        color: white; 
        font-weight: 700; 
    }
</style>