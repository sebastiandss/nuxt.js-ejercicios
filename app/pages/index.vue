<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

// --- CONFIGURACIÓN ---
const targetText = "Lorem ipsum dolor sit amet consectetur adipisicing elit. Aspernatur doloribus voluptates est minus nemo labore consequuntur animi laborum quisquam sint! Accusamus earum quisquam, soluta illo minus a deserunt animi quam." 

// --- ESTADO ---
const userInput = ref('')
const startTime = ref(null)
const timer = ref(60)
const isFinished = ref(false)
const intervalId = ref(null)
const personalBest = ref(0) // Estado reactivo para el récord

onMounted(() => {
    // Buscamos si ya existe un récord guardado en el navegador
    const storedBest = localStorage.getItem('typing-personal-best')
    if (storedBest) {
        personalBest.value = parseInt(storedBest)
    }
})

// --- LÓGICA DE FINALIZACIÓN ---
const finishTest = () => {
    if (isFinished.value) return 
    
    isFinished.value = true
    clearInterval(intervalId.value)

    // 1. Calculamos resultados finales
    const finalWpm = calculateWpm()
    const finalAcc = calculateAccuracy()
    const finalRaw = calculateRawWpm()
    const timeUsed = 60 - timer.value

    // 2. Lógica de Personal Best (Récord)
    // Comparamos el resultado actual contra el que teníamos guardado
    let isNewRecord = false
    
    if (finalWpm > personalBest.value) {
        isNewRecord = true
        personalBest.value = finalWpm // Actualizamos la vista actual
        localStorage.setItem('typing-personal-best', finalWpm.toString()) // Guardamos persistente
    }

    // Redirección
    router.push({
        path: '/results',
        query: {
            wpm: finalWpm,
            acc: finalAcc,
            raw: finalRaw,
            time: timeUsed,
            isRecord: isNewRecord // Pasamos "true" o "false" a la otra página
        }
    })
}

// --- MANEJADORES DE INPUT ---
const handleInput = () => {
    if (!startTime.value && userInput.value.length === 1) {
        startTime.value = Date.now()
        intervalId.value = setInterval(() => {
            if (timer.value > 0) {
                timer.value--
            } else {
                finishTest()
            }
        }, 1000)
    }

    if (userInput.value.length === targetText.length) {
        finishTest()
    }
}

const resetTest = () => {
    userInput.value = ''
    timer.value = 60
    startTime.value = null
    isFinished.value = false
    clearInterval(intervalId.value)
}

// --- CÁLCULOS MATEMÁTICOS ---
const calculateWpm = () => {
    if (userInput.value.length === 0) return 0
    const timeElapsed = (60 - timer.value) || 1
    return Math.round((userInput.value.length / 5) / (timeElapsed / 60))
}

const calculateRawWpm = () => {
    if (userInput.value.length === 0) return 0
    const timeElapsed = (60 - timer.value) || 1
    return Math.round((userInput.value.length / 5) / (timeElapsed / 60))
}

const calculateAccuracy = () => {
    if (userInput.value.length === 0) return 100
    const correct = processedChars.value.filter(c => c.class === 'correct').length
    return Math.round((correct / userInput.value.length) * 100)
}

// --- COMPUTADOS VISUALES ---
const processedChars = computed(() => {
    return targetText.split('').map((char, index) => {
        let colorClass = ''
        if (index < userInput.value.length) {
            colorClass = userInput.value[index] === char ? 'correct' : 'wrong'
        }
        return { char, class: colorClass, isCurrent: index === userInput.value.length }
    })
})

const wpm = computed(() => calculateWpm())
const accuracy = computed(() => calculateAccuracy())

onUnmounted(() => clearInterval(intervalId.value))
</script>

<template>
  <div class="main-container">
    <TypingHeader :best="personalBest" />
    
    <TypingStats :wpm="wpm" :accuracy="accuracy" :timer="timer" />

    <TestOverlay 
      :showOverlay="!startTime || isFinished" 
      :isFinished="isFinished"
      @action="isFinished ? resetTest() : $refs.hiddenInput.focus()"
    >
        <main class="typing-area" @click="$refs.hiddenInput.focus()">
            <textarea 
                ref="hiddenInput" 
                v-model="userInput" 
                @input="handleInput" 
                class="hidden-input" 
                :disabled="isFinished" 
                autofocus
            ></textarea>
            
            <div class="text-render">
                <span 
                    v-for="(item, index) in processedChars" 
                    :key="index" 
                    :class="[item.class, { 'cursor': item.isCurrent }]"
                >
                    {{ item.char }}
                </span>
            </div>
        </main>
    </TestOverlay>

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
        display: flex;
        flex-direction: column;
    }
    
    .typing-area {
        font-size: 1.8rem; 
        line-height: 1.6; 
        position: relative; 
        color: #4b5563; 
        margin-top: 2rem;
        user-select: none; 
    }
    
    .hidden-input {
        position: absolute; 
        opacity: 0; 
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        cursor: default;
    }
    
    .correct {
        color: #29d631; 
    }
    .wrong { 
        color: #ef4444; 
        text-decoration: underline; 
    }
    
    .cursor {
        background-color: #e2b714; 
        color: #111; 
        border-radius: 2px;
    }
    
    .footer { 
        text-align: center; 
        margin-top: auto; 
        padding-top: 4rem;
    }
    
    .restart-btn { 
        background: #27272a; 
        color: #9ca3af; 
        border: none; 
        padding: 10px 24px; 
        border-radius: 6px; 
        cursor: pointer; 
        font-weight: 600; 
        transition: all 0.2s;
    }
    
    .restart-btn:hover {
        background: #3f3f46;
        color: #fff;
    }
</style>