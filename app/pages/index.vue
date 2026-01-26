<script setup>
    import { ref, computed } from 'vue'

    const targetText = "Lorem ipsum dolor sit amet consectetur adipisicing elit. Aspernatur doloribus voluptates est minus nemo labore consequuntur animi laborum quisquam sint! Accusamus earum quisquam, soluta illo minus a deserunt animi quam." // El texto completo
    const userInput = ref('')
    const startTime = ref(null)
    const timer = ref(60)
    const isFinished = ref(false)
    const personalBest = ref(0)

    const handleInput = (e) => {
        if (!startTime.value && userInput.value.length === 1) {
            startTime.value = Date.now()
            const interval = setInterval(() => {
                if (timer.value > 0) timer.value--
                else {
                    clearInterval(interval); isFinished.value = true
                if (wpm.value > personalBest.value) personalBest.value = wpm.value
            }
            }, 1000)
        }
    }

    const processedChars = computed(() => {
        return targetText.split('').map((char, index) => {
            let colorClass = ''
            if (index < userInput.value.length) {
                colorClass = userInput.value[index] === char ? 'correct' : 'wrong'
            }
            return { char, class: colorClass, isCurrent: index === userInput.value.length }
        })
    })

    const wpm = computed(() => {
        if (userInput.value.length === 0) return 0
        return Math.round((userInput.value.length / 5) / ((60 - timer.value) / 60 || 1))
    })

    const accuracy = computed(() => {
        if (userInput.value.length === 0) return 100
        const correct = processedChars.value.filter(c => c.class === 'correct').length
        return Math.round((correct / userInput.value.length) * 100)
    })

    const resetTest = () => {
        userInput.value = ''; timer.value = 60; startTime.value = null; isFinished.value = false
    }
</script>



<template>
  <div class="main-container">
    <TypingHeader :best="personalBest" />
    
    <TypingStats :wpm="wpm" :accuracy="accuracy" :timer="timer" />

    <main class="typing-area" @click="$refs.hiddenInput.focus()">
      <textarea ref="hiddenInput" v-model="userInput" @input="handleInput" class="hidden-input" :disabled="isFinished" autofocus></textarea>
      <div class="text-render">
        <span v-for="(item, index) in processedChars" :key="index" :class="[item.class, { 'cursor': item.isCurrent }]">{{ item.char }}</span>
      </div>
    </main>

    <footer class="footer">
      <button class="restart-btn" @click="resetTest">Restart Test</button>
    </footer>
  </div>
</template> 



<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&display=swap');
    .main-container {
        min-height: 100vh; 
        background-color: rgb(15, 15, 15); 
        padding: 3rem 10%; 
        font-family: 'Sora', sans-serif; 
    }
    .typing-area {
        font-size: 1.8rem; 
        line-height: 1.6; 
        position: relative; 
        color: #4b5563; 
    }
    .hidden-input {
        position: absolute; 
        opacity: 0; 
        pointer-events: none; 
    }
    .correct {
        color: #22c55e; 
    }
    .wrong {
        color: #ef4444; 
        border-bottom: 2px solid #ef4444; 
        background: rgba(239, 68, 68, 0.1); 
    }
    .cursor {
        background-color: #475569; 
        color: white; 
    }
    .footer { 
        text-align: center; 
        margin-top: 4rem; 
    }
    .restart-btn { 
        background: #1e293b; 
        color: white; 
        border: 1px solid #334155; 
        padding: 12px 30px; 
        border-radius: 8px; 
        cursor: pointer; 
        font-weight: 600; 
    }
</style>