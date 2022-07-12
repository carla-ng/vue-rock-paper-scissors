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
  const play = (c) => {
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
    wins.value = localStorage.getItem('wins')
    draws.value = localStorage.getItem('draws')
    losses.value = localStorage.getItem('losses')
  }


  // Reset round
  const resetRound = () => {
    choice.value = null
    computerChoice.value = null
    verdict.value = null
  }

</script>

<template>
  <h1>Hello, World!</h1>
</template>
