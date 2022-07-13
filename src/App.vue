<script setup>
  import { ref, computed, onMounted } from 'vue';


  const wins = ref(0)
  const draws = ref(0)
  const losses = ref(0)

  const choice = ref(null)
  const computerChoice = ref(null)
  const verdict = ref(null)

  const outcomes = {
    'rock': {
      'rock': 'draw',
      'paper': 'loss',
      'scissors': 'win'
    },
    'paper': {
      'rock': 'win',
      'paper': 'draw',
      'scissors': 'loss'
    },
    'scissors': {
      'rock': 'loss',
      'paper': 'win',
      'scissors': 'draw'
    }
  }


  // Main game functionality
  const play = c => {
    choice.value = c
    
    const choices = ['rock', 'paper', 'scissors']
    const random = Math.floor(Math.random() * choices.length)

    computerChoice.value = choices[random]
    const outcome = outcomes[choice.value][computerChoice.value]
    
    if (outcome === 'win') {
      wins.value++
      verdict.value = 'You won!'
    } else if (outcome === 'loss') {
      losses.value++
      verdict.value = 'You lost!'
    } else {
      draws.value++
      verdict.value = 'It\'s a draw.'
    }

    saveGame()
  }


  // Get percentage of wins
  const winPercentage = computed(() => {
    const total = wins.value + draws.value + losses.value
    return total ? (wins.value / total) * 100 : 0
  })


  // Save game in localStorage
  const saveGame = () => {
    localStorage.setItem('wins', wins.value)
    localStorage.setItem('draws', draws.value)
    localStorage.setItem('losses', losses.value)
  }


  // Load saved game from localStorage
  const loadGame = () => {
    wins.value = parseInt(localStorage.getItem('wins')) || 0
    draws.value = parseInt(localStorage.getItem('draws')) || 0
    losses.value = parseInt(localStorage.getItem('losses')) || 0
  }


  // Remove data from localStorage
  const resetValues = () => {
    wins.value = 0
    draws.value = 0
    losses.value = 0

    saveGame()
    resetRound()
  }


  // Reset round
  const resetRound = () => {
    choice.value = null
    computerChoice.value = null
    verdict.value = null
  }


  // Load game
  onMounted(() => {
    loadGame()
  })

</script>


<template>
  
  <div class="bg-white text-gray-600 text-center min-h-screen flex flex-col">
    <header class="container mx-auto p-6">
      <h1 class="text-4xl font-bold">Rock, Paper, Scissors</h1>
    </header>

    <main class="container mx-auto p-6 flex-1">

        <div v-if="choice === null" class="text-lg mb-8">
          Please, select an option:
        </div>

      <div v-if="choice === null" class="flex items-center justify-center">
        <button @click="play('rock')"
                class="bg-white rounded-lg shadow-lg w-64 md:p-12 mx-6 transition-colors duration-300 hover:bg-pink-500">
          
                <img src="./assets/RockIcon.svg" alt="Rock" class="w-full">
        </button>

        <button @click="play('paper')"
                class="bg-white rounded-lg shadow-lg w-64 md:p-12 mx-6 transition-colors duration-300 hover:bg-green-500">
          
                <img src="./assets/PaperIcon.svg" alt="Paper" class="w-full">
        </button>

        <button @click="play('scissors')"
                class="bg-white rounded-lg shadow-lg w-64 md:p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500">
          
                <img src="./assets/ScissorsIcon.svg" alt="Scissors" class="w-full">
        </button>
      </div>

      <div v-else>
        <div class="text-2xl mb-4">
          You picked <span class="text-pink-500">{{ choice }}</span>.
        </div>

        <div class="text-2xl mb-4">
          The computer picked <span class="text-green-500">{{ computerChoice }}</span>.
        </div>

        <div class="text-5xl mb-12">{{ verdict }}</div>

        <button @click="resetRound" class="bg-pink-500 text-white rounded text-lg py-2 px-4 hover:bg-pink-400">Play another round</button>
        
        <br>

        <button @click="resetValues" class="bg-pink-500 text-white rounded text-lg mt-6 py-2 px-4 hover:bg-pink-400">Reset game</button>

      </div>

      <div class="mt-20 text-2xl mb-4">
        <span class="font-bold">Wins:</span> {{ wins }}  .  
        <span class="font-bold">Draws:</span> {{ draws }}  .  
        <span class="font-bold">Losses:</span> {{ losses }}
      </div>

      <div class="text-lg">
        Win rate: {{ Math.round(winPercentage) }}%
      </div>

      
    </main>

  </div>

</template>
